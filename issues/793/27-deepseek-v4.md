---
layout: article
title: "DeepSeek V4：几乎踏到前沿"
issue: 793
number: 27
category: data
original_url: "https://simonwillison.net/2026/Apr/24/deepseek-v4/"
hn_url: "https://news.ycombinator.com/item?id=47977026"
date: 2026-05-08
---

## 文章摘要

Simon Willison 评测了 DeepSeek 新发布的两款预览版模型 **V4-Pro** 和 **V4-Flash**——都采用 MoE（专家混合）架构、原生支持 100 万 token 上下文，并以 MIT 协议开源权重。V4-Pro 总参数 1.6T、激活参数 49B，是目前**最大的开放权重模型**，体量超过 Kimi K2.6（1.1T）和 GLM-5.1（754B）；V4-Flash 总参数 284B、激活 13B，定位轻量级。两款模型都强调长上下文下的效率改进：在 1M token 场景下，V4-Flash 的单 token FLOPs 仅为 V3.2 的 10%，KV cache 占用降到 7%。

定价层面 DeepSeek 几乎是把价格屠夫做到底。**V4-Flash** 定价 $0.14/百万输入、$0.28/百万输出，直接打掉 OpenAI 的 GPT-5.4 Nano；**V4-Pro** 定价 $1.74/百万输入、$3.48/百万输出，是当前最便宜的"准前沿大模型"。Simon 在文章中给出的判断很克制：从 DeepSeek 自己公布的 benchmark 看，V4-Pro 距前沿（GPT-5.4、Gemini-3.1-Pro）大约**落后 3-6 个月**，但考虑到价格档差距，这个性价比足够诱人。

Simon 通过 OpenRouter 实测了两款模型，重点跑了他常用的 SVG 生成基准（"画一只骑滑板的鹈鹕"等）——结果可用，Pro 略胜 Flash 一筹，但都没有惊艳到能让 Claude Sonnet 4.5 或 GPT-5.4 紧张的程度。换句话说，DeepSeek 这次的杀手锏不是绝对智能，而是**"开源 + 价格 + 长上下文"的组合**：你可以本地部署，可以拿到完整权重，可以以 1/10 的成本干活，对预算敏感的工程团队来说是非常诱人的选择。

文章还提到一个值得关注的趋势：开放权重模型的体量正在与闭源前沿模型趋同——以前差一个数量级，现在只差一个季度。

## HN 评论精华

1. **wg0**：分享了一个真实的成本对比——他用 V4-Pro 跑完一个体量不小的 TypeScript 代码库分析任务，**总共花了 $0.09**；而同样的任务用 Claude Opus 估算要 $9-13。两个数量级的差距对企业级用户来说不容忽视。

2. **cassianoleal**：用并行 subagent 跑完一次完整重构，**总成本 $0.95**；之前用 Claude Opus 烧光 $10 预算还没跑完，DeepSeek 在 agent 工作流里的性价比"野得吓人"。

3. **cedws**：DeepSeek 的另一个优势是**审查更宽松**——做一些逆向工程相关的任务，GPT 和 Claude 会拒答甚至给账号警告，DeepSeek 直接干活。这对安全研究、漏洞分析、老软件维护这类合法但敏感的领域意义重大。

4. **rurban**：分享了一套多模型分流策略——预算任务用 DeepSeek Flash，中等任务用 Kimi，难题留给 Claude。整个 ARM 编译器开发项目跑下来 **$15-30**，比单押 Claude 省 80% 以上。

5. **yogthos**：与其让模型在巨大代码库里"瞎逛"烧 token，不如**用 tree-sitter 把代码 parse 成 Prolog 图谱**，模型通过 MCP 查询特定关系。这种结构化检索能让 DeepSeek 这类便宜模型也跑出大模型级的代码理解效果。

6. **onlyrealcuzzo**：反驳"LLM 不值"的论调——按时薪计算，工程师 1 小时成本 $50-200，而 LLM 即便重度使用一天也就几美元，**ROI 上完全不构成讨论**。DeepSeek 把这个 ROI 又拉高一个数量级。

7. 评论区普遍认同：DeepSeek V4 的发布让"前沿"和"开源"之间的差距进一步缩小，对闭源厂商的定价权是持续压力——OpenAI 和 Anthropic 接下来会被迫在企业 API 价格上做让步。
