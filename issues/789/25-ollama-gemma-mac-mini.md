---
layout: article
title: "2026 年 4 月在 Mac mini 上设置 Ollama 和 Gemma 4 26B 的快速指南"
issue: 789
number: 25
category: data
original_url: "https://gist.github.com/greenstevester/fc49b4e60a4fef9effc79066c1033ae5"
hn_url: "https://news.ycombinator.com/item?id=47624731"
date: 2026-04-10
---

## 文章摘要

这篇由 greenstevester 发布在 GitHub Gist 上的指南，详细介绍了如何在 Apple Silicon Mac mini（24GB 统一内存）上配置 Ollama 运行 Google 最新的 Gemma 4 模型，并实现开机自启动、模型预加载和持久化保活。

指南首先讨论了模型选择的问题。虽然标题提到了 26B 参数版本，但作者在实际测试中发现 26B 变体会占用约 17GB 内存，仅给 macOS 和其他进程留下约 7GB 空间。在并发 Ollama 请求的压力下，系统会频繁触发内存交换（swap），导致系统无响应甚至进程被杀死。因此作者最终推荐使用 8B 版本（Q4_K_M 量化），加载后仅占用约 9.6GB 内存，在 24GB Mac mini 上留有约 14GB 的充裕空间。对于拥有 32GB 或更大内存的 Mac，26B 模型则是可行的选择。

安装流程从通过 Homebrew 安装 Ollama 开始，Ollama 会自动集成 Apple 的 MLX 后端以优化 Apple Silicon 芯片上的推理性能。安装完成后，用户需要启动服务并确认菜单栏图标出现，然后拉取所需的 Gemma 4 模型。

在自动启动配置方面，指南建议在 Ollama 菜单中启用「登录时启动」选项，或在系统设置中进行配置。为了保持模型在内存中常驻，需要设置 `OLLAMA_KEEP_ALIVE="-1"` 环境变量（可添加到 `~/.zshrc` 中或通过专用 Launch Agent 设置），因为 Ollama 默认会在 5 分钟无活动后卸载模型。

更深层次的模型可用性控制则通过创建自定义 Launch Agent 实现。该 Launch Agent 会每隔 5 分钟向 `ollama run` 发送一个空提示，从而保持模型在内存中处于预热状态，确保随时可以快速响应请求。指南还包含了完整的 plist 配置文件模板，用户可以直接使用。

这篇指南的价值在于它不仅仅是简单的安装教程，而是基于作者在实际使用中踩过的坑（如内存不足、模型被卸载等问题）总结出的生产级最佳实践，对于想要在本地 Mac 上运行大语言模型的开发者非常有参考价值。

## HN 评论精华

1. **量化和分词器的早期问题**：有评论者警告说，对于刚发布的新模型，分词器（tokenizer）实现中已经发现了若干问题，使用 imatrix 的量化版本也可能存在兼容性问题。用户在使用工具调用（tool calls）等功能时可能会遇到异常，建议等待 llama.cpp 等项目修复相关 PR（如 #21343）后再尝试。

2. **Ollama 与原生推理的性能对比**：一位在实际 Mac Mini M4（24GB）上做过基准测试的用户指出，Ollama 的推理速度实际上比直接使用 llama.cpp 要慢。他建议对性能有较高要求的用户可以考虑直接使用 llama.cpp 或 MLX 框架来获得更好的推理速度。

3. **26B A4B 混合专家架构的澄清**：有评论者解释了 Gemma 4 26B 实际上采用了混合专家（Mixture-of-Experts）架构，推理时只有约 4B 参数处于激活状态，这就是为什么它能在 12-14GB 显存中运行，同时保持接近 26B 密集模型的推理质量。这个信息对于理解内存需求非常关键。

4. **是否能替代 Claude Code 的讨论**：有用户询问是否有人将本地运行的 Gemma 4 作为 Claude Code 等云端编程助手的替代方案。讨论中有人分享了使用 OpenCode 搭配本地 Gemma 4 的经验，但普遍认为在复杂编程任务上，本地模型与云端大模型仍有明显差距。
