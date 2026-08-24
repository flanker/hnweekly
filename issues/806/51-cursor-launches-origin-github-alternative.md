---
layout: article
title: "Cursor 推出 Origin，对标 GitHub 的代码托管平台"
issue: 806
number: 51
category: startup_news
original_url: "https://cursor.com/changelog/origin-code-hosting"
hn_url: "https://news.ycombinator.com/item?id=49334209"
date: 2026-08-21
---

## 文章摘要

2026 年 8 月 17 日，Cursor（母公司 Anysphere）发布 Origin——自家的 Git 代码托管服务，以 early beta 形式向所有付费用户开放（企业版组织的管理员可选择退出）。首批功能刻意做得「朴素」：仓库、Pull Request、代码浏览、以及与 GitHub 的双向同步。新增的 Codebase 标签页是 Origin 仓库的入口，点 +New 建仓后页面会给出安装 CLI、clone 或 push 本地项目的命令；codebase 名字会成为所有仓库 URL 的一部分（`cursor.com/codebase/acme-corp`）。GitHub 仓库可以和 Origin 自托管仓库并列摆放：连接 GitHub、选组织、挑仓库同步进来，同步是实时的，但 push 仍然走 GitHub——GitHub 保持为「源自那边」内容的真实来源。PR 评论双向同步：在 Cursor 里评论会发到 GitHub，在 GitHub 上回复几秒内出现在 Cursor。App 生态已接入 Vercel（每个 PR 自动预览部署）、Depot 和 Buildkite（两者都能跑现有的 GitHub Actions workflow，Buildkite 还支持自己的原生 pipeline）。

真正的技术料在配套博客《Git at Any Scale》（作者 Vicent Martí，libgit2 作者）里。文章先复盘业界历史：Shawn Pearce 在 Google 用 JGit + 分布式哈希表存 Git 对象，因为 Git 协议强制走 packfile，`git clone` 性能太差而作废；GitHub 早年试过 NFS、GFS、DRBD 等分布式文件系统方案，全部撞墙，因为 packfile 里对象是随机排布 + delta 压缩的，每次 Git 操作都要在几 GB 数据里随机游走，网络文件系统扛不住。GitHub 2013 年做出的 Spokes 成了行业标准，三个正确选择是：不分布 Git 本身而在 packfile 层做、把数据以真实 Git 仓库形式存在本地 NVMe、复制但保持强一致。Spokes 用 3PC（三阶段提交）同步 reference transaction（packfile 可以无同步地并行 fan-out，因为提交在 ref 更新前不可达）。但 3PC 的水平扩展性有硬伤：延迟受集群最慢节点约束，副本越多 push 吞吐越差；三副本对 2026 年的巨型 monorepo（尤其是 CI 流量）不够用，而对 agent 批量生成的海量小仓库又太奢侈——「下限太高，上限太低」。加上磁盘仓库即真实来源，仓库只能当宠物养，需要外部数据库维护巨大路由表 + 校验和 + 修复任务。

Cursor 的方案叫 Continuity：核心原语是存在 S3 兼容对象存储里的 write-ahead log。每次 push 作为独立对象写入 WAL，未完全持久化前绝不 ack；push 只有在本地仓库副本上成功 prepare reference transaction 并把指针记入 WAL index 后才可见，从而让所有 push 线性化。没有路由表、没有关系数据库、没有选主：仓库位置用 rendezvous hashing 推算，本地没有就从 WAL 物化出来；WAL 更新靠 S3 的原子 CAS，任何节点都能当 primary。复制用 UDP gossip 做乐观通知，丢包也无所谓——每个副本记住自己追到的 WAL index ETag，读请求时对 S3 发条件 GET，304（平均低于 10ms 的纯元数据操作）就直接服务，200 就先追平再服务。压缩（repack）只由 primary 做，副本直接从 S3 下载压好的 pack，用带宽换 CPU。压力测试跑到 100 个副本，读吞吐线性增长且不影响 push；S3 Standard 下可持续 120 pushes/s，换成 S3 Express One Zone 超过 300 pushes/s，此时瓶颈已是 Git 自身在磁盘上做 compaction 的速度。作者点名对比 Azure DevOps（把 packfile 放 blob 存储、ref 放 MS SQL Server），认为 Git 数据一致性高于一切，才选了不依赖外部数据库的 WAL 设计——因为每一次 push 和 repack 都有完整溯源，踩到 Git 的 bug 时可以精确定位并回滚，还能任意 rewind/fast-forward 副本。基准数据用的是 Cursor 自己的 monorepo `everysphere`。

## HN 评论精华

这条帖子拿到 595 分、约 410 条评论，但讨论主线几乎与技术无关：绝大多数篇幅在争论「要不要把源码交给 Elon Musk 旗下的公司」（Cursor 已被 xAI/SpaceX 系收购，评论里各种「spacexai」「SpaCursoreXAI」的调侃），以及 GitHub 最近频繁宕机的成因。真正讨论 WAL + 对象存储架构的只有零星几条。Origin 团队成员、Graphite 联合创始人 tomasreimers 全程在场答问。

- **tomasreimers**（Origin 开发者、Graphite 创始人）开场 AMA。被问「和 GitHub 到底有什么不同」时直言：「今天几乎没有区别。我们有意作为 GitHub 的替代品发布，在功能上和它正面硬碰。」承诺未来几周推出 agent 集成、「不必逐行读代码就能理解 agent 写的代码」、以及自动把 PR 推到可合并状态。确认 Origin 完全建在 Graphite 的技术之上，并透露已关联 Graphite 账号的用户「可能有惊喜」。对 jujutsu 支持表示「非常感兴趣，谁能帮我们联系上 JJ 维护者」。
- **tomasreimers** 也确认了当前最大限制：Origin 仓库只对 Cursor team 成员私有可见，没有公开仓库、没有免费额度，要和组织外的人共享代码必须同步回 GitHub。nerdypepper 因此质问「不能匿名浏览仓库，这怎么算 GitHub 替代品」。
- **guhcampos** 五小时后开怼：「你说『happy to answer any questions』，结果只回答了唯一一个不让你为难的问题。」引出一长串关于「该不该把 IC 工程师当靶子」的争吵（runeblaze、fatal94 认为该找决策者，mrheosuper 反驳「不想答就别说 ask me anything」）。**dang** 在下游一条被 flag 的人身攻击评论下出面提醒守则。
- **_kidlike** 追问「为『代码变化速度超过基础设施承载能力的时代』而生」到底是什么意思，还是纯 clickbait。neuronexmachina 贴出 `cursor.com/blog/git-at-any-scale`，_kidlike 读完的结论是「那确实是 clickbait，S3 也是基础设施啊；而且这暗示 13 年来没人试过给 Git 做更高效的系统？太离谱了」。
- **devdoshi** 是少数为技术点赞的：「为 WAL + 对象存储这个组合投一票，非常强的组合，终于看到更多人采用了，历史细节也很有意思。」并推荐关注 atomic.dev。
- **信任问题占据半壁江山**：dbbk、nullbio、stefan_、genxy 反复提到「Grok 被抓到在后台上传用户整个代码库和敏感 .env 文件」这件事——stefan_ 的段子是「来 Origin 吧，你的代码已经在这儿了」。real-hacker 贴出实际报错：「Legacy Privacy Mode 会禁用代码存储，无法建立 Codebase」，而唯一的另一个隐私模式就是允许 Cursor 用于训练。viraj_shah 认为战略意图明显：「拿到完整版本历史，就拿到了『代码怎么生成、出什么 bug、怎么修』的人机全闭环训练数据。」
- **GitHub 为何在挂**：chris_money202 的说法是 Azure 容量见底，GitHub 大量服务免费或深度打折（尤其公开仓库）不产生利润，所以在 Azure 紧张的容量里不是优先级；LLM 带来的用量暴涨（2024 年底）撞上 Azure 从 2019-2020 起把数据中心全滚给 OpenAI 训练。0xy 则称 GitHub 正在回头加大 AWS 用量，原本计划 2027 年完全撤出 AWS 的方案因为「Azure 扩不上去」而搁置。joshuat 提出反面意见：「我一天大半时间在 GitHub 里，宕机确实变多了，但说它是一团糟有点过了。」
- **命名之争** 由 croes 挑起（「一家叫 cursor 的公司还能指望什么」）。**jjcm** 提出了最有技术含量的担忧：「以后说『push to origin main』有两种含义了，LLM 可能在你不知情的情况下把代码推到另一个 provider——这是天才增长手段和域名抢注之间的一线之隔。」rzzzt 补充 EA 的游戏平台 Origin 直到去年才关停。
- **去中心化替代方案**成为最热的一条子树：xvilka 力推 Radicle 和联邦化 Forgejo；LelouBil 详细推荐基于 ATProto 的 **Tangled**（自托管 git/issue/PR/CI，但保留 GitHub 式社交功能，公司消失后 AppView 仍可自建），被 verdverm 反驳「它把 nix/rust 强加给用户，门槛太高，而且主站现在就转圈打不开」。bilalq 的反驳最切中要害：「自建 git 服务器本来就不难，GitHub 的价值在生态和集成——Sentry、Linear 都围绕它做了专门 UX，那需要中心化或至少一个标准。」icy 则吐槽 Forgejo 联邦化「每次都被提，但多年来没有任何实质进展」。其他被提到的还有 sourcehut、Codeberg、GitSocial（存 S3）、ngit/gitworkshop.dev、Reticulum 上的 Rngit。
- **skissane** 提了个务实问题：有没有计划兼容 GitHub 的 API？「我大量工具链假设代码在 GitHub 或 GHE 上，这本身就是锁定。就像别家都抄 OpenAI 的 API 一样，模仿在位者的 API 作为事实标准是合理的。」justincormack 推断既然 PR/issue/评论能双向同步，数据模型基本相同，做兼容 API 应该不难。
- **arjie** 提出了一个少见的性能视角：他之所以要自研软件，是因为 agent 的动作速度远超人类，而 SaaS 的 rate limit 太低——他已经自己替换掉了 git 托管、CI/CD、知识库和任务管理四类 SaaS，「一台 5950x 能扛住的负载，所有商业 SaaS 都会给我限流；它们的用例是慢慢工作的人，我的用例是高频工作的机器」，并希望能有独立租户让自己随便打自己的实例。
- **dutchCourage** 代表了「期待落空」派：「以为会看到比 GitHub 克隆更多的东西，有点失望。在 agentic 工作流时代，协作和版本控制有大量可创新空间。」elpakal 顺势提出更激进的想法：「也许该重新审视 git 本身，如今它的 object 和 history 恐怕不如产生代码变更时用的 context/instruction 重要。」
- **零散但有信息量的**：mcny 指出注册强制要求手机号（adeelk93 认为这能降低 AI 垃圾注册）；romanovcode 注册直接被「Access blocked, please contact support」挡住且找不到 support 入口；3182876 算账说 Cursor 号称值 600 亿美元、高于有 1440 亿营收且盈利的 Mercedes-Benz 集团，「但这网站能吃满 100% CPU」；poilcn 抱怨 Cursor 客户端里塞了无法跳过的横幅广告；newspaper1 质疑帖子被重发且旧评论时间戳被重置。
