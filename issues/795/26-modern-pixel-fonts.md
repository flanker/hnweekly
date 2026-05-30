---
layout: article
title: "几款值得一看的现代像素字体"
issue: 795
number: 26
category: design
original_url: "https://unsung.aresluna.org/a-few-interesting-modern-pixel-fonts/"
hn_url: "https://news.ycombinator.com/item?id=48271448"
date: 2026-05-29
---

## 文章摘要

字体研究者 Marcin Wichary 在这篇文章里盘点了几款近期出现的、各有意思的**像素字体（pixel fonts）**，并借此讨论一个常被忽视的问题：一款像素字体好不好用，往往不在于它好不好看，而在于那些看不见的「字体工程」功夫。

文中介绍的几款字体各有侧重：**Analog Mono**（作者 Andrew Gleeson）改进自 1990 年代消费电子产品上的经典 OSD 字体 VCR OSD Mono，技术上修正了原版基线过低、导致下伸部被不自然上提的问题；**Coral Pixels**（吉田久美子）是一款彩色字体，刻意把 1990 至 2000 年代那种**彩色边缘（color fringing）**重新搬了出来——这种效果原本是次像素渲染（subpixel rendering）的副作用，如今被当成怀旧的视觉元素来用，已上架 Google Fonts；**Two Slice**（Joseph Fatula）走极简路线，整个字高只有 **2 个像素**，作者自嘲它「勉强还能读」。

文章的重头戏是 Vercel 推出的 **Geist Pixel**。Wichary 强调，它的定位不是「装饰性的新奇玩意儿」，而是一个**系统级组件**——真正花心思的地方是那些看不见的活儿：字偶距（kerning）、元数据、字形以及垂直度量（vertical metrics）。这些恰恰是像素字体在实际生产环境里最容易出问题的地方，比如在不同视口尺寸下的缩放表现、以及与排版系统的冲突。

作者的核心论点是：早期的像素字体大多优先考虑美感，而 Geist Pixel 代表了一种转向——「排版的严谨性」。决定一款像素字体能否在真实产品中可靠工作的，是那些不起眼的度量、字距和元数据，而不只是像素画本身好不好看。

## HN 评论精华

评论区一半在安利各自的私藏像素字体，一半在吐槽文章本身有 AI 味。

- **Boltgolt**：直接挑刺文中那句「Geist Pixel 不是新奇玩意儿，它是系统扩展」，配一句「Okay LLM」（暗讽 AI 文风）。**sphars** 出来澄清：这其实是 Vercel 官方介绍 Geist Pixel 博客里的原话，并附上链接。**nnevatie** 仍坚持「一眼就看出 slop，直接从脑子里弹出去了」。
- **rebolek**：「我想要更好的 Topaz，那是我最爱的字体。」**LocalH** 说自己至今仍在终端和 IDE 里用 Topaz（有时 1.x 有时 2.x）。**thristian** 顺势分享了自己做的 Topaz Unicode，目标是「更 Topaz」。
- **sambishop**：作为低分辨率软件爱好者，他认为「真正的 GOAT、自 2003 年起从未被超越」的是 **04b-03** 字体；还笑称如今玩字体的潮流都在「把软件做得像奇幻魔法书」。
- **sssilver**：「Two Slice 的可读性高得离谱。」**jareklupinski** 则吐槽自己卡在了解码「tends」这个词上。
- **bitwize**：火力对准彩色像素，「吉田久美子该被送上战争罪法庭」，认为 ClearType 那种刺眼的彩边「就该留在过去」。
- **RedNifre**：对 Coral Pixel 表示困惑——次像素渲染的本意就是让文字在不显得花花绿绿的前提下更锐利，那种彩边只有在截图后放大才会出现，「感觉极其小众」。**zeckalpha** 回应：「这取决于你显示器的 DPI 和你的眼镜度数。」
- 一串字体安利：**namibj** 推荐 **dotsies**（5 高 1 宽、无横向间距，为视觉密度而非像素稀缺设计）；**efskap** 推荐 **unscii**（为 ASCII art 而生，在终端里也好用，至今还在更新 Unicode）；**msephton** 提到来自日本的 **Elisa** 字体；**sheept** 和 **vibbix** 都推了 **Sans Nouveaux**。
