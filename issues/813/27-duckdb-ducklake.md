---
layout: article
title: "DuckDB 的 DuckLake"
issue: 813
number: 27
category: data
original_url: "https://github.com/duckdb/ducklake"
hn_url: "https://news.ycombinator.com/item?id=49996149"
date: 2026-10-09
---

## 文章摘要

链接指向 GitHub 上的 duckdb/ducklake 仓库，也就是 DuckDB 的 DuckLake 扩展。仓库简介说 DuckLake 是一种「集成了数据湖和目录（catalog）的格式」。

**核心设计。** DuckLake 是一种开放的 Lakehouse 格式，建立在 SQL 和 Parquet 之上：数据以 Parquet 文件形式存放，元数据存进一个「目录数据库」。这和 Delta Lake、Apache Iceberg 不同，那两种格式靠对象存储上的 JSON/Avro 日志或清单文件来记录哪些数据文件有效，DuckLake 则直接把这些信息放进一个支持事务的数据库。根据评论里 DuckLake 首席开发者的说明，目录库可以是 DuckDB、SQLite、PostgreSQL、DuckDB + Quack，或者处于实验阶段的 MySQL。

**怎么用。** 安装只需要在 DuckDB 里执行 `INSTALL ducklake;`，开发版可以从 `core_nightly` 渠道强制安装。之后用 `ATTACH 'ducklake:metadata.ducklake' AS my_ducklake (DATA_PATH 'file_path/')` 挂载，这个例子把元数据放在一个 DuckDB 文件里，数据写到指定目录下的 Parquet 文件中。挂载后就能用标准 SQL 建表、插入、更新、查询。README 演示了几个 Lakehouse 常用功能：

- **更新**：直接执行 `UPDATE` 语句修改行。
- **时间旅行**：用 `AT (VERSION => 2)` 查询某个历史快照，看到更新之前的数据。
- **模式演进**：`ALTER TABLE ... ADD COLUMN` 加新列，已有的行自动补 NULL。
- **变更数据流**：用 `table_changes('my_table', 2, 2)` 列出某个快照区间里的插入、删除等变更，结果带快照 ID、行号和变更类型。

**开发与测试。** 扩展用 C++ 编写，源码依赖以子模块形式固定在 `.github/duckdb-version` 指定的 DuckDB 版本上。测试可以针对不同的目录后端分别运行，包括 PostgreSQL、SQLite 和启用删除向量的配置，也可以把 DuckDB 核心测试跑在 DuckLake 存储上。项目欢迎外部贡献，开发分支是 main。

从评论看，DuckLake 规范已经发布 1.0，MotherDuck 在推广它（还提供 O'Reilly《DuckLake: The Definitive Guide》的免费下载），社区里也已经有不依赖 DuckDB 的独立实现。

## HN 评论精华

这个帖子评论不多（30 多条），但有 DuckLake 首席开发者和一个第三方实现的作者参与。

**DuckLake 只是一份规范，不一定要用 DuckDB**

- **smithclay** 提醒大家，这一点很多人不知道：DuckLake 其实是一份数据湖规范，只是恰好在 DuckDB 里能用。Rust/DataFusion 生态有独立实现 datafusion-ducklake，Quack 协议也带来不少可能性。他还建议把智能体的运行轨迹（agent traces）都存进 DuckLake。
- datafusion-ducklake 的作者 **eddietejeda** 介绍了背景：他们以前为特定场景自己写目录服务，需求一变就很难维护，而 Apache Iceberg 对他们的低延迟场景来说太重。DuckLake 只是一份规范，他们就自己实现了一版，性能和以前专门定制的目录一样好。目录存储用的是 Postgres，他说「用事务数据库存事务性数据，没有比这更简单的了」。规范还把时间旅行、快照这些难做的部分讲得很清楚，他形容 DuckLake「简直是天赐之物」。被问到延迟目标时，他说是在极高并发下保持亚秒级响应。
- **wodenokoto** 原以为目录就是一个 DuckDB 文件，看官网示例默认用 PostgreSQL，就开玩笑说这应该叫「PostgresLake」。**paragraft** 指出官网那只是一个默认显示 Postgres 的选项卡，SQLite 和 DuckDB 也都支持。
- DuckLake 首席开发者 **pdet** 说明，任何数据库只要满足三个条件就能当 DuckLake 目录：支持主键、支持基本数据类型（INTEGER、VARCHAR、TIMESTAMP），并且支持 ACID。各家 SQL 方言有些差异，DuckDB 的扩展目前支持 DuckDB、SQLite、Postgres、DuckDB + Quack 和 MySQL，其中 MySQL 还比较实验性。

**为什么还用 C++**

- **loufe** 质疑 2026 年了为什么新项目还用 C++，不用内存安全语言。**esafak** 说 DuckDB 从 2018 年就有了，它的衍生项目自然用 C++。**Tanjreeve** 认为数据库领域的重心本来就在 C/C++：真正写存储引擎时到处都是 unsafe 代码和裸指针，Rust 也不会自动变安全，「实现 DBMS 的难点不在选语言」。
- **pdet** 解释说，选 C++ 主要是为了和 DuckDB 内部（表扫描器、类型转换函数等）紧密集成。DuckLake 本身完全不必用 C++，他列出了几个其他语言的实现：Rust 的 ducklake-sdk 和 datafusion-ducklake、Scala 的 ducklake-spark、Kotlin 的 trino-ducklake。

**成熟度和适用场景**

- **engineeringwoke** 说软件还很「alpha」：v1.5.4 里带过滤条件的目录计数是坏的，切到 main/v2 分支修好了，结果 DuckDB v2 的 SQL 解析器又慢了 10 倍。**jauco** 附和说规范是 1.0 了，但软件还不是 1.0 的质量，用之前先翻翻 bug 列表。好用的时候确实很好用，至少比自己手搓多层 Parquet 存储强。
- **snapetom** 问它是不是「内置 SQL 引擎的 Delta/Iceberg」。**Lucasoato** 解释说，关键区别在于记录哪些数据文件有效的日志不存成 JSON/Parquet 文件，而是直接放进数据库：速度快很多，代价是多一个依赖，不过用基于数据库的目录时本来也要有这个依赖。**efromvt** 调侃说兜了一圈又回到了 Hive Metastore。
- **tomwphillips** 认为 DuckLake 的设计很优雅，比竞品好，但 Lakehouse 并没有厂商宣传的那么通用：访问控制受限于底层存储桶，没有列级、行级权限，也没有列脱敏，所以不适合标准的企业 BI 分析。**efromvt** 建议把存储桶锁死，在 Spark、Presto 等引擎层做权限控制。tomwphillips 回应说，Lakehouse 的卖点恰恰是哪个引擎合适就用哪个，要保证每个引擎都正确执行权限策略，工作量太大。

**其他**

- **celias** 提到 MotherDuck 官网可以免费领 O'Reilly 的《DuckLake: The Definitive Guide》。好几位用户说填了邮箱却只收到欢迎邮件，**robflaherty** 给出了另一个能用的表单链接。**suchire** 调侃说明明可以叫「Duckpond」（鸭塘）。
