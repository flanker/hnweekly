---
layout: article
title: "OpenKnowledge：AI 优先的开源 Obsidian/Notion 替代品"
issue: 799
number: 21
category: show_hn
original_url: "https://github.com/inkeep/open-knowledge"
hn_url: "https://news.ycombinator.com/item?id=48675435"
date: 2026-06-26
---

## 文章摘要

OpenKnowledge 是一款本地优先（local-first）的所见即所得（WYSIWYG）Markdown 编辑器，定位为面向 AI 协作的知识管理工具，自称是开源的 Obsidian/Notion 替代品。它把自己描述为"一个漂亮的、本地优先的 WYSIWYG Markdown 编辑器，集成了 Claude、Codex 等 AI 工具（harness）"。

核心特性包括：真正的所见即所得编辑体验，类似 Google Docs 那样的协作文档手感；与 Claude、Codex 及兼容 agent 的 AI 协作能力；支持 MCP（Model Context Protocol）以及面向 LLM wiki 和知识图谱的 agentic 搜索；通过 git/GitHub 实现团队协作与自动同步；为工程规范和报告提供丰富的组件支持；同时提供双重界面——桌面应用内的终端 UI 和一个 Web 界面。

在可用性方面，macOS 用户可以下载原生桌面应用（DMG），Linux、Windows 和 Intel 版 Mac 用户则通过 CLI（需要 Node.js 24+）使用 Web 版本。项目主要用 TypeScript 编写（占代码库 97.8%），采用 GPL-3.0 许可证开源，接受公开 PR。它把自己定位为适合个人笔记、知识库、规范文档，以及 LLM 驱动的"第二大脑"的工具。

不过，从 HN 讨论来看，社区对它的实际完成度和差异化定位存在不少质疑，认为其理念有前景但执行尚不完整。

## HN 评论精华

- **kylenessen（高赞）**：认为该项目只是在基础功能外包了一层好看的外壳，使用体验"像一个穿着考究、但功能很基础的包装层（a well dressed wrapper for pretty basic functionality）"，缺乏像 VS Code 那样深度集成的 AI 能力。

- **pcthrowaway**：一针见血地指出矛盾——"号称完全本地，却无法接入任何本地 LLM？"，质疑其隐私主张与实际依赖专有服务之间的不一致。

- **syabro**：抱怨性能慢，且在未经询问的情况下修改了系统配置——"你没问我就改了我的 .zshrc"。

- **tekacs**：报告了数据丢失问题，称用 Codex 初始化时它"毁掉了我的 Codex config.toml 文件"。

- **jfim**：详细对比了与 Obsidian 的功能差距，指出 Obsidian 通过 Dataview 等插件支持跨文档查询，能生成仪表盘和自动化视图，而 OpenKnowledge 缺少这类功能。

- **kbar13**：精炼地点出了市场定位难题——"Obsidian：对 LLM 友好但不擅长协作；Notion：对 LLM 不友好但擅长协作"，暗示 OpenKnowledge 想两头通吃却尚未做到。

- **joshka**：从 UX 角度批评 Electron 的局限，指出要达到"原生应用"的品质，必须在文本选择、滚动和平台特定交互上下功夫。

- **abdullin**：分享了较为高阶的 agentic 工作流，提出企业级场景需要原子化的多文档变更集（atomic multi-document changesets），这是项目尚未覆盖的用例。

- 创始人 **engomez** 在评论区积极回应，承诺近期改进：接入本地模型、支持 OpenCode、加入应用内聊天功能（"几天内上线"）。整体氛围是：架构有前景，但被不完整的执行和与现有方案不清晰的差异化所拖累。
