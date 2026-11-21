---
layout: article
title: "Terminactory：一边等编码代理，一边经营终端工厂"
issue: 819
number: 58
category: fun
original_url: "https://gitlab.com/snipedbywifi/terminactory"
hn_url: "https://news.ycombinator.com/item?id=50497132"
date: 2026-11-19
---

## 文章摘要

Terminactory 是用 Rust 与 ratatui 制作的终端工厂游戏，作者把它定位为等待编码代理长任务时的轻松消遣。根据[仓库 README](https://gitlab.com/snipedbywifi/terminactory/-/blob/main/README.md)，玩家采矿、加工、出售产品，再通过研究扩展到更复杂的工业链；没有敌人突袭或随机设备故障，离开后工厂仍能获得收益。它还提供游戏内的虚构浏览器，把百科、生产统计、研究树、论坛和背景故事放在同一个世界里，玩家可以只经营工厂，也可以探索叙事。

与普通终端游戏相比，项目的特别之处是接收代理消息：作者提供 Claude Code 集成，让任务进度或最终回复以游戏里的“电报”出现，也描述了其他代理可采用的消息接口。这里介绍的是设计目标，本期未配置任何代理钩子、运行代码或检查消息传递的安全性。作者坦承实现大量借助 AI，创意和取舍由自己决定；评论后又表示重写了 README，并计划逐步重写背景故事，因此不能把“准备人工重写”描述成全部已经完成。

原项目页只提取到仓库简介，本期补读了公开原始 README。当前文档列出紧凑布局、自定义显示和存档机制，但“终端原生”并不自动保证低资源消耗或所有终端上的可用性。HN 中对界面拥挤的反馈与作者随后增加预设的回应，反而展示了从可运行原型到适合日常使用仍需迭代的地方。

## HN 评论精华

- [Joker_vD](https://news.ycombinator.com/item?id=50497787) 把游戏内电报联想到 NetHack 的邮件信使，指出把外部通信融入游戏并不是全新概念，新场景赋予了老机制新的用途。

- [gigel82](https://news.ycombinator.com/item?id=50502407) 认为终端界面过于拥挤；作者 [snipedbywifi](https://news.ycombinator.com/item?id=50504359) 随后表示新增了更简洁的 calm 预设。这里保留了反馈与回应，而非声称已经独立复测。

- [nickpsecurity](https://news.ycombinator.com/item?id=50499613) 提出如果性能和资源开销合适，工厂模拟或可成为规划算法与代理架构的实验环境。这是研究用途建议，项目并未因此成为经过验证的评测平台。
