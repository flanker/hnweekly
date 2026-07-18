---
layout: article
title: "Inkling：我们的开放权重模型"
issue: 801
number: 29
category: data
original_url: "https://thinkingmachines.ai/news/introducing-inkling/"
hn_url: "https://news.ycombinator.com/item?id=48924912"
date: 2026-07-17
---

## 文章摘要

Inkling 是 Thinking Machines Lab（由前 OpenAI CTO Mira Murati 创立的实验室）于 2026 年 7 月 15 日发布的开放权重（open-weights）模型，权重开源在 Hugging Face 上。这是该实验室首次"发布模型而非博客文章"，被 HN 评论普遍称为"迄今最强的西方开放权重模型"。

**架构与规模**：Inkling 是一个 Mixture-of-Experts（MoE）Transformer，总参数 9750 亿、激活参数 410 亿，支持最长 100 万 token 上下文，在 45 万亿 token 的文本、图像、音频、视频数据上训练。每层有 256 个路由专家、每 token 激活 6 个，另加 2 个共享专家。较特别的设计包括：用相对位置编码而非 RoPE，注意力和残差分支里引入短卷积；大矩阵权重用 Muon 优化器、其余用 Adam。原生多模态处理文本、图像、音频——视觉用 40×40 像素的 patch 编码，音频用 dMel 频谱图，采用无编码器（encoder-free）架构。

**关键能力**：模型提供可调的"思考强度（thinking effort）"旋钮，范围 0.2 到 0.99，官方称其在 Terminal Bench 2.1 上"以约三分之一的 token 量匹敌 Nemotron 3 Ultra"，成本效率突出。在 agentic 能力（编码、工具使用）上表现优异，演示包括生成 Web 应用、PDF、多人游戏（部分经过 40 轮人类反馈迭代）。后训练先用合成数据自举，再做大规模强化学习（连续训练中超过 3000 万次 rollout），一个有趣的涌现现象是模型的推理"随时间变得更简洁，丢掉了语法赘余却仍可读"。在 epistemics 与安全上，模型被训练成校准置信度、遵循指令、抗审查，在 FORTRESS 等安全基准上"能拒绝更多有害请求而不误伤良性的相似请求"。

**定位与生态**：官方明确表示 Inkling 并非"当今最强的综合模型"，而是一个**可定制的基础模型**。还有一个更小的 Inkling-Small（总参数 2760 亿、激活 120 亿，仍具多模态，尚未发布）。模型可通过实验室自家的微调平台 Tinker（五折优惠）以及 TogetherAI、Fireworks、Modal、Databricks、Baseten 等 API 使用。Thinking Machines 的商业模式核心正是 Tinker——把权重开源、靠帮客户做 SFT/RL 微调与托管来变现。

## HN 评论精华

- **ls_stats** 道出了很多人的期待："美国需要自己的 DeepSeek 或 Z.ai"，不少人（包括他本人）之所以支持中国开放模型，只是因为别无选择，而 Thinking Machines 或许能填补这个空缺。
- **verdverm** 泼了盆冷水：Inkling 在 agentic 工作流上不如 GLM 5.2，体量却更大，"既然大 30%、还没 GLM 5.2 好，我为什么要折腾它？"由此引发关于"首发模型是否值得期待"的讨论——**InsideOutSanta** 反驳说 GLM 5.2 是经过大量后训练迭代才到今天的状态，作为首发，Inkling 已经很强、潜力很大。
- 一场关于模型代际的精彩考据由 **spwa4** 贡献：他细数了 GLM 5.2 相对各家的位置——能胜过 OpenAI 3 月的 GPT 5.5、Anthropic 4 月的 Opus 4.7，但被 GPT 5.6 Sol、Opus 4.8（Fable）超越，结论是"开源现在能在 OpenAI/Anthropic 发布约 3 个月后达到 SOTA"。**suprjami** 补充说这个差距在持续缩小，新出的 Kimi K3 已经只落后两个月，还调侃"正如 Google 2023 年那份备忘录所言——谁都没有护城河，开放权重终将胜出"。
- **开放权重模型到底靠什么赚钱** 是反复出现的疑问。**tfehring** 指出 Thinking Machines 的主打产品是 Tinker，即客户付费托管其微调工作负载和微调后的模型；**janalsncm** 半开玩笑地列出"1. 魔法 2. 托管 3. 打击竞争对手 4. 从开源社区白嫖 R&D 5. 更多魔法"。也有人（**tyre**）质疑"开源但卖托管"这条路历史上尸横遍野，大客户做大后就会自建。
- **alansaber** 的评论很接地气："我从没想过会有真的发模型而不是发博客的一天。Figure 3 那个演示只是个 localhost 的 Chrome 截图，反倒让我对自己感觉好多了。"**pr337h4m** 则认为 Thinking Machines 是少数几家（甚至唯一）在这个层级做出"既独特又有用"东西的实验室，而非单纯模仿他人。
- 关于基准的普遍警惕：多位评论者（如讨论 Gemini 3.5 Flash 的 **speedping**）强调"基准从不讲述全部故事"，有些开源模型早已被"刷榜（benchmaxxed）"，真实工作中的表现可能与榜单数字大相径庭；不过多模态输入（尤其视觉）被公认为实打实的加分项。
