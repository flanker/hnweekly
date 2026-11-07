---
layout: article
title: "在 Minecraft 与 Mindustry 中观察 AI 群体协作"
issue: 817
number: 23
category: data
original_url: "https://labs.ycombinator.com/projects/games/index.html"
hn_url: "https://news.ycombinator.com/item?id=50296486"
date: 2026-11-06
---

## 文章摘要

YC Labs 与 Olam Labs 把多个 AI 代理放入 Minecraft 和 Mindustry，观察长期协作、分工与竞争。Minecraft 的 Civ50 实验让 50 个 GPT-6.1 Sol 代理运行三小时，目标是建立繁荣文明，并提供共享环境、目录及私信。报告描述它们快速采集木材，却在第一夜的大量死亡后转向保守：挤进避难所、分配食物、记录配给与编写规章。约 5,900 条消息没有自动转化为高效率，目录排序还意外影响了领导者的形成。

其他实验使用 Bed Wars 和 Mindustry 对抗任务。报告称七局 Mindustry 中 GPT 队伍以 6 比 1 获胜，并比较通信密度、角色划分、临时决策与查阅游戏源码等行为。它展示聊天、运行记录和回放，也开源相关实验环境，让读者能把有趣叙事与具体行为对应起来。

这些实验揭示了「会写计划」与「能在动态世界持续执行」之间的落差。物资交接错误、工具接口问题、行动延迟以及过度协调都会限制表现。胜负同时依赖模型、代理运行框架、提示、工具和游戏规则，不能据几局比赛得出模型普遍能力或人类社会行为的定论。本文根据公开报告与文本记录整理，未重跑实验，也未逐段核验所有视频。

## HN 评论精华

- **实时游戏可能放大推理延迟。** nsingh2 认为大型推理模型更适合回合制；lukan 和 InsidiousWombat 则建议让模型负责高层规划，把频繁动作交给普通脚本。
- **先检查工具环境。** mplappert 指出物资交接与库存访问看起来很笨重；peri-cl [进一步分析](https://news.ycombinator.com/item?id=50304402)工具调用和基础动作障碍，认为不能把失败全部归于模型能力。
- **程序化协调的喜剧效果。** peri-cl 注意到一次鸡肉误领后出现书面分配流程；aizk 用「害怕审计的中层经理」形容这种状态。这是对观察到的行为的比喻，并非心理学诊断。
