---
layout: article
title: "AWS 收购 DuckLabs"
issue: 807
number: 57
category: startup_news
original_url: "https://ducklabs.com/news/2026/08/26/ducklabs-to-join-aws"
hn_url: "https://news.ycombinator.com/item?id=49448321"
date: 2026-08-28
---

## 文章摘要

> 核实说明：这里的 DuckLabs 确实就是 DuckDB 背后的那家公司——公告署名人是 Mark Raasveldt 和 Hannes Mühleisen，即 DuckDB 的两位创造者，公司位于阿姆斯特丹，从 CWI（荷兰国家数学与计算机科学研究中心）分拆而来。原文标题是《DuckLabs to Join AWS, Projects to Remain Open Source》（DuckLabs 加入 AWS，项目保持开源），HN 提交者把标题改成了「AWS Acquires DuckDB」，引发大量误解，后被版主 dang 修正。

2026 年 8 月 26 日，DuckLabs 宣布将加入亚马逊云科技（AWS），预计 9 月初生效。团队会整体留在阿姆斯特丹，继续做 DuckDB、DuckLake、Quack 以及更广泛的社区工作。

**为什么卖。** 公告用了相当大的篇幅讲这个决定的来龙去脉。五年多前创立 DuckLabs 时，DuckDB 正开始有起色，第一批「出资优先开发某功能」的商业合同开始出现，风投也在打电话。他们选了另一条路：完全由创始人和开发团队持股的自举公司（bootstrapped）。这个选择让他们能够耐心地建设、把技术放在第一位。团队从围绕一个野心勃勃的开源项目聚起来的一小撮人，长到阿姆斯特丹的 30 多人。

DuckDB 今天每天下载量超过一百万次。但两位创始人写道，现有模式的局限也越来越清楚：他们担心 DuckDB 的增长最终会超出自己的支撑能力，让这家小公司成为项目、团队和基于它做生意的人的瓶颈；同时他们也担心，把 DuckLabs 扩张成一个庞大的销售、支持和运营组织，会把注意力从当初让 DuckDB 成功的技术工作和开源社区上拉走。他们的合作模式在面对高度技术化的组织（往往是自身就有深厚数据库能力的公司）时效果最好，但要服务更广泛的用户群，需要解决更完整、更专门化的问题，服务不同行业的需求，在基础设施上投入更多，还要触及那些根本不会想到去主动寻找一个分析型数据库的人。他们相信「DuckDB 革命」还能再涨两个数量级，为此需要一套不同的组织架构。

**为什么是 AWS。** 公告说双方已经紧密合作了一年多（AWS 方的表述是约两年），这段经历让他们对下一步有信心。AWS 的 Andy Warfield（杰出工程师兼副总裁）的引语说 DuckDB「今天已被 S3 客户广泛使用且深受喜爱」，并称 DuckLabs 是他共事过的技术最深、最谦逊、速度最快的团队之一。

**什么不变。** 这是外界最关心的部分，公告写得很明确：DuckDB、DuckLake、Quack 及 Duck Stack 的其他开源组件将继续以 MIT 许可免费开源；非营利的 **DuckDB Foundation** 继续托管这些项目的治理；DuckLabs 团队继续贡献代码并整体留在阿姆斯特丹。CWI 在基金会的代表、数据库架构组负责人 Peter Boncz 的引语点明了产权关系：**当年 DuckLabs 从 CWI 分拆出来时就成立了这个基金会，开源 DuckDB 的全部 IP 归基金会所有，今后依然如此。**

**什么会扩展。** 未来 DuckDB Foundation 会增设**技术顾问委员会**（technical advisory board），让主要社区成员对项目技术方向提供输入；同时他们计划**开放扩展栈**，让由其他开发者和组织签名的扩展也能在 DuckDB 中运行——这条对社区扩展生态的意义不小。

公告还收录了几条背书：MotherDuck CEO Jordan Tigani 说「亚马逊押注 DuckDB 会带来大量动能、强化生态」；Fivetran CEO George Fraser 说「亚马逊是 DuckLabs 的理想归宿，DuckDB 是厂商中立开放数据栈的中心」；图宾根大学教授 Torsten Grust 从教学和研究角度强调 DuckDB 内核可被检视和修改的重要性。另据 AWS CTO Werner Vogels 的博客补充，**DuckLabs 是作为 AWS 的子公司（subsidiary）存在，而不是被并入 AWS 内部。**

## HN 评论精华

这条帖子拿到 1100 分，是当周社区情绪最强烈的一条。整体基调是**「为创始人高兴，为项目担心」**——祝贺和哀悼交织，几乎没有中间地带。

**先是关于「到底收购了什么」的澄清，这是全帖最重要的一条线：**

- **daviewales** 指出标题有误导性：AWS 收购的是 DuckLabs，不是 DuckDB；DuckDB 源码仍归非营利的 DuckDB Foundation 所有，并引用了 Peter Boncz 那段关于基金会持有全部 IP 的原文。**dang**（版主）随后回复「已修正，谢谢」。**Scubabear68** 说他第一次看到标题时着实吓了一跳。
- **aratob** 补充了完整的三方关系：AWS 没有拿到 MIT 许可的 DuckDB 技术本身；他们拿到的是 DuckLabs——那家位于阿姆斯特丹、由创造者持股并雇佣主要贡献者的服务与开发公司；而 MotherDuck 是一家美国的风投支持公司，云数据平台以 DuckDB 为中心，近期还扩展了 Python 流水线、agentic 上下文层和可视化层。
- **saxomoose** 提出了一个尖锐的财务问题：如果 IP（主要资产）不在交易范围内，这笔交易的对价是怎么算的？纯粹的 acqui-hire 吗？他随后自己找到了答案：Werner Vogels 的博客说「DuckLabs 将作为子公司加入 AWS」。**otterley** 也贴了同一链接。
- **jonnat** 把这个问题推到了最锋利的地方：「没人清楚地说明 AWS 从这里得到了什么。他们没拿到 DuckDB 的 IP。DuckLabs 团队为发展 DuckDB 和 DuckLake 所做的一切工作，原则上本来就在转化成 AWS 的算力和存储需求。那么 AWS 究竟想要什么，又会怎样改变 DuckDB 来达成它？」
- **puszczyk** 问 MotherDuck 和 DuckLabs 什么关系。**batdata** 现身表示自己来自 MotherDuck：「我们从第一天起就和 DuckLabs 团队紧密合作，这个合作会继续下去」，并贴了官方博客。**skeeter2020** 则给出更悲观的读法：AWS 要么很快也收购 MotherDuck，要么（更可能）留着它作为「我们绝不会掐死社区」的门面。

**担忧派（声量最大）：**

- **hobofan** 的话被反复引用：「在所有大公司里，亚马逊大概是对『让技术上有趣的项目活下去』最不上心的那个，下一次重组时他们一定会因为某个愚蠢的理由把它推平。」**wavemode** 反驳说他的印象恰恰相反，AWS 的项目往往能活很久，虽然收购的开源项目不多但据他所知都还在跑。**ChuckMcM** 认为「最不上心」这个头衔应该归甲骨文，**shevy-java** 和 **fullstop** 则把矛头指向 Google 和 killedbygoogle.com。**LoganDark** 说他至今为 Cloud9 被砍而耿耿于怀。
- **thataccount** 一句话预言了很多人的恐惧：「再见 DuckDB，你好突然冒出来的、带着你离不开的特殊功能、只在企业版上跑的 Enterprise DuckDB。」**luca4** 列了前车之鉴清单：Terraform、Docker、Elasticsearch、InfluxDB，「『我们还是好人，相信我们，什么都不会变』——然后就会被 fork 并用 Rust 重写」。**igtztorrero** 更简短：历史会重演，就像 MySQL 被 Oracle 收购之后停滞。
- **jknoepfler** 给出了最理性的框架：「这会是对 DuckDB 所选开源模式的一次很好的检验。据我理解 DuckDB 是 MIT 许可、由独立的非营利基金会治理，这个安排就是为了防止 BSL 化。我很好奇这条防线守不守得住。我不会押注它守得住，但可以抱点希望。」**shimman** 直接泼冷水：「当核心成员被一家上市公司持有时，基金会又有什么用？这个模式一点都不新鲜，企业利益永远优先于社区利益。」
- **flakiness** 做了点侦查工作：duckdb.org/roadmap 的 Google 缓存里还有一句「基金会与 DuckLabs 不接受外部投资者（如风投）资助」，但页面标注「最后更新 2026 年 8 月」，现在这句话已经找不到了。他猜是因为归属变了所以措辞需要重写。**tyre** 附和：「大概是亚马逊说『先撤下来，我们的律师会批一版新说法』。」
- **cmiles8** 从人的角度担心：「为创始人高兴，但说实话为团队难过。这一年从 AWS 内部传出来的消息不太好，那地方听起来一团乱，我 LinkedIn 上顶尖人才一直在往外走。希望他们让这个团队照旧运转，别被外面那些乱七八糟的东西污染。」**wpietri** 说得更动情：他去年开始用 DuckDB 就爱上了它——小、专注、扎实、不废话，「我喜欢它的地方很大程度上来自它是由一小群不关心『最大化股东价值』（即最大化高管的晋升和奖金）的技术人做的。也许这次会是罕见的例外，创始人熬过公司政治继续做自己的事。但我很沮丧，因为这种乐观我听过太多次，见到成真的次数太少」。**rklaehn** 反驳：他们做了了不起的东西并有了一次不错的退出，代码留在非营利基金会下开源，「我不觉得这有多糟。如果工作环境太差，人们直接辞职去为基金会工作就是了」。
- **janpeuker** 和 **skeeter2020** 从技术方向上表达不安，两人都注意到 DuckDB 2.0 的预告：「DuckDB 从第一天起就是进程内数据库。但人们一直非常执着地要求客户端/服务器模式，我们终于妥协了。」skeeter2020 说这个开场白让他很不舒服——这个需求已经被大量项目和产品满足了，看起来像是为了抓住 AI 工作流而「跟风」；「DuckDB 一直感觉是『分析数据版的 SQLite』，但这些变化加上现在收购主导技术方向的组织，恐怕是永久性的偏离。AWS 对 DuckDB 该成为的样子完全不必要，MongoDB 就是前车之鉴」。
- 一批人给出了纯粹的情绪反应：**simlevesque** 说他在会议上脱口喊出了「Oh shit！」；**dmoose** 说他上周还在聊「Duck 和 Kagi 是我最希望永远别被收购的两个东西」，结果就来了；**Boxxed** 问「我们能不能别再把所有东西并到少数几家巨头手里了」，**oblio** 回应「这会先变得更糟再变好，所有超大规模云商都会不可避免地滑向完全垂直整合，希望他们别走到『拥有私人武装』那一步」。**nightish** 感叹：「还记得大公司只是给开源项目捐款帮忙，而不是收购和主导它们的时候吗？」

**乐观派 / 商业逻辑分析：**

- **georgewfraser**（Fivetran CEO，公告里也有他的引语）给出了最正面的论证：「AWS 是 DuckLabs 的好归宿。他们只想让人用更多算力和存储，所以有一定程度的技术中立性。这正是让 DuckDB 能沿着自然方向继续生长、而不被某种围墙花园式的数据平台战略扭曲的关键。」
- **karakanb** 说他意外地感到惊喜：AWS 多年来在数据领域一直是弱势玩家、落后于所有其他数仓厂商，这次可能带来 DuckDB 与 S3 上海量数据的有趣整合，做出比 Athena 更精简、更快、更现代的替代品。**pepperoni_pizza** 补充说 Athena 已经有 Trino 和 Spark 引擎，完全可以再加一个 DuckDB。**smithclay** 希望看到 S3 Tables 支持在 DuckLake 和 Iceberg 之间选择目录（Cloudflare 的 R2 Data Catalog 也走对了同一条路）。**pantsforbirds** 想要 RDS + S3 版的 DuckLake，**gigatexal** 猜这会成为 Redshift 的新引擎。
- **jakozaur** 给出历史模式：AWS 很少收购创业公司，但「利用别人的技术」是它的典型套路——它曾授权 ParAccel 做成 Redshift，Athena 基于 Trino（Presto 的分支）。在 Snowflake 和 Databricks 于智能时代大赚特赚的当下，把 DuckLabs 收入囊中是抢份额的方式。**senderista** 补了一例：Blazegraph 被重新包装成 Neptune，而原项目在收购后被关停。
- **tyre** 对「创始人财富自由」的说法泼冷水：开源公司值不了那么多钱，走到风投级盈利能力极其罕见。他猜 AWS 是想要官方托管版本，同时不想再经历 Redis 那样的许可证风波——上次的结果是自己养 Valkey，那还不如直接把团队买下来。「DuckDB 团队大概拿到了不错的一揽子待遇和涨薪，但要说拿到几亿美元不太可能。」**echelon** 也说没提价格通常就意味着不是「代际财富」。**fidotron** 加了一句：「也是欧洲人，所以几乎肯定把自己卖便宜了。」
- **pkilgore** 一句话总结了交易结构的巧妙：「用这一个小技巧规避反垄断审查。」

**技术替代与 fork 讨论：**

- **rustyconover** 在多处推销自己的成果：他已经做了 Haybarn（query.farm/haybarn），一个 DuckDB 发行版，从 1.5.3 起持续发布所有版本和全部社区扩展；他还提到 2.0 计划支持用自己的密钥签名扩展并自建扩展仓库，而 1.5 版的 Haybarn 已有自己的签名密钥、发布扩展的速度快得多。**d0100** 顺势问了个实际问题（他们目前允许加载未签名扩展然后锁死配置）。
- **tormeh** 推荐 Apache DataFusion：设计成库但也能独立使用，有 CLI、Python 和 Java 绑定以及 Rust 库，「从我的经验看它比 DuckDB 更好地集成进 Rust 应用」，月活贡献者超过 100 人。**hobofan** 给出中肯的反驳：DataFusion 有各个零件（尤其只做数据分析时），但离嵌入式数据库还远——索引、事务、一等公民的存储格式，这些 DuckDB 自带，DataFusion 没有。**tobilg** 提到 DataFusion 也能用 DuckLake。**willbeddow**（Krea 的研究团队大量使用并扩展了 DuckDB 和 DuckLake）说他相信 DuckDB 开创的开放嵌入式查询引擎路线大于任何单个项目，他对更广的生态（尤其是 DataFusion）仍然乐观，但也直言「AWS 显然把这当成对抗 Databricks 的产品套件里的又一块拼图」。
- **adsharma** 提出了一个少见但重要的角度：为什么一个广受好评的 MIT 项目还需要「源码发行版」？因为如果你不在 DuckLabs 工作，给 DuckDB 贡献代码并不容易——他上次看时，一个简单的 bug 修复要跑 5 小时 CI。他列举了 Haybarn 和 Pygmy-Goose 两个发行版，后者专注于让 agentic 工作流更快（拆分仓库、让 git worktree 变便宜、5 分钟缓存 CI）。他还指出好几个「基于 DuckDB 做图数据库」的努力最后都重新造了一套列式代码库，KuzuDB 团队 2022 年做过 GRainDB，2023 年决定自己写（现在叫 LadybugDB），缺乏对外部贡献者友好的流程很可能是原因之一。
- **alexwennerberg** 给出一条从治理角度出发的反思：「我很庆幸自己坚持用了 SQLite。治理是任何开源项目的重要组成部分。SQLite 保持了范围窄而专注，而 DuckDB 的哲学是做越来越多，把自己变成一个通用的数据科学工具。我很老派地相信『做一件事并做好』。」
- **m1keil** 开玩笑说「等着 QuackDB 开源分支随时诞生」，**Keyframe** 提议叫 MallarDB（绿头鸭），**Bluestein** 贡献了全帖最佳双关：「Embrace, extend, quack.」（拥抱、扩展、嘎嘎叫——套用微软的 EEE 战略）。**log101** 则预言：「一个月后：《用 Claude Ultra 把 DuckDB 用 Rust 重写》。有个奇特现象：大厂拿优秀开源项目当小白鼠试验 agent 驱动的开发方式，参见 Bun、AstroJS。」
- **jefecoon** 提了个很实际的问题（帖中未见官方回复）：Quack、DuckLake、实时物化视图这些工作会不会继续全速推进？**dzonga** 说他最期待的正是实时物化视图。
- **0898** 谦逊地问了个基础问题：「数据库不是已经解决的问题了吗？为什么有这么多种？为什么有的快有的慢？」得到了一串耐心的好答案——**koolba** 说性能、访问模式、吞吐、延迟、并发、工作负载、严谨性、类型系统、可扩展性，每一项都有取舍，技术的每一次迭代不只是修正过去的错误，而是解决当下的问题；**toast0** 类比轮式交通工具和桥梁的种类；**fishtoaster** 用 Postgres 做「全能数据库」和大规模分析场景的对比展开解释；**xnx** 直白概括：DuckDB 是处理「笔记本电脑规模的表格型分析数据」的最佳工具。**phainopepla2** 回答另一位提问者：DuckDB 是 OLAP（分析），常规事务还是用 SQLite。
