---
layout: article
title: "pg_durable：微软开源数据库内的持久化执行引擎"
issue: 797
number: 21
category: data
original_url: "https://github.com/microsoft/pg_durable"
hn_url: "https://news.ycombinator.com/item?id=48414367"
date: 2026-06-12
---

## 文章摘要

pg_durable 是微软开源的一个 PostgreSQL 扩展，它把「持久化执行」（durable execution）这一现代编排模式直接搬进了数据库内部。它让长时间运行、可容错的 SQL 函数能够对自身进度做检查点（checkpoint），并在崩溃或重启后从中断处恢复，而无需依赖外部基础设施。

它要解决的痛点是：团队管理复杂工作流时，逻辑往往散落在 cron 任务、worker 进程、消息队列和状态表之间，既脆弱又难以追踪。一旦在长任务中途重启，就意味着已经成功完成的工作要重做一遍。

主要特性：

- **持久化检查点**：函数状态落盘到 PostgreSQL，可在崩溃和重启中存活。
- **SQL 原生 DSL**：用 `|=>`、`~>` 等可组合操作符定义工作流，支持顺序、分支、循环与 join。
- **零外部依赖**：完全运行在 PostgreSQL 内，不需要 Redis、Temporal 等外部服务。
- **数据库原生原语**：内建调度、条件判断与并行执行。
- **多用户隔离**：通过行级安全（RLS）实现用户间隔离。

实现上，pg_durable 用 pgrx（Rust 编写 PG 扩展）构建，分三层：SQL DSL、进程内后台 worker 承载编排运行时、以及 duroxide-pg 状态提供者把运行时元数据持久化到专用 schema。调用 `df.start()` 启动工作流后，后台 worker 逐步执行并在步骤间打检查点；数据库崩溃后从最后一个检查点恢复，而非从头重跑。

## HN 评论精华

**levkk** 将 2026 称为「Postgres 队列之年」，列举了 DBOS、pgQue 等项目，但表示自己更倾向于把队列逻辑放在代码和 Git 里而非数据库中——不过他承认更好的工具可能会改变这种看法。

**dpark** 力挺存储过程：它能通过单调递增 ID 实现版本管理，通过数据库 setup 做单元测试，并把计算下推到数据层以减少 I/O，反驳了对存储过程可行性的质疑。

**junto** 提出存储过程在测试和版本管理上的难题；**dpark** 回应称成熟团队可通过 CI/CD 流水线和数据库级变更管理来处理，类似于容器版本管理。

**joelthelion** 质疑为何要把长任务压到本已承压的数据库上；**gdecandia** 以 pg_cron 为先例，并指出在 AI 流水线中数据库直接发起 HTTP 调用反而能减少往返。

**微软贡献者**（gdecandia、abeomor）透露该方案已在微软内部用于 AI 工作流，存在更高层语言封装；并提到 Azure PostgreSQL 现已支持 pg_textsearch（混合检索）与 pg_diskann（1.6 万维向量），回应了此前关于功能落后于 AWS 的批评。
