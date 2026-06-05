---
layout: article
title: "OpenRouter 完成 1.13 亿美元 B 轮融资"
issue: 796
number: 47
category: startup_news
original_url: "https://openrouter.ai/announcements/series-b"
hn_url: "https://news.ycombinator.com/item?id=48338660"
date: 2026-06-05
---

## 文章摘要

OpenRouter 宣布完成 1.13 亿美元的 B 轮融资。本轮由 Alphabet 旗下的成长基金 CapitalG 领投，跟投方阵容豪华，包括 NVIDIA 旗下的 NVentures、ServiceNow Ventures、MongoDB Ventures、Snowflake Ventures、Databricks Ventures、AMP PBC、Pace Capital，以及现有投资方 Andreessen Horowitz（a16z）和 Menlo Ventures。

OpenRouter 的定位是一个位于"AI 智能体（agent）与模型提供商之间"的路由与网关层，统一处理生产级 AI 应用所需的路由、可靠性、成本优化与合规等问题。开发者通过它的单一 API，即可访问来自众多提供商的 400 多个模型，无需逐一对接每家厂商各不相同的接口。

公司公布的增长数据相当亮眼：每周处理的 token 量在六个月内从 5 万亿暴涨到 25 万亿，按此势头年化处理量有望突破一千万亿（quadrillion）token；平台服务的开发者已超过 800 万。新融资将用于扩展底层基础设施、强化企业级能力、并投资更智能的路由系统，帮助团队"为每一个请求找到最合适的模型和提供商"。此外，公司还计划支持多模态推理（图像、音频、语音、视频、向量嵌入），并提供工作区（workspace）和零数据保留（zero-data-retention）等企业级管控功能。

## HN 评论精华

**simonw**（Simon Willison）坦言自己花了很久才接受 OpenRouter 的价值：起初他不理解为什么要在自己和 LLM 之间架一个代理，但实际用下来发现它确实提供了显著价值——一是迄今为止门槛最低的"试遍所有模型"的方式；二是它提供消费上限（billing caps），多数模型厂商至今不提供这个功能，但如果你要把任何东西部署到公网，硬性上限非常有用，能避免被人滥用导致一夜之间烧掉 100 万美元；三是它的模型排行榜虽有缺陷，却是判断哪些模型受欢迎的有趣信号。

**minimaxir** 作为重度用户认为它是尝试新模型最好的方式，但不理解为何有人会用它去跑 Claude Opus 这类昂贵模型做完整的智能体后端——在那种成本量级下，5% 的加价相当可观，不如直接走源头厂商的 API。不过他也承认大量用户确实在这么做，对 OpenRouter 而言这是纯利润。**nadermx** 一句话点破："便利是要加价的"（Convenience has a markup）。

关于"为什么需要一个公司来当代理，而不是用开源库"的质疑，**542458** 和 **Oli_dev** 给出了关键答案：开源库解决不了支付/计费问题，而能够购买信用额度、且额度不被锁定在单一厂商，正是 OpenRouter 的核心价值，为此付 5% 是值得的。**SilverElfin** 则提出了更宏观的观点：OpenRouter 最大的意义在于它制造了模型之间的竞争——越多人使用开源权重模型或非头部厂商的模型，未来想"封杀"这些模型就越难。

讨论中还顺带吐槽了一番 Cursor 的模型支持：**827a** 指出 Cursor 对非"四大实验室"（OpenAI、Anthropic、Google、xAI）模型的支持极弱，OpenCode 在广度上要好得多。**fontain** 追问为何选 OpenRouter 而非 Cloudflare 的免费 AI Gateway 或 Vercel 的付费方案，simonw 坦承自己之前都没注意到这两家也在提供同类产品，可见这一赛道竞争正在升温。
