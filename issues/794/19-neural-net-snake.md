---
layout: article
title: "Show HN：看一只神经网络在浏览器里学会玩贪吃蛇"
issue: 794
number: 19
category: show_hn
original_url: "https://ppo.gradexp.xyz/"
hn_url: "https://news.ycombinator.com/item?id=48136981"
date: 2026-05-22
---

## 文章摘要

作者搭了一个完全跑在浏览器里的强化学习训练 demo——**tinyppo-snake**。打开页面你看到的是一个网格化的贪吃蛇环境和一组实时训练指标：episode 数、policy loss、value loss、entropy、梯度范数、20 步滚动均分、当前 best window。左边一只蛇在你眼前学习，右边图表逐渐爬升。

用的算法是 **PPO（Proximal Policy Optimization）**——OpenAI 的经典策略梯度方法，2017 年至今仍是 RL 工业级首选之一。整个训练循环、推理、3D 渲染都在浏览器里跑，底层框架是作者自己的 **Gradient Explorer**。用户可以选预设学习率（1e-3 / 3e-3 / 1e-2），也能开多个 grid 同时跑做并排对比、看哪一组超参数收敛得更稳。

**为什么有意思？**最直观的——零安装、零后端、点开就能围观一个神经网络从乱撞墙到能跑出 4000 分。次直观的——它把训练过程**可视化**到所有指标实时联动：奖励曲线一抬头你能立刻在左边看到蛇变得"会拐弯了"；学习率拉得太高崩了你能在 entropy 突然飙升时直接看到。对很多没系统学过 RL 的人来说，这是一份**比任何 textbook 都直观的入门材料**。

## HN 评论精华

- **ldoughty**（带回了一份 bug 报告）：训练快到 4000 分时突然崩了，之后所有 episode 都拿 0 分。`peak 3959.3 best window`，`avg500 -4.6` 一路烂尾。这是**强化学习里经典的"灾难性遗忘"或者策略坍缩**——网络发现某个高奖励路径之后过度自信、entropy 太低、再也跳不出局部最优。**austinthetaco** 在 10x10 网格也遇到一样的情况："到 3600 突然图表横平、左边一直 restart、分数掉到 -10。"

- **simedw**：发现一个 UI 影响训练的 bug——"**从 training 切到 watch 再切回来，训练分数会掉一大截。**"很可能是 watch 模式下的渲染开销或者梯度状态没保住。

- **ziofill**（指出奖励函数设计问题）：蛇被惩罚"不能尽快吃到苹果"——但这违背贪吃蛇的本质："**贪吃蛇玩的是长度，不是吃苹果的速度**。"如果奖励里塞了时间惩罚，agent 会学到的是"快速吃苹果"，而不是"长得更长"。这是 RL reward shaping 一个非常经典的踩坑——你以为加个时间项是无伤大雅的加速器，实际上重塑了 agent 的目标函数。

- **jesuo**（描述了另一种失败模式）：他看到的蛇陷入了一种"阴阳图"循环——蛇一直沿着固定路径走、永远绕开苹果、累积负分、却不再调整策略。"**程序写得不好——它不会从错误中学习。**"评论里这是对所有 RL demo 共通的不满：浏览器算力 + 小模型很容易在某一条 loss 平台上死掉。

- **joshka**：提到一个相邻项目——`bones-ai/rust-snake-ai-ratatui`，**纯 Rust 写的、跑在 Ratatui 终端 UI 里的 Snake AI**——同样的 idea 但完全是终端美学。

- **starshadowx2**（运营警告）：Bitdefender 把这个站标为"可疑活动"，原因可能是 `.safetensors` 权重文件、`.wa` （WebAssembly）资源、还有 Sokol 框架的 demo 文件触发了启发式规则。作者要做用户教育/白名单工作。

- **snats**：上月也做过类似实验——给文字渲染库的字符位置训了个 RL agent、再做可视化让它 displace 文本。

- **Mariajaved906**：从更高视角下评论——"**在浏览器里用 WebGPU 直接跑 PPO 训练**，这是轻量级 AI 实验未来方向的一瞥。"过去 RL 教学都是 Colab/Jupyter，这种"打开网页就开始训练"的形态正在变得可行。

- **anthonycoslett**：把它当数字艺术看——蛇在格子里反复学习、抽象出策略的过程"催眠且有美感"，建议作者考虑把训练过程做成画廊展览。
