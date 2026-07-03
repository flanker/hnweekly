---
layout: article
title: "Tokenmaxxing 已死，Tokenmaxxing 万岁"
issue: 800
number: 9
category: favorites
original_url: "https://12gramsofcarbon.com/p/agentics-tech-things-tokenmaxxing"
hn_url: "https://news.ycombinator.com/item?id=48708795"
date: 2026-07-03
---

## 文章摘要

所谓 "tokenmaxxing"（堆 token），指的是高管激励员工在各种任务上大量消耗 AI token。最经典的例子是 Meta——它把绩效考核与 token 用量挂钩，结果员工为了刷指标做出各种毫无意义的行为，比如让 agent 之间互相对话，纯粹为了把数字做大。

文章的核心论点是：tokenmaxxing 并非无心的管理失误，而是一种有意为之的策略。高管把"必须花掉 token"当作一种粗暴的强制手段，用来突破组织内部（尤其是持怀疑态度的资深员工）对 AI 采用的抵触。作者梳理了这一模式的演变：最初对 AI 工具的抵触已基本瓦解；与此同时，随着 OpenAI 和 Anthropic 上调 API 价格，token 成本大幅上升，传统的 tokenmaxxing 激励也随之消失。

但作者认为 tokenmaxxing 会以新形态回归，关键变量是"复利式正确性"（compounding correctness）。在过去，token 用得越多、误差不断累积，结果反而越糟；而更新一代的模型呈现出相反趋势——投入更多 token 能换来更好的结果，这就为大规模 token 投入提供了真正的 ROI 依据。作者还区分了两类 tokenmaxxing：一是面向开发者的（用 Claude Code、循环式调用），二是面向流水线的（用 agent 替代确定性代码，往往是浪费）。他预测的终局是"黑灯工厂"（dark factory）——几乎无需人类介入、但需要大量 token 投入的全自动软件开发。

## HN 评论精华

评论区就"tokenmaxxing 究竟是精明策略还是愚蠢跟风"展开了激烈交锋。

- **aurareturn** 支持文章观点，认为 tokenmaxxing 只是逼员工认真用起 AI 的手段：一旦员工学会了 AI 能做什么、不能做什么，公司自然会把考核标准调回来。"没有哪家公司蠢到永远按 token 花费来衡量绩效、还给无限预算——这从来就是一个过渡性的临时措施。"

- 这一"善意解读"遭到多人反驳。**herval** 说，听过一些 VP 和 C 级高管在这场"Tokenmaxxing 郁金香狂热"中的论调后，他认为把这些命令解读成"有意引导员工"太过宽容了——大多数公司要么纯粹是"别人做什么我就做什么"，要么是想看看"程序员 Joe 能不能顶替整个团队好让我们裁掉其他人"。**dspillett** 印证道，他所在的金融服务公司之所以"AI 狂热"，只是因为竞争对手宣布"AI first"后股市反应热烈，他们不想掉队，"并不是因为觉得这是个好主意"。

- **arexxbifs** 言辞最激烈：这根本不是什么策略，而是一群此前追捧区块链和元宇宙的无能管理者在炒作驱动下的愚蠢之举，"完全没有成本或后果分析"，所以一旦补贴过的 token 稍微涨价就立刻收手。**witx** 也直言 tokenmaxxing 从来不是聪明的有意策略，而是 FOMO、向投资人释放"我们跟上了炒作"的信号、以及回收数据中心投资的混合产物。

- **aurareturn** 与 **therein**、**witx** 之间围绕"Uber 类比"来回交锋，气氛颇为火爆。**SecretDreams** 和 **jdiff** 则给出冷静判断：除了少数管理良好的公司，普通一线员工往往比管理层更聪明，别把管理层想象成在下什么"我们看不懂的五维象棋"——那些决策连他们自己当时都说不清。

- 另一条务实支线值得一提：**ido** 和 **linsomniac** 指出，有些开发者确实需要被"推一把"甚至强推出舒适区（就像当年推动他们用调试器、用更强的 git 客户端一样）。linsomniac 分享说，他所在公司在 AI 开销上一直很节省，但发现 Claude Code 能承担大量日常工作后，多数员工却仍停留在"cost-maxing"（省钱最大化）状态，"想象不出怎么会用到额度上限"。
