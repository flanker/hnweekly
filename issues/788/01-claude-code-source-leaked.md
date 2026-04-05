---
layout: article
title: "Claude Code 源代码通过 NPM Source Map 文件泄露"
issue: 788
number: 1
category: favorites
original_url: "https://twitter.com/Fried_rice/status/2038894956459290963"
hn_url: "https://news.ycombinator.com/item?id=47584540"
date: 2026-04-03
---

## 文章摘要

2026 年 3 月 31 日，安全研究员 Chaofan Shou 发现 Anthropic 旗舰产品 Claude Code（其 AI 编程命令行工具）的完整源代码通过 NPM 注册表中的 source map 文件意外泄露。这一事件迅速在技术社区引发广泛讨论，HN 上获得了超过 2000 点赞和 1000 多条评论。

**泄露方式：** 在 `@anthropic-ai/claude-code` NPM 包的 2.1.88 版本中，一个 59.8 MB 的 JavaScript source map 文件（.map 文件，本用于内部调试）被意外包含在公开发布的包中。根本原因是 `.npmignore` 或 `package.json` 中的 `files` 字段配置错误。值得注意的是，Anthropic 团队后来使用了 `npm deprecate` 而非 `npm unpublish` 命令来处理，导致 source map 在标记弃用后仍然可以被下载。

**泄露规模：** 数小时内，这个约 512,000 行的 TypeScript 代码库就被镜像到 GitHub 上，被数千名开发者分析。公开的 GitHub 仓库迅速获得了超过 1,100 个 Star 和 1,900 多个 Fork。

**泄露内容揭示了以下关键信息：**

1. **功能标志（Feature Flags）：** 共发现 44 个功能标志，涵盖了已完整构建但尚未发布的功能。这些功能在对外发布的构建版本中被编译为 `false`。
2. **内部代号：** 泄露证实 "Capybara" 是 Claude 4.6 变体的内部代号，"Fennec" 对应 Opus 4.6，而 "Numbat" 仍处于测试阶段。
3. **未发布功能：** 包括 "Kairos"（助手模式）、"Buddy System"（一个类似电子宠物的 ASCII 艺术伴侣系统，后被确认为愚人节彩蛋）、以及 "Undercover mode"（用于从员工开源贡献中剥离 Anthropic 内部数据的模式）。

**Anthropic 官方回应：** 发言人声明称"今天早些时候，一次 Claude Code 发布包含了一些内部源代码"，并强调"这是由人为错误导致的发布打包问题，而非安全漏洞。我们正在推出措施以防止此类事件再次发生。"

**代码质量争议：** 泄露的代码引发了关于 AI 生成代码质量的激烈讨论。有用户指出其中存在一个长达 3,167 行、拥有 486 个分支的巨型函数，缺乏适当的错误处理，以及违反关注点分离原则的单体文件结构。

## HN 评论精华

**关于代码质量的两极评价：**

用户 mohsen1 详细分析了泄露代码中的问题模式，包括超长函数、过高的圈复杂度（cyclomatic complexity 达 486）和嵌套异步操作缺乏错误处理。这引发了社区对"AI 写的代码到底质量如何"的深入讨论。

批评方认为，"vibe coding"（不注重架构标准地生成代码）会创造出不可维护的系统，代码应该保持人类可审查。务实派则反驳说，如果代码是"LLM 维护的"，那么模块化等传统指标就不那么重要了——只需更新规格说明让 AI 重新生成即可。有开发者直言"对 AI 生成的代码做代码审查毫无意义"。

**关于 NPM 包管理的失误：**

多位评论者指出 Anthropic 使用 `npm deprecate` 而非 `npm unpublish` 是一个明显的操作失误，因为 deprecate 只是标记警告，不会阻止下载。这暴露了团队在 NPM 包管理流程上的经验不足。

**关于"吃自己的狗粮"（Dogfooding）：**

社区热议 Anthropic 是否真的在用 Claude Code 来开发 Claude Code。泄露的代码在某种程度上证实了这一点，但代码质量问题也让人质疑这种做法的效果。

**趣味发现：**

有人注意到代码中 spinner 加载动画使用了 "reticulating splines" 等有趣的动词短语，这是对经典游戏 SimCity 2000 的致敬，引发了一波怀旧讨论。

**对 AI 行业的反思：**

评论中反映出对 AI 生成物的普遍怀疑态度。用户们指出，抵触情绪主要源于质量问题和归属问题，而非原则性的反对。这次泄露事件也被部分人视为一次"意外的最佳 PR 事件"——让公众看到 Anthropic 确实在大规模使用自己的工具。
