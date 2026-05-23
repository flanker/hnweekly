---
layout: article
title: "亚马逊员工在"token 刷分"：被 KPI 逼出来的 AI 使用量"
issue: 794
number: 40
category: working
original_url: "https://arstechnica.com/ai/2026/05/amazon-employees-are-tokenmaxxing-due-to-pressure-to-use-ai-tools/"
hn_url: "https://news.ycombinator.com/item?id=48110529"
date: 2026-05-22
---

## 文章摘要

Ars Technica 转载 FT 的爆料：**亚马逊员工正在大规模"tokenmaxxing"**——用一款内部 AI 工具自动跑无意义任务，只为把自己的 token 消耗数据冲上去给管理层看。亚马逊今年内推开了一个叫 **MeshClaw** 的内部工具（看名字明显是对标 Anthropic 的 Claude Code / OpenClaw），允许员工搭建能连入企业软件、代为执行任务的 AI agent。结果员工立刻把它武器化了：搭一堆"代我做事"的 agent 跑在后台，**专门为了刷 token 消耗**。

诱因很直白。亚马逊给了硬目标——**超过 80% 的开发者每周必须使用 AI**，今年早些时候还上线了内部的"token 消耗排行榜"。官方口径是"这些数据不会进绩效评估"，但多个内部员工反馈："经理在看，他们追踪的时候就会制造扭曲激励，有人就是要在这上面赢。" 类似的故事在 Meta 也已经发生过——他们的员工同样在排行榜上互相 tokenmaxx。这是教科书级别的 **古德哈特定律（Goodhart's Law）**：一旦某个测量值变成目标，它就不再是好的测量值。

文章还点出一个潜在风险：员工承认 MeshClaw 的默认权限范围让他们"害怕"——它能触发代码部署、整理邮件、和 Slack 交互；一旦在自动循环里出错，后果不仅是浪费 token，而是真的会**改坏生产系统**。FT 引述的内部备忘录把这工具吹成"夜里 dream 整合学到的东西、监控你 meeting 中的部署、清晨醒来时邮件已分类完毕"——读起来像营销文案，但员工补一句："I'm not about to let it go off and just do its own thing."

## HN 评论精华

- **x187463**（高赞）：把 token 数当 productivity metric，就跟当年统计敲键盘数一样荒唐——别管我，我就在键盘上滚脸，星期五请假。"每次敲键盘都有一个真实成本，加起来可能超过我的工资。"

- **Analemma_**：最让人崩溃的是——那些平时一定会嘲讽"按 LOC 衡量程序员就是傻 X 公司"的聪明人，对 token 排行榜居然全盘照收。亚马逊有 token mandate，Google 据说有 token leaderboard，朋友们的创业公司都按 token 用量打分。"就像看着你那些理性朋友集体精神崩溃，集体疯狂。"

- **KyleTheDev**（前亚马逊 SDE）：直接反驳官方"不进绩效评估"的说法——**它就是被用在绩效评估里**，加上一堆同样傻的指标（PR 数、PR 评论数），逼得内部出现"自动 LGTM 评论"之类的脚本来刷指标，指标越变越烂。

- **sarchertech**：在另一家大型 SV 科技公司，至少一半同事在 token max。AI 是真有用，但"engineering org 整体业务价值翻倍"是吹的；公司无法测 individual 价值，但它能测 token，于是 token 就成了指标。"我已经把不少美股大盘股的钱挪出去了。"

- **some_furry**（贡献骚操作）：把 agent 接一个 Python 脚本无限自检自答就能刷指标了——甚至中间塞一段塔罗牌占卜让它行为非确定。"亚马逊管理层想下五维棋？那就一起玩 Balatro 吧。"

- **i7l**：管理层签字同意把 token 数当指标，本身就证明亚马逊这种"号称技术公司"的管理层有多无能。**delfinom**：因为多数管理者技术上根本跟不上自己管的人，只能退回到数字上才能参与讨论。**Terr_** 引用 McNamara Fallacy 四步：能量化的就量化（OK）；不好量化的随便估个数（误导）；最后不能测的就当不重要（盲目）。

- **traderj0e**："当一个 measure 存在，它就会变成 target——这就是我再也不写 TODO 的原因。"

- **wordpad** 提了个阴谋论：管理层不是真在乎个人 productivity，**他们就是想烧钱训练数据 / 跟随 AI 泡沫**——把员工 token 用量推上去本身就是公司层面的目标。**cousinbryce**：更可能是为了最大化训练数据。

- **this_user**（替管理层辩护）：有人解释这是**故意烧钱让员工探索 AI 的可能性**——成本 known，回报 unknown，但你赌"探索"。**themafia** 怼回去：研究不是我的 job title，你都不知道你买的东西能干嘛你买它干嘛？让员工帮你做产品调研？

- **Argonaut998**：在 GitHub Copilot 上一天只有 300 次 request 额度，**Opus 4.7 在 Copilot 上是 7.5x 倍率**，**ddtaylor 说他遇到的甚至是 27x 倍率**——一天根本用不完一个真正的复杂任务。管理层到底想让我用多还是用少？没人说得清。

- **H8crilA**：最重要的技能是"不要从人群里冒出来"。在苏联是这样，在军队是这样，在科技公司也是这样。
