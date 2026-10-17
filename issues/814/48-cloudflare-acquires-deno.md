---
layout: article
title: "Cloudflare 收购 Deno"
issue: 814
number: 48
category: startup_news
original_url: "https://deno.com/blog/cloudflare"
hn_url: "https://news.ycombinator.com/item?id=50019911"
date: 2026-10-16
---

## 文章摘要

10 月 9 日，Node.js 和 Deno 的创造者 Ryan Dahl 在 Deno 官方博客发文《Deno 加入 Cloudflare》，宣布 Deno 整个团队加入 Cloudflare。同一天，Cloudflare 博客也发布了由 Ryan Dahl 和 Workers 技术负责人 Kenton Varda 联合署名的文章。本文综合两篇公告整理。

**Deno 这边：从运行时到 celld。** Ryan 回顾说，多年来团队一直在尝试让服务器软件更简单：重新思考模块如何分发、JavaScript 运行时能提供怎样的安全保证、一套完整的工具链应包含什么、如何把应用打包成独立可执行文件。后来兼容 Node.js 也成了重要工作，因为用户希望既享受 Deno 的改进，又能接入现有的 JS 生态。但他的野心一直不止于运行时：早在《JavaScript Containers》一文里他就设想过计算、存储和通信协同工作，让每个应用不必各自拼装基础设施。Deno Deploy 朝这个方向迈了一步，但构建和运营它也让他看到开发者体验之下仍有大量复杂性。于是有了 celld：它基于 Cloudflare Workers 的编程模型，让开发者从一开始就构建分布式应用，同时系统本身易于运维，扩展能力内建在编程模型里，而不是每个应用自己搭的基础设施。在他看来，从 Deno 到 Deno Deploy 再到 celld 的演进，正好解释了为什么要加入 Cloudflare：团队将与 Workers 和 Durable Objects 团队合并工作，目标是让这种编程模型成为构建服务器的默认方式，无论跑在 Cloudflare 网络上还是自有基础设施上。

**代价：Deno 运行时和 Deno Deploy 将停止。** 加入 Cloudflare 也意味着选择重心：团队今后的开发将投入这个共享平台，不再继续开发独立的运行时和托管服务。Ryan 承认这对基于 Deno 构建产品的人是重大变化，并给出具体安排：Deno 运行时再支持一年，每月发布包含 bug 修复和安全更新的版本，之后停止开发，但 Deno 仍然开源，欢迎其他人接手；Deno Deploy 继续运营六个月后关闭，付费客户迁移到 Cloudflare Workers 时会获得迁移支持；包注册中心 JSR 继续运营，基础设施迁到 Cloudflare；继续维护 rusty_v8，并推进把它集成进 workerd。他还特别提到 AI：Durable Objects 集廉价的无服务器执行、持久状态、WebSocket 和高层 JavaScript 接口于一体，特别适合做智能体框架，这是他最想探索的方向，也是 celld 聚焦 Durable Objects 的原因。

**Cloudflare 这边：Ryan 的叙述。** 在联合博文里，Ryan 从当年在柏林一间仓库里用 500 行 JavaScript 写 IRC 服务器、现场观众参与演示 Node.js 讲起：Node.js 探索的是纯异步编程模型能带来什么，那个演示之所以激动人心，是因为代码少得惊人。他创建 Deno 是想找到更多这样强大的抽象，但 Deno 改善了写 JavaScript 的体验，却没有从根本上改变开发者需要在运行时周围拼装的东西，更大的问题在于网络应用：分布式计算、状态协调、数据存储和按需自动扩缩。那个 IRC 演示一直让他耿耿于怀：单机、单线程，用户多了就会变慢，真正扩展需要多台机器共享频道，还要存消息历史和用户账号。编程模型能否从一开始就把跨机器分区纳入设计？读到 Cloudflare 的 Durable Objects 时他豁然开朗：它是"带 SQLite 数据库的分布式单例"，每个 DO 就像一台可单独寻址、带关系数据库的小服务器，JavaScript 单线程执行、易于推理，能处理 WebSocket，本地 SQLite 可同步访问。用一个 DO 对应一个聊天频道，数据和连接就自然分片了。在此之上还能构建队列、KV、持久化执行乃至 Git 存储。问题是在 Cloudflare 之外运行这套模型一直很难：workerd 虽已开源，但其 Durable Objects 只支持单实例。他想要的是分布式版本，于是开始做 celld：一个用 Rust 写的单一二进制，唯一的外部依赖是对象存储，运维时只需管理一组 celld 实例和一个存储桶。Deno 团队加入 Cloudflare，就是要把 celld 和 workerd 合在一起，让在自有基础设施上构建和运维分布式应用变得极其简单。

**Kenton Varda 的叙述：锁定论是错的。** Kenton 说，Deno 团队今年 8 月发布 celld 时，Cloudflare 很高兴，这可能让一些人意外。网上流行一种说法：Cloudflare 故意让 Workers 与其他云平台不同，好制造"锁定"，尤其 Durable Objects 不仅写法不同、架构也不同，一旦陷得太深就走不掉。按这种理论，作为 Workers 和 DO 的开源实现，celld 是威胁。他认为这种理论是错的：Workers 不同是因为它更好，它让一个应用以极低的成本跑在全球数百个地点，定价不是噱头，而是架构确实更高效；"绑定"式的环境设计让访问外部资源既方便又安全；Durable Objects 让实时协作和分布式系统变得容易。要更好就得不同，如果能既有这些好处又完全兼容传统平台，他们求之不得。他说锁定其实伤害 Cloudflare：2022 年向 Shopify 等客户推销 Workers for Platforms 时，对方明确表示除非运行时开源否则不可能采用，于是他们开源了 workerd，而且就是生产环境跑的那份代码。确实有前客户借助 workerd 迁走了，但没有开源他们根本不会签约，而每走一个就有很多留下来，"开源、给人逃生舱，是好生意"。他也承认 workerd 一直有个大缺口：Durable Objects 只能单实例运行，代码里从首个版本起就有相关的 TODO 注释；Cloudflare 生产环境的 DO 路由实现是一头"巨兽"，依赖大量由 SRE 团队运维的外部服务，不是自托管用户想要的东西，他们一直没抽出时间重做（他去年春天试过一次，"尴尬的是没成功"）。celld 正是他们想做而没做的：与 Cloudflare 完全兼容、又专注于可自托管和可扩展。Ryan 是 Node.js 的创造者，没有他也许就没有 Workers，Deno 团队又懂得如何围绕自托管运行时打造开发者体验，这恰恰是 workerd 一直没做好的。计划是由 Ryan 和 Bert Belder 领导新项目，让 workerd 自托管成为一等公民，把 celld 的代码和理念合并回 workerd；Kenton 说他自己会用它在家里托管一个 Cloudflare OS 实例。

## HN 评论精华

帖子拿到 1389 分、700 多条评论，是本周最热的讨论之一。标题说的是"加入"，但评论区几乎一致认为这是一次"收购人才、关停产品"的 acquihire，焦点集中在 Deno 运行时一年后停止开发这件事上。

**"埋在最后的关键细节"。** 最长的一串讨论由 theodorejb 开启，他直接引用了"运行时再支持一年后停止开发"那段，指出除非有人接手，Deno 将不再有人维护。simonw 不满这一点写在 Deno 的公告里，Cloudflare 的公告却只字不提；rancar2 则认为两篇文章面向不同读者，Cloudflare 那篇开头就让读者去看 Deno 博客。binlog 说这么重要的细节埋在文末太离谱，不少公司近年全面押注 Deno，有的甚至从 Node.js 迁了过去，"这就是追新而不守旧的风险"。coldtea 反问：押注一个牵引力只有 Node 百分之一的小众玩家，现在才惊讶？maherbeg 和 chaosharmonic 提到 Slack 的插件 API、Netlify 和 Supabase 的边缘函数都曾基于 Deno（aantti 补充说 Supabase 边缘函数用的是自己基于旧版 Deno 的 edge-runtime）。也有不少人认为迁移成本不高，redox99 说一天就能迁完，matesz 说 Deno 提供的功能 Node 基本都有；flohofwoe 则说 Node 至今做不到 Deno 那样在单个脚本里直接 `import ... from "npm:bla@^5"`，这对非 JS 项目里的工具脚本是杀手级特性。

**Ryan Dahl 亲自回应。** mark_and_sweep 问放弃运行时是谁的决定。Ryan（账号 rough-sea）回答说这是双方共同的决定，他也同意：他是最在乎 Deno 成功、投入最多的人，但他不再认为这里是他能做最重要工作的地方。Deno 有一些好想法、工程质量也好，但最终并没有在解决大问题；它被"Node 兼容性的引力井"吸了进去，被迫和 Node 行为完全一致，"既然 Node 能用，为什么要重新实现 Node？边际的性能、体验或安全收益是不够的"。他想做的是强大的新抽象，celld 只依赖对象存储来协调和持久化，运行得非常好，它不是换个 API 访问文件系统和网络，而是一种全新的服务器开发模型。Deno 将保持 MIT 许可，他希望有人接着往前走。jflatow 追问既然如此为什么当初要追 Node 兼容，并表示愿意出力让项目延续；NetOpWibby 说他要做一个去掉 Node 兼容的分叉，后来贴出了 ende.sh。Ryan 在另一处补充，计划是把 celld 的模式带进 workerd：一组 workerd 实例用一个存储桶做协调和存储，建立在 workerd 上意味着精力不分散，Cloudflare 上和自托管的 API 完全一致。

**Kenton Varda 的辩护。** kentonv 在评论区多次请大家读 Cloudflare 那篇博文："它不只是公关稿"。tancop 认为收购一家公司并明确打算关掉它的主产品"非常可疑"，Kenton 回应说他理解为什么看起来像这样，但事实只是 Deno 团队做出了转向 celld 的战略决定，Cloudflare 乐于支持。yencabulator 引《权力的游戏》说"言语如风"，认为 workerd 离生产可用的 Workers 替代品还很远，Cloudflare 敢在自己平台上运行它的关键部分并没有开源；Kenton 回答，那部分主要是防御恶意租户的多租户隔离，本来就是一套需要主动管理的流程，没有哪个流行运行时会包含它，自托管自己的应用并不需要，托管不可信租户可以把 workerd 放进微虚拟机。Wasmer 创始人 syrusakbary 担心独立的 Node 运行时越来越少（Bun 被 Anthropic 收购，Deno 不再开发），认为 workerd 推动的语义只能跑在 Cloudflare 上，比如 D1、KV、Queues 都离不开 Cloudflare，并顺带推荐了自家的 Edge.js；Kenton 回应说项目目标之一就是让这些接口可插拔，并提供能跑在通用基础设施上的参考实现，celld 已经做了很多，还说有人在完全不用 Cloudflare 的情况下在生产环境跑 workerd，"其中一个用户的身份非常讽刺"，但不便透露。pjjpo 说 Kenton 那篇博文是他读过最尴尬的文章之一，Kenton 问哪里尴尬，对方说太像营销而非技术；Kenton 回应说那本来就是讲商业策略的文章，这个策略正是他力主并争取到支持的，他以为一篇明确支持开源、反对锁定的文章会让技术读者高兴。

**celld 才是真正的标的。** ttul 认为这次收购与开源项目 Deno 无关，买的是一支做出了"不烂的自托管 Workers"的团队，本质是"把互补品商品化"。vazark 说 Cloudflare 收购的是 celld，Deno 只是作为附带品被体面地安葬了。ryanrasti 说比起 Deno 停更，更有意思的是 celld 模型并入 workerd：完全依靠对象存储来实现持久化和协调，大大简化了自托管，而 S3 协议已经普及到可以自己部署。k9294 和 networked 则担心这是为了尽早扼杀潜在竞争者，celld 是目前几乎唯一的 Durable Objects 替代品，有人随即列出 Phoenix 团队的 durable_server、Rivet 等方案。

**对开源和 VC 模式的反思。** sholladay 说他因为 Ry 最初的愿景在 Deno 生态投入很多，但从 Deno 把 npm 兼容列为优先事项起就预感到今天，表面积从优雅简洁变得臃肿，"他们顶不住 VC 的压力，放弃了从第一性原理重建 Node"；好在早期 Deno 好到 Node 抄了它的一些特性。前员工 carefulfungi 不同意，认为逼 Deno 走向兼容的是 Bun 凭 npm 兼容带来的采用率，而正是 VC 资金让慢慢来的策略成为可能。pseudosavant 和 zamadatix 总结说，Deno 对 Node 社区是成功的：ES 模块、fetch、TS 支持、Web Streams、内置测试、监听模式等后来都被 Node 吸收。Jgrubb 认为这种"抽地毯"是开源之死，ForHackernews、locknitpicker 等则反问那些押注 Deno 的公司为什么不出钱资助自己依赖的基础设施。miki123211 说 Deno 像个"人才池"，真正目的是吸引顶尖工程师，再像足球运动员一样"卖给"别的公司，Keybase、Astral、Bun 都是例子；senko 反驳说用过 ruff 和 uv 的人不会认为 Astral 的产品是次要的。benrutter 问买下公司再关掉主产品是否违反美国反垄断法，WorldMaker 说美国反垄断法几乎只管托拉斯和垄断，相关执法机构目前也被削弱。

**JS 运行时该怎么选。** gen2brain 的困惑很有代表性："一个运行时花了大量 token 把 Zig 项目重写，一个本来就是 Rust，一个被收购，还有一个用 Go 重写，到底怎么回事？"（jerf 指出被 Go 重写的是 TypeScript 编译器，与运行时无关。）多数回答是：用 Node，它像 Java 一样会被永远支持。bel8 提醒 Node 的 TS 支持只是剥离类型，不支持 enum 等语法；flohofwoe 补充说 `deno run` 其实也只剥离类型。围绕 Bun 也有一番争论：有人因为 Anthropic 收购、或因其用 AI 大规模从 Zig 改写为 Rust 而弃用，也有人认为改写后的 Bun 性能更好、迭代更快。sixdimensional 列出了近期开发者工具被收购的清单：Cursor 归 SpaceX、Astral 归 OpenAI、Bun 和 Stainless 归 Anthropic、Astro、Deno 和 VoidZero 归 Cloudflare、NuxtLabs 归 Vercel、Hugging Face 归 NVIDIA，评论里又补了一长串。还有人关心 rusty_v8、JSR 和标准库的去向，以及 Deno 当年为解放"JavaScript"商标发起的诉讼会不会就此搁置。
