---
layout: article
title: "Ask HN：你最近在做什么项目？"
issue: 790
number: 11
category: ask_hn
original_url: "https://news.ycombinator.com/item?id=47741527"
hn_url: "https://news.ycombinator.com/item?id=47741527"
date: 2026-04-17
---

## 文章摘要

这是 2026 年 4 月 Hacker News 上经典的「Ask HN: What Are You Working On?」系列讨论帖，开发者们在这里分享自己正在进行的副业项目、创业尝试或纯粹为了好玩而做的技术实验。由于是 Ask HN 帖子没有独立正文，本文主要基于评论区内容撰写。

这一期的项目列表非常多样，涵盖了 AI 应用、硬件结合软件、游戏开发、工具类 SaaS、公共交通规划以及公益项目等多个领域。值得关注的项目包括：**Financial Censorship Monitor**（kokkis）——一个记录金融系统被用来封禁记者和活动人士账户的网站，使用 Claude API 结合人工研究，创建者还借机讨论了比特币作为对抗金融管控工具的价值；**Still Kicking**（cmos）——基于 mmWave 毫米波雷达和 ESP32 的老人看护系统，不使用摄像头即可检测跌倒和睡眠质量，已经获得多家居家养老服务商的兴趣；**Tela**（paulmooreparks）——通过加密 WireGuard 连接为 TCP 服务做隧道的自托管中继，完全运行在用户态，无需 root 或 TUN 适配器，跨 Windows/Linux/macOS 打包为单一 Go 二进制；**Tiled Words**（paulhebert）——一款手工设计谜题的每日单词游戏，获得 Playlin 年度玩家选择奖，最近上线了跨设备同步进度的账户功能；**PlantLab**（ctbellmar）——针对大麻种植的 AI 植物健康诊断 API，使用 Vision Transformer 模型在 110 万张图片上训练，18ms 推理时间，可直接对接家庭自动化系统；**Rail Raptor**（marcusdev）——完全离线、客户端运行的英国火车行程规划器，采用 Raptor 算法，整个时刻表仅 6MB；以及 **Spacedeck X**（stanko）——一款结合弹幕射击与卡牌构筑的 JS 游戏，使用 Kaplay 框架开发，正在考虑登陆 Steam。整体氛围延续了 HN 一贯的独立开发精神——大量从自身需求出发、注重技术细节、愿意分享完整栈的小型项目。

## HN 评论精华

1. **金融审查监控项目**：kokkis 分享的 Financial Censorship Monitor 引发了关于加密货币与言论自由关系的讨论。评论者普遍认同金融基础设施正在成为言论控制工具，但也有人指出比特币的实际可用性和普惠性仍然有限，更适合作为极端情况下的备用方案而非日常替代。

2. **毫米波养老硬件**：cmos 的 Still Kicking 项目获得高度关注，评论者对不使用摄像头的隐私友好设计、以及 mmWave 在穿墙/穿窗限制下的实际精度展开讨论。多位评论者表示家中有老人，愿意成为付费客户，并建议增加服药提醒和主动视频呼叫功能。

3. **AI 辅助独立开发成为主流**：多个项目提到使用 Claude API、Codex 或本地 LLM 加速开发。一位评论者总结道：2026 年的 HN "What are you working on" 帖子里，几乎一半项目要么由 AI 辅助构建，要么本身就是 AI 产品，这与 2023 年的氛围已经有本质区别。

4. **离线优先工具重新流行**：paulhebert 的 Rail Raptor 和其他几个客户端优先的工具获得好评。评论者感慨，随着 WASM、SQLite、以及浏览器存储能力的成熟，过去必须依赖云端的复杂应用现在可以完全跑在客户端，既省服务器成本也保护了用户隐私。

5. **从副业到商业化的现实困境**：多位评论者在回复中讨论了从"好玩项目"转向"收费产品"的难度。cmos 坦言虽然有养老机构表达兴趣，但硬件 SKU 管理、售后支持和认证合规是独立开发者难以独自承担的重负，考虑寻找硬件合伙人。
