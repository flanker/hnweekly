---
layout: article
title: "Polars 2.0 发布"
issue: 813
number: 24
category: data
original_url: "https://pola.rs/posts/release-polars-2/"
hn_url: "https://news.ycombinator.com/item?id=49977177"
date: 2026-10-09
---

## 文章摘要

Polars 是用 Rust 编写、主要通过 Python 使用的高性能 DataFrame 库，常被视为 pandas 的替代品。创始人 Ritchie Vink 在官方博客宣布 Polars 2.0 正式发布。此前一篇文章已解释过升级大版本号的理由，这篇则专门介绍新功能。作者说，2.0 原本并不打算做成大规模功能更新，但仍然带来不少值得兴奋的内容，主要有五点：默认启用初版的外存（溢出到磁盘）支持、大量核心性能改进、一等公民级的 SQL 支持、新的 Map 数据类型，以及对数据类型更严格、更显式的处理。

**SQL 与性能。** 2.0 标志着 Polars 正式把 SQL 当作一等公民，过去几个月 SQL 覆盖面大幅扩展。为了让 SQL 跑得快，优化器和执行引擎做了多项改进，重点包括连接重排序、更好的公共子计划消除，以及动态谓词和布隆过滤器。团队在 AWS 的 c7a.4xlarge（16 vCPU、32 GB 内存）和 c7a.metal（192 vCPU、384 GB 内存）上，用基于 TPC-H 和 TPC-DS 生成的数据，将 Polars SQL 与 DuckDB 1.5.6、DuckDB 2.0 alpha 和 DataFusion 54.0.0 对比。每条查询以热缓存方式运行 5 次、取最好成绩，每条查询单独开进程，超时 60 秒；查询语句由 DuckDB 自带函数生成，数据用 DataFusion 团队的生成器制作。结果是：Polars 和两个版本的 DuckDB 跑完了全部查询，DataFusion 有个别查询超时或内存溢出；默认配置的 Polars 在几乎所有基准中最快。唯一的弱项是在 192 线程的大机器上处理小数据量时，Polars 有一段固定开销——限制到 32 线程后反而在所有基准中都有竞争力甚至领先。团队已找到原因，计划下个版本修复，并公开了基准测试仓库供复现。附录还显示，在 SF100 数据规模下从 16 核扩到 192 核，Polars 在 TPC-H 上提速 3.8 倍，扩展性优于其他引擎。

**流式引擎与外存成为默认。** 这是影响最大的改动：对 LazyFrame 调用 `collect` 现在默认使用流式引擎，多数查询的内存占用和性能都有大幅改善。之所以需要升级大版本，是因为流式引擎在连接、分组、逆透视等操作中默认不保证行顺序，需要顺序时要显式设置 `maintain_order=True`。外存功能也默认开启：内存用到约 80% 时开始溢出到磁盘，默认磁盘预算 64 GB；目前排序、窗口函数和许多表达式已支持，连接和分组将在之后跟进。作者认为这会让普通数据从业者在大内存负载下更稳健。

**新的 Map 类型。** Polars 现在直接支持 Arrow 的 MapType，可以理解为 Python 字典式的键值映射。此前它会被读成「由 key/value 结构体组成的列表」，现在则有专门的表达式，例如按固定键或按另一列的值取值、判断是否包含某个键、获取键列表和值列表等。

**更严格的 Polars。** Polars 的理念是尽早失败：错误应在一开始就抛出，而不是在流水线跑了 20 分钟之后；数据不匹配时的隐式行为应当需要主动开启，而不是默认发生，因为这类行为会掩盖 bug。作者特别指出，在 AI 驱动开发兴起后，这种严格性更有价值——代理可以调用 `collect_schema()`，在不实际计算数据的情况下解析类型、提前发现模式层面的错误，让人和代理都能更快迭代。

文章最后表示，接下来会继续改进外存支持和大核数扩展性，目标是让 Polars Cloud 成为最快的分布式引擎，同时已经开始开发 GeoPolars，并提供了迁移指南。

## HN 评论精华

**与 DuckDB 的竞争和基准测试的可信度**

- **dkgs** 调侃这是否与 DuckDB 即将发布 2.0 有关。Polars 核心开发者 **orlp** 回应：2.0 早已计划，团队原本说 1.x 会很快过渡，结果拖了很久；第一批 2.0 分支的 PR 在 6 月就已合入。
- 曾做过 TPC 基准测试的 **popularonion** 提醒：不要把这类博客理解为「A 比 B 快 X%」，影响因素太多，更应理解为「团队专注优化了性能，某些负载会更好」。orlp 表示认同，「每个人都会赢下自己的基准测试」，因为自家可以反复迭代，别人只有一次开箱即用的机会；但他强调已尽量公平：所有引擎都用默认设置，查询来自 DuckDB，数据生成器来自 DataFusion 团队，机器是任何人都能租到的 AWS 实例。唯一的例外是额外跑了限制为 32 线程的 Polars，以说明小数据上还有很大改进空间。
- orlp 还解释了大机器上小数据变慢的原因：目前的连接操作会为每个线程做 T 个分区，总共 T² 个分区，在 192 核机器上这个开销不可忽视；数据只有几千万行时，每个分区就太小了。他会尽快修复。
- **fphhotchips** 认为 TPC 基准远不够全面，尤其完全不涉及 JSON 等半结构化数据，而这是如今几乎每个组织都会遇到的；**andriy_koval** 则认为 TPC 比流行的 ClickBench 多样得多，JSON 展开后也能回到关系型范畴，两人争执不下。
- **niltecedu** 对结果有些意外：在他们的负载中，DataFusion 一直与 Polars 不相上下，DuckDB 反而慢得多。**jdefting** 想看内存对比，说自己遇到过 Polars 比 DuckDB 更吃内存的情况，orlp 指出每条查询的峰值内存数据都在仓库里。**kaathewise** 分享经验：Polars 大量依赖多线程，读取超大批次的 Arrow IPC 文件时曾遇到内存溢出，把 `POLARS_MAX_THREADS` 设为 1 后解决，速度反而更快。

**Polars 能否完全取代 pandas**

- **esco2292** 认为 99.9% 的场景下都可以，唯一例外是地理空间数据，因为 GeoPolars 尚未成熟。Ritchie Vink（**ritchie46**）补充说 GeoPolars 目前在仓库的 dev 分支上开发，**benrutter** 这才发现项目并未废弃。
- **derriz** 说是 API 让他离开了 pandas：用了十多年，任何稍复杂的查询都还得上网搜。**entropicdrifter** 打了个比方：pandas 就像《龙与地下城》，规则是遗留包袱和现代理念的混合体，学起来很费劲，于是很多人学会后就不愿尝试其他更简洁的系统；**ezst** 说这段话几乎完美描述了 git 的现状。
- 也有人列出 pandas 的优势：**SukadarBukadar** 认为 pandas 更擅长小规模原地修改、逐行操作、转置和多轴操作，与绘图、统计库的互操作也更好；处理超大数据集时他会选 DuckDB，因为复杂连接时 DuckDB 能更好地把中间结果溢出到磁盘，而 Polars「直接崩掉」（他还没试过 2.0）。**gremlinunderway** 抱怨 Polars 难以把多列与固定文本拼接成新列，orlp 当场演示用 `pl.col.x + " text " + pl.col.y` 就能做到。
- **jt-s** 问 SciPy 等库是否接受 Polars 的 DataFrame，**niksmather** 指出可以用 `.to_numpy()` 转换；**0cf8612b2e1e** 推荐与具体 DataFrame 库无关的 Narwhals，它能让第三方库同时支持 pandas、Polars 等多种后端。
- **Vaslo** 说整个团队都在迁移到 Polars 和 DuckDB；**tomrod** 表示今后新项目都会在 DuckDB、Polars 和 PyArrow 中选择，并感谢 pandas 当年相对 SAS 带来的进步；**genxy** 补充说 pandas 本身又受 R 的 data.frame 启发。

**其他**

- **Kinrany** 问 Polars 为什么不和 DataFusion 合并成一个生态，**esafak** 引用 Polars 创始人早年的说法：不想增加不必要的依赖复杂度。
- **raoulj** 发现 SQL 接口还不支持 asof join，orlp 请他提交 issue。**Centigonal** 说外存支持正是当年迫使他离开 pandas 和 Polars 的最大原因，对此非常期待。
- **jadbox** 问为什么不直接用 PostgreSQL 或 SQLite，**cle** 和 **Tuna-Fish** 解释：后两者是面向事务的数据库，为了数据持久性要付出最多两个数量级的速度代价；对于可以重新生成的数据，做分析时把 15 分钟的查询缩短到 10 秒非常有用。
- 还有人眼尖地指出，公告第一段把「enthusiastic」拼成了「enthousiastic」。
