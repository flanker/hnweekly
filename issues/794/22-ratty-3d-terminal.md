---
layout: article
title: "Ratty——一个能在终端里嵌 3D 图形的 TempleOS 式实验"
issue: 794
number: 22
category: code
original_url: "https://ratty-term.org/"
hn_url: "https://news.ycombinator.com/item?id=48093100"
date: 2026-05-22
---

## 文章摘要

**Ratty** 是 Orhun Parmaksız（Ratatui 作者之一）做的一个**GPU 渲染、支持 inline 3D 图形的终端模拟器**。它的灵感写在主页上很直白——"**inspired by TempleOS，用 Rust 和 Ratatui 构建**"。Terry Davis 当年在 TempleOS 里可以把 3D 模型像 emoji 一样嵌在文档里——Ratty 让这个想法在 2026 年以现代 GPU 管线重新出现。

**核心功能**——光标本身是一只**会自转的 3D 老鼠**（可以换成你喜欢的任何 3D 模型）。整个终端不是一张文字平面，而是一个**3D 画布**，你可以把 3D 模型和精灵图直接插到指定的"行列"位置，它们会跟随终端文字一起滚动、定位。

**渲染流水线**分三段：(1) PTY 把 escape sequence 喂给 vt100 crate 做终端模拟；(2) Ratatui 生成终端 buffer，用 Vello + Parley 把它渲染成 GPU texture；(3) **Bevy 游戏引擎**把这张 texture 放到 3D 场景里渲染，配上相机、灯光、动画模型。换句话说，这本质上是个**披着终端外皮的 Bevy 应用**。

**Ratty Graphics Protocol (RGP)** 是它定义的协议——通过 APC escape sequence 让应用程序往终端里发 3D 内容：

```
ESC _ ratty;g;<verb>;<parameters> ESC \
```

四种基础操作——能力查询、资源注册、对象摆放、删除。配套有一个 `ratatui-ratty` widget 库，让已有的 Ratatui 应用可以直接集成 3D 图形。

代价也很坦诚——**约 300MB RAM、显著 CPU 占用**——作者在 README 里有一句让 HN 笑翻的话："**Don't worry, all of these dependencies are worth it.**"——典型的 Rust + Bevy + Vello + Parley 全家桶。它不打算和 Alacritty / kitty 在轻量化上竞争，它在跟 TempleOS 在精神上对齐。

## HN 评论精华

- **pjmlp**（历史课）：抛出了所有 Lisp/Smalltalk 老人都会跳出来说的话——"**UNIX 还在追赶 Xerox 工作站的 REPL 体验，更别说 Lisp 机器了。**"他贴了 1981 年的视频，那时候 Xerox 工作站已经能 inline graphics 了——我们绕了 45 年又回来。

- **noelwelsh**（理论高度）："没理由终端只支持文本。数据科学 notebook 已经展示了一种演化路径。这个空间里发生着很多有趣的事，**kitty 大概是其中最激进的创新者**。"——但他也说，"**我不觉得有一个整体的愿景**"，每个终端各自往不同方向加图像。

- **zackoverflow**（业内人）：表示 **kitty graphics protocol 已经能做到很多类似的事**——他贴了自己用 kitty 渲染 3D 图形的 demo。但关键的差异是 vsync——"**Ratty 看起来也没解决 vsync 问题**——渲染没对齐的话，终端模拟器读 framebuffer 的瞬间应用程序正在写，就会出现视觉伪影。"

- **joouha**：发现 Ratty 的博客文章里提到了**新提案 glyph protocol**，这正是他半年前抱怨缺失的东西——一个终端图形的更通用底层协议。

- **iugtmkbdfil834**（致敬）："R.I.P. Terry. 愿你不被遗忘。"——TempleOS 的精神继承在这个项目里很明显。

- **CTDOCodebases**：被作者那句 "**Don't worry, all of these dependencies are worth it**" 笑到——一个 300MB 内存的终端模拟器附带 Bevy 游戏引擎，确实只能用这种坦白卖萌的方式辩护。

- **ghostoftiber**：联想到 compiz 时代——**当年所有人因为"窗口在立方体上"和"果冻窗口"沸腾，然后立刻去装了它**。"我就是那个人，我刚刚立刻装了 Ratty。"

- **sigseg1v**：提出了几个实操问题——既然渲染能力这么强，**能不能也用来把终端里的 2D 图像做得更好**？以及"**这玩意通过 SSH 怎么办？**"——因为它是 GPU 加速的，远程登录场景明显出问题。

- **amelius**：一句话总结这个领域——"**终端正在慢慢变成一个完整的 web 浏览器。**"

- **darkwater**（最实在的问题）："**那么在 Ratty 里 `cat` 一个 3D 模型文件会发生什么？**"——这是协议设计应该回答的核心问题。

- **mncharity**（深度长评）：从 VR/AR 角度补了一层——他多年前做过浅 3D UI 用于软件开发（几厘米深，避免 VAC 眼疲劳）。wiggle 3D、webcam 头部追踪透视、shutter glasses 立体——他把可能的实现路径全列了一遍。在他看来 Ratty 这种"字符有深度"的能力**是 Lisp 机器都没有的新东西**，但他建议先用 fg/bg 颜色、unicode、动画原型 UX 概念，再考虑要不要真上 3D。

- **arkwin**："**离电影《Hackers》里那种终端又近了一步——我完全支持。**"

- **JSR_FDED**：完美总结这个项目的美学——"**纯 Hollywood OS——疯狂敲'上传病毒'之类的咒语，现在还得加上一个莫比乌斯带式扭曲的终端。**"
