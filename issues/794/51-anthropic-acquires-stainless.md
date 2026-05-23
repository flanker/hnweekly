---
layout: article
title: "Anthropic 收购 Stainless：把 SDK 工厂搬进自家屋"
issue: 794
number: 51
category: startup_news
original_url: "https://www.anthropic.com/news/anthropic-acquires-stainless"
hn_url: "https://news.ycombinator.com/item?id=48182281"
date: 2026-05-22
---

## 文章摘要

Anthropic 宣布**收购成立于 2022 年的 Stainless**——这家公司是这两年 LLM 厂商背后最低调但最关键的工具供应商之一，干的事情是"把 OpenAPI 规范自动生成多语言（TypeScript / Python / Go / Java / Ruby / Kotlin）原生 SDK、CLI 工具，以及最近兴起的 MCP server"。Anthropic 官方所有 Claude API SDK 从最早期开始就出自 Stainless 之手；OpenAI、Cloudflare、Lithic 等大量 API 公司也都是它的客户。

Anthropic 给出的官方理由是**"agents 的能力上限是它能连接什么"**——Claude 想成为靠谱的代理，必须能轻松接入越来越多的第三方 API，而 SDK / CLI / MCP server 是这条接入链的物理实现。Stainless 在做的工作恰好把 OpenAPI → MCP server 这一步打通：任何有 OpenAPI spec 的服务，理论上一键就能变成 Claude 可调用的工具。这次收购把 Anthropic 从"模型公司"向"代理基础设施公司"再推一步。

**最关注的争议点**是：Stainless 同时服务 Anthropic 的所有竞争对手。被 Anthropic 收编后，OpenAI、Google 的 SDK 是不是仍然能拿到同样的服务、同样的优先级、同样的工具升级？官方公告里没明确说，但社区普遍预判 Stainless 不会一刀切，但**对手厂商的 SDK 体验和路线会逐渐脱钩**——这是过去几次类似收购（GitHub 之于 Microsoft、Mailchimp 之于 Intuit）的标准剧本。

## HN 评论精华

- **emmanuelsemugga**：理性视角——"Stainless 做的是无聊但极重要的工作。把 API 规范变成干净的 SDK 和工具集，是 Claude 代理在真实产品里能不能跑起来的决定因素。如果 Anthropic 真打算把 agents 做大，**把这个团队收进来完全合逻辑**。"

- **firemelt**：业务模式问句——"所以是说我有像 Twilio 这样的服务，写好 OpenAPI 规范交给你们，你们就帮我生成 SDK 给我的客户？" 这是大量从业者读完公告的第一反应，**Stainless 的核心价值在工程师圈外几乎不被看见**。

- **NagatoYuzuru**：一句话写出了开源 / 中立工具消费者的失望——"今天本来心情很好，因为找到了完美契合需求的工具，结果发现它刚被收购完事了。Anthropic 漂亮。" 这条评论被大量同情。

- **rikima_**：警觉派——"惊讶要滑这么远才能看到有人指出这点。**这类收购是有警示意义的**。"指的是 LLM 厂商正在系统性地把"和 LLM 接入相关的中间层"（SDK 工具、Memory 服务、MCP 服务、IDE 插件）一家家收编，未来开发者面临的将是几座割裂的高墙，而不是一个开放生态。

- **garad**：道德派——"作为客户，看到你们向那些会伤害用户的商业实践妥协是很糟的。如果是 aquihire，能不能把产品开源让我们继续用？但看起来收购目的是**砍掉 Anthropic 竞争对手的 SDK 体验**，你们卖掉了灵魂。"

- **firef1ie**：来自 Stainless 早期听众的祝贺——他几个月前听过 Stainless CEO 在 Dan Shipper 节目上的访谈，当时就觉得"这家公司位置太好了"，处在 AI 与所有平台之间的中转节点，被收购是必然。

- **shaneos**：回应一个质疑创始团队资历的人——"Sam 在 Stainless 工作的时间比除创始人外任何人都长，他当然有资格谈公司气质。'不要听专家说，他们都太懂了所以有偏见'这种风气正在毒化美国的公共讨论。"

- **关于"和 MCP 协议绑定"的讨论**：评论者点出 MCP 协议是 Anthropic 主推的、用来让代理调用工具的标准。**Stainless 是把 OpenAPI 一键转 MCP server 的最快路径**，所以这次收购本质是 Anthropic 给自己的 MCP 标准买下了"最重要的 onboarding 工具"，巩固协议地位。

- **对开发者的实际影响**：评论共识是短期内 Anthropic SDK 体验会更顺、文档更同步；中期看，**非 Anthropic API 客户在 Stainless 的优先级会被悄悄降低**，类似 Microsoft 收购 GitHub 后 GitLab 等竞品在工具链里被慢慢边缘化。
