---
layout: article
title: "Clerkenwell：一个手写的 Clojure 笔记本"
issue: 815
number: 26
category: code
original_url: "https://handwritten.danieljanus.pl/2026-10-20-clerkenwell.html"
hn_url: "https://news.ycombinator.com/item?id=50167657"
date: 2026-10-23
---

## 文章摘要

这篇文章本身就很特别：Daniel Janus 的这个博客叫 handwritten，文章是在电子纸平板上手写、再以 SVG 图片发布的，页面上另附了一份文字转录稿。他介绍的 Clerkenwell 是一个「手写的 Clojure 计算笔记本」，可以理解为电子纸上的 Jupyter。

**它做什么。** 几个月前作者发布过 Edsger，一个运行在 reMarkable 2 上的手写 REPL，他自己说那更像是实验、概念验证和「艺术宣言」。Clerkenwell 延续了「东西自动出现在电子纸平板上」这个主题，但实用得多，他已经差不多每天都在用。它像 Jupyter 或 Wolfram Notebook 一样混合代码、文字和图像，区别是你主要用平板和手写笔来表达想法：在纸上写下思路、推演某段代码或某道数学题。平板与电脑上 Emacs 里运行的 Clojure（CIDER）REPL 双向绑定：在 Emacs 里求值，结果会自动出现在电子纸上，可以渲染成原始数据、图片、表格、图表或格式化的 Markdown（输出类型由 Kindly 区分，图表通过 Plotje 生成）；反过来，在平板上随手写的东西也会自动渲染到 Emacs 里。用户可以在 REPL 输出上随意涂画。目前只支持固件 3.27.1.0 的 reMarkable 2，也就是作者自己那台；其他 reMarkable 型号移植起来应该不难，Supernote、Onyx 等设备则需要较大的架构调整。

**为什么要做。** 作者一直喜欢手写，记纸质日记，攒了一堆各种主题的笔记本，空白本子的库存「够用一千年」。他很早就发现纸笔能以文本编辑器做不到的方式帮他理清思路，因为纸笔天然只能做一件事；确诊 ADHD 之后，这一点对他更重要，而 reMarkable 保留了这种专注感。几周前，他在 reMarkable 上琢磨一段代码时，旁边开着 REPL 查底层细节，结果发现自己在手工把 Clojure 表达式和结果抄到笔记本上。他觉得这说不通，一定有更好的办法，于是找 Opus 讨论，借用 Edsger 的部分代码，最后做出了 Clerkenwell。

**「颠覆性的 slop」。** 作者毫不掩饰：代码一看就是 LLM 写的，啰嗦，夹满了难以消化的「LLM 腔」注释，堆着价值可疑的测试，有些过度设计，但「基本能用」。README 里也写明几乎所有代码和部分设计由 LLM 生成，人工审查状态是「粗略筛查」，能确认没有恶意代码但还没完全读懂，README 和文档则由人亲手写。他给自己定的规矩是：slop 必须证明自己存在的价值。Clerkenwell 的价值在于用途，即用来研究软件是怎么运作的。他不认同把思考整个外包给机器、不在乎输出怎么工作、做了什么、有哪些权衡的做法。每次生成自己不完全理解的代码（包括 Clerkenwell 本身），他都会不安，觉得自己欠下了「理解债」，并打算还清，因为他看重问责，希望代码真正属于自己、能为它担保，不想让理解机制的「肌肉」萎缩，更因为想要理解本就是他的一部分。所以他称之为「颠覆性」：别的工具鼓励以非人的速度堆功能，Clerkenwell 却鼓励慢下来，去咖啡馆或图书馆，断开网络，用临时网络连接设备，享受思考。它和他的 GPD Micro PC 很搭，甚至可以拿来研究它自己的代码，他希望它随时间变得更简洁。

**后续计划。** 功能上他觉得已经差不多够了，除非真有需要不会再加，没用的功能反而可能删掉。他更想扩大受众：移植到更多设备，扩展到 Clojure 以外的后端，比如支持 Jupyter 内核协议，或利用 nREPL 的语言无关性。名字来自伦敦以钟表业闻名的 Clerkenwell 街区，钟表是需要精确和细致思考的手艺；同时致敬 Clojure 笔记本环境 Clerk，也让人想到留下大量笔记的物理学家 James Clerk Maxwell。他还会在华沙的 Clojure Warsaw #1 聚会上做现场演示。

## HN 评论精华

这是一篇 Show HN，只有 27 分、4 条评论，讨论非常少，作者也没有在帖子里补充说明，下面如实列出全部内容。

**简短的称赞。** mannycalavera42 为作者打气，说很喜欢这个项目；neocanable 说「喜欢，真的是手写的！」，指的是整篇博文都是手写稿；MiroslavPokorny 感谢分享。

**名字引起的回忆。** ericio 说一看到 Clerkenwell 这个名字，就想起曾经用 Alexsey Budovskey 的动画短片《Bathtime at Clerkenwell》哄儿子开心，那是孩子洗澡时最爱看的片子，每次都能逗得他咯咯笑。

除此之外没有关于技术实现、LLM 生成代码或电子纸工作流的讨论。
