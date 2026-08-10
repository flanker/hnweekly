---
layout: article
title: "DuckDB：装进笔记本的数据电动工具，现在也能在 Clojure 里用了"
issue: 804
number: 27
category: data
original_url: "https://techascent.com/blog/just-ducking-around.html"
hn_url: "https://news.ycombinator.com/item?id=49175924"
date: 2026-08-07
---

## 文章摘要

这是 TechAscent 团队 2023 年的一篇博客（HN 标题带了 2023 的年份标记），介绍他们的 Clojure 库 tmducken——把 DuckDB 接入他们的列式内存数据处理平台 tech.ml.dataset（TMD）。

文章先说清需求从何而来。TMD 是内存中的列主序数据处理平台，当数据大到装不下内存时，你可以继续用 TMD 对数据采样、或过滤出相关子集来适应环境限制，持久化则可以走 nippy、arrow 或 parquet。但当数据规模再上一个台阶——比如上百 GB 且带有关系型特征的一堆 CSV 文件——现有工具就开始变得笨重，人很容易被诱惑去搞那些「非函数式的 Spark 集群烂摊子」。作者的态度很明确：本地磁盘够大，本地芯片够快，不需要做任何激进的事。关系型数据库天然适合内存外存储和快速关系查询，问题是怎么在不放弃函数式编程和 TMD 列主序处理模型的前提下利用它。JDBC 加 Postgres 是第一个答案，但为了把数据经 JDBC 送进 TMD 而做一次完整的行到列转换、还得走一个低效且非批量的 API，实在让人恼火。

DuckDB 在 2021 年 5 月通过一个 GitHub issue 进入他们视野，当年 12 月 tmducken 就完成了与 DuckDB C 绑定的最小集成。但早期版本里查询结果一次性全部返回、必须装得下内存，而且当时 DuckDB 还没有专门的高性能追加或插入机制，IO 成为性能瓶颈，所以 Postgres 仍然是 TMD 的辅助处理系统。两年过去，DuckDB 改进巨大——最关键的是 C 接口现在为插入和查询都提供了批量机制，这使得处理超大 join 成为可能。

接下来是实测演示。素材是一个 50GB 的 CSV，含三年交易数据共 4 亿行。用 duckdb CLI 一句 `CREATE TABLE data AS FROM "data.csv"` 导入，耗时 1 分 50 秒，生成的 .ddb 文件只有 18GB——而且这里面已经包含了 DuckDB 自动创建的所有索引。在 Clojure 里通过 TMD 访问同样简单：初始化、`open-db`、`connect`，然后一个 `COUNT(*)` 在 4 亿行上耗时约 10 毫秒。

然后作者虚构了一个典型的业务加码场景：老板说还有一份数据集记录每个 sku 对应哪些颜色，也得进库。他在 Clojure 里现场生成一份 3.5 万行的颜色数据集，用 `create-table!` 和 `insert-dataset!` 塞进去。接着就是重头戏——把 4 亿条交易和这份平均每个 sku 约 3.51 种颜色的表做 join。**在笔记本上 join 出 14 亿行，耗时 2.5 秒**；回答老板真正的问题（2021 年 3 月各颜色各卖了多少件）耗时约 1.08 秒。用作者的话说：「1 秒后我们就能知道。这是答案。」

文章还展示了两种更适合 Clojure 侧处理的路径。第一种是把查询结果作为一系列 dataset 归约（reduce）掉——例子里对老板最爱的那个 sku 的全部交易按时间排序后归约，1 秒完成，作者强调重点不在归约函数本身多简单，而在于这条路径提供了一种**永远不会耗尽内存**的任意处理机制。第二种更进一步：DuckDB 支持零拷贝查询路径，只要查询结果的任何一个 chunk 都不需要逃逸出归约函数，就可以通过 `{:reduce-type :zero-copy-imm}` 选项开启，理论上这是可用的最低内存路径。

文末还补了几条 DuckDB 的小知识：它自动为所有数值数据建立 minmax 索引（也叫 BRIN 索引），几乎不增加原始数据体积却能显著提升查询性能；对唯一列或主键列会自动创建 ART 索引；用户可以选择性地为分类列建索引，代价是磁盘占用增加和事务可能变慢。DuckDB 用标准 C++11 编写因而可移植性不错，当时（2023 年 9 月）src 目录约 10 万行 C++ 代码；MIT 许可、GitHub 开放开发模式、社区响应迅速——作者称「以如此开放的方式开发出这么高质量的电动工具，是值得敬佩的」。

作者的总结落在同一个点上：DuckDB 与 TMD 互补性很好，极大提升了小团队高效管理和处理大数据集的能力，而无需求助于昂贵的分布式方案；这套集成体现了高质量高效计算工具的价值——让别人拿着更钝的工具就得上集群的活儿，在笔记本上用函数式方案就能解决。

## HN 评论精华

这条帖子 146 分、22 条评论，讨论主要围绕 DuckDB 的实际生产用法展开，Clojure 反而不是重点。

**生态与替代品**

- **kianN** 说自己是 tmducken 的粉丝，在生产系统里大量使用；不过最近的新项目开始尝试 ducktape，性能印象深刻，而且在插入和查询时支持更复杂的类型。他强调自己与该项目无关，只是想给这个较新的库一点关注——它由 tmducken 的一位活跃贡献者创建。
- **hrrld**（tmducken 作者本人）现身：「看到这个太酷了——DuckDB 对付 CSV 上的 SQL 简直是个怪物。」kianN 回应说他们把 DuckDB 当作整个技术栈的主力，并感谢 TechAscent 在 Clojure 生态上的工作，称这些数据科学包是他公司决定用 Clojure 搭建数据科学生态的一个重要转折点。
- **mnming** 补充了另一个相关的 Clojure + DuckDB/DuckLake 工具 o11ylite（他本人是维护者），提到自己最初也考虑过 tmducken。

**生产架构：用 DuckDB + Parquet 替代 ClickHouse**

- **encoderer** 分享了最有信息量的一条实践：他们在 Cronitor 目前用 ClickHouse，但下一个产品要弃用它，直接构建在 Parquet 和 DuckDB 之上。他的判断是「AI 时代可观测性的未来是自托管在 NVMe 上、由廉价且无限的对象存储兜底」，他不想把客户对话和智能体的思考过程发给 Sentry、BetterStack 这类巨型多租户 SaaS 数据库。
- **reacharavindh** 以「老派存储人」的身份表示困惑：NVMe 是本地 PCIe 快速存储（或 PCIe over network 的 NVMe fabric）的协议，而 S3、R2 这类对象存储是横向扩展的非 POSIX 桶存储，因为网络调用要慢得多，这两者怎么对应得上？**sethev** 猜测：可观测性通常是看一小片热数据，另有大量偶尔才需要的冷数据，所以 NVMe 是热数据缓存、对象存储放冷数据。**encoderer** 确认了这个理解，并补充说旧数据在问题已被定位和调查时很重要，但大多数负载看的是系统当前状态；所以热数据缓存在本地 SSD（不是 NAS/EBS），S3 永远是真相来源，而 DuckDB 跑在本地 SSD 的 parquet 文件上已经快到不需要传统数据库。他们的可观测性智能体用的是 DuckLake 的「湖仓」架构。

**DuckDB 本身的能力**

- **eterm** 称 DuckDB CLI 是个「大杀器」，能加载从 gzip 压缩的 JSON Lines 在内的各种文件——你可以把压缩日志直接塞进一个目录，需要时仍然能轻松用 SQL 查询它们。
- **HackerThemAll** 为不了解的人科普了 DuckDB 的扩展系统：可以直接使用 PostgreSQL、MySQL、SQLite、SQL Server 这类 OLTP 数据库，也能接云数仓/数据湖和大数据格式（Iceberg、Delta、Snowflake、Hive、ORC、Parquet、AVRO）、其他数据源（ODBC）和存储（S3）等等。**walthamstow** 表示受教，打算把 DuckDB CLI 变成自己在公司访问所有数据库的唯一入口；HackerThemAll 回应说这确实能自动化大量工作，比如各数据源之间的数据搬运、ETL/ELT 转换，甚至只是 CSV 转 Parquet、ORC 转 JSON 这类简单转换，最后祝他「嘎嘎愉快」。
- **alex_smart** 提了个直白的问题：为什么不直接用 JDBC 驱动？**didibus** 引用原文回答：JDBC 没有提供达到同等性能所需的批量 API，而 DuckDB 的 C 接口现在为插入和查询都提供了批量机制。
- **didibus** 另外感叹：如今单节点在大数据查询上真的能做很多事，太多人明明写个单节点小脚本就够，却直接上 Spark 集群。

**一段关于「语言还重不重要」的小争论**

- **solarized** 说自己现在不太在意语言了，LLM 已经「完美地」解决了这层抽象，他大部分查询都是 LLM 生成的，还自建了 API 把 LLM 接到各种数据库上。
- **sakjur** 简短回敬：「那这大概不适合你。」
- **didibus** 认真解释了他理解错了什么：这篇文章讲的不是「怎么用 Clojure 查数据库」这种无聊意义上的事，而是——下次你要分析大数据集时，让你的 AI 把它装进 DuckDB；如果需要跑 SQL 做不了的查询，就让它按这篇博客的做法来，查询会用 Clojure 跑得飞快。它要替代的是 Spark 集群这类东西。
- **davidpapermill** 补充：架构重要、效率重要、可扩展性重要，而这些都受语言选择影响；如果他们当初用 Python 开发，现在扩容时会非常痛苦。**c0_0p_** 则更不客气：「这类观点既不有趣、不相关，也没有帮助。」
- 另外 **ambicapter** 觉得原文说 DuckDB 团队的开放开发方式「值得敬佩」还不够，应该加上「极其」，后面再补一句「最为卓越」；**arikrahman** 附议，并说用 Clojure 是锦上添花。
