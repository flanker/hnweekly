---
layout: article
title: "DuckDB v2.0 预览：从进程内数据库走向「DuckDB 即服务器」"
issue: 806
number: 25
category: data
original_url: "https://duckdb.org/2026/08/17/duckdb-20-highlights"
hn_url: "https://news.ycombinator.com/item?id=49330781"
date: 2026-08-21
---

## 文章摘要

DuckDB 的两位作者 Mark Raasveldt 和 Hannes Mühleisen 在 2026 年 8 月 17 日发文预告了将在今秋发布的 v2.0，代号「Cyanoptera」（取自美洲西部的一种红棕色鸭子 Anas cyanoptera）。自 3 月的 v1.5 以来累计超过 10,000 次提交。作者说，大版本号不是仪式性的：v2.0 带来了全新的 SQL 解析器、新的默认存储格式、重写的 C API 和少量精心挑选的破坏性变更。如果说去年是「湖仓之年」，这一版开启的是「DuckDB 作为服务器之年」。文章自嘲这是一篇 listicle，共十条加一条彩蛋。

**1. DuckDB 即服务器：quack 扩展与 CONNECT 语句。** DuckDB 从第一天起就是进程内数据库，但用户「非常执着地」要求客户端／服务器模式，团队终于妥协。`quack` 扩展实现了 DuckDB 之间通信的原生协议，在 DuckCon #7 前作为预览发布，v2.0 转正。服务端 `CALL quack_serve(token = 'my_token')`，客户端 `ATTACH 'quack:server.example.com' AS qk (TOKEN 'my_token')` 然后 `CONNECT qk`，之后的查询在服务端执行、结果流式返回，最后 `DISCONNECT`。`CONNECT` 取代了此前 `remote.query($$...$$)` 的临时写法，而且不限于 Quack：配合新的远程下推优化器（#22914），它能把 SQL 直接推给 PostgreSQL 和 MySQL 执行，而不是把整张表拉过网络。作者强调 DuckDB 从第一天就是带完整 MVCC 和事务隔离的多连接事务型数据库，只是单用户场景下没人用到；实测它在不少事务型负载上足以与 PostgreSQL 竞争。长期运行也带来新挑战，v2.0 因此重做了指标层（#22799）、加强了日志与可观测性。有意思的是，预览发布后几周内社区就自己造出了 Quack 协议的独立客户端。

**2. VARIANT 成为一等公民。** VARIANT 在 v1.5 落地，作者形容它是「打了兴奋剂的 JSON」——每行可以有不同结构，但它不是文本格式：DuckDB 会自动探测半结构化数据里隐含的公共结构并做「shredding」（切分），从而存储压缩好、查询执行快，且完全不需要声明 schema，天然适合实时日志摄入。v2.0 把整条链路打通：从存储直接做 shredded 执行（#20912）、把字段提取下推进扫描（#22478）、Parquet 的 shredded VARIANT 读写，以及 `variant_type` / `variant_keys` / `variant_contains` 等函数族。长期计划是用 VARIANT 来支撑普通的 JSON 类型，让现有 JSON 负载不改一行查询就获得这些收益。

**3. 触发器。** 长期呼声最高的功能之一，这次一次给全：BEFORE 和 AFTER、FOR EACH ROW 和 FOR EACH STATEMENT、通过 `REFERENCING OLD/NEW TABLE` 使用过渡表、单个事件挂多个触发器、触发表上支持 RETURNING、以及 DROP TRIGGER。经典用法是审计表。团队也打算内部用触发器来实现后续若干特性。

**4. SQL 方言扩充。** NEAREST 连接（#24137）把 top-k 相似度检索变成了 join 子句，写法是 `INNER JOIN products t APPROX NEAREST 2 BY SIMILARITY array_cosine_similarity(...)`，服务向量／嵌入负载；CTE 内可以写 DML（#21634、#21997、#24217），例如 `WITH moved AS MATERIALIZED (DELETE FROM staging RETURNING *) INSERT INTO archive SELECT * FROM moved`；嵌套 schema（#23492、#24222）允许 `CREATE SCHEMA finance.reports`；新的变量语法（#21194）让 `$x` 可以出现在任何表达式位置，不必再写 `getvariable(...)`；JSON 变更函数 `json_set` / `json_insert` / `json_replace` / `json_remove`（#23786）终于能原地改文档；递归 CTE 支持 `USING KEY` 聚合（#19481），可以在纯 SQL 里写迭代算法。此外还有 SQL 标准的 `FETCH FIRST 2 ROWS ONLY`（#23533）、`OVERLAY()`（#22456）、GROUP BY 中的 UNNEST（#23644），以及 MERGE / `UPDATE ... FROM` 在多行匹配时的明确语义（#24058）。

**5. 异步 I/O。** DuckDB 早就能并行读对象存储，但同步访问限制了上限。v2.0 在整个引擎里引入异步 I/O，使 I/O 层与查询处理层各自独立扩展。Parquet 先落地（#23662），随后是 CSV（#23961）和 DuckDB 自有格式（#24654），另有异步 Parquet 写入（#23283）和新的 MMAP、DIRECT_IO 模式（#22988）。本地存储也有小幅收益，但网络存储才是收益大头。

**6. 全面变快。** 部分聚合被下推到 join 之下（#22572）、冗余聚合被复用（#24543）、递归 CTE 引擎重写（#22211）、聚合超出内存时溢写磁盘（#24499）、Windows CLI 的多线程结果物化快了约 2.2 倍（#24036）。文章给了一个笔记本上就能跑的微基准：在一百万条边的图上做单源可达性，纯递归 CTE 写法，v1.5.4 耗时 4.90 秒，v2.0 预览版 0.12 秒，约 40 倍提升。行组裁剪也大幅扩展：min-max 索引（zone map）和 Parquet Bloom filter 现在能对 struct、list、decimal、UUID、IN 过滤条件甚至函数谓词做跳过，`WHERE contains(message, 'ERROR')`、`WHERE substr(code, 1, 3) = 'NL-'` 这类查询都能裁掉行组。查询规划也变得分区感知（#22336），DuckLake、Iceberg 和 S3 上的 Hive 分区 Parquet 都能被优化器充分利用，分区写入也重做了（#22225、#22620）。

**7. 存储格式 v2.0.0。** 默认存储格式版本升到 v2.0.0（#22875）。列元数据改为惰性加载（#22333），宽表打开更快；DICT_FSST 字符串压缩默认开启（#23733）；删除记录紧凑存储（#24336）；读取时做更强的损坏校验。带大索引和宽表的数据库打开更快、内存占用大幅下降。新格式还允许 checkpoint vacuum 通过增量重映射行 ID 来压缩带 ART 索引的表，避免整体重建索引（#23653）。今年晚些时候 ART 索引将纳入缓冲管理（#21458、#23605），从而可以在内存压力下淘汰，并解除「ART 索引必须完全放进内存」的限制。

**8. 全新 SQL 解析器。** DuckDB 一直用的是派生自 PostgreSQL 的解析器，v2.0 换成自研的、基于 PEG 的可扩展解析器（#22194），源自 2024 年那篇关于运行时可扩展解析器的文章。扩展现在能挂进语法本身，意味着会出现暴露全新 SQL 语法的扩展；错误信息也带上了精确的源码位置。团队称设计上与旧解析器兼容，用户不该感知到差异。

**9. 干掉 ICU。** 时区、日历和排序规则一直由 ICU 库支撑，但 DuckDB 只用到其中一小片却要在每个发行版里背着它。v2.0 彻底移除 ICU 库，`icu` 扩展自己实现这些功能（#24463、#24403），时区数据直接由 IANA 数据库构建并压缩到约 45 kB。微基准（MacBook）显示：2500 万行做 `AT TIME ZONE 'Europe/Paris'` 从 0.24 秒降到 0.11 秒（2.2 倍），500 万行用德语 collation 过滤从 0.15 秒降到 0.06 秒（2.6 倍）。

**10. 扩展写一次、自己托管。** 目前大多数扩展（包括官方的）都编译时依赖不稳定的 C++ API，每个 DuckDB 版本都得重新构建发布。v2.0 带来重做的 C API（#24702 是第一部分）：用带版本的 YAML 规范描述、规范 schema 本身也带版本，并有代码生成工具（#24135），每个符号都打上生命周期与稳定性标签（#24435）。大部分 API 将标记为稳定并冻结，提供跨版本稳定的 ABI。C API 之上还提供一层薄的 C++ API，编译进扩展、只与稳定 C ABI 通信，覆盖标量／聚合／表／cast／copy 函数（支持命名参数与变长参数）、SQL 语句解析与检查、预备语句、replacement scan、自定义文件系统、直接访问向量缓冲区、流式消费结果等。Rust 绑定也在做。分发方面，v2.0 将允许注册自己的可信仓库（#24777，仍在进行中）：`CREATE EXTENSION REPOSITORY my_repo FROM 'https://extensions.example.org'`，仓库由名字、URL 前缀和一组被信任用于签名的 RSA 公钥组成，前缀可以是本地路径、https 或 s3。创建时 DuckDB 会拉取公钥并固定下来，打印每个 key 的 SHA-256 指纹供带外核对；也可以直接用 `USING PUBLIC KEY` 传入。固定的仓库能跨重启存活、支持多 key 轮换，可通过 `duckdb_extension_repositories()` 审计或 `DROP EXTENSION REPOSITORY` 移除。

**彩蛋：** 今秋起 DuckDB Foundation 将增设「利益相关方顾问委员会」，为 DuckDB、DuckLake 和 Quack 的路线图提供输入。文章最后提醒 v2.0 还会带一小组破坏性变更，包括新的默认存储格式和 lambda 语法迁移的收尾。

## HN 评论精华

这条帖子拿到 718 分、131 条评论，是本期数据类里讨论最热的一条。主线有三条：一是「DuckDB 正在从进程内引擎变成云数仓底座」这个方向判断，以及 DuckDB 与 MotherDuck 的关系澄清；二是一大串真实生产用法的分享（这部分信息密度最高）；三是与 SQLite、ClickHouse 的对比，以及仍然缺失的功能（增量物化视图、有序表）。

- **c9cf35860db4** 定调：过去一年的增强让人感觉 DuckDB 正在从「极其出色的进程内执行引擎」转变为「可以做云数据仓库底座的引擎」，尽管创始人此前对做这件事很有保留，但他感觉这已经在路上了。
- **ethagnawl** 一度以为 MotherDuck 就是 DuckDB 主力开发者的公司，后来自己更正并删掉了错误评论。**ryguyrg**（MotherDuck 联合创始人 Ryan Boyd）出来澄清：两家合作紧密但是独立公司，MotherDuck 专注云数仓和面向客户的分析。ethagnawl 补充了一个耐人寻味的观察：DuckDB 近期的一些特性和扩展（如 DuckLake、Quack）看起来会削弱 MotherDuck 层叠在上面的付费功能，但反过来看，MotherDuck 也是 DuckDB 团队试验未来特性的试验场。
- **TheFlyingFish** 回答「相比 SQLite 有什么优势」时给出最清楚的对比：DuckDB 面向分析负载、压缩列存，「改写一行」在 SQLite 里更高效，「把某列全部求和」在 DuckDB 里则快得多；两个项目的气质也不同，SQLite 一贯以牺牲功能换简洁，DuckDB 对功能的态度是「越多越好」——这次的招牌功能客户端／服务器模式就是例子，很多人希望 SQLite 也有，但看起来不太可能。**briHass** 从另一个角度补刀：SQLite 基本没有类型系统，日期／时间尤其是个坑，他只把 SQLite 当作单个应用存状态和配置用。
- 生产用法这一串很值得一读。**AlfeG** 分享把整个 MSSQL 数据库实时镜像进 DuckDB 做复杂报表，全部在进程内，DuckDB 数据库映射到临时存储、应用重启时重建，同一个查询在 MSSQL 上要 40 多秒，在 DuckDB 上 2 毫秒，团队里还有人不信「一个 60MB 的库加 250MB 内存开销能比 MSSQL Server 更快」（技术栈是 .NET 10 + DuckDB.NET）。**jtbaker** 列了三种：K8s 节点上跑 ETL 流水线（用流式处理引擎所以 pod／节点可以更小）、分发给内部团队的 CLI 做后处理并把 .db 上传到对象存储、SvelteKit 应用用 node 绑定挂载存储桶上的 .db 做 BI 探索（百万行级，放 PG 会很重）。**arealaccount** 和 **drums8787** 都在浏览器里用 DuckDB WASM + Parquet 做仪表盘，后者是把生成的 SQL 跑在用户自己的表上喂给浏览器内仪表盘。**phqb** 用 duckdb-go 当 Go 服务的查询引擎，把热数据从 Postgres 移到 S3 上的 Parquet 或 Iceberg。**MatthausK**（dltHub/dlt 的 CEO）说上个月有超过 9 万用户用 dlt 把数据加载进 DuckDB，并透露 Stellantis（Chrysler、Jeep、Peugeot 母公司）这类 Fortune 100 企业在混合云湖仓生产环境里用 DuckDB，「大家低估了 DuckDB 能处理的数据量」。
- **mediaman** 给中型企业做了个多租户平台，每个租户一个 DuckDB，典型数据量 5–150 GB。他和 **otter-in-a-suit** 都承认这不是 DuckDB 的经典场景（本来是给笔记本上跑本地数据的），但取舍上仍划算。mediaman 讲清了并发限制：如果不是同一进程，DuckDB 不能同时有 writer 和 reader 在同一文件上（多 reader 没问题），绕法是单进程同时拥有读写，或者把更新以 Parquet 文件形式发到对象存储、让 DuckDB 管目录；Quack 协议基本解决了这个问题。他喜欢「租户 A 就是能对一个独立的只读、禁 ATTACH 的 duckdb 文件跑任意 SQL」这种 Unix 式的简洁。**boc** 补充 DuckLake 支持用 Postgres 做 catalog，可以兼得 Postgres 的并发与 DuckDB 的读 Parquet 能力。
- **dangoodmanUT** 点出最大的缺口：至今没有增量物化视图，而所有零件（export state、agg_state、finalize）都已就位，他猜是在回避与 ClickHouse 正面开战。「增量物化视图是 ClickHouse 最好的功能，如果 DuckDB 加上，最后的护城河就只剩分布式查询执行了。」**hantusk** 指出社区已有 openivm 扩展实现。
- **amluto** 的许愿是原生的「有序表」：ClickHouse 和各类时序／日志库都内建表有序的概念并据此优化，而 DuckDB 逻辑上是行的袋子，只能靠索引或大块扫描；他猜有序提示还能让压缩效果更好。**tpetry** 贴了 duckdb 上对应的讨论（#8444），指出这是牵动很多模块的大改动。
- **fifilura** 提问 DuckDB 是否不擅长把工作分发到其他机器，**abirch** 回答开箱即用确实不行，但有 DuckLake、Quack，甚至 DeepSeek 基于 DuckDB 做了分布式的 smallpond。
- **badatnames** 抱怨这篇博文「AI 味太重」，尤其是那种破折号句式。**drannex** 反驳说他没闻到，反而看到多处 AI 会改掉的语法瑕疵和个人写作癖好，这种写法在技术文章里很正常。**ostwilkens** 则指名「A major version bump is not something we do lightly, and it is not just ceremony」这句最扎眼。
- **jeffbee** 对「我们重新实现了 ICU」只回了一个尖叫表情。**luizfelberti** 恳求把扩展仓库的 RSA 换成 minisign 之类。**aleda145** 对稳定 C++ 扩展 API 很兴奋，说他几个月前做的 dry run 扩展以后可以只构建一次。**Kvarnek** 一句话总结：「单写者限制一直是别扭的地方，Quack 终于解决了它。」
