---
layout: article
title: "在 Raspberry Pi Pico 2 W 上运行微型游戏世界模型"
issue: 816
number: 68
category: fun
original_url: "https://github.com/SheepSeb/tinyworldformer"
hn_url: "https://news.ycombinator.com/item?id=50295450"
date: 2026-10-30
---

## 文章摘要

tinyworldformer 用两个小型神经网络组成游戏循环：世界模型根据当前画面和动作预测下一帧与奖励，Actor 再根据画面选动作。项目把循环 Transformer 的同一组权重重复使用，以节约参数，并结合 int8 量化与 Rust 实现，让部分小游戏在 Pico 2 W 上运行。

这不是一个通用模型同时掌握所有游戏。项目为不同游戏训练独立网络，画面中的 token 还显式携带方向等状态。作者用传统规则实现作参考，对预测帧和奖励逐步检查，并记录 Agent 利用模型错误取得虚假高分的问题。

最值得借鉴的是验证方法：离线准确率高，不保证 Agent 实际游玩时不会进入模型没见过的状态。作者通过重新采集真实游戏数据和调整训练修补漏洞。芯片上运行推理及学习控制策略，也不意味着世界模型本身是在微控制器上从零训练的。

## HN 评论精华

- **训练与部署要分开说。** 作者 sheepseb 回答，世界模型训练使用 RTX 4060，控制 Agent 可以直接在 Pico 上通过 stream-ac 学习。
- **每款游戏有独立模型。** andai 询问是否共用同一网络，svantana 从参数规模提出疑问，sheepseb 明确表示当前是每个游戏一个网络。
- **Agent 会利用“幻觉”。** xpct 担心控制器主动寻找模型漏洞；作者承认出现过打砖块异常得分、远距离射击等现象，并说明曾用输入噪声等方法修正。
- **状态并非神奇推断。** noduerme 追问运动方向，作者解释方向被编码进 token，Doom-lite 也使用小地图。这个澄清比“网络自己理解了一切”的说法更有工程价值。
