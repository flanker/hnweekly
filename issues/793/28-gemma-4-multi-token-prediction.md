---
layout: article
title: "加速 Gemma 4：用多 Token 预测 Drafter 提速推理"
issue: 793
number: 28
category: data
original_url: "https://blog.google/innovation-and-ai/technology/developers-tools/multi-token-prediction-gemma-4/"
hn_url: "https://news.ycombinator.com/item?id=48024540"
date: 2026-05-08
---

## 文章摘要

Google 为 Gemma 4 系列模型发布了一组**多 Token 预测（MTP）drafter**，配合推测解码（speculative decoding）可以在不损失输出质量、不改变推理逻辑的前提下，把推理速度**提升最多 3 倍**。这组 drafter 适配主流推理框架——LiteRT-LM、MLX、Hugging Face Transformers、vLLM、SGLang、Ollama 都已支持，权重以 Apache 2.0 协议在 Hugging Face 和 Kaggle 上开放下载。

技术原理上，MTP 的核心是把一个**轻量级 drafter 模型**和重量级 target 模型配对：drafter 一次预测多个 token，target 模型并行验证；如果 target 同意 drafter 的草稿，就在一次前向传播里**接受整段序列并多生成一个 token**。这能直接命中 LLM 推理的根本瓶颈——标准推理是"内存带宽受限"的：每生成一个 token，处理器都要把数十亿参数从显存搬一遍，算力大量闲置。MTP 通过批量化让搬过来的参数顺手多算几个 token，从而把闲置算力榨干。

工程上还有几个关键优化：drafter **共享 target 模型的 KV cache 和激活**，避免重复计算；针对 Gemma 4 的边缘版本（E2B、E4B），Google 在 embedder 层引入了一种聚类技术进一步加速生成。基准测试上，NVIDIA RTX PRO 6000 跑 26B 模型时**等待时间减半**；Apple Silicon 上以 batch size 4-8 跑 26B MoE 模型，加速比约 **2.2 倍**。对本地部署、边缘设备、消费级硬件运行 Gemma 的开发者来说，这是一次纯增量收益。

值得关注的是 Google 把 drafter 与多家第三方推理框架同时打通——尤其是 MLX 和 Ollama 这类社区栈——意味着普通开发者无需改动应用层就能享受加速，是开源 LLM 生态成熟度的一个标志。

## HN 评论精华

1. **libraryofbabel**：解释了推测解码为什么能"零质量损失"——较小的 drafter 生成草稿 token，大模型并行验证；本质是利用了"很多场景下下一个 token 是显而易见的"这一事实（比如 `function ` 后面大概率是函数名）。模型不接受不合预期的草稿就行了。

2. **janalsncm**：补充了验证机制的细节——不是简单贪心解码，而是**按 softmax 概率比做概率性接受**，确保最终采样分布与原模型一致。这点是为什么"无质量损失"成立的数学基础。

3. **mike_hearn**：从硬件角度澄清——Transformer 在前向传播里其实**已经在为整个上下文窗口算下一个 token 的概率**，只是平时只取最后一个。利用这点把权重矩阵的 IO 开销摊到多个 token 上，**能耗没省，但延迟近乎免费**。

4. **mungoman2**：一句话点破——LLM 推理是"**memory bandwidth bound**"，算力闲置严重。MTP 像 prefill 一样把多个 token 打包，复用搬过来的权重，本质上不是在抢算力，而是在抢被浪费的算力。

5. **zozbot234**：MTP 的收益在**单用户/低并发**场景最大，因为算力充裕；高并发批处理本来就在用大 batch 摊薄 IO，MTP 的边际收益变小，但 KV cache 较小的模型（如 DeepSeek）依然能从中获益。

6. **WarmWash**：实测观察——Gemma 系列**单次输出 token 数本身就比同类模型少很多**（更"言简意赅"），benchmark 上落后 5-10%，但耗时只有 1/10。叠加 MTP 之后实际工程价值进一步放大。

7. 评论区另有一条共识：Google 把 drafter 和 vLLM、MLX、Ollama 同步打通这一点很专业——对比某些公司只支持自家闭源 stack，Gemma 在"开源能用"这件事上做得越来越扎实。
