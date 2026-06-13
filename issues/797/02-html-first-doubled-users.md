---
layout: article
title: "构建 HTML 优先的网站让我们的用户一夜翻倍"
issue: 797
number: 2
category: favorites
original_url: "https://mohkohn.co.uk/writing/html-first/"
hn_url: "https://news.ycombinator.com/item?id=48475483"
date: 2026-06-12
---

## 文章摘要

作者讲述了如何用"HTML 优先"的思路重建一个失败的公用事业公司表单应用，从而让完成提交的用户数量翻倍。所谓 HTML 优先，是指先用语义化 HTML 构建一个无需 JavaScript 即可正常工作的完整网站，再通过 Web Components 以渐进增强的方式叠加 JavaScript，确保服务在各种设备与网络条件下都能运行。

原先基于 React 的应用之所以失败，是因为它必须依赖 JavaScript 才能工作，从而把网络条件差、设备老旧或不支持 JavaScript 的用户挡在了门外。更隐蔽的是：基于 JavaScript 的分析工具根本"看不见那些被 JavaScript 故障弹走的用户"，连流失都无从统计。

重建后的版本使用 Astro 取代 React，主要特性包括：

- 多步骤表单向导，每一步都在服务端持久化数据（包括图片上传）。
- 以 HTML5 原生表单校验为基础，辅以一个自研 Web Component（后开源为 validation-enhancer）增强。
- 核心功能完全不依赖 JavaScript。

作者总结的关键经验：公共服务有义务服务所有人；成熟的软件工程意味着以可靠性而非追逐潮流框架为先；表单校验不必复杂，HTML 内置校验加少量增强就胜过复杂的库；数据持久化很重要（有用户在开始一个月后才完成表单）；"按照 3G 网络下的老旧设备来构建"——能在那里跑通，就能在任何地方跑通。

## HN 评论精华

**chao-** 指出，许多初/中级工程师缺乏 Web 基础知识，因为他们先学框架而非 HTML/HTTP 基础。他强调这些人"并不笨"，只是受训练营式教育路径影响而存在知识断层。

**igsomething** 分享了真实案例：一个性能严重低下的政府网站被强制要求使用 React，尽管大家都认为它并不合适，说明制度性约束往往妨碍开发者做出更好的技术选择。

**yesco** 谈到职业路径的时序问题：开发者有时先学会了复杂的框架概念，反而把简单的 HTTP/HTML 基础当成了"高深玄学"。

**rhelmer** 认为开发者在 SPA 生态之外缺少熟悉的"积木"。如今 Astro、Next.js 等现代框架已经能同时支持静态生成与交互性，把"快速的 HTML 与渐进增强"结合起来弥补了这一缺口。

**meander_water** 指出实际权衡：基础 CRUD 站点用 HTMX/HTML 更轻松，但复杂组件与 OAuth 集成有大量现成的 React 库，而 HTML 方案的替代品有限，框架选择因场景而异。

**WorldMaker** 提到前后端团队之间的壁垒：HTML 优先需要更紧密的协作，当团队各自为政、校验逻辑各不相同时，渐进增强会显得更难落地。
