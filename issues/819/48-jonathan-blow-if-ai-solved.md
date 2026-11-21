---
layout: article
title: "Jonathan Blow：如果 AI 解决了编程，软件为什么还是老样子？"
issue: 819
number: 48
category: watching
original_url: "https://www.youtube.com/watch?v=IRK8h67ol7Q"
hn_url: "https://news.ycombinator.com/item?id=50484658"
date: 2026-11-19
---

## 文章摘要

《The Students》这一期由游戏开发者 Jonathan Blow 与 Wolf Tivy 对谈。[节目方提供的简介](https://podcasts.apple.com/gb/podcast/jonathan-blow-if-ai-solved-coding-why-is-my-software/id1893143937?i=1000801730418) 将话题归为软件复杂度上升、今天的 AI 为何未必能交付比人更好的代码，以及如果机器真能做到，软件会发生什么变化。标题不是一项行业调查的结论，而是一个反问：如果编写代码的边际成本已经下降，为什么用户不一定同时看到更快、更稳、更易维护的软件？本期核对的是发布方的节目元数据和讨论，不把听众对具体观点的复述当成逐字稿。

把这个问题用到自己的项目，首先可以区分生成速度、交付周期和交付质量。写出第一版函数可能只要几分钟，但需求怎么划界、接口错误是否好定位、一次改动会不会给升级和维护留下隐患，都不由代码行数决定。团队若要检验助手的真实作用，可以在同一类任务里同时记录评审返工、复现失败所需时间，以及发布后回归，而不是只数一天合并了几个改动。游戏社区对小团队能力的反例也提醒我们，工具是否进步，不同类型的软件可能会给出不同答案。

## HN 评论精华

- **独立团队给出反例。** [barbariangrunge](https://news.ycombinator.com/item?id=50486239) 以近年的独立即时战略游戏为例，认为小团队能做出复杂、表现好的作品，但不清楚 AI 占了多少功劳。

- **用调试反馈衡量工具。** [corysama](https://news.ycombinator.com/item?id=50487577) 回忆早期 Xbox 项目已有直接在设备上继续编辑、逐步查看 GPU 活动的工作流，认为不少现代流程未必更顺手。
