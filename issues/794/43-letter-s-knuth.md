---
layout: article
title: "Donald Knuth 1980 年的论文：字母 S"
issue: 794
number: 43
category: learn
original_url: "https://gwern.net/doc/design/typography/1980-knuth.pdf"
hn_url: "https://news.ycombinator.com/item?id=48216016"
date: 2026-05-22
---

## 文章摘要

这是 Donald Knuth 1980 年发表在《The Mathematical Intelligencer》上的论文，题目就叫《**The Letter S**》。整整九页全是公式，**就为了用数学定义出一个长得对、看起来不别扭的大写 S**。背景是 Knuth 当时已经为 TeX 设计了 METAFONT 字体定义语言，整个 Computer Modern 字体家族里其他 25 个大写字母都搞定了，只有 S 让他写了一整篇论文——这件事本身就足够说明问题。

**为什么是 S？** 26 个大写字母里，几乎所有都可以由"一个主椭圆"或者"直线加椭圆"构成，结构上有锚点可以参考；只有 S **没有任何稳定的轴或基本图形**——它从头到尾是一段连续变化曲率的曲线，没有直线、没有圆心、没有对称结构来约束它。更要命的是，**视觉上对称的 S 其实"看起来"不对**——人眼习惯上半圈比下半圈略小、留白略多，否则会显得头重脚轻。所以 Knuth 不仅要数学定义这条曲线，还要在数学里塞进"看起来好看"这个主观约束。

论文里 Knuth 还借机解释了为什么他要写 TeX 和 METAFONT——他想让《计算机程序设计艺术》（TAOCP）第二版印刷出来和第一版排版一致，但出版商告诉他他们已经不用 Linotype 热铅排版机了。Knuth 一气之下决定**亲自重新发明排版**，把 TAOCP 放一边搞了十几年的字体研究。这篇 S 的论文，就是这场"yak shaving"的副产品之一。当年他给妻子看初步的字母设计，他妻子说了一句被引为典故的话：**"为什么不把它们做成 S 形？"** ——意思是他第一版那个字母完全不像 S。

## HN 评论精华

- **bombcar**（高赞短评）："我刚花了 30 分钟读了一篇 detailed mathematical 版本的'画一个 S，再画一个更不一样的 S'。"——把强博的《Trogdor》梗用在 Knuth 上，神来一笔。

- **jll29**：九页全是公式只为一个字母——这就是 Donald Ervin Knuth 的 rigor / 完美主义 / OCD 招牌。**感谢他让我们的版面更美**，每次在 Overleaf 按"compile"我都心存感激，也督促自己写的内容值得这样的排版。

- **commandlinefan**：我怕**我们再也不会有第二个 Knuth 了**——就算有，今天的世界也没地方让他待。

- **WillAdams** + **dhosek** + **zvr**（typography 老炮聊起 METAFONT 的内幕）：**WillAdams** 引 Knuth 妻子的"为什么不弄成 S 形"那段。**zvr** 补正：METAFONT 不是第一个 outline-based 字体（Karow 的 Ikarus 更早），但 METAFONT 的真创新在于"**pen** 抽象"——你描述一条路径，软件用一支或多支虚拟笔来追踪它，模拟真实的书写过程；这正是 Hermann Zapf 不喜欢 METAFONT 的原因——他习惯传统方式逐曲线设计字母，METAFONT 的"过程化"路线不合他的工作方式。

- **bananaflag**：S 比其他字母难在哪？——**antonvs** 给了相对完整的解释：26 个大写字母里，S 是**唯一不能用一个主椭圆或直线 + 椭圆组合**来定义的字母；它整条曲线 governed by continuously changing curvature，没有稳定轴；而且**naively 对称的 S 反而不好看**，需要额外人为调整。整套数学定义因此变得很难写。

- **noufalibrahim**：手写书法里 S 也是 notoriously 难写的字母——小错误连外行都看得出，但**一旦写对就极美**。在 cancellaresca corsiva（意大利体）里尤其明显，因为字母不是刚性直线、是流动曲线。同一个字符既是设计师的噩梦也是炫技场。

- **drfuchs**（亲历者）：1980 年是他本人在 Stanford 操作 DEK 为此事专门买的 Alphatype CRS 照排机，**亲手输出 TAOCP 第二卷第二版的相机原稿**。Knuth 重写了 8080 汇编的固件，drfuchs 写了从 DVI 到照排机的驱动。这段被埋在评论里的回忆很多人没读到。

- **fraserphysics**：这篇 S 论文当年发在《The Mathematical Intelligencer》同期里，**下一篇就是 David Ruelle 的"Strange Attractors"**——80 年代的杂志含金量。

- **golem14**：突然觉得自己的拖延症没那么严重了——Knuth 为了印 TAOCP 第二版排版漂亮一点，拐去搞了十几年字体技术。**andyferris** 接梗：人类历史上多少伟大发明都是 yak shaving 的副产品。
