---
layout: article
title: "高级工程师为什么总解释不清自己的价值"
issue: 794
number: 41
category: working
original_url: "https://www.nair.sh/guides-and-opinions/communicating-your-expertise/why-senior-developers-fail-to-communicate-their-expertise"
hn_url: "https://news.ycombinator.com/item?id=48109460"
date: 2026-05-22
---

## 文章摘要

作者抛出一个让很多资深工程师听了不舒服但又点头的观察：**senior dev 解释不清自己的价值，是因为他们在解决一个业务根本不关心的问题**。业务侧（marketing、sales、product）跑的是 **Speed Loop**——市场反馈、降低不确定性、抢窗口；senior dev 跑的是 **Stability Loop**——系统可靠性、复杂度管理、长期可维护。两套循环关心两件事，但句子结构看起来很像，所以双方都以为对方在听自己说话。

典型场景：业务方要"加个功能"，senior 立刻说"我们真的需要吗？""加这个会拉高复杂度、维护成本、可理解性。""能不能先用现有方案凑合，回头有空再做？"——他自以为在帮公司守门，业务方听到的却是"这人不想做事"。作者把这种人画了一张挺刻薄但精准的肖像："The avoider, the reducer, the recycler. They want to avoid development as much as they can."

文章给的解决方案不是要 senior 妥协，而是**换一句话来表达同一件事**："Can we try something quicker?"——这句话承认了业务方需要速度，同时把 senior 自己擅长的"资源优化"塞到回答里。底层逻辑是：你的专长不是"管理复杂度"，**你的专长是用更少的资源达成同样的目标**，这个 framing 正好能落进 Speed Loop 的语言里。

最后一段把 AI 接进来：AI 加速 Speed Loop（写代码快），但破坏 Stability Loop（没人审查、没人对结果负责）。这反而让 senior 的价值更明确——不是写代码的人，而是**作为 editor 维护系统连贯性的人**。AI 还做不到的那一件事是：take responsibility。

## HN 评论精华

- **JohnMakin**（高赞）：作者结论我不反对，但**满篇 AI prose smell**，读着特别出戏。**jewel** 补充：可能不是 AI 写的，是作者读太多 AI 文本以后被影响了写作风格。**alwa**：愿意相信作者是 copywriter 出身——它的 punchy staccato 节奏和反问句虽然像 AI，但每次反问背后真有 payload，不像 LLM 那种空转。

- **lnenad**（不同意全文）：作为资深工程师，最讨厌这种 blanket statement。"Do we really need that?" 这类问题在 firmware for CT scanner 上是救命的，在 100-客户 SaaS 上可能是绊脚石。**每个系统都不一样**——文章把"reducer"画成贬义脸，但很多场景"少做"才是对的。

- **bilekas**：作者把 senior dev 写成 "avoider, reducer, recycler" 有失公允——好 senior 的本事恰恰是**判断什么时候该 reduce、什么时候该主动 introduce 改进**。一刀切批评只会让真正的 senior 觉得被冒犯。

- **someone654**：替作者辩护——文章里其实写了"Yes, yes, of course this is simplistic"，是故意用极端例子讲框架，作者并没主张要无脑 speed。

- **dasil003**：这就是为什么需要够 senior 的工程领导（IC 和经理都算）——没有他们，"非技术 stakeholder 说什么就做什么"会形成责任真空，迟早炸锅，最后**最不会 CYA 的人背锅**。

- **Yokohiii**：很多还不错的 dev **不敢说"不"**，也不去谈一个平衡的方案——他们要么默默照做，要么沉默积怨。

- **iJohnDoe**（站在管理者视角）：几乎所有 CEO / 总裁现在心里都在想"AI agent 才是未来，不再需要开发者拖慢业务进度"。然后他甩出一句反讽："Well, those developers are about to have the worst day ever when every single person on the planet can generate code." **mschuster91** 加佐证：AWS 一年内两次大规模故障，"slop 进自己生产环境"——AI 失败的代价正在显形。**bigfishrunning**：让 CEO 自己 vibe code 吧，业务会失败，世界会变烂，最终大家会从废墟里学到点东西。

- **hosh**：复杂度不是单维度——还有 maintainability、scalability、reliability、resilience、anti-fragility、extensibility、versatility、durability、composability。**能从 solution space 多维度而不是单一维度谈 trade-off** 才是 senior → staff+ 的分水岭。

- **bob1029**（实用主义）：要说服业务，得用 customer perspective 包装论据。"这样能在 X 天内交付 Y 功能给已经抱怨 Q 个月的 W 客户。"远比"这样有 future extensibility / 更易单元测试"管用。

- **t43562**：所有事情都是 **TRADEOFFS**——这是非程序员最难理解的事，也是程序员一辈子要学的事。

- **goosejuice**：作为典型的 "avoider, reducer, recycler"，在某些团队里确实会被孤立。窍门是：**把复杂度的代价翻译成非工程师能感受到的语言**，比如"这会让客服每周多接 10 个工单"。在同一批 leadership 下混几年、几轮"vocal but compromising"之后，复杂度真的爆了的时候，你会因为"早就说过"而拿到信任。

- **BiraIgnacio**：任何领域的 senior 都需要 above-good 的沟通能力。**nathanielks** 反驳：可惜很多 senior+ 工程师**沟通极烂但技术极强**。**lwhi** 给个柔性结论：那他们就别去带团队。
