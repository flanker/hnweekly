---
layout: article
title: "OpenAI 模型登陆 Amazon Bedrock：Sam Altman 与 Matt Garman 对谈"
issue: 792
number: 50
category: startup_news
original_url: "https://stratechery.com/2026/an-interview-with-openai-ceo-sam-altman-and-aws-ceo-matt-garman-about-bedrock-managed-agents/"
hn_url: "https://news.ycombinator.com/item?id=47939320"
date: 2026-05-01
---

## 文章摘要

2026 年 4 月 28 日，Stratechery 的 Ben Thompson 同时采访了 OpenAI CEO Sam Altman 与 AWS CEO Matt Garman，宣布双方达成深度合作：OpenAI 前沿模型登陆 Amazon Bedrock，Codex 进入 Bedrock，并推出 **Bedrock Managed Agents（由 OpenAI 提供模型支持）**——三项均处于 limited preview 阶段。这次合作直接受益于同一周微软与 OpenAI 解除独家与营收分成协议：Azure 仍是 OpenAI 的"主要云伙伴"，但 OpenAI 现在可以自由把产品分发到任何云厂商，许可证转为非独家（至 2032 年），AGI 触发条款也被移除。

Bedrock Managed Agents 不是简单的 API 转售，而是把 OpenAI 模型与 AWS 的身份、权限、网络、日志、VPC 数据隔离深度绑在一起：用户得到的是一个有状态的 Agent 运行时，内置记忆、工具调用、企业身份与审计，所有客户数据保留在 AWS 环境中。Altman 强调如今已经无法把 model 与 harness 分开看待——"我不再认为模型与外壳是两件可以分开做的事"——它们要在 post-training、prompt 设计与工具调用接口上协同迭代。AWS 的策略是"做中立基础设施"：不与客户竞争自有前沿模型，靠帮助合作伙伴成功来变现，与 Google Cloud "从芯片到模型到应用全栈一体"的路线形成鲜明对比。Garman 在采访中指出"绝大多数高速增长的初创公司仍在 AWS 上扩张"，这正是 Bedrock 接住 OpenAI 流量的底盘。Altman 同时透露了一个反直觉的需求曲线："愿意不计价格只求多算力的客户，远多于在跟我们砍价的客户"——AI 推理需求仍处于"任何价格都吃得下"的早期阶段。两位 CEO 也承认，企业 Agent 架构的中间层（数据库连接、SaaS 集成、Agent 身份/权限模型）尚未稳定，未来几年的工程方向仍是开放问题。

## HN 评论精华

- **真正的看点是产品形态**：高赞评论梳理出三件事——OpenAI 模型上架 Bedrock、Codex 进 Bedrock、推出 Bedrock Managed Agents。前两者类似常规上架，第三者才是新品类：把 Agent 的状态管理、工具调用、权限与企业身份做成"托管服务"。
- **AWS 内部"特种部队"**：有自称内部人士的评论指出，Bedrock 推理引擎实际上由 EC2 的极少数 Principal Engineer 维护，被允许绕开 AWS 内部的繁琐流程；这种"高层拍板、SWAT 团队两周交付"的模式与普通 L5/L6 工程师的体验形成强烈反差，被批评为 AWS 工程文化中的代理人问题。
- **与 gpt-oss 模型的复用**：评论指出 Bedrock 此前已支持 gpt-oss-20b，如果 OpenAI 商用模型与开源版同源，工程接入难度并不高；但训练侧的自定义 kernel 和优化才是"OpenAI 自家版本"难被复刻的核心。
- **OpenAI 仍缺算力**：有评论引用 CNBC 报道——OpenAI CFO 同期承认未来计算成本是公司最大担忧；这与 Altman "客户不在意价格只要更多算力"的说法形成呼应：需求继续爆炸，供给端 OpenAI 自己也在头疼。
- **AWS 的中立卡位**：评论者认为 Garman 的关键 quote "we view our success is if the partners are successful" 是对 Google Cloud 的精准卡位——后者用 TPU + Gemini + Vertex 全栈打捆销售，前者则用"模型市场+管理化服务"承接所有第三方流量；这种哲学差异在企业市场可能比技术领先更重要。
- **关于 Agent 的中间件层**：评论延续 Thompson 的提问，指出企业 AI 的真正瓶颈在于"Agent 与既有 SaaS、数据库、内部权限"的连接层；MCP/A2A/Bedrock Managed Agents 都是在抢这个位置，但还没有任何一种范式赢家通吃。
- **对 OpenAI "独家"措辞的重新解读**：Altman 在访谈中说 Bedrock Managed Agents 当前是"与 Amazon 独家"，但又强调"时间长着呢"——HN 评论解读这是典型的"先以独家换上线速度，后逐步放开"，与之前 Azure 的剧本一脉相承。
- **企业实际担忧**：来自一线工程师的评论吐槽 Google 在 Gemini 3 灰度发布上反复横跳，相比之下 AWS 把 OpenAI 与企业身份/数据隔离打包卖出去，在合规敏感的传统行业里反而可能跑得更快。
