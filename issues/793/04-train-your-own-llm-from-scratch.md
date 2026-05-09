---
layout: article
title: "从零训练你自己的 LLM"
issue: 793
number: 4
category: favorites
original_url: "https://github.com/angelos-p/llm-from-scratch"
hn_url: "https://news.ycombinator.com/item?id=48017948"
date: 2026-05-08
---

## 文章摘要

`angelos-p/llm-from-scratch` 是一个 GitHub 上的实操工作坊仓库，目标是让学习者**亲手敲完一个能用的 GPT 风格语言模型**——从分词到 transformer，再到训练循环和文本采样，每一层都自己写。它的灵感来自 Andrej Karpathy 著名的 nanoGPT 视频，但形式上更接近"分章节、可在笔记本上跑完"的练习册。

**技术栈**很克制：PyTorch + Python 3.12+，硬件上同时支持 Apple Silicon 的 MPS、NVIDIA 的 CUDA 和纯 CPU，也兼容 Google Colab。**模型规模**约 1000 万参数（6 层 transformer、6 个注意力头、384 维 embedding）；在 M3 Pro 笔记本上**完整训练大约 45 分钟**——这是这个项目的核心卖点：你不需要租 GPU，本机就能从头跑完一遍。

**两个实用决定**值得说一下。一是**字符级分词**而非 BPE——作者明确解释："小数据集上 BPE 几乎跑不出有意义的统计，大多数 token 二元组太稀疏了"，所以索性回到最朴素的 char-level，这样在 ~1MB 的莎士比亚全集上也能学到东西。二是**数据集**故意选莎士比亚而非 Wikipedia——既小又有特色，用 char-level 模型也能产生像模像样的输出，作为教学反馈极佳。

**课程结构**分 6 个章节：(1) 分词；(2) Transformer 架构（含注意力机制）；(3) 训练循环（损失、优化器）；(4) 文本生成与采样；(5) 端到端管线；(6) 自己尝试缩放和换数据集的"竞赛"环节。整体定位是入门到中级——读完之后你不会立刻能复刻 GPT-4，但你会真正理解每一行 PyTorch 代码在干什么。在 LLM 越来越被当作黑盒 API 使用的今天，这种"扒开看看里面有什么"的项目对工程师来说仍然很有价值。

## HN 评论精华

1. **jvican**：推荐想更深入的人去看 Stanford 的 **CS336** 课程——除了分词、训练这些基础，CS336 还覆盖了 scaling laws 的直觉、GPU kernel 优化和 profiling 这种"系统思维"，是更工程化的路径。

2. **JoeDaDude** 与 **gchadwick**：联合推荐 Sebastian Raschka 的 *Build a Large Language Model (From Scratch)* 一书——更系统、章节配套代码扎实，"适合那些真的想搞明白底层机制的人"，可以和这个 repo 互补。

3. **ofsen**：质疑这个项目和 Karpathy 的 nanoGPT 视频是否高度重合——本质上就是把 nanoGPT 缩到 ~10M 参数，让它能在笔记本上跑完。回复者认为定位仍然不同：视频形式 vs 可读 commit 历史的 repo 形式，对不同学习风格的人各有用处。

4. **eiskalt**：抠"from scratch"这个词的字面意思——既然用了 PyTorch，张量和反向传播就不是你写的，怎么能叫"从零"？他自己在用 Rust 标准库手写一个真正零依赖的版本作为对照。**wrs** 给了一句精辟回复："在 ML 圈里，PyTorch 基本就是 stdlib"，这种用法已经被默认接受。

5. **baalimago**：从命名学上吐槽——~10M 参数的模型严格说来不算"Large"，应该叫 SLM（small language model）。**真正的 LLM 离开数据中心连热身都做不到**。这是教学项目和真正前沿研究之间最大的认知差距。

6. **kriro**：分享了自己在 20 张 GPU 上训练 ULMFiT 的经历——光是把数据管道、checkpoint、监控搭好就够头大，提醒读者这个 1000 万参数的小项目最多让你理解原理，**真正的工程难点（分布式训练、显存管理、容错）**完全没碰。

7. 评论区还有一波讨论关于"这个项目能不能让你搞懂注意力机制"——支持者认为读懂代码胜过读十篇论文，反对者认为不画几张图、不推导一遍 softmax 缩放因子就盲跑代码，得到的是"会改 Python 但不懂数学"的伪理解。两派的分歧本质上是**学习风格**之争：动手 vs 推导。

8. 还有人指出真正缺失的是"训练完之后怎么办"——评估、对齐、量化、部署这些后训练步骤是工业 LLM 的另一半工作量，希望作者能加一章 fine-tuning + RLHF 的简化版来形成完整闭环。
