---
layout: article
title: "PGSimCity：用 3D 城市演示 PostgreSQL 是怎么运转的"
issue: 803
number: 2
category: favorites
original_url: "https://nikolays.github.io/PGSimCity/"
hn_url: "https://news.ycombinator.com/item?id=49063754"
date: 2026-08-01
---

## 文章摘要

PGSimCity 是一个可以在浏览器里直接打开的交互式 3D 可视化项目：它把一个 PostgreSQL 集群变成一座可以俯瞰、可以点选、可以第一人称走进去、还可以亲手把它搞崩的城市。作者是 Nikolay Samokhvalov（HN 用户名 samokhvalov，Postgres 社区知名人物）。项目自我定位很明确：面向那些工作能力很强、但从来没有真正运维过数据库的工程师——那些需要理解"为什么一次 checkpoint 会让延迟飙升""为什么一个被人遗忘的事务会让一张表永久膨胀""`synchronous_commit` 到底在跟你收什么费"的人。它是独立的非商业教育项目，与 EA 无关，不含任何 SimCity 的代码或素材。

城市的分区结构就是 PostgreSQL 的架构图。北面上空是"客户天空"，代表从应用层到达的连接；postmaster 是那位监工，为每个连接 fork 一个 backend，自己从不碰数据；backend 排（16 个后端进程）的灯光本身就是它们的状态，包括那个臭名昭著的 `idle in transaction`；中央是 `shared_buffers` 缓冲池，最多 1024 个代表性帧，旁边坐落着 `wal_buffers`、ProcArray、锁表、CLOG 和 buffer mapping table；"开挖区"是数据目录，即内存与存储的交界；地下的存储区把堆文件画成 8 KiB 页组成的田野、把 B 树画成真正的树，还有 TOAST、FSM、可见性映射、OS 页缓存和磁盘；东边是 WAL 区，backend 和 walwriter 往 `pg_wal` 写 WAL，archiver 复制完成的段，walsender 则独立地边生成边流式发送；西边是维护场，住着 checkpointer、bgwriter、autovacuum launcher 及其 worker；南边是两个备库，各有独立的 walreceiver、回放 WAL 的 startup 进程和各自的延迟；外圈还有 WAL 归档、基础备份、PITR、延迟回放、leader lease 与重新加入等"业务连续性街区"；backend 上方悬着"查询实验室"，选中一个 backend 就能看到它的语句展开成 parse → rewrite → plan → execute。颜色一律是语义化的、绝不装饰：WAL 是琥珀色，脏页红、干净页蓝，vacuum 紫，checkpoint 粉，bgwriter 青，复制橙，存储绿，索引水蓝，锁红。

值得动手试的东西不少：按 `T` 开启 14 章导览，跟着一个连接从客户端一路走过规划、缓存、WAL、checkpoint、vacuum 和复制；按 `Enter` 追踪单条语句，选 Non-HOT UPDATE 并放慢播放，就能看清它何时进入缓冲池、何时产生 WAL、何时在提交处等待；跑"缓存颠簸"场景会把 `shared_buffers` 压到 16 MiB（低于手动控件的 128 MiB 下限），于是时钟扫描开始狂转，backend 不得不先写掉自己的脏受害页才能读下一页；跑"work_mem 悬崖"能看到 Sort 与 HashAggregate 在 2 MiB 时下盘、4 MiB 时无需重新计划就装得下；打开"长事务"后 xmin 水位线会下沉变红，autovacuum 照样跑遍各表却报告零可回收行，而 sessions 表一直膨胀；"checkpoint 风暴"能看到 checkpointer 的飞轮加速、fsync 阶段的震颤，以及每次 checkpoint 开始后涌入 WAL 区的整页写洪流。按 `G` 可以以 1.7 米的身高在城市里步行，按 `E` 去扳动 autovacuum 的操纵杆或推开 postmaster 的门。

作者在准确性上的自我约束值得一提。README 里专门有一节"该有多信任这东西"：项目仍是 0.x，这座 3D 城市是 PostgreSQL 的模型而非模拟器，城里没有跑任何 PostgreSQL 源码，数字都做了人眼可看的缩放。它以 PostgreSQL 18 主线为目标，18.4 是经过复核的参考版本，机制层面的说法都对照 `REL_18_STABLE` 源码验证过；文中甚至主动披露了一处尚未对齐的历史简化（动画里的 32 帧固定环 buffer 与 PG 18 的 ring-sizing 规则不符，因此不得作为版本数值证据）。项目做过四轮评审，三轮专家评审对照官方文档和源码而非记忆核对 PostgreSQL 正确性，另有一次审计把建筑、邻接关系和动画都当作"论断"来查，每条发现都交给一位被指派去反驳它的评审员独立复核。确定性测试套件会因为一个红色测试而让 CI 失败，钉住的检查包括 WAL 触发阈值近似式、缓存命中率公式 `blks_hit / (blks_hit + blks_read)` 以及时钟扫描 `usage_count` 上限为 5。

技术栈上，它只有 three.js（r185）一个打包运行时依赖，加 TypeScript 和 Vite，没有框架，唯一外部服务是 Plausible 分析。架构靠三条规则维系：`world/layout.ts` 是地理的唯一真相来源；模拟层永不 import three.js，世界层也永不改写模拟状态，二者只在 `SimState` 相遇；渲染在不同主题下用不同方式承载语义（夜间靠霓虹，白天靠色相与明度而非泛光）。独立的 Query flow 和 Machine 页面还提供可选的 PGlite 模式——真正编译成 WebAssembly 的 PostgreSQL 在浏览器里跑，负责提供解析、计划、目录、缓冲区计数、错误和结果，而可视化模型负责补上那些本来看不见的内部结构，每个界面都会分开标注这两类来源。项目以 Apache-2.0 开源。

## HN 评论精华

作者本人 **samokhvalov** 全程在评论区回复，最有信息量的内容也来自他的自述。他坦白说这一切"从昨天的一条 prompt 和好奇心开始，就想看看 Opus 5 能干出什么"，结果第一眼看到的东西太惊艳，于是他花了整个周末打磨。他把原始 prompt 原封不动贴了出来，大意是：让模型扮演资深 Postgres hacker，做一个浏览器里的 3D 可视化，涵盖 checkpointer、bgwriter、autovacuum、walwriter、backend、walsender 等全部主要组件，可缩放、可动画、有控件，"想象我们要建一座完整城市的 3D 模型，就是那个级别"，目的是让非数据库专家的工程师也能轻松理解；他要求模型深入思考技术选型并直接实现。他说后来他又拿了大量关于内部机制和行为的材料回去反复打磨改进。他还透露到当时为止已消耗约 38.6 亿 token（还不算后来接手大量编码苦力活的 GPT 5.6 Sol），并向追问的 **hectormalot** 确认：大部分 token 走了缓存，而且是个人的 Claude Max 20x 订阅。

围绕这个数字的讨论本身就挺热闹。**nugator** 算了笔账（38.6 亿 × 5 美元/百万 ≈ 1.93 万美元）追问真实成本；**svobodamartin** 表示自己每周三个项目高强度编码也才 5500 万 token，差两个数量级，不解从何而来。**luciana1u** 贡献了本帖最妙的一句吐槽："花 38.6 亿 token 造一个数据库的可视化模拟器，而那个数据库跑起来只需要其中零头的算力——我们在用昂贵的大脑给便宜的大脑画像。"

正面评价占多数但几乎都带着相同的改进建议。**layoric** 的置顶反馈是：导览（Take tour）噪音太多，屏幕上的框和变化太多以至于抓不到重点，而且应该改成交互式而不是自动切到下一个主题，做这么多信息的被动观察者非常令人困惑。**titzer** 附议：3D 做得惊人，然后 80% 的视觉空间被完全遮挡它的弹窗占据，应该有醒目的降噪开关，弹窗也该半透明。**npunt** 的建议同样简洁：砍掉约 50% 的 UI，改善相机的平移缩放手感。**jedberg** 说自己对 Postgres 内部相当熟悉，但这个东西还是把他绕晕了，太busy；他希望有个减速按钮，被 **augunrik** 指出左下角就有暂停和降速到 0.1×，但他仍认为 0.1× 都太快，"我应该能一条条地发射事务然后看着它流过去"。**graypegg** 给出了最有分量的一条批评：你的人脑限制同样是读者的限制，用 LLM 能造出东西，但到了某个"细节堆砌"（greebling）的程度，它就不像是给另一个人类体验的了——不用 LLM 时你被迫做的那些取舍，恰恰是因为你的脑子装不下这么多东西，而用户的脑子也一样。

关于"这是 vibe coding 产物"的争议是本帖第二条主线。**manytimesaway** 一边提醒 SimCity 仍是 EA 的活跃商标建议改名，一边直言"因为我不想用也不想推荐 vibe-coded 的东西，就这么简单"，并说判据除了 token 数和 git 历史，最主要是"它用了和其他所有 vibe-coded 应用一模一样的 CSS、字体和间距"。**notachatbot123** 提出了更实质的质疑：考虑到这是 vibe-coded 且不到 48 小时，它到底真实准确吗？还是会导致错误结论和反知识？**j1436go** 简洁说明为何在意："因为它降低了我对所呈现信息真实性的信任。"**parthdesai** 的回应是：不完全确定，但这个 vibe coding 的人确实非常懂 Postgres。**DonHopkins** 则贡献了本帖最有考古价值的一条：他当年为尊重 EA 的商标和其对 GPL-3 的附加条款，把开源版 SimCity 改名为 Micropolis（Will Wright 最初的工作代号），并从 Micropolis 商标持有人那里拿到了非商业用途的授权，因此建议这个项目可以安全地叫 "PGMicropolis"。

很多人被这种形式本身点燃了灵感。**Curtis_Guan** 说数据库内部调度过去要靠一堆架构图才讲得清，既然开源了，同样的思路可以复用到云计算、Kubernetes 等领域；**tptacek** 说他一直想用 Factorio 作视觉隐喻来讲 Fly.io 的部署系统和状态跟踪；**seeken** 正在 vibe-code 一个 "Doom for Beam"，让人在 BEAM 虚拟机里像逛工厂车间一样跑动，看模块和函数如何连接、跑得多"热"、有没有在冒火花；**num42** 想做同风格的 CPU/整机模拟器；**robertlagrant** 最想要的则是让它变成一个真实 Postgres 的实时可视化，包括来自其他 Postgres 的复制流和前面 pgbouncer 在干什么。**thelastgallon** 问能不能用游戏来教 CS（操作系统、数据库、编译器），**xerox13ster** 回答 Turing Complete 就让他学会了造计算机和汇编编程。**antonyragleap** 提出了一个具体的功能请求：把 MVCC 和事务可见性可视化出来，会让它作为学习工具的价值再上一层。

轻松的一面：**arjie** 抱怨 "Rendering The First Frame…" 居然不是 "Reticulating Splines…"（SimCity 的经典加载提示）；**deadbabe** 提议拍一部叫《Postgres》的电影，讲住在 Postgres 城里的人，风格照《七宗罪》，台词是"每座城市都有秘密，我们的秘密叫索引"；**harvie** 感叹"我们等到 GPU 加速的 PostgreSQL 都比等到 GTA VI 快"；**bibimsz** 只留下一句"我全都看懂了"。**gregsadetsky** 报告了缩放时的闪烁问题，作者确认是共面地面的 z-fighting 并已用显式偏移修复。
