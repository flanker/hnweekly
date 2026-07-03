---
layout: article
title: "在 Claude、Codex 和 Cursor 里直接做智能模型路由"
issue: 800
number: 24
category: code
original_url: "https://github.com/workweave/router"
hn_url: "https://news.ycombinator.com/item?id=48688700"
date: 2026-07-03
---

## 文章摘要

Workweave Router 是一个"即插即用"的代理层，能根据每个请求的特征智能地把 API 请求路由到最合适的 AI 模型。它作为编程工具与多家 AI 提供商之间的中间层运行，在 50 毫秒内为每个查询选出表现最佳的模型。核心卖点是"只需改一个 endpoint，就能把成本削减 40%–70%"。

在技术实现上，它使用一种基于 embedder 的聚类打分算法（Avengers-Pro）来为每个请求挑选模型，而不是依赖手工配置。它支持多家提供商——Anthropic、OpenAI、Google Gemini，以及通过 OpenRouter 接入的开源模型；并原生兼容 Anthropic Messages API、OpenAI Chat Completions 和 Gemini 三套接口，支持流式输出、工具调用和视觉能力。安全方面，提供商密钥加密保存在本地设备上，客户端用独立的 router token 认证；可观测性方面则内置 OTLP tracing，可对接 Datadog、Grafana 等平台。

集成层面，它能通过简单的配置改动接入 Claude Code、Codex、opencode 和 Cursor，并提供开关命令在不丢失设置的情况下切换路由的开启/关闭。部署上既可以用 npm 安装器走托管部署，也可以用 Postgres 在 8080 端口自托管整套系统。

## HN 评论精华

讨论几乎变成了作者 **adchurch**（Workweave 团队）与网友之间的一场答疑，焦点高度集中在"这真的能省钱吗"。

- 多位评论者（**stpedgwdgfhgdd**、**_pdp_**、**ai_slop_hater**、**k9294**）都提出了同一个尖锐质疑：在编程 agent 场景里，缓存（prompt cache）至关重要，中途切换模型会导致缓存失效（cache miss），反而更贵。作者反复强调这正是他们的关键设计——路由器是**缓存感知（cache-aware）**的：一旦开始用某个模型，切换到另一个模型的阈值就会提高，只有当省下的钱或质量提升足以抵消缓存失效的代价时才会切换。他称这是其他"无状态"路由器普遍忽视的问题，并称自己实测省下了约 40%。
- 关于"省钱原理"，作者解释：有些 agent 会话可以整段交给小模型处理；尤其是 subagent 用的是全新的上下文窗口，即便主 agent 需要前沿模型，一个任务简单的 subagent 也能被路由到小模型。他们在真实 agent 会话上训练路由模型，如果发现小模型搞不定某类任务就不再分配，并设有"救援"护栏——让大模型来接管失败的小模型。
- **arendtio** 问这和 Cursor 的 "auto" 模式有何区别，作者爆料 Cursor 的 auto 其实就是它自家的 Composer 模型，而他们是真正在多个模型间路由。**alansaber** 也附和说这正是他一直回避 Cursor auto 模式的原因。
- **emilio_srg2**、**gautam_io** 关心计费问题，作者回应：如果你有 Claude/Codex 订阅，它会优先用订阅（并按补贴价来做路由决策），只有订阅用尽或没订阅时才回退到 API 计费——很多人正是靠它让 Claude 订阅额度更耐用。
- 也有质疑声：**g00k** 说自己的提示方式会随所用模型而变，不相信路由器能靠"用词"判断该用哪个模型；**slopinthebag** 则对项目页里"我们几乎所有代码都用 AI 写"的表述反感，认为这作为营销很失败，作者坦然接受了这一批评。
