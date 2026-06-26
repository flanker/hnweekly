---
layout: article
title: "Anthropic 指控阿里巴巴非法窃取 Claude 模型能力"
issue: 799
number: 3
category: favorites
original_url: "https://www.reuters.com/world/china/anthropic-says-alibaba-illicitly-extracted-claude-ai-model-capabilities-2026-06-24/"
hn_url: "https://news.ycombinator.com/item?id=48664814"
date: 2026-06-26
---

## 文章摘要

> 注：路透社原文因访问限制无法直接抓取，本摘要综合 CNBC、Bloomberg 等多家媒体报道及 HN 讨论撰写。

Anthropic 指控阿里巴巴非法窃取其 Claude 模型的能力，并称这是该公司迄今为止已知规模最大的同类攻击。据报道，这场行动发生在 2026 年 4 月 22 日至 6 月 5 日之间，通过近 2.5 万个欺诈账户与 Claude 进行了超过 2880 万次对话交互。Anthropic 称，操作者隶属于阿里巴巴及其人工智能实验室阿里巴巴 Qwen（通义千问）团队，他们使用商业代理服务来绕过禁止中国实体访问该模型的地理限制。

这一指控被称为"蒸馏"（distillation）行为。所谓蒸馏，是指用一个更强模型的输出来训练一个能力较弱的模型。威胁行为者会将这一过程武器化：利用专有 AI 系统的"输入-输出对"来训练自己的"学生"模型，从而以极低成本复现目标系统的性能，这也被称为"模型提取攻击"（model extraction attack）。

这一指控通过一封日期为 2026 年 6 月 10 日、致参议院银行委员会主席 Tim Scott 和资深成员 Elizabeth Warren 的信件提出，最早由 Bloomberg 于 6 月 24 日报道。如果属实，阿里巴巴这场行动的规模将远超此前 Anthropic 在 2 月发现的几起蒸馏行动：当时 Anthropic 称 DeepSeek 的操作涉及超过 15 万次交互，Moonshot AI（月之暗面）超过 340 万次，MiniMax 则超过 1300 万次——而阿里巴巴一家的规模就超过了这三家的总和。Anthropic 还就此事通知了白宫，美国国会正准备据此考虑对中国 AI 实施制裁。

阿里巴巴方面否认了这些指控，表示其不使用专有 AI 模型的输出来训练自己的模型，且其 AI 开发符合适用的知识产权法律。不过在最初被问及时，阿里巴巴一度对指控不予置评。

## HN 评论精华

**tristanj**（787 分）提供了最深入的背景分析：中国的"中间商"会把多个 Claude Max 账户池化，利用支付欺诈手段，再将对话日志转卖给中国 AI 实验室用于模型蒸馏。这种做法以相当于 API 价格 7-9 折的折扣补贴访问成本，可能迫使 DeepSeek 和 GLM 为竞争而降价。

**areoform** 提出了一个理论视角：从长远看，蒸馏尝试是徒劳的。模型本质上是数学函数，只要有足够的输入-输出数据，根据 Stone-Weierstrass 定理就能逆向逼近出近似函数。他打趣道："如果人人都能用上 Mythos，那就没人真正'独占'Mythos 了。"

**gruez** 提出了质疑：他怀疑中间商的定价是否真的比正常注册账户更便宜，因为 Anthropic 会封锁中国银行卡并要求身份验证，使中国用户很难开个人账户。**neves** 则更激进，将整个叙事斥为"美国宣传"，认为这套说法无法自圆其说——为何中间商会亏本运营？

**paxys** 解释了套利逻辑：200 美元的 Claude Max 订阅每月可提供价值约 2800 美元的 token，足以支撑 9 折的盈利空间。**dannyw** 指出，开发者会合法地在 EC2 和云服务器上使用 Claude Code，因此简单地封禁数据中心 IP 并不可行，否则会误伤真实客户。

**bryceneal** 则代表了一种不在意 Anthropic 损失的态度，理由是模型审查和糟糕的用户政策，他宁愿用中国的替代品，尽管那些产品也有自己的限制。
