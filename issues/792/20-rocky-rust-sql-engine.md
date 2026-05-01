---
layout: article
title: "Show HN：Rocky —— 用 Rust 写的 SQL 引擎，支持分支、回放与列级血缘"
issue: 792
number: 20
category: show_hn
original_url: "https://github.com/rocky-data/rocky"
hn_url: "https://news.ycombinator.com/item?id=47935246"
date: 2026-05-01
---

## 文章摘要

Rocky 由 Hugo Correia 开发，定位是"数据仓库流水线的信任系统"——一个用 Rust 写的转换/控制平面，本身不存数据也不做计算，而是把存储和计算依然交给 Databricks、Snowflake 这类现有数仓，只在上层叠加治理、血缘、契约和实验能力。它要解决的是 dbt 这一类工具长期欠缺的问题：编译期保证、列级影响分析、安全的实验分支。

核心特性可以分四块。第一，**列级血缘**：Rocky 的编译器解析 SQL 时会穿越 join、CTE、window function，把每一列从下游事实表反向追到源头 seed，因此可以在合并前就回答"修改这一列会炸掉哪些下游"，比基于运行日志事后解析的表级工具粒度细得多。第二，**编译期数据契约**：缺失列、不安全的类型变更会以诊断码形式在转换执行前报错，而不是上线后才出问题；上游 schema 漂移也能被立即侦测，必要时重建目标表。第三，**分支与回放**：通过 schema 前缀建逻辑表副本（计划接入 Delta SHALLOW CLONE 与 Snowflake zero-copy clone），可以隔离地跑实验、看结果，再决定 promote 或丢弃；同时引擎能完整回放某次 SQL 跑过的输入和过程。第四，**治理面**：列分类标签、按环境的脱敏策略、8 字段审计轨迹、合规汇总、按模型的成本归因，以及 PR 时的血缘 diff（与 GitHub 集成），还包括 LLM 辅助生成模型的反馈校验循环。

工程结构是个多语言 monorepo：Rust 引擎是包含 20 个 crate 的工作区，对外暴露 CLI；Python 端用 wheel 包封装 CLI 与 Dagster 集成；编辑器侧是 VS Code 扩展（TypeScript），通过 LSP 和 AI 接入。

## HN 评论精华

- **关于 LLM 痕迹的批评**：多位评论者指出 Show HN 帖子的措辞和早期回复读起来"太像 AI 写的"。作者坦诚承认自己第一次发公开项目比较紧张，确实用 LLM 帮忙润色营销文案，但强调代码本身和架构决策都是为了解决真实生产问题做出的。这件事在评论区引发了一波关于"AI 生成宣发文 vs 真实工程"边界的讨论。

- **关于先行工作与署名**：有评论者贴出一篇 arXiv 论文，主题是"correctness-by-design pipelines"，覆盖了类似的设计思想。作者回应会认真读一下并在文档里补充对应引用。这是对独立开发者一个很好的提醒：在 dbt/dbtMeta/SQLMesh 这样已成熟的赛道里，新项目最好显式地说明定位差异。

- **关于命名冲突**：用户指出 Rocky 容易和 Rocky Linux、RocksDB 混淆。作者解释名字来自他养的狗，并欢迎大家提改名建议。

- **关于状态化模型与快照隔离**：有评论问到分支上的快照表如何处理。作者承认目前的"快照隔离"还不完整——分支上的 snapshot 表会从空开始，而不是继承主表的历史，这是接下来要补的洞。

- **关于成本归因**：评论指出 Snowflake 的计费模型是 credit/秒级，跟单纯统计扫描字节数不一致。作者回应 Rocky 的 cost 字段会同时记录 bytes scanned 与 duration，而不是只取一个数字，给后续做不同维度的成本汇总留出空间。

- **与 dbt 的对比**：尽管原始评论里没有完整的对比表，但讨论的方向集中在"dbt 把测试和数据契约推到 runtime，Rocky 把它们提前到 compile time"——也就是说 dbt 跑了一次才知道列没了，而 Rocky 在 SQL 还没下发之前就能拒绝合并。

- **整体氛围**：评论者对"列级血缘 + git 风格分支"这个组合普遍持正面态度，认为这正是大数据团队在 PR 评审时缺的东西，但也提醒作者要把宣传与实际能力对齐，特别是那些还在 roadmap 里的特性（Delta SHALLOW CLONE、Snowflake zero-copy 等）应该明确标注当前状态。
