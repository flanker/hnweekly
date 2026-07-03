---
layout: article
title: "Claude Sonnet 5 发布"
issue: 800
number: 26
category: data
original_url: "https://www.anthropic.com/news/claude-sonnet-5"
hn_url: "https://news.ycombinator.com/item?id=48736605"
date: 2026-07-03
---

## 文章摘要

Anthropic 于 2026 年 6 月 30 日发布了 Claude Sonnet 5，定位为 Sonnet 系列中能力最强的一代。官方强调该模型主打「智能体（agentic）」能力——自主规划、工具调用、多步骤任务执行，把过去需要更大、更贵模型才能完成的工作下放到 Sonnet 这一价位段。相比前代 Sonnet 4.6，新模型在推理、工具使用、编程和知识类工作上都有提升，并宣称在保持较低价格的同时「缩小了与 Opus 4.8 的差距」。

在基准测试方面，Sonnet 5 在智能体搜索（BrowseComp）和计算机操作（OSWorld-Verified）等任务上有明显改进，并提供了比 Sonnet 4.6 更宽的「成本-性能」调节空间——在某些「努力程度（effort level）」设置下甚至能逼近 Opus 4.8 的表现。早期测试者反馈称它擅长完成此前 Sonnet 版本会卡壳的复杂多步骤工作流，为软件工程类智能体提供了「强有力的执行层」。

定价上，发布期优惠价（截至 2026 年 8 月 31 日）为每百万输入 token 2 美元、每百万输出 token 10 美元；9 月 1 日起恢复标准价 3 美元 / 15 美元。模型已在 Free、Pro、Max、Team 与 Enterprise 各档位开放。安全方面，Anthropic 称其不良行为发生率低于 Sonnet 4.6，网络安全（攻击）相关能力则显著弱于 Opus 系列，并默认开启网络安全防护。

## HN 评论精华

- **andai**：贡献了讨论中最具技术洞察的观点。他注意到在 Agentic Search、Agentic Computer Use 等多张图表上，Opus 4.8 在帕累托前沿上反超 Sonnet 5——也就是说做某些难任务时，Opus 4.8 反而更便宜且效果更好。他总结出一条经验：靠拉高测试时计算量（最大推理、更多轮次、模型融合）去「模拟」大模型，既达不到同等质量，又常常比直接用大模型更贵。「如果你在做难的事，就直接上更大的模型。」

- **wolttam**、**conradkay**：持负面态度。wolttam 惊讶于 Anthropic 竟推出了一个「性能不如开源前沿、价格却更高」的模型。conradkay 引用系统卡指出其在 CyberGym 漏洞发现上还不如 Sonnet 4.6，且认为在性价比上还输给仅 744B 参数的 GLM 5.2。

- **phillipcarter**、**mchusma**：更务实。前者表示自己早已用 Sonnet 替代 Opus 处理绝大多数编程任务，稍加拆解任务就能以更低成本获得接近的产出。后者认为 2 美元 / 10 美元的发布价才让它有吸引力，且 low/medium/high/xhigh 各档差距拉得更开，方便应用调优；但直言全价下就不划算了。

- **alvis**：吐槽各模型「努力程度」的含义不断漂移——「今天 Sonnet 5 的 medium 大约相当于 Sonnet 4.6 的 low」。

- **mag7269**、**tokengod**、**mesmertech**：反映社区期待——有人催更新版 Haiku（4.5 已近一年），有人喊话要 Fable，也有人把这次「平淡」的发布解读为下一代 Opus 临近的信号。
