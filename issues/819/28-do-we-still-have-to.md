---
layout: article
title: "保留规范化表结构，也能高效做多表搜索吗？"
issue: 819
number: 28
category: data
original_url: "https://www.paradedb.com/blog/joins"
hn_url: "https://news.ycombinator.com/item?id=50484998"
date: 2026-11-19
---

## 文章摘要

ParadeDB 讨论的是一个具体的数据库难题：相关性搜索需要把用户、文章、评论等表连接起来，再按组合分数选出前若干条结果。传统执行方式可能先生成庞大连接结果，最后才排序截断；把数据提前摊平到搜索引擎虽能加快读取，却增加同步、存储和更新成本。

作者介绍了基于 Apache DataFusion 的列式执行路径。对于 Top-K 查询，Rank Join 按分数顺序读取输入，利用候选结果的分数上界判断何时可以停止，并用按连接键的直接查找补全候选，而不是无条件处理全部组合。对于必须统计所有命中的分面聚合，则使用不同策略：为行标记是否命中、按连接键协同分区，并把真正需要的列延迟到过滤后读取。两类查询不能简单共用“找到前二十条就停”的技巧。

文中的 StackOverflow 基准展示了显著加速，但“最高千倍”对应特定数据、查询和基线；文章也比较了手工改写 SQL 后更快的 PostgreSQL 方案，不能把最慢基线当成所有使用方式。普通查询仍走 PostgreSQL 原有执行路径。本文未运行基准或验证属性测试，结论应限定为这种搜索连接的优化，而不是宣布反规范化在所有数据库工作负载中都已失去价值。

## HN 评论精华

- [dan_thinks](https://news.ycombinator.com/item?id=50513163) 赞赏 Top-K 优化，同时认为标题过宽：某些用例不必反规范化，不代表所有用例都不再需要。

- [amluto](https://news.ycombinator.com/item?id=50514286) 质疑示例搜索语义是否符合用户意图，提出分别搜索不同实体再组合结果可能更合适。

- [gopalv](https://news.ycombinator.com/item?id=50513783) 把实现与既有 Top-K 剪枝研究联系起来，并讨论分布式执行时传播候选边界的可能性。
