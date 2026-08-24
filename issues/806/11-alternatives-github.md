---
layout: article
title: "Ask HN：GitHub 的替代品有哪些"
issue: 806
number: 11
category: ask_hn
original_url: "https://news.ycombinator.com/item?id=49331033"
hn_url: "https://news.ycombinator.com/item?id=49331033"
date: 2026-08-21
---

## 文章摘要

提问者 dhruv3006 只写了一句话：「GitHub 过去几个月一直在持续宕机——换到替代品有意义吗？」这条提问收获 649 分、220 条评论，成了一份相当完整的 2026 年代码托管平台现状盘点。

被提到最多的选项分成几类。**Forgejo / Gitea 系**几乎是自托管派的默认答案：Gitea 是主项目，Forgejo 是社区驱动的分支（当年分家有过一场大家都记不清细节的 drama），Codeberg 是 Forgejo 的公共托管实例，Codefloe 是另一个带自定义补丁的托管实例。多位评论者报告 Forgejo 在便宜 VPS 或家庭服务器上跑得又快又省资源，日常维护几乎只有一件事——挡 AI 爬虫；常见做法是本地仓库同时 push 到自建实例和 GitHub，后者只当镜像用来提高可见性。Gitea 的项目负责人 techknowlogick 本人在帖子里反复出现，一边道谢一边邀请大家进聊天室反馈，还主动打听「相比 GitLab，Gitea 还缺哪些功能」，因为他们正在做 backlog 梳理。

**GitLab** 是大型组织迁移的现实选项，功能最接近 GitHub，并且有可自托管的开源社区版；有人指出仓库组织方式比 GitHub 好得多（org 下可以有子团队各自拥有仓库，而不是一个平铺列表），CI 也更受偏爱。但自托管 GitLab 的经验分享构成了整条帖子里信息量最大的一段（见评论精华）。

其他被提名的还有：**SourceHut**（sr.ht，CI 出色、刻意低技术含量、不试图克隆 GitHub，但仍是邮件补丁流程）、**Bitbucket**（多人称赞干净好用，但已经砍掉自带 issue tracker 强推 Jira）、**tangled.sh / tangled.org**（基于 atproto 的新型联邦化 forge，支持 stacked PR、jujutsu、Nix VM 里的 CI，创始人在帖子里答疑）、**Radicle**（radicle.xyz，真正去中心化，主机宕机也能本地继续处理 issue，还能通过网络里其他节点同步）、**Fossil**（把 issue、wiki、PR 全部存进仓库本身）、**Gogs**、**RhodeCode Enterprise**、**Pijul Nest**、**Google Cloud Secure Source Manager**、**Gitoro**（EU 托管）、**oak.space**（非 git 的新方案）、**Soft Serve**（charmbracelet 出的、带 Git-LFS 的极简方案），以及最原始的 gitolite + CGit/GitWeb 甚至直接 ssh 上 git --bare init。CI 单独一档：Woodpecker CI、DroneCI、Buildkite（有 GitHub Actions 适配器）、Semaphore、Circle、preloop（100% 实现官方 runner 协议、跑在隔离 microVM 里的 GitHub Actions 替代品）、DSCI（单 Go 二进制、内嵌 podman/docker runner、用通用编程语言而非 YAML 写流水线）。还有人提到 Cursor 即将推出的 Origin，以及 european-alternatives.eu 上的欧洲替代品清单。

rhdunn 给了一份被广泛认可的决策树：想要「像 GitHub 一样」→ Forgejo / Gitea；想要最省事地托管 git 仓库 → GitLab、Codeberg；已有自己的基础设施 → gitolite + CGit/GitWeb；只想托管仓库 → gitolite 管 SSH/认证/建仓，CGit 或 GitWeb 做前端；需要类似 GitHub Actions → GitLab、Forgejo、Gitea 自带 CI，或用外部 CI；需要 issue 管理 → 上述三家都有，也可以用 Jira、Trello 到 Obsidian 等各种替代品。als0 补了第 7 条：如果你想要最多的贡献者，就得承认 GitHub 有着庞大的社区——他亲眼见过一个项目搬离 GitHub 后贡献量显著下滑。

## HN 评论精华

649 分、220 条评论。讨论主线有三条：自托管 GitLab 的真实成本、Codeberg 的反 AI 政策与可用性、以及「换到另一个中心化 forge 到底算不算解决问题」的哲学争论。同时值得注意的是，很多人并不认同前提——beanjuiceII 直接说「我用得挺频繁，并没有觉得它在持续宕机」。

- **plqbfbv** 的长贴是全场最有价值的经验报告：他们公司自托管 GitLab 六年多，自建 runner、每天上班前自动升级 docker 镜像。大体运转良好，但踩过几次坑——某次 Docker 升级必须回滚；某次内置的 pg_shared_buffers 默认只有 1 MB，导致大实例根本无法完成 schema 升级；某个大版本破坏了流水线预期，逼他们一次性改 200 多个仓库（之后就锁定大版本了）；近来还几乎每周收到高危漏洞补丁通知，他猜是 LLM 在扫代码找 bug。但结论是：**他后悔迁到了 GitHub**——自托管实例的宕机时间远少于 GitHub，虽然慢一点、运维负担重一点，「GitHub 完全没到 Enterprise-ready 的水准，各方面都是降级」。GitLab 的权限粒度更细、文档更好、集成更好、UI 明显被认真打磨过（尽管新账号要花十分钟才能在侧边栏迷宫里钉好常用项），而且你能看代码、必要时给镜像挂个打过补丁的版本。他给出的规格建议：50～100 人的小团队自托管，至少 16 GB（最好 32 GB）内存、4 核、体面的 SSD，加 1～3 个能随时扑上去的维护人员；runner 用一个小 k3s 集群最理想。
- **cortesoft** 点出自托管最核心的价值不是性能而是控制权：升级时机由我们定，不在真正需要 git 基础设施的时段动手，事故响应节奏也由我们掌握，而不是听凭另一家公司的升级排期和事故处理流程。他们后来还从付费 Enterprise 降到免费版，发现免费版基本满足所有需求。**formerly_proven** 则对 plqbfbv 的做法提出保留：「如果我要一个可靠的生产服务，我不会每晚把 docker 镜像自动升级到 `*`。」**znpy** 报告了另一种更保守的实践：单台 VM（8 核 64 GB、本地 vmware 集群的 SSD）支撑约 120 名开发者，只暴露在企业网和 VPN 内，每半年在深夜升级一次、升级前先做整机快照，体验非常愉快；他现在用 GitHub 和 Bitbucket，作为用户和管理员都很怀念。
- **scientifik** 以前 GitLab 员工的身份补了一个关键判断：GitLab 作为产品不错，但 GitLab Inc 不擅长支持广义社区，尤其是中小规模的自托管用户（大企业客户才有白手套服务）。「可惜，几年前他们明明有机会把自己定位成 GitHub 的可行替代品，却错失了。」**jrey2112** 提醒别忘了当年 GitLab 突然涨价、免费版功能一夜之间变味那件事——他当时正准备从免费转付费，价格几乎翻倍，于是转投 Gitea。**wongarsu** 认为 runner 是 GitLab 最大的故障点、需要不少微管理，如果从零开始他会考虑用 GitLab 做源码管理但把 CI 交给托管服务。
- **Codeberg** 引出两条重要的免责提醒。**Anon1096**：如果你换平台的唯一理由是可用性，Codeberg 会让你大失所望——他们自己的状态页显示两周可用性只有「一个 9」，如果套用大家爱拿来批 GitHub 的那种「跨全部产品线」的统计口径，整体大概是零个 9。**bdlowery** 和 **babelfish**：只要你在项目里用了任何形式的 AI 辅助编程，就违反 Codeberg 的服务条款，仓库会被封。**axegon_** 恰恰因此选择了 Codeberg 并设了年度捐赠——真正把他推离 GitHub 的是 GitHub 不断把 Copilot/ChatGPT 塞到他脸上，而 Codeberg 在这件事上立场明确。**godwinson\_\_4-8** 则毫不客气：Codeberg 附赠免费的「欧式社民道德优越感」，别管它在更小的负载下性能更差、并且把一切 LLM 相关的东西当敵人对待；「你若认同，那你找到家了；只想托管代码而不想买这套世界观的人，最好看看别处。」
- **gritzko** 提出全场最尖锐的哲学质疑：他强烈反对「GitHub 的替代品是另一个中心化 forge」这个假设。git 本身是完全去中心化的，Linux 内核最初的开发流程也是。把所有蛋放进同一个时不时不可用的服务，本身才是问题；把蛋搬进另一个桶不是解法，单点故障才是。git 有 plumbing、porcelain 和「github」三层，第三层也必须去中心化，那样选哪个 forge 就成了便利性选择而不是必需。**anon7000** 冷静作答，列了四条现实原因：人们不喜欢基于邮件的补丁流程；GitHub 把一切做得极简单且免费，很多开源项目正因为它易用、跨仓库协作无痛才繁荣；真正在意去中心化的人极少（只要你本机有完整仓库副本，很难说去中心化还给了你什么）；大多数人也不想自建 git 服务器。**rsyring** 更不客气：HN 上竟然有人不明白 GitHub 为什么能拿下这么大份额——因为它加在「易于去中心化的 git 内核」之上的那些东西对多数团队**极有价值**；如果去中心化真要赢，就得在功能和开发体验上匹配 GitHub 的基本盘。「即使有这么多宕机，人们还是留在那里。如果搬走真那么容易，他们早搬了。这件事说明了些什么，请不要忽略它。」
- **Pxtl** 提出另一个具体损失：GitHub 的统一性让我们能跨全部开源世界搜索、复用别人项目里的流水线 action、在一个仪表盘里看自己在各项目的贡献和关注。他不想看到 forge 巴尔干化把这些丢掉，但也指出这类联邦数据并不重。**dboreham** 说 Gitea 曾有联邦化的工作，他没跟进结果；实际解法就是把 Gitea 仓库镜像到 GitHub，这样可被发现、release 可下载，缺点是用户会困惑为什么不能开 issue。**esseph** 反问：你的首要关切竟然不是 GitHub 的可靠性、暗黑模式或安全性，而是网络效应？源码仓库、制品仓库、PR、CI 本该各自独立，「巴尔干化一直都存在，不在 GitHub 上的东西多得是」。
- **sssilver** 希望有一种把 PR、issue、wiki 都存进仓库本身的约定（像 Fossil 那样），那样所有 GitHub 和 GitLab 就只剩提供 UI 的份，从设计上无法扣押你的数据。**rwl** 说他最近开始用 Fossil 很满意，唯一舍不下 git 世界的是 magit 那种逐 hunk 暂存的魔法，但 wiki 和 issue 在仓库里实在太合理，Fossil 的简洁对受够了命令行 git 的人是一股清风。**paularmstrong** 推荐 git-bug，**jolaflow** 推荐 git-native、厂商无关、带 TUI + 浏览器 GUI + MCP server、还有事件溯源可回放的 Epiq。
- **icy** 以 tangled.org 创始人/CEO 的身份现身介绍：全联邦化，git 仓库和 CI runner 都能自托管，特色功能包括 stacked PR、基于 Nix 的 CI 和完全开放的 atproto 协议。跟帖问题很集中：**paulhebert** 和 **whycombinetor** 都问变现路径——没有私有仓库（因为一切都在 atproto 上）、CI 免费、没有定价页，「用它的话我是不是就是产品？」**AndrewHampton**、**jkl5xx** 和 **ch71r22** 都指向同一个缺口：私有仓库。jkl5xx 说得最直白：「tangled 感觉像未来，但它急需私有仓库，才能吃下当下这波 GitHub 出走的势头。」**theamk** 和 **WhyNotHugo** 则对首页上「social coding」和「X 关注了 Y」的社交调性明确反感，认为这让人想起 Facebook、社会操纵和开发者倦怠，也让人怀疑项目长期方向——「大多数项目只需要 git 托管 + web UI + 简单的 PR + 克隆别人仓库的能力」。
- 实践层面几条常见配置反复出现：**yogsototh**、**jm4**、**sandcat_**、**madebywelch**、**amysox** 等人的方案基本一致——自托管 Forgejo/Gitea 作为 source of truth（jm4 用便宜 VPS 加每小时 borg 备份到 rsync.net），同时自动镜像到 GitHub 提高可见性。sandcat_ 原本只是出于好奇在 homelab 上装了 Forgejo，「结果快得难以置信，我把所有东西都搬过去了；相比之下在公司用 GitHub 很痛苦——就在今早我们还因为 GitHub 宕机推迟了一次发布」。**vehemenz** 说他们组织的 GitHub Enterprise 从不宕机、功能几乎一样只是落后几个月，但 **jtokoph** 反驳他上一家的 GHE 天天挂，撑不住那种规模所需的 CI 和自动化频率，「所以我完全能想象云版扛不住新的 AI 规模」，**herpdyderp** 补了一句「我的 GitHub Enterprise Cloud 现在就在挂」。
- **dxbhack** 给了一句最克制的建议：不要为了反应而迁移，而是把宕机当成评估替代方案的契机，确保 GitHub 一次宕机不会让关键工作停摆。**matheusmoreira** 提了一个几乎没人回答的问题：GitHub Sponsors 让自由软件开发者能挣点钱，哪个替代品有类似机制？
