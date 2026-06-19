---
layout: article
title: "Show HN：Paca — 面向人机协作的轻量级 Jira 替代品"
issue: 798
number: 20
category: show_hn
original_url: "https://github.com/Paca-AI/paca"
hn_url: "https://news.ycombinator.com/item?id=48515385"
date: 2026-06-19
---

## 文章摘要

Paca 是一个可自托管的开源项目管理系统，定位为 Jira、ClickUp 等企业级工具的轻量替代品。它最大的特点是把 AI Agent 当作「一等公民」的 Scrum 团队成员，而非边缘化的自动化工具。其核心理念是：「AI Agent 应当参与 Scrum 流程，而不只是孤立地产出结果。」团队围绕一个 P-A-C-A 循环（Plan 规划、Act 执行、Check 检查、Adapt 调整）运转，人类与 AI 在同一块看板、同一个冲刺、朝着共同目标协作。

主要能力包括：人类与 AI Agent 共享一块实时更新的 Scrumban 看板，Agent 可以认领任务、编写 BDD 规格、参与系统设计文档（SDD）撰写，并与人类队友一同调整；内置 AI 聊天让用户用自然语言规划工作、创建任务、管理文档；一切都可通过项目级配置文件进行配置，并提供基于 WebAssembly 的插件系统（支持 Go、Rust 或 AssemblyScript 编写后端扩展、前端模块），每个插件都在沙箱中运行，采用基于能力的权限模型。

技术栈方面：前端使用 React、TanStack Start 与 shadcn/ui；后端为 Go（Gin 框架）；实时通信用 Node.js + Socket.IO；Agent 执行采用 Python FastAPI 配合 OpenHands SDK（隔离容器）；数据层为 PostgreSQL 与 Valkey；并通过 MCP（Model Context Protocol）服务器接入外部 AI Agent。整体强调「AI 原生、免费、轻量、开源」，以最小化的核心加可选扩展、完整自托管和 Apache 2.0 许可，与厂商锁定、功能臃肿的同类产品形成对比。

## HN 评论精华

- **dagss**：询问当前 AI 辅助开发的工作流，想了解 Paca 与 git worktree 等现有模式相比如何，以及在用 Paca 之后是否还需要 GitHub。
- **flo_r**：指出一个尚未解决的难题——如何判断产出是真的完成，还是只是在 Agent 看来「像是完成了」。
- **jing-ny**：建议把「做事的 Agent」与「宣布完成的 Agent」分开，套用 RACI 原则，强调「负责执行 ≠ 负有最终责任」。
- **tom-wal**：感谢有了这个免费替代品，表示因此省下了每月 10 美元的 Linear 订阅费。
- **sambucini**：欣慰地发现一个具备扎实 CLI/MCP 能力的开源项目管理工具，指出这一品类选择不多。
- **smrtinsert**：认为 Paca 切中了独立「vibe 工程师」需要结构化任务跟踪的需求。
- **Tsarp**：对推广前景持怀疑态度，认为自定义工作流往往高度个人化，不同团队需要的是不同的那 20% 功能，照搬他人方案常常行不通。
