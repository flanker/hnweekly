---
layout: article
title: "DeepInfer：开放 DeepSeek 推理核心，但不是完整托管服务"
issue: 816
number: 26
category: data
original_url: "https://github.com/deepseek-ai/DeepInfer"
hn_url: "https://news.ycombinator.com/item?id=50163277"
date: 2026-10-30
---

## 文章摘要

DeepInfer 是 DeepSeek 开放的推理框架，仓库描述它源自大规模生产服务，并提供离线推理所需的核心组件。当前列出的模型为 DeepSeek-V4.1-Flash，硬件面向 NVIDIA SM100 GPU 和华为 Ascend 950 NPU。它整合优化算子、注意力及专家并行、异步调度、推测解码和结构化输出等功能。

对评测与复现尤其值得注意的是，仓库将批次不变推理和确定性采样明确列为可配置能力，且部分功能限定 NVIDIA 平台。另一方面，README 同样列出未包含的组件：完整 KV 缓存系统、预填充与解码等阶段的服务化拆分，以及跨副本负载均衡。开源核心可以帮助其他团队复用优化，却不能把「可运行离线推理」理解为复制代码后就得到同等规模的在线服务。项目中的性能结果也绑定特定模型、卡数和硬件环境，不适合直接套用到普通开发机。

## HN 评论精华

**最重要的提醒是缺了什么。** _aavaa_ 指出 KV 缓存系统没有开放；GreenWatermelon 引用 README 的缺失列表，提醒读者分清推理引擎和完整服务。

**商业数字引来大量推测。** schneehertz 转述单卡小时收入等说法，其他用户据此讨论利润和竞争格局。这些评论引用外部材料，并未独立证明整套成本、利用率或盈利能力，不宜当作通用结论。

**共同实现可能减少服务差异。** swiftcoder 认为开放优化有助于不同提供商采用一致方案；rektlessness 则强调距离开箱即用的生产集群还有工作。关于开源动机的地缘政治判断分歧很大，应视为观点而非技术事实。
