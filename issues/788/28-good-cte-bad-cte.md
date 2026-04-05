---
layout: article
title: "好的 CTE 与坏的 CTE"
issue: 788
number: 28
category: data
original_url: "https://boringsql.com/posts/good-cte-bad-cte/"
hn_url: "https://news.ycombinator.com/item?id=47571330"
date: 2026-04-03
---

## 文章摘要

这篇由 Radim Marek 撰写的深度技术文章（约 18 分钟阅读量）系统性地探讨了 PostgreSQL 中 CTE（Common Table Expressions，公共表表达式）的内部优化机制，覆盖从早期版本到 PostgreSQL 18 的演进历程。

CTE 是使用 `WITH` 子句定义的临时结果集，本质上是命名的子查询，用于提升 SQL 的可读性和组织性。文章的核心论点是：**规划器对 CTE 的处理方式因写法不同而截然不同**，理解这些差异对查询性能至关重要。

**优化屏障时代（PostgreSQL 12 之前）**：在 PG 12 之前，CTE 始终被"物化"（materialized），即作为独立的优化屏障存在。规划器无法将 CTE 内联到主查询中进行联合优化，这意味着外部的过滤条件无法下推到 CTE 内部，经常导致性能问题。

**PostgreSQL 12 的 CTE 内联**：PG 12 引入了 CTE 内联机制，允许规划器在满足条件时将 CTE 展开为子查询进行优化。文章详细列举了 8 种情况的决策矩阵：(1) 单次引用且无副作用时会被内联；(2) 多次引用时会被物化；(3) 递归 CTE 始终物化；(4) 包含数据修改操作（DML）的 CTE 始终物化；(5) 包含 VOLATILE 函数时物化；(6) STABLE 函数可以内联；(7) 可通过 MATERIALIZED/NOT MATERIALIZED 提示强制控制行为；(8) FOR UPDATE/FOR SHARE 会触发物化。

**统计信息黑洞**：物化的 CTE 会丢失统计信息，导致规划器做出次优决策。PG 17 引入了统计信息传播机制来缓解这一问题。

文章还深入讨论了**可写 CTE**（在 CTE 中嵌入 INSERT/UPDATE/DELETE 语句）的强大功能和陷阱——你无法在同一语句中读取刚写入的数据，以及元组重排（tuple shuffling）问题。**递归 CTE** 部分详解了 UNION 与 UNION ALL 的区别。文章最后探讨了分区裁剪丢失、预处理语句缓存、work_mem 溢出、安全屏障视图等边缘情况，并给出了 CTE、子查询和临时表的选择指南。

## HN 评论精华

**CTE 的本质定位**：多位评论者（yen223、atombender）指出 CTE 本质上是代码组织工具而非优化手段，数据库通常会将其内联并有效优化。不应期望 CTE 本身带来性能提升，它的价值在于让复杂查询更易读、更易维护。

**实际迁移经验**：swasheck 分享了从 SQL Server 迁移到 PostgreSQL 时遇到的 CTE 行为差异，赞扬作者对社区反馈的积极响应。siddboots 指出 PostgreSQL 是最后一个移除 CTE 优化屏障的主流数据库。

**临时表策略**：jpalomaki 建议在分析查询中将 CTE 拆分为带索引的临时表以获得更好性能。andsbf 反驳说在查询数据库副本（replica）时无法使用临时表，这是一个实际限制。

**递归 CTE vs CONNECT BY**：bob1029 认为 Oracle 的 CONNECT BY 在深度优先层级查询中性能更优，但 hans_castorp 反驳称 CTE 功能更强大，支持更多场景。

**AI 辅助 SQL 优化**：faangguyindia 声称使用 Claude 重写查询将计算成本降低到原来的五分之一，但 listenallyall 对此持批评态度，认为 AI 工具不能替代深入的 SQL 专业知识。

**可写 CTE 的惊喜**：ctippett 表示惊讶地得知 DML 语句可以嵌套在 CTE 中，这个功能虽然强大但鲜为人知。

**CTE 调试工具**：uwemaurer 分享了一个可以逐步执行 CTE 并可视化中间结果的 UI 项目，对理解复杂 CTE 查询非常有帮助。
