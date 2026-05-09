---
layout: article
title: "我正式从 Emacs 退役了"
issue: 793
number: 12
category: favorites
original_url: "https://nullprogram.com/blog/2026/04/26/"
hn_url: "https://news.ycombinator.com/item?id=47906651"
date: 2026-05-08
---

## 文章摘要

Chris Wellons（nullprogram.com 作者，Emacs 圈内的资深开发者，写过 Elfeed 等知名包）发了一篇克制但分量很重的博文：他正式从 Emacs 退役。这件事不是临时起意——他从大约十年前开始逐步过渡，先是引入 modal editing，后转向 Vim 风格的按键，最后才走到完全弃用 Emacs 这一步。整整 20 年的日常依赖，就这样收尾了。

让人意外的是，这次"分手"几乎没有怨气。Wellons 没有抱怨 Emacs 慢、不抱怨 Elisp 难、也不抱怨生态老化。他给出的真正理由是：**自己积累的能力终于到了可以快速重写关键工具的程度**。多年来他在 Emacs 中"宿主"了大量个人应用——其中最重要的两个是 `M-x calc` 计算器和 Elfeed 这个用了 13 年的 RSS 阅读器。如今他用 C++ 加 wxWidgets 把它们重写为原生跨平台 GUI 应用：**stackcalc** 替代 calc、**Elfeed2** 替代 Elfeed，覆盖 Windows、macOS 和 Linux。

技术选型上，他对比了 Dear ImGui 等 immediate-mode 方案，最终选择 wxWidgets，理由是后者提供了完整的"平台层"——文件 I/O、路径处理、剪贴板、字体度量这些杂事都已经被吃掉了，让他能专注业务逻辑。这种选择体现了一个老派工程师的判断：**与其每个项目都从零拼装，不如用一个略重但稳定的框架，它会把跨平台坑替你填好**。

文章的真正主题不是 "Emacs 不行了"，而是 "我自己变强了"。当年他需要 Emacs 是因为 Emacs 是当时最快做出"小工具"的环境；现在他用现代 C++、libc++、自有的 stb 风格小库就能在几天内完成同样的功能，并且产物是一个独立、可分发、不依赖任何运行时的原生程序。**Emacs 完成了它的历史使命**，对 Wellons 个人而言。

## HN 评论精华

1. **iLemming**：站在反方——他认为现在反而是 Emacs 的"第二春"。把 LLM 接到 Emacs 的活体 REPL 上，模型可以做实证推理而不是凭空猜测；Emacs 的文本提取、buffer 操控能力在 LLM 工具链时代依然是别处难以复制的护城河。

2. **turtleyacht**：澄清一个常见误解——Spacemacs 不是"另一个 Emacs"，而是元配置（meta config），价值主要在于让初学者绕过原生键位，对资深用户帮助有限。这条提醒新读者别在 Emacs 入门时迷失在 distro 选择里。

3. **farfatched**：质疑"配置摩擦"概念——一个深度定制的 Emacs 配置可以稳定运行多年不需要碰，反倒是各种"极简编辑器"才需要不停搭脚手架。所谓"极简等于低维护"未必成立。

4. **computably**：把"用 AI 写代码"和"被 AI 接管"区分开。Wellons 在博客里提及 LLM 加速了他重写工作，这是工具性的，与所谓"AI 致幻"完全是两回事。这个区分对当下的 AI 讨论很关键。

5. **Pay08**：盛赞 Magit 是 UI 设计的典范——把 Git 这种复杂工具变得几乎"自我教学"，而又不牺牲高级功能。这条评论被普遍认同为 Emacs 即使作为整体退役、其内部的 Magit 仍然是无法替代的孤品。

6. **mattdeboard**：补充——离开 Emacs 十年，依然找不到能匹敌 Magit 的工具。这种"肌肉记忆资产"是 Emacs 用户最难放弃的东西，也是 Wellons 这种"清空门户"式离开如此罕见的原因。

7. 评论区的整体情绪不是"Emacs 死了"，而是"Emacs 在某些人的人生阶段完成了任务"。Wellons 的文章被理解成一种健康的告别——工具是手段不是身份，用了 20 年依然可以在合适的时候放下，这种工程理性反而是 Emacs 文化里最值得保留的部分。

8. 也有人指出一个隐含信号：Wellons 把工具迁出 Emacs 是因为"GUI 跨平台"如今变得足够容易了。这件事对于其他还把生活宿主在 Emacs 里的人也是个提醒——也许你的某些 Elisp 工具现在用 wxWidgets/Tauri/Rust 重写一周就能搞定，并且能给家人朋友也用上。
