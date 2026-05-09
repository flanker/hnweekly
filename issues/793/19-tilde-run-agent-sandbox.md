---
layout: article
title: "Tilde.run：带事务、可版本回退文件系统的 Agent 沙盒"
issue: 793
number: 19
category: show_hn
original_url: "https://tilde.run/"
hn_url: "https://news.ycombinator.com/item?id=48037724"
date: 2026-05-08
---

## 文章摘要

**Tilde.run** 是 lakeFS（开源数据版本管理项目）团队的 Oz Katz（HN ID **ozkatz**）发起的新产品——一个面向 AI Agent 的"事务式"沙盒。一句话定位："**让 AI agent 跑在生产数据上，但不会把生产数据搞坏。**" 与 e2b、Modal、Daytona 这类"提供干净容器"的通用沙盒不同，Tilde 的差异化卖点在**版本化的文件系统**：每次 agent 运行都被视作一笔事务（transaction），要么完整提交，要么整个回滚，没有"半成品"中间状态。

技术实现上，Tilde 通过 **FUSE 把 lakeFS 仓库挂载进沙盒**，agent 可以像操作本地文件一样读写——一个 `cat`、一个 `mv` 就够了，不需要专门的 MCP/Skill 接口。底层支持把 GitHub 仓库、S3 桶、Google Drive 等异构数据源统一成一个**版本化的 POSIX 文件系统**。网络访问默认禁止（default-deny）：agent 想出站，必须经过沙盒里的 forward proxy，目标 host、path、method 都要显式白名单。每次执行的 inbound/outbound 请求、每个文件 diff 都被完整记录，便于人在回路（human-in-the-loop）审核 agent 的每一步操作——产品文档把这套工作流类比为"agent 提 PR、人 review、合并"。

定位上 ICP 是中小型独立软件供应商（SMB/mid-sized ISVs），需要 agent 接触敏感数据但又必须可审计、可回滚的团队。语言层面支持 Python、Node.js，以及任意 Docker 镜像；当前免费的 sandbox 配置固定在 2 核 4GB 无 GPU，可配置性即将上线。Tilde 本身是闭源 SaaS，但底层的 lakeFS 是开源的（[treeverse/lakeFS](https://github.com/treeverse/lakeFS)）——你也可以自己用 lakeFS + FUSE-Mount 搭一套相似方案。

## HN 评论精华

- **"版本化存储才是真亮点"**（**bstrama**）：最高赞肯定了 Tilde 的差异化：versioned storage sandbox 是它真正区别于其他 agent 沙盒的地方。

- **"我没看出版本控制的必要"**（**esafak**）：核心质疑是——agent 要么不改外部状态（那 git 就够了），要么改了外部状态（git 也救不了），版本化文件系统能解决什么？作者 ozkatz 的回应分三层：(1) 人在回路可以批准 agent 的更改；(2) 错误改动可以回滚；(3) 时间线和历史可审。**bossyTeacher** 进一步举例反问"agent 删了一个数据库列、列里数据丢了，你怎么回滚？"——作者承认这是产品边界，目前只能管 Tilde 自己挂载的文件系统。

- **"为什么不直接用 btrfs 快照？"**（**Galanwe**）：吐槽味很重——"btrfs subvolume snapshot 一行命令就能做的事，包装成'量子化 agent 版本沙盒'就值 10 亿。" 作者承认 snapshot 容易，但 lakeFS 的卖点是**结构化的人在回路评审 + 跨数据源原子提交**，不只是单机快照。

- **"演示视频太啰嗦"**（**whalesalad**）：批评作者放出的 2 分钟 demo 80% 都在配置权限（确实繁琐），最后几秒才出现"agent 删除文件被批准"的核心场景——这不是好的 sales pitch。作者 ozkatz 接受批评。

- **"agent 沙盒泛滥已经成 HN 烦点"**（**mc-serious**）：他直白说现在每天都有新的 agent sandbox 上 HN，AI 风格的 landing page、动画、文案太多反而成了红旗，希望产品页能更直接、更少包装。

- **"持久化文件系统才是真痛点"**（**kushalpatil07**）：他自己做 agent 时苦于"沙盒一关文件就没了"，只好自己拼 S3 + Docker。Tilde 的持久化挂载正好戳中这个需求。**thepoet** 趁机推荐了 [instavm.io](https://instavm.io)，**theaniketmaurya** 推荐了 [SmolVM](https://github.com/CelestoAI/smolVM)，可见这条赛道竞争激烈。

- **"Anthropic 一旦下场会团灭一片创业公司"**（**debarshri**）：宿命论调——agent 沙盒迟早被基础模型公司或 K8s 官方方案（Agent Sandboxes）整合。**empath75** 也说"k8s 已经有官方解决方案了。"

- **更广的网络环境感慨**：评论区还跑题到 AI 内容污染社交平台——**zuzululu / Karrot_Kream** 讨论 HN/Reddit 的 AI 评论越来越多、信任度下降，部分人开始减少社交媒体使用、转向线下连接。这场跑题反而说明：agent 沙盒所要解决的"信任问题"，不只是技术问题。
