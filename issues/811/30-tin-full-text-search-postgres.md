---
layout: article
title: "Tin：为 Postgres 打造的全文搜索"
issue: 811
number: 30
category: data
original_url: "https://planetscale.com/blog/introducing-tin"
hn_url: "https://news.ycombinator.com/item?id=49766611"
date: 2026-09-25
---

## 文章摘要

这是 PlanetScale 于 2026 年 9 月 16 日发布的产品公告，作者是 Eric Ridge（ZomboDB 的作者，长期做 Postgres 与搜索引擎的结合）和 Patrick Reynolds。文章宣布推出 **TIN**（"Text INdex" 的缩写），一个 Postgres 全文搜索扩展，即日起以 GA 版本向 PlanetScale 上所有 Postgres 和 Neki 数据库开放。用法很简单：`CREATE INDEX ... USING tin(text_column)` 建索引，然后用新的 `==>` 运算符查询，例如 `WHERE text_column_name ==> 'some words'`。

**他们对一个「好的文本索引」列出了明确的要求：** 支持布尔表达式、短语查询和 span（跨度/邻近）查询；支持对词项做模糊、通配符和正则匹配；支持大小写与重音折叠；支持 `COUNT(*)` 查询和按 BM25 打分的 top-k 查询。放在 Postgres 里，它还必须能处理 join、横跨全文列和其他类型列的复杂 WHERE 条件、持续写入、复制、备份以及正确的事务可见性。文章说 Postgres 已有至少三种文本搜索索引，但没有一种能同时满足这些要求，而 TIN 可以，并且「快得令人难以置信」。

**三个典型场景**：电商平台要找包含全部关键词的前十个商品（`ORDER BY tin.score(ctid) DESC LIMIT 10`）；法律取证平台要返回包含一组关键词中任意一个的所有邮件，完全不关心排序；图片标签平台要精确统计带某个标签（如短语 "san francisco"）的照片数量。此外绝大多数应用都要在持续查询的同时插入、更新、删除文档，并要求提交后立刻能被搜到。

**基准测试。** 他们测过全部 Wikipedia、2.3 TB 的 Reddit 评论，以及一个名为 "pile" 的 797 GB 混合语料（开放获取论文、法律文书、公有领域书籍和 Enron 邮件）。文中公布的主要结果基于 Stack Exchange 问答导出数据：85 GB、1.5 亿篇文档。由于没有标准查询轨迹，他们随机截取 2 到 15 个词的子串，各按「合取（全部包含）」「析取（任一包含）」「短语」三种方式解释，共 3,762 条查询（9 月 20 日的勘误说明原文误写为 1,719 条）。测试机是 AWS i7i.8xlarge（本地 NVMe、支持 AVX-512），每个引擎跑在限制为 8 vCPU、32 GB 内存的 Postgres 18.6 容器里，刻意让索引装不进缓冲区。压测工具是 ParadeDB 自家的 Benchmarker（他们 fork 了一版，加入预热和读字节数、WAL 写入量指标），只改了 `max_parallel_workers`、`shared_buffers`、`maintenance_work_mem` 三个参数。对照组是 ParadeDB v0.25.2、Timescale 的 pg_textsearch v1.4.0 和 Postgres 18.6 内置的 GIN，其中只有 ParadeDB 能跑完全部测试。

主要数字如下：

- **建索引**：TIN 用 8 分 10 秒、32 GB 内存建出 50.7 GB 的索引；ParadeDB 19 分 20 秒、需 64 GB；pg_textsearch 26 分 49 秒、需 128 GB；GIN 耗时 2 小时 9 分、需 64 GB，但索引最小（28.0 GB）。除 TIN 外，其他三个都在 32 GB 限制下失败，只好临时加内存。
- **混合查询 top-10**：TIN 每秒查询数是 ParadeDB 的 25 倍（199 对 7.9），p99 延迟低 26 倍。GIN 在析取查询上内存耗尽，pg_textsearch 只支持析取，都没法参加。
- **合取与短语 top-10**：TIN 的吞吐是 ParadeDB 的 10 倍、GIN 的 541 倍，GIN 的 p99 高达 288 秒。
- **析取查询 + 每秒 1,000 次并发 UPDATE**：TIN 吞吐是 pg_textsearch 的 36 倍、ParadeDB 的 57 倍。十分钟内 TIN 完成了 27 万次更新，ParadeDB 18.5 万次，而 pg_textsearch 只有 735 次，原因是持续的读流量让写请求永远拿不到所需的锁，几秒钟后写入就停滞了。ParadeDB 则是牺牲读性能来接纳写入，吞吐从 17 QPS 跌到 2.2 QPS。
- **索引完全装进内存时**（Wikipedia 8 GB 语料，析取查询计数）：TIN 达到 10,260 QPS、p99 2 毫秒，ParadeDB 291 QPS，GIN 1.4 QPS。

文章特别强调「每次查询读取的 MB 数」这一列：TIN 每次查询读取的数据量通常只有对手的几分之一到十分之一。这不仅让它自己快，也减轻了对块缓存和 I/O 的压力，同一台服务器上的其他查询也能保持快速。结论是在各种场景下 TIN 吞吐至少高 8 倍，而且在每秒更新数百行的情况下性能只小幅下降。

**为什么这么快：核心是直接用 ctid 当文档 ID。** 传统文本索引把索引分成段（segment），段内 n 篇文档被编号为 1 到 n，连续编号便于用差分编码和位打包高度压缩倒排表，但不同段的编号互相独立（段 4 里的 42 号和段 7 里的 42 号是两篇不同的文档）。TIN 也分段，但不为了编号，而是直接使用 Postgres 的 ctid。ctid 是每个元组版本在堆中的物理位置，48 位，高 32 位是页号、低 16 位是页内偏移，形如 `(190, 17)`，可以 O(1) 直接定位。Postgres 内部处处都用 ctid：索引访问方法必须返回 ctid，位图扫描基于 ctid 位图，B-tree、GIN、GiST、hash 的倒排项也都是 ctid。凡是使用连续编号的搜索系统，最终都得把编号转换回 ctid，ParadeDB 和 pg_textsearch 为此各自维护了一个映射结构。一次搜索命中 1,000 万行，就要查 1,000 万次映射，而 TIN 完全省掉了这一步。

**48 位不连续的 ID 怎么压缩？** 常规压缩手段对它都不好用：差分编码在每个页边界处断裂，普通位图又太稀疏。但 Postgres 页的一些性质让两级位图编码变得可行：8KB 的页最多只能容纳 291 个元组（8192 减去 24 字节页头，再除以每个非空元组至少 28 字节），对于带 TEXT 列的表，一页往往只有 32 个或更少元组。于是页号列表足够稠密，可以用位图表示；每页内的偏移列表也足够稠密且小，可以用微型位图表示。整体下来，高频词接近每个 posting 1 bit，中频词约 7 bit，罕见词接近 25 bit，只出现一次的词根本不用位图存储。

**跳过工作与向量化。** TIN 的页级位图宽 256 位，正好装进一个 AVX2 寄存器；每个偏移级位图能装进一个 AVX-512 寄存器或两个 AVX2 寄存器。合取和析取查询就是对这些寄存器做 AND 和 OR 指令，计数查询直接用 CPU 原生的 POPCNT，基本避开了昂贵的循环和分支。比如查询 `the AND rareword`，TIN 先把两个词的页级位图按 256 页一批做 AND，交集里没有的页，其偏移级位图根本不用解码。对 `the OR rareword` 这样的计数查询，由于元数据里存有每个词精确的 posting 数，只要两个词的页级位图没有交集，析取的计数就是两者之和，连倒排表都不用读。需要返回行时，位的位置本身就是 ctid，无需查盘；而且同一段返回的 ctid 天然按堆顺序排列，Postgres 回表读取时自然是顺序访问，这个优化等于白送。

**MVCC 正确性。** TIN 保证结果对当前快照可见。对于需要返回列数据的查询，本来就要回表，Postgres 会顺便告知元组是否可见。对于 `COUNT(*)` 这类可以像 Index Only Scan 一样执行的查询，如果所有堆页都被标记为 all-visible，TIN 一个堆页都不用碰；如果堆被修改过，TIN 用自己的页级位图与 Postgres 的可见性映射（它本身也是页级位图）直接求交，只有落在非 all-visible 页上的 ctid 才需要回堆检查。通常 Postgres 索引返回所有匹配的 ctid、由执行器逐个检查可见性，而 TIN 通过规划自定义扫描（custom scan）把可见性检查挪进了自己内部，从而能利用向量指令。删除方面，TIN 为每个段维护一个「存活位图」（liveness bitmap），VACUUM 确认某个 ctid 已被删除时清除对应位；查询碰到被标记的页组时，再把偏移位图与存活位图做 AND，确保不返回也不计入真正已删除的元组。

**段与合并。** 首次建索引时，TIN 把表的堆页分成 n 份，生成 n 个不可变段；之后的数据变更写入可变段（便于插入但查询效率较低），由后台 worker 逐步提升为不可变段，再在后台把小段合并成大段。使用连续编号的系统在合并时必须给所有文档重新编号，整段数据要重新打包、压缩、写入，接近两倍的存储开销。TIN 没有这个问题：`(190, 17)` 在任何段里含义都一样，页级和偏移级位图也一样，合并时很多位图可以原封不动地复用，甚至不用复制，只需把磁盘上位图的所有权从旧段转移给新段，大幅降低了写放大以及合并的 CPU 和 I/O 成本。

文章总结：TIN 在每项基准中至少快 8 倍，全部源于「把 ctid 作为索引中每个 posting 的原生格式」这一选择带来的连锁效应。

## HN 评论精华

帖子获得 230 分、约 98 条评论。TIN 的主要开发者 zombodb（Eric Ridge）、PlanetScale CEO samlambert 以及文章合著者 dukepiki 都在评论区现身回应。讨论主要围绕四条线展开：TIN 只在 PlanetScale 云上提供引发的开源之争、Postgres 内置全文搜索到底差在哪、这波「人人都在做 BM25」是否是 AI 编程的产物，以及多语言支持。

**最高赞的讨论是：这不是「Postgres 的全文搜索」，而是「PlanetScale 托管版 Postgres 的全文搜索」。** Tiberium 指出，按官方文档，本地开发和 CI 用的版本（GitHub 上的 planetscale/lead）主要用于测试语法，性能特征完全不同，高性能版本只在他们的云服务上提供。noir_lord 说这对 PlanetScale 来说越来越常见，Neki 也是如此，这直接让他排除了使用他们的可能，「Postgres 的许可证允许这么做，但对我个人来说总觉得不是滋味」，而标题也因此有点误导。dbbk 反问本地测试为什么需要超快的搜索，Onavo 回答「为了避免厂商锁定」，dbbk 又回一句「这就是 Postgres 搜索，随便哪个编程 agent 五分钟就能帮你换成别的」。

zombodb 追问 noir_lord 为什么不喜欢 Postgres 这么宽松的许可证。noir_lord 澄清：他不讨厌许可证本身，而是讨厌「在开放的东西上构建封闭扩展」这种模式，完全专有没问题、完全开源没问题，拿开源当地基再把一部分锁起来才让人反感。他也承认这不是 PlanetScale 一家的问题，其他云厂商都这么做，而且不开源大概是怕被别家拿去用。zombodb 的回应很硬：使用开源软件的人有遵守许可证的义务、尊重前人工作的道德义务和尽力改进它的社会义务，但「没有义务把自己的成果免费给你」。他说 TIN 推动了大量对 Postgres、LLVM 和 pgrx 的上游贡献，「Postgres 18.6 今天变得更好，部分就是因为 TIN。不客气。」noir_lord 对这句「不客气」很不买账：公司出于自身利益往上游提交改进不值得感谢，就像 Intel 往 Linux 内核提交代码一样，「我不指望雇主感谢我，我指望他们付我工资」。两人最后以「看来我们在开源理念上处于两个极端，祝好运，2026 真是疯狂的年代」和平收场。ksec 则挖苦了 noir_lord 的立场前后矛盾：既说「完全专有没问题」，又说改进上游不值得感谢。

旁支讨论里，xyzzy_plugh 指出「没有义务免费给你」是对开源的狭隘理解，GPLv3 和 AGPLv3 就可能要求向客户公开成果；jamienk 借题发挥，列出了依赖专有扩展必然会遭遇的一串结局：关门、拒绝互通、强迫你遵守古怪规则、釜底抽薪、被黑、撒谎、背刺。aarondf 反对「道德义务」一说：开源是许可证，不是道德框架。collinmcnulty 则认为「能在开源之上提供一些专有软件」恰恰是吸引企业投资开源的诱因，没有这些投资就不会有今天这么多开源软件，这笔交易很划算；xnickb 把话题引向了资本主义批判。Cyph0n 的看法是，自从大模型在不署名的情况下拿开源代码训练，这种「开放核心 + 专有功能」（如 MongoDB）或「开源代码 + 私有测试」（如 SQLite）的模式只会越来越普遍。znpy 提到这篇文章之前已经发过一次但没人理，并质问「这是 21 世纪的拥抱、扩展、消灭吗？」；rs_rs_rs_rs_rs 回应说 PlanetScale 的 CEO 多次提到正是因为 Amazon 的做法才不愿开源，znpy 则认为这理由站不住，完全可以学 Grafana 用 GPLv3 来防。franckpachot 的批评最尖锐：这是在把开源、无厂商的社区版 PostgreSQL 变成专有数据库，每家托管服务都有自己独特的语法、功能和跑分，「人们选择 PostgreSQL 是为了自由，不是为了把应用绑死在一个只有一个所有者、一个维护者的扩展上」。

另外 pqdbr 抱怨 PlanetScale 不支持裸金属，samlambert 回复「我们支持 AWS、GCP 里的裸金属，Azure 也快了」，引来 SahAssar 一句吐槽：「需要裹一层 AWS/GCP/Azure 的金属，看起来可不怎么『裸』。」

**「Postgres 不是早就有全文搜索吗？」** 不少人提出这个疑问，groundzeros2015 甚至说「请去读 Postgres 手册，它内置的搜索能力很强」。dragonwriter 补充了历史：全文搜索从 2003 年的 7.4 版起就在 contrib 里，2008 年 8.3 版并入核心。真正把差别讲清楚的是 simonw 和 Boxxed：内置的 tsvector/tsquery 很好，但缺一个关键功能，即基于整个文档集合的排序。内置 FTS 只根据单行数据打分，而 BM25 这类相关性算法会考虑全语料统计，查询里越罕见的词，匹配到它的文档得分越高。「这正是所有这些扩展在提供的东西。」Boxxed 说得更直接：内置搜索做不了任何依赖全语料统计的打分，tf-idf 和 BM25 统统不行，他的经验是结果差得多。groundzeros2015 最后也承认「谢谢你指出了一个真实的差别」。

zombodb 给出了 TIN 相比内置 FTS 的完整清单：更好的性能、更低的运维开销、不需要额外存一份 tsvector 数据、带优化 top-k 输出的 BM25 打分、运行时可调的打分参数、基于表达式的得分加权、强大的 span 查询（「打了类固醇的邻近搜索」）、无损的词位置信息、索引可直接回答的否定表达式（找出所有不含某个词的文档）、全文命中高亮、优化过的精确 `count(*)`、通过模糊匹配/通配符/正则/字典范围做词项扩展，以及刻意精简的 SQL API。他还提到一个有趣的特性：TIN 的查询语言可以用在任何文本字段上，根本不需要索引，比如直接对 `pg_stat_activity` 的 query 列做 `==> 'select OR copy'` 查询。

**这波「人人都在做 BM25」是 AI 编程的产物吗？** andrenotgiant 盘点了近来的一串产品：ParadeDB 的 pg_search、Timescale 的 pg_textsearch、Neon 与 Databricks 的 Lakebase Search，现在又加上 PlanetScale，认为这是 AI 编程生产力在现实中显现的例子，「你可以直接让 agent 读一读 BM25 然后在你选的系统里实现它」，并担心这会导致激烈的同质化。反驳来得很快：CodesInChaos 指出 ParadeDB 基于 Tantivy，而 Tantivy 早于 AI 编程；ParadeDB 开发者 philippemnoel 确认他们在大模型出现之前就开始了这个项目，不过也同意近年其他几个 Postgres 全文搜索项目很大程度上得益于 vibe coding。Boxxed 说 BM25 是最简单的部分，「大概十来行代码，也许二十来行」，真正的工作在于索引设计：内存和磁盘上的数据结构、保持同步、容错、批处理等等，「如果你让 Claude『实现 BM25』，你得不到想要的东西」。ceuk 也认为难点在于硬核的存储引擎工作，比如在每秒上千次更新下合并段，他就遇到过 Claude 说「已经到性能底线了」，但坚持下去还能再省下可观的时间。sandeepkd 提醒文章里根本没提 AI 或 LLM，除非明确说明，否则别稀释开发者的功劳。

samwillis 给出了折中观点：即便 PlanetScale 用了 agent 开发，能达到这种性能也是因为他们有一支经验极其丰富的 Postgres 团队，对内部机制的理解让他们能引导模型走向文中的架构，「世界级专家 + LLM = 移山」。geraneum 的说法是 LLM 在「兑现已有的潜力」，没有底层潜力，结果就只能是垃圾。throwaway7783 讲了一段有意思的经历：他正在给原生 Postgres 副本做搜索和分析，LLM 自己得出了用 ctid 当文档 ID 的结论，读到文章那一段时感觉很魔幻，而他自己并不是 Postgres 内部机制专家；what 回应「或者它只是读过无数篇做同样事情的文章」。cjonas 好奇仅凭这篇文章加一个 PlanetScale 实例作对照，agent 能复刻到什么程度，dukepiki 随即指出已经有人在尝试了（TeamSpringbird/stannum）。有人把文章称为「slop」（AI 垃圾文），dukepiki 明确反驳：PlanetScale 的博客都是人写的，这篇是他和 TIN 的主要开发者合写的，还有几位「非常人类的」同事审阅过。

**用 Postgres 内置搜索还是外部 Lucene？** bob1029 说他在 SQLite 和 MSSQL 里做全文搜索总觉得关系模型和文档存储之间存在明显的阻抗失配，一直偏好「SQL 做记录系统 + 外部维护 Lucene 索引」的混合架构，问现在是否已经没必要这样了。zombodb 的回答干脆利落：「是的。」gfody 认为在数据库里直接放大型倒排索引很常见，用 BM25 加 Unicode 的 BreakIterator 就像把 Lucene 放进数据库；那些拼命把搜索移出数据库的地方，往往是想避免数据库太大，结果数据库照样很大，两头吃亏。zombodb 补充：外部搜索系统还带来一整套同步、重新同步/重建索引、把主键传回数据源查询、应用侧 join 与数据补全的基础设施和流程，「非常残酷」，全放在一处能消除整类开发和运维问题。

**多语言支持是公认的短板。** Doohickey-d 指出所有 Postgres 全文搜索方案在多语言上都做得不好：TIN 没提 CJK 支持（中文会被切成一字一词，能用但低效），似乎也没有词干提取（databases 到 database），只靠大小写和重音折叠「能用但很粗暴」。他希望能有按记录指定语言的分词和词干提取、按词边界切分的 CJK 支持，以及可选的折叠，「大多数方案似乎都假设内容是英文」。kiwicopple 推荐了在这方面最成熟的 PGroonga，scottsiu 也表示期待 CJK 支持改进。

其他零散评论：usernametaken29 说 SQLite 的 FTS 开箱即支持 Lucene 式查询且性能很好，好奇 Postgres 为什么不直接照搬；xcc3641 解释 SQLite FTS 依赖单写者锁下的影子 B-tree，而 Postgres 的索引访问方法必须把倒排项映射到物理 ctid，还要经受 MVCC 可见性检查和堆元组的频繁变动。immmmmm 提醒 TIN 这个名字与 PostGIS 里的 TIN（不规则三角网）数据类型可能冲突；dorianmariecom 则顺手玩了个谐音梗：「it does what it says on the TIN」（名副其实）。adityapatadia 说看到这些更新就觉得 MongoDB 好多年前就有了，被 stickfigure 纠正：Postgres 的全文搜索已经存在几十年，Mongo 早期的搜索比 Postgres 的还差，大约五年前才加入更成熟的搜索功能。
