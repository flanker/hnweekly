---
layout: article
title: "DiffusionGemma：文本生成速度提升 4 倍"
issue: 797
number: 22
category: data
original_url: "https://blog.google/innovation-and-ai/technology/developers-tools/diffusion-gemma-faster-text-generation/"
hn_url: "https://news.ycombinator.com/item?id=48478471"
date: 2026-06-12
---

## 文章摘要

DiffusionGemma 是 Google 推出的实验性开源模型，它采用基于扩散（diffusion）的文本生成方式，而非传统的逐个 token 顺序生成。它是一个 26B 的混合专家（MoE）模型，能够「同时生成整块文本」，在 GPU 上带来最高 4 倍的文本生成提速。

与一次只产出一个词的自回归模型不同，DiffusionGemma 的工作方式类似图像生成器：先从「一块由随机占位符 token 构成的画布」开始，再通过多轮迭代锁定正确的 token，并以它们作为上下文线索去修正其余部分，直到文本收敛为最终输出。

4 倍提速的来源是把解码瓶颈从「内存带宽」转移到「算力」。模型在每次前向传播中并行生成 256 个 token，在 NVIDIA H100 上可达每秒超过 1000 个 token。不过这一优势主要体现在本地、单用户推理场景；高并发的云端部署收益会递减。

模型细节与权衡：

- **架构**：总参数 26B，推理时仅激活 3.8B。
- **显存需求**：量化后约 18GB VRAM。
- **性能**：RTX 5090 上 700+ token/秒，H100 上 1000+ token/秒。
- **代价**：输出质量低于标准 Gemma 4 模型。
- **优势**：双向注意力支持代码填充（infilling）和实时自我纠错等任务。

## HN 评论精华

**vineyardmike** 盛赞 Mercury 扩散模型带来的快速、交互式结对编程体验，认为小模型比苦等缓慢的 SOTA agent 更实用，更接近 AI 时代之前的编码工作流，「更有趣」。

**onlyrealcuzzo** 认为，配合衡量代码复杂度的自动化指标，更快更便宜的模型可以胜过更慢的模型，强调结合测试基础设施做快速迭代的价值。

**samuelknight** 解释了扩散模型对边缘设备的关键优势：并行 token 计算缓解了内存带宽瓶颈，而这正是顺序自回归解码在带宽有限的消费级硬件（如 LPDDR）上的痛点。

**SwellJoe** 指出随着补贴 token 的终结，成本效率会越来越重要：DeepSeek 比竞品便宜 10 倍仍然可用，并质疑 Gemini 的竞争劣势究竟反映真实局限，还是出于聚焦边缘部署的战略选择。

整体讨论围绕速度与质量的权衡展开：共识是收益集中在本地/单用户推理，而非高并发云端服务；高 QPS 环境可高效批处理自回归请求，从而抵消扩散模型的并行优势。也有人担忧思维链可读性下降、工具调用支持不足以及复杂推理基准表现下滑。
