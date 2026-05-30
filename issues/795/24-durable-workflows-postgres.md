---
layout: article
title: "做持久化工作流，一个 Postgres 就够了"
issue: 795
number: 24
category: data
original_url: "https://www.dbos.dev/blog/postgres-is-all-you-need-for-durable-execution"
hn_url: "https://news.ycombinator.com/item?id=48313530"
date: 2026-05-29
---

## 文章摘要

这篇 DBOS 的文章论证：构建**持久化工作流（durable workflows）**，你其实只需要一个 Postgres。**持久化执行（durable execution）**的本质是周期性检查点（checkpointing）——「程序运行时，定期把进度写到数据库；一旦崩溃或失败，就能从上一个检查点重新加载、从最后完成的那一步恢复」，原理就像玩游戏存档。

文章对比了两种架构。**外部编排（external orchestration，如 Temporal、Airflow、AWS Step Functions）**：由一个中央编排器服务协调全部工作流执行，worker 把每步结果回传给编排器，编排器记录检查点并派发下一步——这套独立系统带来额外的运维复杂度。而 **DBOS 的 Postgres 直连方案**：应用服务器直接与 Postgres 表通信，客户端通过在 workflows 表里建一条记录来提交工作流，服务器用数据库锁机制**协作式地出队（dequeue）**工作流，worker 把每步输出直接检查点到 Postgres，**无需中央编排器**。

它列举的关键优势包括：**可扩展性**——单个 Postgres 实例可处理「每秒数万个工作流」，还能通过分布式或分片 Postgres 进一步扩展；**可观测性**——工作流数据就在可查询的 Postgres 表里，「几乎任何工作流可观测性查询都能用 SQL 表达」，无需额外系统就能实时监控；**可靠性**——消除了编排器及其数据存储这两个故障点，缩小了攻击面与运维依赖；**协调性**——靠 Postgres 的完整性约束保证每个工作流恰好由一个 worker 执行，自动检测并阻止重复工作。

## HN 评论精华

- **anupamk**：认为该方案各有取舍——专用编排器把应用逻辑与工作流管理解耦，让工作流成为「一等公民」更易理解；在多团队的大型分布式系统中，集中式编排的总成本更有说服力。
- **llimllib / vrm**：指出 Armin Ronacher 的 `absurd` 项目是 Postgres 持久化工作流的另一种实现；其 Rust 衍生版在吞吐要求不高时保持客户端极简，连编程智能体都能轻松理解整个系统。
- **saxenaabhi**：基于实际使用对比 DBOS、Restate.dev 和 Cloudflare Workflows——DBOS 在「原子消息绑定 Postgres 事务」这类对可靠性要求极高的场景「绝对出彩」，其他几家则适合不同用例。
- **throwaw12 / jhot**：询问 DBOS 对比 Temporal 的体验，提到 Temporal 的负载大小限制（>2MB 文件需绕路）；有 1.5 年 Temporal 经验者建议用一层轻量 REST API 包装来处理事件驱动触发，让事件保持无逻辑。
- **pants2**：觉得 Temporal 过于复杂、其 Cloud 定价高得离谱，更愿意在 Postgres 里自建而非维护独立基础设施。
- **temporal_thr123**：运行大型本地 Temporal，认为它设计糟糕且慢，称数据库会成为瓶颈，还有「每个分片单 I/O、外加互斥锁」这类限制性设计决策。
- **cyberpunk / lll-o-lll**：前者也遇到 Temporal 扩展问题，称中等规模部署跨环境需 200+ vCPU、年成本约 100 万美元；后者质疑这种「百万级」基础设施说法，反问到底是 Temporal 的根本设计问题，还是用错了场景。
- **joshka / hmaxdml**：前者警告「从简单起步」最终仍要自己实现重试、退避、超时、取消、版本化、可见性和运维工具——这些工作流引擎本就具备；后者补充在 Postgres 上构建可靠的持久化工作流引擎本身就很难，足以让专门团队（如 DBOS）来做。
- **nulltrace**：指出 Postgres 的实际限制——worker 数量上升时，vacuum 难以处理死元组（dead tuples）、可见性映射退化、规划器不再有效使用索引。
- **doginasuit / UltraSane**：感叹 SQLite 和 Postgres 都像「魔法」般的基础技术，并赞赏讨论中来自资深从业者的、有信息量的中肯批评。
