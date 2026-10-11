---
layout: article
title: "Supabase 收购 Turso"
issue: 813
number: 55
category: startup_news
original_url: "https://supabase.com/blog/supabase-is-acquiring-turso"
hn_url: "https://news.ycombinator.com/item?id=49934784"
date: 2026-10-09
---

## 文章摘要

2026 年 10 月 2 日，托管 Postgres 平台 Supabase 的 CEO、联合创始人 Paul Copplestone 在官方博客宣布收购 Turso。Turso 是用 Rust 从头重写 SQLite 的公司，同时运营一个面向海量小型数据库的云服务。同一天 Turso 创始人 Glauber Costa 也在自家博客发了一篇《Turso 加入 Supabase，让每个智能体都有自己的数据库》，两篇合起来能看清这笔交易的逻辑。交易金额两边都没有公布。

**收购的理由是“智能体需要的数据库”。** Copplestone 开篇就说，AI 让构建者能写出海量软件，智能体正在为它们做的原型、探索、仪表盘和应用开出数以百万计的数据库。按他的说法，Supabase 现在每周新开的数据库已经超过一百万个，他们判断随着智能体写的软件越来越多，数据库需求会超出现有基础设施的承载能力。在他看来，智能体创建数据库应该像创建文件一样容易，也几乎不用考虑成本；小型负载不该每次都去开一台专用机器，数据库应当创建便宜、随用随取，需要时又有一条通往生产环境的清晰路径。他的分工是：SQLite 适合这种小而按需的负载，应用规模变大后该用 Postgres，而 Supabase 想让开发者从原型到生产都拥有同一套开发体验。

**Turso 带来了什么。** 按 Supabase 的介绍，Turso 用 Rust 重建了 SQLite，并做出一个云平台：单台服务器就能管理数百万个数据库，需要时加载，不用时挂起。这样就能按需给每个智能体开一个数据库，可以开在 Turso Cloud 上，也可以开在客户自己的云里，文中点名的客户有 Superhuman、Sauna.ai、CTO.new 和 Mastra。Costa 在 Turso 那篇文章里补充了技术背景：SQLite 小、快、到处都能跑，但从来不是为现代服务端负载设计的，单写者限制一直拖着它；Turso 以开源方式从头重写，解除了并发写入等核心限制，云服务采用无本地磁盘、WAL 存放在 S3 上的架构，所以能以远低于传统方案的成本按智能体开库，也支持 BYOC（部署在客户自己的云账号里）。

**对现有用户的承诺。** 两篇文章都强调对现有用户“什么都不变”：Supabase 继续围绕 Postgres 发展，Turso 继续做 SQLite；Turso 平台照常运行，数据库、API 和工作流都不受影响；Turso Database 继续开源并积极开发（项目是 MIT 许可）。Turso 那篇文章还给出了一条“毕业路径”：SQLite 不够用时迁到 Supabase 生态里的标准 Postgres，再往上可以用 Supabase 的分片方案 Multigres 扩到 PB 级；未来几个月两边会有更深的集成。人事上，Glauber Costa 以“智能体服务负责人”（Head of Agentic Services）的身份加入 Supabase，主导这条智能体基础设施线，联合创始人 Pekka Enberg 和团队其他人一起加入。Costa 说他从一开始的目标就是为客户服务十亿个数据库，加入 Supabase 让这个目标近了一大步。Copplestone 那篇的结尾把这次收购放在公司使命下：Supabase 要“存储世界的数据”，而智能体会制造多得多的数据。

## HN 评论精华

本帖 221 分、约 110 条评论。讨论有三条主线：Turso 用户担心被收购后项目夭折，Turso 的技术成熟度（尤其是 ClickBench 上的糟糕记录）受到质疑，以及“为什么要重写 SQLite”的老话题。Turso CEO glommer（Glauber Costa）、联合创始人 penberg（Pekka Enberg）和几位 Supabase 员工都在评论区回应。

**用户的担忧，以及创始人的回应。** kilroy123 直接说“作为 Turso 客户，我不想要这个”，drewbitt 只回了一长串“Nooooo”。brainless 说他真心希望 SQLite（以及 Turso 的重写版）能在 LLM 催生的大量小应用里胜出，猜测是 Turso 的托管业务收入不够才卖掉的。glommer 两次出来澄清：“我们经营得很好，这是一次战略收购，我爱上了 Supabase 对 Turso 未来用法的设想。”brainless 坦言这种表态反而让人更难选 Turso，因为母公司即使本意良好也常常砍掉被收购的产品。glommer 回应说，确实有收购会失败，但大多数失败的收购一开始就注定了，本质是创始人套现或人才收购；他手里有数不清的选择，Turso 今年收入增长超过 5 倍，没理由接受一个成功希望不大的报价。JoshTriplett 很喜欢 Turso 这项技术，既把它当成更好的 SQLite，也看好它将来成为可嵌入、兼容 Postgres 的数据库，所以“真心希望它别变成又一个‘不可思议的旅程’”（这是对创业公司被收购后关停时常用措辞的调侃）。theLiminator 说他见过太多技术被收购扼杀，举了 pg_mooncake 为例。vmg12 则正好相反：他原本担心 Turso 这个开源项目的命运绑在 Turso 公司的成败上，好几次因此选了 SQLite，现在反而会更放心地选 Turso。

**Supabase 打算怎么用 Turso。** Supabase 员工 saltcod 说，他们把 Turso/SQLite 视为 Postgres 在特定场景下的补充，博客里还没提，但 Turso 将支撑 Supabase 最小的实例规格“Spark”。kakugawa 把路径概括为：先自托管 Turso，平滑升级到 Turso 托管云，再到 Supabase 的托管 Postgres，何况 Turso 还在实验 Postgres 前端；saltcod 确认“这正是计划”，parthdesai 补了一句“然后希望再到 Multigres 分片 Postgres”。lukasschwab 认为，对 Supabase 上绝大多数项目来说，SQLite/Turso 的开发体验可能都比 Postgres 好，尤其是本地测试和临时环境测试。sarkarghya 注意到这一领域的收购正在扎堆，此前 Databricks（它已收购 Neon）买下了做 PGlite 的 Electric。

**ClickBench 上的尴尬记录。** f311a 希望收购后 Turso 能投入更多资源修 bug，他翻出 ClickHouse 的 ClickBench 基准测试仓库里几次添加 Turso 的尝试，每次都撞上新 bug，“不该比 SQLite 慢 X 倍”。menaerus 把那些 issue 里的原话贴了出来：数据加载了一周，速度掉到每秒 4KB，加载完要好几年；一年后仍然没加载完，去掉 fsync 也不行。他说自己笑得有点过分，又问 Supabase 为什么要买一个连数据集都加载不进去的产品。gpm 提醒那是项目早期的事，menaerus 反驳说项目第一个提交在 2023 年 8 月，而最新一次 PR 显示性能问题仍未解决。dangoodmanUT 引用 PR 标题“加入 Turso（慢得令人难以置信）（而且跑不起来）”，调侃 ClickHouse 的 Alexey 本性难移。penberg 回应说他们清楚 ClickBench 的问题，第一次被加进去时他就明确说过太早了；最近那次尝试是公平的，但问题全在跑基准之前的数据导入阶段，所以团队把精力优先放在了 0.8 版本（即解除单写者瓶颈的并发写入）上，导入性能在计划内但不是最高优先级。penberg 的回复还暗示 f311a 就是 Alexey 本人，rendaw 和 hitekker 追问依据，hitekker 还指出 penberg 的回复原本带脏话后来删掉了，f311a 则声明自己和 ClickHouse 毫无关系。

**为什么要重写 SQLite？** 这是另一条热闹的支线。aniceperson 质疑 vmg12：你真要放着久经考验的 SQLite 不用，去用一个“vibe coding 出来的项目”？glommer 回击说 Turso 早在 vibe coding 出现之前就开始了，如今他们像大多数人一样用 AI，但所有代码合入前都由人审查，“说 Turso 是 vibe coding 项目非常不厚道”。vmg12 说他要的是 SQLite 没有、也永远不会有的功能，具体是多个并发写者和更好的搜索扩展。arnath 和 dd8601fn 问：为什么要重写一个公认测试最充分、管理最好、部署在几乎所有设备上的软件，图个名声吗？回答有几类：levkk 说这就是推动领域进步的方式，“好还不够”；duped 说 Turso 本质上是一个与 SQLite 兼容（文件格式和 C API 都相同）的嵌入式数据库，需要异步 I/O、并发写入和本地副本时就是好选择；JoshTriplett 列出他心目中“更好的 SQLite”：更多异步、更多并发、更多 SQL 方言、更好的默认配置、能从安全语言直接调用而不用经过不安全的 FFI 层，以及更开放的社区。loufe 指出 SQLite 的完整测试套件是私有的，tancop 觉得这很怪：SQLite 免费发布“认证过”的二进制，却不公开测试代码，和其他项目的惯例正好相反。smt88 认为有了 LLM 和庞大的人写测试套件，重写不再像过去那样是坏主意，而 Rust 编译器足够严格，人和 LLM 都容易据此发现问题；kvirani 怀疑以后大多数测试套件都不会是人写的，甚至不会被人完整审阅。shimman 则认为这就是风投对开源做的事，“他们来不是为了建设社区，是为了赚钱”，glommer 说钱在他的优先级里排在第一千位甚至更低，Turso 有活跃的开源社区和大量外部贡献者，而且完全是 MIT 许可。

**SQLite 作者本人现身。** d1l 说 SQLite 作者 Richard Hipp 曾打趣“你要是觉得能做出更好的 SQLite，尽管来试”，账号名就叫 SQLite 的用户回复：“我不记得我说过这话。”d1l 找出一段演讲视频，指出第 54 分钟左右的原话大意是“很多人试过，欢迎你也试试”。hitekker 看完视频给出更完整的转录：被问到如果年轻开发者想 fork SQLite 做个更好的版本该怎么劝时，Hipp 说很多人试过，你也可以试，但他觉得时机已经过去了，这需要二十五年时间和近乎狂热的投入。hitekker 认为他的语气是在提醒别白费力气，而不是热情鼓励。

**其他声音。** deaux 说他们曾在某些区域遇到 Turso 侧网络问题导致的 100 毫秒以上延迟，持续很久状态页也没显示，于是离开了这个平台，“如果只打算好好支持美国区，就别提供其他区”；glommer 道歉并解释，如果只是少数服务器上的局部问题，状态页可能检测不到。vira28 说自己之前的 Postgres 创业公司失败了，认为 Turso 在重写和技术可能性上花了太多时间，而不是去找产品市场契合。codedump 说在智能体时代 SQLite 似乎不够用了，他写语义查询工具时一直在找一个能同时支持 SQL、向量查询和图查询的嵌入式数据库。gloosx 则问了一个朴素的问题：现有数据库基础设施到底哪里不行，智能体为什么需要自己的基础设施？这个问题帖子里没有人正面回答。
