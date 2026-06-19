---
layout: article
title: "如何在 macOS 上搭建本地编码智能体"
issue: 798
number: 25
category: code
original_url: "https://ikyle.me/blog/2026/how-to-setup-a-local-coding-agent-on-macos"
hn_url: "https://news.ycombinator.com/item?id=48507020"
date: 2026-06-19
---

## 文章摘要

本文介绍如何在 macOS 上完全离线地搭建一套本地编码智能体。作者选用 **llama.cpp**（而非 Ollama）作为推理运行时，并启用 Metal 加速。主力模型是 GGUF 格式的 Gemma 4 26B-A4B，配合一个 Q8 的多 token 预测（MTP）草稿模型做投机解码，再加一个多模态投影器以支持图像输入。

硬件方面，作者在一台 Apple M1 Max（64GB 统一内存，macOS 15.7.7）上测试。主模型文件约 16GB，连同草稿模型和投影器整套约 17GB。搭建步骤大致为：

1. 用 CMake 以 Metal 和 Accelerate 标志编译 llama.cpp；
2. 用 huggingface-cli 从 Hugging Face 下载模型；
3. 启动 llama.cpp 服务器，开启 MTP 与多模态支持，监听 localhost:8080；
4. 配置编码智能体 **Pi**（通过 `~/.pi/agent/models.json`）指向本地的 OpenAI 兼容端点；
5. 验证连通性，并将其设为默认模型。

主要收获：MTP 把生成速度从 58.2 token/s 提升到 72.2 token/s（约 24% 增益）；llama.cpp 表现优于专为 macOS 优化的 MLX；Qwen3.6 35B 编码能力更强但更慢（约 55 token/s）。整套方案借助 OpenAI 兼容 API，实现了带多模态能力的离线运行。

## HN 评论精华

**c-hendricks**：指出若只用 llama.cpp，其实不必动用 huggingface-cli——直接传 `-hf ...` 即可自动下载模型，用 `LLAMA_CACHE` 还能改下载目录。

**Aurornis**：质疑基准测试只生成约 128 token 太短，MTP 加速取决于预测 token 的接受率，而早期输出接受率偏高，短测试容易得出虚高的加速结论；建议用 llama.cpp 自带的基准工具扫参数。

**reenorap**：吐槽所有本地 AI 文章只谈每秒 token 数，从不提答案质量——他宁可多等一会儿换取更好的结果，"快速喂给我垃圾"并无用处。

**多位评论者（sleepybrett、smetannik、bicepjai 等）**：纷纷质疑为何不直接用 Ollama 或 LM Studio——后者内置本地服务器，两分钟就能接上 opencode，省去折腾 llama.cpp 的麻烦。

**jumploops**：分享在 128GB M4 Max 上跑 DeepSeek v4 Flash 的体验，称其知识储备接近 GPT-4 级，长链路工具调用尤其出色，适合做"简单任务的机器编排器"。

**dofm**：反馈在自己的 M1 Max 上 MTP 提速有限，且 Gemma 4 的 MTP 头偶尔会破坏 Opencode 中的标记格式甚至漏掉停止符，已暂时弃用 MTP。
