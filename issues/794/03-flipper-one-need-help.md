---
layout: article
title: "Flipper One 来了：一台彻底开源的赛博掌机，但他们需要你"
issue: 794
number: 3
category: favorites
original_url: "https://blog.flipper.net/flipper-one-we-need-your-help/"
hn_url: "https://news.ycombinator.com/item?id=48220647"
date: 2026-05-22
---

## 文章摘要

Flipper 团队官宣了 **Flipper One**——一台与广受欢迎的 Flipper Zero 完全独立的新设备。Zero 玩的是**离线点对点协议**（NFC、RFID、IR），而 One 直接跳到**网络协议栈**：Wi-Fi、以太网、5G。形态上是一台基于 Linux 的"赛博掌机（cyberdeck）"。他们的目标定得非常野心勃勃——做出"**世界上最开放、文档最完备的 ARM 计算机**"，要求是 mainline kernel 完整支持、消灭一切专有固件、并且通过 M.2 和 GPIO 模块做成**可扩展的硬件平台**。

之所以发这篇文章，是因为他们公开承认：**这事自己干不完，需要社区帮忙**。技术上需要援手的活儿包括：完善 RK3576 SoC 的 mainline Linux 内核支持、解决最后那个二进制 blob（DDR trainer）、协助电源管理、USB DP Alt-mode、NPU 驱动、硬件视频解码、以及测试 MediaTek MT7921AUN Wi-Fi 芯片组的网络分析能力。非技术方面也很欢迎社区参与：M.2 扩展模块的设计、GPIO 外设、小屏 Linux 的 UI 框架 FlipCTL 架构、桌面环境的选型，乃至卫星通信合作伙伴的选择。

定价上据 TechCrunch 透露目标在 ~350 美元，性能定位约等于 Raspberry Pi 5 这个量级，并不是一台笔记本。所有开放任务、架构讨论都已经放在 **Flipper One Developer Portal**（docs.flipper.net/one），同时他们也在招聘一名 Developer Portal & Community Manager，专门做开发团队和社区贡献者之间的"翻译官"。这种做法在硬件圈里相当稀罕——大部分硬件厂只在产品定型之后才"开源一些组件"作为营销动作，Flipper 是在产品**定型前**把整个开发过程铺到桌面上。

## HN 评论精华

1. **swaits**：戳中重点——这设备最有意思的不是规格，而是"**零 blob、一切开源**"这条原则；文档和代码都能被检视，这才是它和市面上一堆"开放硬件"的本质区别。

2. **zhovner**（项目作者本人现身）：确认 Wi-Fi 芯片 MT7921AUN 有 mainline 开源驱动；整个系统**无需任何二进制 blob 即可启动**。

3. **d3Xt3r**：推销键盘版形态——参考 GPD Pocket 系列的体验，强调对 hashcat 这类工具来说，真正能跑得动的 compute 价值很大。

4. **embedding-shape**：欣赏其形态和连接性——One 服务的人群和 Zero 完全不同，可以接外置键盘而不强行做一体化，反而更灵活。

5. **NeckBeardPrince**（怀疑派）：市场到底在哪？真正的专业渗透测试人员不太会买这种东西，目标用户更像 "**script-kiddies** 和爱好者"。

6. **Karliss**：纠偏定价预期——TechCrunch 提到约 350 美元；性能更接近 Raspberry Pi 5，不要拿它当笔记本比较。

7. **azalemeth**：换个视角看性价比——但凡稍微像样的射频测试设备都是上万美元起步，350 美元能玩 RF 分析其实是"**白菜价**"。

8. **lxgr**：脑洞——如果功率够，本地 LLM 推理也能塞进来，做出全新的语音交互产品形态。

9. **GuB-42**：本质区分——Flipper Zero 在**物理层**搞事情，Flipper One 在**网络层**搞事情，它们是两个完全不同的项目，不要混着看。

10. **整体氛围**：相比 Zero 早期的玩具感，社区对 One 的期待明显更"严肃"——很多人把它当作市场上少有的、**真正彻底开放的 ARM Linux 平台**对待，把它跟 Pine64、System76 这些品牌放在一个比较框架里讨论。
