---
layout: article
title: "用 AI 把代码写得更好，而不是更快"
issue: 795
number: 19
category: code
original_url: "https://nolanlawson.com/2026/05/25/using-ai-to-write-better-code-more-slowly/"
hn_url: "https://news.ycombinator.com/item?id=48272984"
date: 2026-05-29
---

## 文章摘要

主流叙事总在鼓吹用 AI 编程工具最大化「速度」和「产出量」，而 **Nolan Lawson** 在这篇文章里反其道而行：他主张用大语言模型来写出**更高质量的代码**，哪怕这意味着开发节奏变慢。核心理念是把 AI 从「生产力 hack」重新定位成**面向已经在乎代码质量的工程师的「协作式质量工具」**。

他详细描述了自己的**多模型代码审查（multi-model code review）**工作流：在一个 PR 上同时跑 **Claude、Codex 和 Cursor Bugbot** 三个审查者，让它们各自独立地找出问题并按严重程度（critical / high / medium / low）分级；由于互相不知道对方结论，可以避免「锚定偏见（anchoring bias）」。随后由一个主智能体汇总各方发现、做验证性研究、生成最终报告。修复时优先处理 critical/high 级别的 bug，再决定 medium/low 级别是否值得花精力。他把这套提示框架做成了一个可复用的「skill」并发布为 GitHub gist，bug 的定义可基于 KISS/DRY 原则、HTML 无障碍、SQL 索引等自定义。

文章给出了几个具体收益：有读者用这套流程发现了 **Postgres 行级安全（RLS）策略**里一个本会随版本上线的安全漏洞；代价则是「烧掉一大堆 token」来尽早发现走不通的方案。Lawson 强调真正的回报不是「10 倍生产力」，而是对**架构和失败模式的更深理解**——顺手修掉评审中发现的历史遗留 bug，比单纯堆功能更令人满足，也让陌生代码库通过「找 bug」的过程变得可学习，效果甚至胜过读文档。文中也保留了重要的 nuance：**初级工程师可能难以分诊（triage）这些发现**，因为理解一个 bug 需要的是经验，而非仅仅检测能力。

## HN 评论精华

- **kiba**：把 LLM 当成学习陌生代码的「家教」，先写出不完美的代码再迭代纠正。反馈循环虽紧凑但慢（一个 JSON + 知识图谱花了 2 小时）。警告过度依赖 AI 生成代码会损害自己的阅读理解能力。
- **scosman**：给出一套结构化流程——5 轮研究/规划、与 AI 交互式决策、分多个阶段实现并配合代码审查循环，整体提速约 5 倍。
- **bottlepalm**：描述「主动监督」打法——审查、回归测试、对 AI 建议提出反对意见，声称能在 v1 的时间里产出「v3 级别」质量；还会并行开多个 AI 会话，让一个去审查另一个的代码，独立验证反馈后再实施。
- **glhaynes**：指出原作者明确说的是达到了「v3 质量」，这反驳了那种「光是 babysitting 却没真正收益」的说法。
- **locknitpicker**：反对「代码自解释」的说法，主张用清晰、经过重构、带描述性辅助函数的代码，而非靠注释。
- **bottlepalm / manmal / surajrmal**：吐槽 AI 注释要么太少（如 Codex）要么过多（近期的 Claude），需要手动调整；近期 Claude 版本注释明显啰嗦，提交前得手动精简；有人推荐参考 antigravity 模块的注释密度。
- **rootnod3**：强调要对所有生成代码保持掌控，必要时自己也能手写、只是慢一些；并反问别人是如何保持上下文的。
- **maximinus_thrax**：质疑在规模化场景下完全掌握代码不现实，现实中的 deadline 让细致监督几乎不可能。
- **efitz**：同时管理 2 个并行 AI 会话已经让人精疲力竭，质疑这种打法能否在普通工作环境里扩展。
- **mohamedkoubaa / DonHopkins**：调侃 AI 宕机时只好退回白板编程；以及在 AI 生成/部署的间隙摸鱼睡觉，引用了 XKCD #303「编译中（剑斗）」的梗。
