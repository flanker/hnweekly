---
layout: article
title: "微软与 OpenAI 终止独家协议与营收分成"
issue: 792
number: 48
category: startup_news
original_url: "https://www.bloomberg.com/news/articles/2026-04-27/microsoft-to-stop-sharing-revenue-with-main-ai-partner-openai"
hn_url: "https://news.ycombinator.com/item?id=47921248"
date: 2026-05-01
---

## 文章摘要

2026 年 4 月 27 日，彭博社披露微软（Microsoft）与 OpenAI 之间持续多年的"独家+营收分成"合作框架正式终结。这是自 2019 年微软首次以 10 亿美元入股 OpenAI 以来，双方关系的又一次结构性重塑。新条款的核心变化包括：第一，微软不再向 OpenAI 支付来自 Azure OpenAI 服务的营收分成（此前业内普遍估计该比例约为 20%）；第二，OpenAI 不再被锁定在 Azure 云上，可以将自身产品和模型对外部署到 Google Cloud、AWS、Oracle 等其他云服务商；第三，微软对 OpenAI 模型与知识产权的使用许可也从"独家"转为"非独家"，但有效期延长至 2030/2032 年附近，确保其继续在 Copilot 等产品中广泛集成 OpenAI 的能力；第四，原协议中颇具争议的"AGI 触发条款"（即一旦 OpenAI 董事会宣布达到 AGI，微软部分权利将自动失效）也被移除，改由更可量化的商业条款替代。

从战略层面看，这一变动意味着 OpenAI 不再仅作为 Azure 的"定向供应商"存在，可以像 Anthropic 一样在多云环境中分发模型——这也是 OpenAI 同期宣布登陆 Amazon Bedrock 的前置条件。对微软而言，放弃营收分成换取的是更大的产品自由度：可以更自由地与 Anthropic、Mistral、xAI、DeepSeek 等其他模型厂商深度合作，自主构建多模型策略，同时保留约 30% 的 OpenAI 股权与对其知识产权的访问权。这次重新谈判也被认为与 OpenAI 推进上市进程、以及微软第二天的季度财报密切相关——双方都需要把模糊的"AGI 条款"变成投资者可以理解的合同语言。从行业生态看，独家排他被打破后，OpenAI 的训练与推理基础设施有了重新洗牌的空间，TPU、Trainium、AMD MI 系列等替代算力都可能被重新评估，整个前沿模型的算力地图正在从"单一押注"走向"多供应商均衡"。

## HN 评论精华

- **被高赞认可的解读**：原协议本质是"以营收分成换取独家使用权"，新协议则是"无独家、也无分成"。微软仍持有 OpenAI 约 30% 股权与 IP 使用权，因此即便不再分成，自有产品（Copilot 系列、Microsoft 365）使用 OpenAI 模型仍可享受授权，许可期一直延伸至 2030/2032。
- **Google 是潜在最大赢家**：几乎所有前沿实验室（Anthropic、xAI、Meta 等）都在不同程度地使用 TPU，唯独 OpenAI 因 Azure 独家协议被锁死。如今限制解除，OpenAI 很可能开始混用 Google TPU 与 AWS Trainium，结束对 NVIDIA + Azure 的单一依赖。
- **AMD 仍未接住红利**：多位评论者引用 2024 年底的分析指出 "AMD 软件栈 bug 频发，开箱训练几乎不可行"，CUDA 护城河依然牢固。即便算力供需紧张，前沿实验室也不愿在训练任务上拿真金白银去验证 AMD 的工具链。
- **为什么要公开发布**：有评论指出，OpenAI 正在筹划上市，且微软是上市公司，原本含糊的 AGI 条款必须被改写为投资者可读的合同条款；与其让传闻发酵（"我能不能用 GCP 跑 OpenAI 模型？"），不如一次性公告披露。
- **微软仍是赢家的另一面**：评论者提醒，过去两年微软已与 Mistral、Anthropic、NVIDIA、DeepSeek 等签署过类似合作。这次解除独家也意味着微软可以名正言顺地把 Claude Code、Anthropic 模型搬进 Azure 与 Copilot 体系，继续靠"基础设施 + 企业渠道"收钱——"模型层正在被商品化，但你仍要为微软的云和 Office 付费"。
- **对 Anthropic 的影响**：Anthropic 模型早已同时上架 GCP/AWS（Azure 的部分仅是计费转售）。OpenAI 终于追平了 Anthropic 的多云分发能力，意味着两家的渠道差距被快速缩小，未来云厂商之间的争夺将更多集中在"管理化的 Agent 服务"和企业集成上。
- **Apple 与中国厂商的位置**：有评论认为 Apple 的 GPU 架构无法承载 100B+ 参数级别的训练/推理负载，仍游离于一线 AI 基础设施之外；DeepSeek、Qwen 等中国厂商是否算"frontier"则引发分歧——支持者认为他们公开权重和方法论，反方则认为暂时仍落后一代。
