---
layout: article
title: "PostHog 为什么把数据仓库产品迁出 DuckDB"
issue: 817
number: 25
category: data
original_url: "https://posthog.com/blog/moving-off-duckdb"
hn_url: "https://news.ycombinator.com/item?id=50338747"
date: 2026-11-06
---

## 文章摘要

PostHog 解释了其数据仓库产品从 DuckDB 与 DuckLake 转向 Trino 和自有 HogLake 表格式的原因。此前团队为了隔离客户的长时间建模查询、获得成本优化器和兼容 dbt 的接入方式，从共享 ClickHouse 集群改用每客户独立 DuckDB 实例。但真实查询比预期更复杂，单节点资源需要提前配置，客户和平台都难以准确预测内存与 CPU 需求，扩大实例或溢写磁盘的成本也逐渐不合适。

团队以自己的约 2 PB、一万亿行、1,530 万快照规模试用 DuckLake，报告目录统计扫描、已删除表残留统计和元数据库连接数量成为瓶颈。清理统计曾让写入快 30 至 50 倍，但问题持续累积；兼容官方目录结构又限制了优化自由。文章还指出 PostgreSQL 协议的长连接、负载均衡和各客户端兼容细节，增加了托管服务的运营成本。

这是一篇特定云服务负载下的架构复盘，不是宣判 DuckDB 不适合分析。作者仍肯定它作为单机引擎的可靠性。新的分布式方案意在更动态地分配查询资源，但正文未提供足以独立比较所有方案的完整基准，HogLake 的长期成熟度也不能从这篇公告确认。尤其应区分「仓库产品的新技术栈」和 PostHog 全部产品都被重写这两种说法。

## HN 评论精华

- **读者要求更多测量。** 0c3ca83 认为文章技术细节不足；作者 roboben 回应团队先尝试测量和优化，之后才决定放弃原架构。
- **工作负载比最初设想更重。** roboben [说明](https://news.ycombinator.com/item?id=50341683)是在开发面向仓储客户的新栈，正文没有充分说明这个范围，不应据此推断整家公司五个月内重写两次。
- **兼容连接不等于分布式执行。** thisisauserid 询问 Quack 能否解决问题；roboben 表示其理解中该协议不能提供所需的分布式查询能力。
