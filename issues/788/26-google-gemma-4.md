---
layout: article
title: "Google 发布 Gemma 4 开源模型"
issue: 788
number: 26
category: data
original_url: "https://deepmind.google/models/gemma/gemma-4/"
hn_url: "https://news.ycombinator.com/item?id=47616361"
date: 2026-04-03
---

## 文章摘要

Google DeepMind 正式发布了 Gemma 4 系列开源模型，这是 Gemma 系列的最新迭代，延续了 Google 在开源 AI 模型领域的投入。Gemma 4 包含多个规模的模型变体，其中最受关注的是 **Gemma-4 27B**（全参数模型）和 **Gemma-4 26B-A4B**（混合专家/MoE 架构，总参数 26B 但每次推理仅激活 4B 参数）。此外还有更小的 2B 和 4B 版本。

Gemma 4 的核心亮点包括：**多模态能力**——模型原生支持图像和文本输入，可以进行视觉理解、图像描述、视觉问答等任务；**推理能力增强**——集成了思维链（chain-of-thought）推理模式，通过特殊的 `<|channel>thought\n` 标记触发深度思考；**工具调用支持**——模型具备结构化的函数调用能力，适合 agent 场景。

在基准测试方面，Gemma-4 31B 在 MMLU-Pro、GPQA、LiveCodeBench 等评测上与 Qwen3.5-35B-A3B 形成竞争。MoE 版本 26B-A4B 因为仅激活 4B 参数，在消费级硬件上的推理效率极为出色。模型推荐使用 temperature=1.0、top_p=0.95、top_k=64 的采样参数，使用 `<turn|>` 作为 EOS 标记。

Gemma 4 采用开放权重发布，允许商业使用和微调，通过 Hugging Face、Kaggle 等平台分发。Google 同时提供了优化的量化版本，支持在从手机到数据中心的多种硬件上部署。该系列模型体现了 Google 在开源模型战略上与 Meta Llama、阿里 Qwen 等系列的直接竞争态势。

## HN 评论精华

**实际性能测试：**

simonw（Simon Willison）用他标志性的"鹈鹕画画"基准测试模型：26B-A4B 版本产出了"出色的鹈鹕"，但 31B 版本初始表现"完全崩溃"（只输出 "---\n"），小模型（2B、4B）生成了无法辨认的输出。这暴露了不同规模模型之间质量差异巨大的问题。

**消费级硬件上的惊人速度：**

多位用户报告了令人兴奋的推理速度——RX 7900 XTX（24GB）在 32k 上下文下达到 100+ tokens/s，M1 Max 64GB 约 50 tokens/s，RTX 4090 上 26B-A4B 约 150 tok/s（vs Qwen3.5 35B 约 100 tok/s）。这证明 MoE 架构在本地部署场景的巨大优势。

**工具调用的 Bug：**

多位用户报告工具调用功能初始不工作，原因是 llama.cpp 的聊天模板存在 bug。LM Studio 团队确认当天部署了修复。neonstatic 测试时间戳计算任务时发现，Gemma-4 "幻觉"了工具执行过程——写出 Python 脚本但并未实际运行，在思考链中假装验证了结果但生成了错误的时间戳。这引发了关于"推理轨迹可能掩盖无能"的深层担忧。

**量化与采样参数讨论：**

danielhanchen（Unsloth 团队）提供了 GGUF 量化版本并解释了 Dynamic 2.0 量化的优势——通过选择性层优化和自定义校准数据集实现更好的压缩效果。关于 Google 推荐的 temperature=1.0 参数，社区展开讨论，传统上 0.7-0.8 被认为是质量最优，但 Google 基于可复现性选择了 1.0。

**真实世界应用案例：**

evilelectron 分享了用于历史土地档案数字化的 pipeline——集成 OCR、全文搜索、嵌入和摘要，用户现在可以用多种语言搜索追溯到 1800 年代的记录，处理延迟极低。

**基准测试的局限性：**

BoorishBears 直言基准测试不够："推理能力远在任何 Qwen 模型之上"，暗示某些模型针对基准测试进行了过度优化，实际使用体验可能与分数不符。
