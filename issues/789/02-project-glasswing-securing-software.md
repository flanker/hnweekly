---
layout: article
title: "Glasswing 项目：为 AI 时代保护关键软件安全"
issue: 789
number: 2
category: favorites
original_url: "https://www.anthropic.com/glasswing"
hn_url: "https://news.ycombinator.com/item?id=47679121"
date: 2026-04-10
---

## 文章摘要

Anthropic 宣布了 Project Glasswing（玻璃翼项目），这是一项将 Amazon Web Services、Apple、Broadcom、Cisco、CrowdStrike、Google、JPMorganChase、Linux 基金会、Microsoft、NVIDIA 和 Palo Alto Networks 等科技巨头联合起来的网络安全倡议，旨在保护全球最关键的软件基础设施。

该项目的核心驱动力是 Anthropic 训练的一个名为 Claude Mythos Preview 的未发布前沿模型。这个通用型前沿模型揭示了一个严峻的事实：AI 模型的编码能力已经达到了能够在发现和利用软件漏洞方面超越绝大多数人类的水平。Mythos Preview 已经在每个主要操作系统和每个主要网页浏览器中发现了数千个高危零日漏洞。

文章列举了三个典型案例：第一，Mythos Preview 在以安全著称的 OpenBSD 操作系统中发现了一个存在 27 年之久的漏洞，攻击者仅通过连接就能远程崩溃任何运行该系统的机器；第二，在被无数软件用于编解码视频的 FFmpeg 中发现了一个 16 年前的漏洞，该漏洞所在的代码行被自动化测试工具命中了五百万次却从未被发现；第三，该模型自主地在 Linux 内核中发现并串联了多个漏洞，实现了从普通用户权限到完全控制机器的提权攻击。

在 CyberGym 评测基准上，Mythos Preview 以 83.1% 的得分显著超过了 Claude Opus 4.6 的 66.6%。Anthropic 承诺为这些安全工作提供高达 1 亿美元的 Mythos Preview 使用额度，并向开源安全组织直接捐赠 400 万美元。此外，超过 40 个构建或维护关键软件基础设施的组织已获得访问权限。

各合作伙伴的反馈十分积极。Cisco 的首席安全与信任官 Anthony Grieco 表示 AI 能力已经跨越了一个门槛，从根本上改变了保护关键基础设施免受网络威胁所需的紧迫性。AWS 的副总裁兼 CISO Amy Herzog 表示他们每天分析超过 400 万亿次网络流量以检测威胁。Microsoft 网络安全和研究执行副总裁 Igor Tsyganskiy 表示在 CTI-REALM 开源安全基准测试中，Mythos Preview 相比之前的模型显示出显著改进。Linux 基金会 CEO Jim Zemlin 强调该项目为开源维护者提供了大规模主动识别和修复漏洞的能力。

该项目还涉及 Anthropic 的负责任扩展政策（RSP），包括不向公众发布 Mythos 模型以及发布单独的自主破坏风险报告。Anthropic 强调这是一个起点，没有任何单一组织能独自解决这些网络安全问题。

## HN 评论精华

1. **对过度炒作的质疑**：最高赞评论表示对每次新模型迭代都被宣称将带来范式转变感到疲惫，认为过度炒作实际上不利于真正的理性采用。这反映了社区中对 AI 行业营销话术的普遍不满。

2. **对网络安全格局的深度分析**：一位评论者指出，即使这只有一半是真的，也代表了漏洞搜索领域的巨大进步。如果 Apple 和 Google 将其应用于移动操作系统代码库，可能会消灭商业间谍软件行业（如 NSO Group），迫使它们更多地依赖社会工程而非系统漏洞。这也可能重塑军事信号情报的格局。

3. **Claude Mythos 系统卡片的深入解读**：一位评论者详细分析了 Mythos 的系统卡片（PDF），指出 Anthropic 首次在部署早期版本供内部使用之前安排了 24 小时的内部对齐审查。他们还注意到 Anthropic 认为 Mythos 作为自主破坏者的风险高于以往模型，为此专门发布了针对该特定威胁模型的风险报告。

4. **软件安全的未来分化**：一位评论者提出了一个发人深省的问题：软件安全是否会收敛到一个漏洞更少还是更多的世界？他认为 AI 前时代的软件质量分布将被大幅放大——大型科技和基础设施公司能够通过抢先投入 token 开支来捕获漏洞，而其余市场则面临"花大钱或被黑"的两难困境。

5. **对访问权限"护城河"的担忧**：一位评论者质疑是否正在形成一个"软件联盟"，拥有特殊权限。他认为 AI 的真正护城河已经被找到——关键问题是你在城堡围墙的里面还是外面。
