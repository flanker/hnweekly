---
layout: article
title: "FablePool——把钱汇集到一条 prompt 背后，让 Fable 公开构建它"
issue: 797
number: 17
category: show_hn
original_url: "https://fablepool.com"
hn_url: "https://news.ycombinator.com/item?id=48496539"
date: 2026-06-12
---

## 文章摘要

FablePool 是一个众筹平台，让一群素不相识的人共同出资资助雄心勃勃的 AI 项目。其核心理念是"把钱汇集到一条大 prompt 背后"——为一个 AI agent 尝试构建某个大型项目提供资金，正如平台所言："An AI attempts the build, in public"（AI 公开地尝试构建）。

运作方式如下：

- **发起项目**：有人发布一条雄心勃勃的指令，并给出预估的 credit 目标（最低 10,000 credits）。
- **社区出资**：支持者可以从 25 credits 起任意金额贡献。
- **公开执行**：AI agent 按里程碑逐步推进项目，每一笔交易都记录在透明的账本上。
- **透明度**：所有支出与进展对社区可见。

执行这些项目的 AI agent 名为 Fable（Anthropic 的 AI agent）。近期示例包括构建 3D 机械引擎平台、创建开源数据库、开发类 Turbopuffer 的搜索系统，以及构建 AI 记忆协议。项目类型从软件开发到基础设施搭建不等，状态标记为"已完成"、"进行中"或"等待资助"，平台还提供点赞、社区评论和投票机制来引导未来方向。本质上，FablePool 试图让 AI 驱动的开发民主化，让社区共同赞助复杂构建并透明追踪进度。

## HN 评论精华

**bensyverson**：建议部分资助的项目应先生成详细实现方案供公开评审；同时对 AI 生成代码使用 MIT 许可证提出疑问，涉及版权层面的影响。

**pjc50**（高赞）：质疑为什么人们热衷于出资让 AI 构建项目，却不去支持那些贡献了数十年、且作品大量进入训练数据的人类开源开发者。

**wongarsu**（高赞）：指出美国版权局的立场表明 AI 无法持有版权，因此代码归属在法律上变得模糊——这与可以通过雇佣协议（work-for-hire）转让版权的人类开发者不同。

**KeplerBoy**：认为 LLM 的产出可预测且便宜（约 200 美元），这让那些人们自己不会动手、又不值得花低薪雇人去做的点子变得可行。

**rickcarlino**：将该平台形容为"vibe coding 的 GoFundMe 时代"——众筹生成 AI 代码却缺乏传统质量保证。

**matthewbarras**（创建者）：承认存在技术问题（估算引擎仍需打磨），并承诺用真实的资助案例改进示例。
