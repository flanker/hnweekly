---
layout: article
title: "让 Papers with Code 重获新生"
issue: 797
number: 37
category: learn
original_url: "https://paperswithcode.co/"
hn_url: "https://news.ycombinator.com/item?id=48443644"
date: 2026-06-12
---

## 文章摘要

Papers with Code 曾是机器学习社区最受欢迎的资源之一：它把学术论文与对应的开源实现代码配对，并维护各类任务的基准榜单（leaderboard），让研究的可复现性和可比较性有了统一的入口。该项目在被 Facebook/Meta 收购后逐渐被荒废，最终关停，社区一度失去了这个重要的工具。

本项目是对原 Papers with Code 使命的一次复兴尝试。它重新把"论文 + 代码 + 数据集 + 基准"这套结构带回来，帮助研究者快速找到一篇论文是否有可运行的实现、用了哪些数据集、在哪些榜单上排名如何。原始域名如今已指向 Hugging Face，社区普遍认为 Hugging Face 是开源资源更合适的托管方。

核心价值在于：

- 把论文与可复现的代码实现关联起来，降低复现门槛。
- 维护跨子领域的任务基准榜单，方便横向比较模型效果。
- 让数据集与论文、代码之间的引用关系更清晰可查。

## HN 评论精华

**jeffreysmith** —— 在 Facebook/Meta 收购后曾负责运营原版 Papers with Code。他坦言平台的使命后来被搁置，很高兴看到它如今归入 Hugging Face 旗下，认为后者是开源资源更称职的守护者。他强调原项目为 AI/ML 研究带来了结构化和可复现性。

**lalaland1125** —— 提出更尖锐的制度性批评：与其依赖外部平台来验证，不如在学术会议上直接"对没有代码的论文予以拒稿（desk reject）"。

**cyril_st_john** —— 期待复兴版能超越 AI 领域，因为"Papers with Code"这个名字本身暗示更广的潜力，对非 AI 领域论文未被覆盖表示遗憾。

**addandsubtract** —— 补充了收购历史的细节，指出原域名现已指向 Hugging Face，并建议加入"论文摘要的向量嵌入（embeddings）"以增强检索、帮助发现相关论文。

**vjsrinivas** —— 询问该复兴版与 Hugging Face 现有的 Trending Papers 页面之间是什么关系，两者功能似有重叠。

**somethingsome** —— 希望能解析论文中用于对比的数据集和基准，因为在各子领域里很难找到可替代的基准做横向比较。
