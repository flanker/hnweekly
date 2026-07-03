---
layout: article
title: "什么是列线图（Nomogram），它为何值得关注"
issue: 800
number: 43
category: learn
original_url: "https://lefakkomies.github.io/pynomo-doc/introduction/introduction.html"
hn_url: "https://news.ycombinator.com/item?id=48689277"
date: 2026-07-03
---

## 文章摘要

列线图（nomogram，又译诺谟图/算图）是一种把数学公式"画"出来的可视化计算工具。使用者用一把直尺横跨若干带刻度的数轴，在已知值上对齐，即可在另一条轴上读出未知解，对含三个或更多变量的方程尤其好用。这种"用眼睛和直尺算题"的方式，曾在电子计算设备普及之前充当工程师们的快速计算器。

列线图由 Philbert Maurice d'Ocagne 于 1880 年发明，早期应用之一是法国铁路建设中的"挖填方"（cut and fill）计算。此后它在众多领域大显身手：医学诊断与血液生理分析、航空导航与座舱仪表、弹道与军事野战计算、化学工程、统计与质量控制、机加工与电气设计等。除了快速计算，列线图还能通过各刻度之间的关系带来直观洞察——"它们让你快速计算，并通过各刻度间的关系提供极大的洞见"。

文章还介绍了 PyNomo：一个用 Python 自动设计列线图的开源软件。用户无需手工绘图，只要编写脚本描述数学关系，工具即可生成可直接出版的 PDF 列线图。它支持九种标准方程类型外加一种通用类型，让不具备高深数学功底的人也能制作列线图。

## HN 评论精华

- 讨论中冒出大量"该知道的经典列线图"分享。**cscheid** 强烈推荐每个人都应熟记一张贝叶斯定理（Bayes' theorem）的列线图，熟到闭着眼都能用；**forgotpwagain** 提到电气工程师的最爱——史密斯圆图（Smith chart），"你爱它还是恨它，取决于你的电磁学课教得好不好"；**cckolon** 爆料美国海军至今仍在用列线图做核反应堆的化学控制。
- 延伸学习资源丰富。**alnwlsn** 推荐 YouTube 频道 Chris Staecker，专门讲解计算机与计算器出现之前人们用来算数的各种巧妙工具；**analogpixel** 给出手工制作列线图的讲解视频；**QuesnayJr** 分享了一篇关于列线图数学原理的老论文。
- 有人分享亲身实践。**JKCalhoun** 说自己自从偶然接触列线图后便着了迷，今年还为"两个并联电阻"做了一张（临摹自一本老书），并吐槽让 Gemini 写代码生成 SVG 的效果远不如自己在 Affinity Designer 里手绘的版本。**onefiftymike** 则正好点出了本文对应的工具 PyNomo，说其中"贷款还款"的示例是他的最爱。
- 少不了文字梗：**LelouBil** 一开始把标题读成了 "Nonogram"（数织/Picross），**nok22kon** 打趣说 "Numogram"（努谟图）因近来的 AI 热潮而更有意思。
