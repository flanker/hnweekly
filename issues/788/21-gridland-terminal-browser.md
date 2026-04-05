---
layout: article
title: "Gridland：构建同时在终端和浏览器中运行的应用"
issue: 788
number: 21
category: show_hn
original_url: "https://www.gridland.io/"
hn_url: "https://news.ycombinator.com/item?id=47505731"
date: 2026-04-03
---

## 文章摘要

Gridland 是一个基于 OpenTUI 和 React 构建的框架，专门用于开发终端应用程序（TUI），其最大特色是：使用 Gridland 构建的应用不仅可以在传统终端中运行，还能直接在 Web 浏览器中运行。Gridland 的官网本身就是用该框架构建的，展示了一个典型的终端风格界面在浏览器中运行的效果。

该框架的核心技术架构建立在 OpenTUI（一个开源的终端用户界面库）之上，并与 React 深度集成，允许开发者使用熟悉的 React 组件模式来构建终端应用。这意味着前端开发者可以利用已有的 React 知识和生态系统来创建功能丰富的终端界面，而无需学习全新的 TUI 专用框架。

Gridland 使用 Canvas 渲染技术来在浏览器中呈现终端界面，相比传统的基于 DOM 的方案（如 xterm.js），Canvas 渲染带来了显著的性能优势——加载时间从 2-3 秒降低到几乎瞬时加载，同时大幅减少了画面闪烁问题。

使用 Gridland 非常简单，开发者可以通过 `bunx @gridland/demo landing` 快速查看演示效果，或使用 `bun create gridland` 创建新项目。框架要求开发者避免使用 Node 或 Bun 特有的导入（specific imports），以确保应用能够在终端和浏览器两个环境中正常运行。对于文件系统需求，框架推荐使用 isomorphic-git/lightning-fs 等跨平台方案。

该框架适用于多种场景，包括但不限于：交互式命令行工具、终端仪表板、开发者工具、教育编程平台等。它的浏览器运行能力解决了终端应用推广中的一个核心痛点——用户无需安装任何东西即可在浏览器中体验终端应用，大大降低了使用门槛。

Gridland 是 OpenTUI 的一个 fork 版本，在其基础上增加了浏览器兼容层和 React 集成，项目代码托管在 GitHub 上，文档托管在官网的 /docs 路径下。

## HN 评论精华

**积极反馈：**

- **kamens** 简洁地表达了喜爱："我喜欢这个。它需要存在，因为它有趣。"项目作者 **rothific** 回应表示感谢，认为"一切都是为了乐趣"。

- **rsafaya** 分享了实际测试结果：在实时 MQTT 数据流仪表板场景中，Gridland 展现了"流畅的更新"和极少的闪烁，验证了框架在实际应用中的性能表现。

**与 xterm.js 的比较：**

- **theturtletalks** 询问 Gridland 是否可以替代 xterm.js。rothific 确认可以，前提是"你也愿意使用 React"。theturtletalks 表示了兴趣，提到最近在 Next.js 项目中使用了 Ghostty-web，暗示对 Web 终端方案有实际需求。

- **rablackburn** 进一步询问与 xterm.js 的具体区别，提到了教育编程方面的应用场景。rothific 强调了性能优势：加载时间从之前的 2-3 秒变为即时加载，Canvas 渲染减少了闪烁。

**技术讨论：**

- **curious1008** 称赞项目"真正有用"，指出让用户尝试终端应用通常需要安装开销，而浏览器运行消除了这一障碍。他还建议 WebSocket 支持可以在浏览器版本中启用实时功能。rothific 确认 WebSocket 方案可行。

- **oDot** 询问是否可以用 OpenTUI Core 替换 blisswriter.app 上的演示。rothific 表示应该可行，因为 Gridland 本身就是 OpenTUI 的 fork。

**总体评价：** 社区对 Gridland 的反应非常积极，主要关注点集中在其与现有方案（特别是 xterm.js）的对比优势、实际应用场景，以及框架的技术限制（需要使用 React、需要避免平台特定导入等）。
