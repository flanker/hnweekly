---
layout: article
title: "什么是 Codemode"
issue: 813
number: 21
category: code
original_url: "https://lucumr.pocoo.org/2026/10/6/codemode/"
hn_url: "https://news.ycombinator.com/item?id=49978333"
date: 2026-10-09
---

## 文章摘要

作者是 Flask 的创造者 Armin Ronacher。一年多前他曾写文章劝大家别往上下文里塞自定义工具或 MCP 服务器，而是「多用脚本」，提出过「代码就是你需要的一切」。如今他参与开发的编码代理 Pi 发布 1.0，却通过一种叫 Codemode 的机制加入了 MCP 支持。这篇文章就是解释两者为何并不矛盾，以及 Codemode 到底是什么。

**为什么 bash 不够用。** Pi 一直偏爱 CLI 和 bash：命令容易组合，模型在训练中也学会了文件系统的规律（写了 `/tmp/test.txt`，之后那里就有这个文件）。但 bash 只能组合「能运行的程序」，而有些东西本质上是模型的原生工具，不是程序。比如读取图片：多模态模型不能用 `cat` 看图，必须由 harness（代理外壳）把图片数据按协议注入给模型；再比如生成和编排子代理，也很难绕开 harness 提供的工具。

**「大脑」与「手」。** Ronacher 把系统分成两半：harness 是「大脑」，运行在一台受信任的机器上；工具真正执行的地方（Pi 现在称为「执行环境」）是「手」，两者往往在同一台机器上，但文件系统和信任级别都不同。用 Gondolin 之类的沙箱，可以把 bash 隔离起来，但 harness 本身不在沙箱里。

**Codemode 是什么。** 它让模型在 harness 这一侧（而不是执行环境那一侧），用一门编程语言来表达和编排复杂操作——本质上就是「在 JavaScript 里发起工具调用」，这样多个调用的组合不必每一步都经过模型的上下文。名字来自 Cloudflare。Pi 的实现运行在 WASM 里的 QuickJS 中，刻意做了限制：没有网络、没有文件系统、没有定时器、内存有限，唯一能做的事就是调用更多工具。它带来几项好处：

- 普通 bash 工具调用只把末尾 2000 行塞回上下文，而通过 Codemode 调用时，大输出能以结构化形式交给脚本处理；
- 能用 Promise 表达并发和简单工作流。常见用法是先探查 5～10 条数据看清格式，再写脚本批量处理剩下的；
- 可以用 `store()` 把中间结果存进会话记录，下一次调用再取出来；
- 能调用那些不适合作为常规工具暴露的 Pi 内部能力，例如图像生成模型、单次分类模型。

文中展示了几段真实会话中由代理生成的代码：调用图像模型生成小狗图片；用 `gh` 拉取 100 个 GitHub issue，并发交给分类模型 Jev 判断情绪、沮丧程度和类型；甚至让 Jev 驱动一个坦克游戏跑 30 步循环来协助调试；以及直接调用 Sentry MCP 列出各组织的项目。Pi 不把 MCP 工具直接暴露给模型，而是让代理在 Codemode 里先做工具搜索，渐进式地发现能力。

**MCP 的现状与诉求。** 问题在于很多 MCP 服务器面向的是还不支持 Codemode 的 harness。Cloudflare 的权宜之计是在 MCP 服务器内部再做一层 Codemode，结果变成「Codemode 套 Codemode」：JSON 被双重转义，小模型容易犯迷糊，内层代码也调不到外层工具。Ronacher 对 MCP 生态提出四点期望：返回结构化内容（善用 outputSchema）；结果格式保持一致，别因为条数多少而改变输出（否则 5 条探查成功、满批量时却失败）；支持大体积二进制数据；以及支持可在多个服务器间组合的工具搜索。

**结论。** 他认为这并没有推翻一年前的观点——MCP 生态恰恰接受了「代码才是有效的方式」。但仍有未解决的问题：Codemode 的持久性（也许要借鉴持久化工作流引擎做快照，或者改用确定性更强的 Starlark）、图片和二进制数据的处理，以及它在小模型上效果不佳。

## HN 评论精华

讨论有相当一部分偏向「这篇文章没把 Codemode 讲清楚」，其余则围绕「为什么是 JavaScript 而不是 bash」以及小模型上的实际效果。

**文章到底讲清楚了没有**

- **troupo** 抱怨标题叫「什么是 Codemode」，正文却写着「如果你不熟悉 Codemode，它基本上就是……」，并引用那段关于 2000 行输出的话，表示完全看不懂。**lpedrosa** 说本以为 Armin 会从第一性原理讲起。作者 **the_mitsuhiko**（即 Ronacher）承认「这已经是我目前最好的尝试，这东西确实很难讲清」。troupo 随后列出了讨论串里好几个「一句话就讲明白」的解释，语气相当尖锐，甚至用「hands and brain」这类说法来嘲讽。
- **sqwxl** 推荐 Cloudflare 的原始介绍文章，troupo 引用其开头一句作为最好的定义：把 MCP 工具转成 TypeScript API，然后让 LLM 写代码调用这个 API。
- **agentdev001** 给出了较清楚的版本：模型本身无法组合它能调用的工具，不能说「把 get_email_id 的输出作为 get_email_metadata 的输入」；Codemode 就是让模型能以编程方式在 harness 层组合工具。**ohgodhelpplease** 的总结更短：给 harness 一个小小的 JS 沙箱，用来组合工具调用，并在结果进入上下文之前处理数据。
- **otikik** 用一个例子说明：有「列出客户」和「查询某客户销售额」两个工具，要汇总名字以 A 开头的客户的销售额。传统方式需要 1 + N 次工具调用，Codemode 只需发一段程序、一次调用。代价是执行方要运行 LLM 生成的不可信代码，必须有严格的沙箱。
- **planb** 和 **pjm331** 都说读到一半才明白，但 pjm331 认为一旦理解就很合理，自从接上第一个数据库 MCP 就一直想要这样的东西。

**为什么用 JavaScript 而不是 bash**

- **ylxdzsw** 做了一个用 bash 作为 Codemode 语言的代理原型，认为效果一样好，而且完全不需要额外提示。Ronacher 回应：模型针对 JS 版 Codemode 做过训练，所需指令更少，Promise 也很适合表达并发；更关键的是 Codemode 运行在「大脑」一侧，而 bash 必须在「手」那一侧运行，大脑和手在不同机器上时，让手回调大脑需要额外搭建通信层，LLM 对此理解得也很差。
- **andreypopp** 认为可以做一些会回调代理的 bash 命令，不觉得这有多「粗糙」；**anilgulecha** 则指出 bash 比 JS 或 Lua 更难做沙箱。
- **CuriouslyC** 提出一个有意思的观点：shell 是从左到右执行的，执行顺序和 token 生成顺序一致，而 Python/JS 的嵌套调用会乱序执行，更容易出错，所以 nushell 这类方向可能比把 JS 当 shell 更好。
- **ssivark** 从系统设计角度认为这是在重新发现同像性（homoiconicity）、Actor 语义和对象能力（object capabilities）这些几十年前的计算机科学原则，并引用 Dan Ingalls 的话：操作系统就是一堆塞不进语言里的东西的集合，本不该存在。**lukebuehler** 同意 harness 正在成为新的系统编程前沿。**nextaccountic** 则认为本质是上下文管理：MCP 调用可能把巨大的 JSON 塞进上下文，用脚本处理就能避免；但他担心 harness 之间耦合越深，锁定效应越强。
- **mijoharas** 补充了一个安全角度：他禁止 bash 访问网络，只通过沙箱外运行的特定工具访问指定服务，Codemode 在保持信任隔离的同时解决了组合问题。

**实际体验**

- **lemontheme** 在原型里用过又拆掉了：本地小模型确实能优雅地在代码里组合工具，但任何一个内嵌调用失败（如参数校验）都会导致整段代码失败，模型就会反复重写大段代码，token 用得反而更多。**hamandcheese** 说这印证了他的担忧：Codemode 在 Opus 和 Sol 这类大模型上效果极好，但不确定能否扩展到小模型。
- **Starlevel004** 观察到用 Codemode 越多，Sol 6.1 给子代理写的提示就越混乱，开始把单词黏在一起、堆砌关键词。
- **lionkor** 则说自己很喜欢它，交互轮数大约减少一个数量级。**dnlgrmm** 认为这是为了一点收益引入了复杂机制，Pi 原本的承诺就是「只要 bash」；**gf000** 举例说，判断 40 个文件哪些可以删时，可以写一个在「大脑」里运行、能直接调用 LLM 的脚本并行处理，而不必花 40×n 轮对话。
- **miguelspizza** 认为 Codemode 直接使用现成的 API SDK（如 Google Workspace）会比把 MCP 的 JSON Schema 转成类型再转回 JSON-RPC 好得多。
