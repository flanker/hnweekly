---
layout: article
title: "用 DuckDB 做全文检索：一份务实指南"
issue: 792
number: 30
category: data
original_url: "https://peterdohertys.website/blog-posts/full-text-search-w-duckdb.html"
hn_url: "https://news.ycombinator.com/item?id=47966254"
date: 2026-05-01
---

## 文章摘要

Peter Doherty 这篇博客全面介绍了 DuckDB 的全文检索（FTS）扩展，目标读者是已经把 DuckDB 当作日常分析工具，但还没意识到它也能做"搜索"的工程师与数据分析师。文章的立场比较克制：DuckDB FTS 不会取代 Elasticsearch 或 PostgreSQL 的 `tsvector`/`tsquery`，但对于"探索性数据工作"（如本地分析邮件归档、爬虫输出、客户工单等），它已经"足够好用"。

**安装与启用**。DuckDB 默认不内置 FTS，需要手动加载：

```sql
INSTALL fts;
LOAD fts;
```

扩展通过运行时下载机制安装，这点也是 HN 评论中讨论较多的安全话题。

**索引创建**通过 `PRAGMA` 完成，可一次覆盖多列：

```sql
PRAGMA create_fts_index('emails', 'id', 'subject', 'body');
```

第二个参数是主键列；后面任意多列将一起被分词索引。可选参数包括：词干化（stemmer，默认是 Porter 风格的英文 stemmer，可换为其他语言）、停用词表、是否做重音归一化（accent folding）等。

**查询机制**基于 Okapi BM25 评分，调用形如：

```sql
SELECT id, score
FROM (
  SELECT *, fts_main_emails.match_bm25(id, 'invoice payment', fields := 'subject,body') AS score
  FROM emails
)
WHERE score IS NOT NULL
ORDER BY score DESC;
```

文章重点解释了 BM25 的两个调参常数：

- **k₁**：控制 term frequency 的饱和度。较低值（如 0.3）让查询近似"这个词出现没出现"的二值判断；较高值（如 3.0）让重复出现获得更高加权。对长篇文章 k₁ 偏大，对短消息偏小。
- **b**：控制文档长度归一化。`b=0.0` 完全忽略长度，`b=1.0` 强烈惩罚长文档。默认 0.75 是经典 IR 教材推荐值。

**与其他方案的对比**：相比 PostgreSQL，DuckDB 缺少 `ts_headline` 这种"高亮匹配片段"功能，需要应用层自己实现。相比 SQLite FTS5，DuckDB 在分析型 SQL（窗口、聚合、Join）上远更强，但 SQLite FTS5 在写入吞吐和并发上更成熟。Elasticsearch 仍是大规模、近实时搜索的事实标准，DuckDB FTS 不打算与之竞争。

作者在结论中给出了实用判断：DuckDB FTS"相当强大，对大多数探索性场景足够"，特别适合需要"先做 ETL/分析、再做检索"的混合工作流——你可以一边做窗口聚合一边按相关性筛选，这是传统搜索引擎做不到的。

## HN 评论精华

- 有评论者指出 DuckDB 真正的差异化在"任意数据源的 SQL 化能力"："你几乎可以把 DuckDB 指向任何数据源——CSV、Parquet、HTTP、S3、JSON——立即得到一个 SQL 表"，这意味着 FTS 也能直接架在远端数据上，而不必先复制本地。

- **conqrr** 描述了一个生产级用例：他们把 CloudWatch/Loki 替换为"S3 + Parquet + DuckDB"——服务直接把日志以 Parquet 形式 emit 到 S3，需要时本地用 DuckDB 查。配合 FTS，等于零成本搭出了一套迷你 Loki，月成本下降一个量级。

- **bambax** 提出一个实用提醒：DuckDB 跨多个外部数据源做 FTS 时，性能取决于源端的 IO 与索引；当查询模式以全文检索为主时，"先把数据完整 import 进 duckdb 数据库"通常更快，因为 FTS 索引必须落在 DuckDB 自己的存储里。

- **SOLAR_FIELDS** 谈到扩展生态的成熟度问题：DuckDB 扩展系统还在早期阶段，依赖 main 分支构建的扩展常遇到 ABI 不兼容；他呼吁更多扩展作者发布稳定夜间构建。

- 关于运行时下载扩展的**安全担忧**：有评论者指出 `INSTALL fts` 默认会从官方仓库拉二进制并执行，对一些合规场景这相当于一次"远程代码执行"。建议引入签名校验或离线 bundle。

- **DuckDB-WASM 的浏览器场景**：一位开发者展示了在浏览器里跑 LLM 基准的项目，全部基于 DuckDB-WASM。读者的反馈是"我们一直把网页当静态展示，其实可以做远不止这些"。这暗示 DuckDB FTS 也可以下沉到客户端，做"零后端"的本地搜索体验。

- **jiehong** 提出 ClickHouse Local 是同类替代品，在分析吞吐上略胜一筹；但 **efromvt** 反驳说 DuckDB 的交互式开发体验更顺滑，REPL、CLI、Python/JS 绑定都更"开箱即用"。

- 关于邮箱发布的延伸用例：有读者提到能否用 DuckDB 搭出"开源邮件归档浏览器"——支持 SQL 查询 + BM25 检索 + Parquet 静态托管，比现有 jmail.world 方案更轻。这条讨论指向了 DuckDB FTS 作为"静态可分发搜索引擎"的潜力。
