---
layout: article
title: "GoModel：用 Go 写的开源 AI 网关"
issue: 791
number: 23
category: code
original_url: "https://github.com/ENTERPILOT/GOModel/"
hn_url: "https://news.ycombinator.com/item?id=47849097"
date: 2026-04-27
---

## 文章摘要

GoModel 是一个用 Go 编写的轻量级开源 AI 网关（AI gateway），通过统一的 OpenAI 兼容 API，为团队提供面向多家 LLM 提供商的代理、计费、缓存、可观测性能力。其定位类似 LiteLLM、Bifrost、TokenProxy 等同类项目，但选择 Go 作为实现语言以追求更小的运行时开销和更可控的供应链。

支持的提供商覆盖了当前主流：OpenAI、Anthropic、Google Gemini、DeepSeek、Groq、OpenRouter、xAI（Grok）、Azure OpenAI、Ollama、vLLM，以及 Oracle 和 Z.ai。核心特性包括：

- **统一的 OpenAI 兼容 API**：基于 Postel 法则（"对发送严格、对接收宽容"）实现，应用只需对接一次。
- **可观测性 Dashboard**：内置 Token 用量、成本、API 调用模式分析。
- **两层响应缓存**：支持精确匹配和语义相似匹配，能显著降本。
- **鉴权**：通过 `GOMODEL_MASTER_KEY` 做 API key 认证，文档明确提示生产部署必须开启。
- **流式输出支持**：原生支持 chat completions 等流式接口。
- **文件、嵌入与批处理**：跟随上游能力支持文件上传、embedding、batch。
- **User-Path 概念**：通过 HTTP header 或 API key 关联到一个用户路径（如 `/team2/john`、`/agents/agent1`、`/client1/app1`），用于细粒度的用量追踪。

部署方式有 Docker、Docker Compose 或基于 Go 1.26.2+ 从源码构建，附带 Prometheus 指标和审计日志。项目维护着活跃的 Discord 社区与公开 roadmap，0.2.0 版本计划加入智能路由（intelligent routing）和预算管理（budget management）。

## HN 评论精华

- **richardbertok** 自报家门，介绍他用 Rust 做的同类项目 TokenProxy：选择 Rust 是为了把运行时开销压到最低，用 Postgres advisory lock 做配额管理避免去 Redis 多走一跳，用二进制 COPY 做高吞吐日志批量写入。他的发言侧面说明：AI gateway 这个赛道目前非常拥挤，每个人都在用不同的语言和数据库做相似的事情。
- **nzoschke** 列了一份"Go 派 AI 工具清单"：boldsoftware/shelley（完整的 Go agent + LLM gateway）、maragudk/gai（Go 接口包装 Anthropic / OpenAI / Google）。GAI 作者 **markusw** 现身评论，分享自己的设计理念："我希望让网关不再必要——成本与 token 追踪通过 OpenTelemetry 完成，回退与重试通过新的 robust 包处理。"这条互动揭示了一个有趣的二元对立：你到底是要"集中网关"还是"通过库分发"？
- **crawdog** 介绍了自家 sbproxy.dev，并给出选择 Go 的另一个理由："Go 的编译时供应链可控，相比 LiteLLM 这类 Python 项目，运行时供应链攻击面要小得多。"**ijk** 顺势提到 **LiteLLM Supply Chain Incident**——这场近期的事故让"供应链安全"成为评估这类网关时的主要维度。
- **mosselman** 提出"统一 API 的真伪"质疑：他玩过几款类似工具，发现真正落地时还是会被强行带回到 provider-specific 的细节——温度参数、reasoning effort、tool choice mode 等。这些差异是 GoModel 这类项目反复被打补丁的根源。他还问到一个敏感问题："会不会像 LiteLLM 那样开源 rug-pull？"作者 **santiago-pl** 给出了 OpenAI 兼容 API 与许可证条款的两条文档链接作为回应。
- **simonw**（Datasette / llm 工具作者）以维护多 provider 抽象层两年的经验给出深度判断：目前最接近"统一标准"的是 OpenAI Harmony / Responses，但采用率不高；老的 Chat Completions 是事实标准，但缺正式 spec，每个 provider 实现都有讨厌的差异。"2025 年特别动荡，因为大家都在以微妙不同的方式给 API 加 reasoning 机制。"他的判断是抽象层还要长期存在。
- **jedisct1** 的总结一针见血：实现基础 OpenAI / Anthropic 协议很简单，看起来"做完了"。但真正的工作才刚开始——"看看 LiteLLM 的 commit 历史和 issue 区，他们仍在与 Gemini 的多轮工具调用搏斗，Kimi 需要硬编码规则（K2.6 因不在列表里目前还不支持）。这是一段没有尽头的、与每个 provider 怪癖搏斗的旅程。"
