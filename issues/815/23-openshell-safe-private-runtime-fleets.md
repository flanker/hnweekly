---
layout: article
title: "OpenShell：为成群自主 AI agent 打造的安全私有运行时"
issue: 815
number: 23
category: code
original_url: "https://github.com/NVIDIA/OpenShell"
hn_url: "https://news.ycombinator.com/item?id=50198675"
date: 2026-10-23
---

## 文章摘要

OpenShell 是 NVIDIA 开源（Apache 2.0）的 AI agent 运行时，自我定位是「为成群自主 AI agent 提供安全、私有的运行环境」。它要解决的矛盾是：agent 能读文件、装软件包、调 API、用凭据时才最有用，但又不能因此获得对你的数据、密钥和网络的无限制访问。OpenShell 的做法是让你在策略（policy）里声明每个 agent 能碰什么，再由它来强制执行。项目近期进入 0.1.x 系列，带来了稳定的发布节奏、新的隔离原语、更多扩展点和新 API。

**两种管控手段。** 一是内核级强制：每个 agent 运行在隔离沙箱中，内核控制限定它能访问哪些文件、能发起哪些系统调用，每一个网络连接在离开沙箱前都要经过策略检查。agent 永远看不到真正的凭据，OpenShell 只在发往已批准端点的请求上才把凭据注入进去。二是经过形式化验证的策略变更：一项策略变更被批准前，OpenShell 用形式化验证找出它会新增的高风险访问，比如带着凭据访问一个新主机，或调用一个新的 API 方法，这类变更会等待人工审核。

**架构。** 根据官方架构文档，系统由几部分组成：网关（gateway）是控制平面，负责身份认证、记录沙箱状态、下发策略和设置、挂载凭据提供方并协调连接；计算运行时负责创建沙箱、启动监督进程（supervisor）和工作负载、搭建两者之间的私有通道和外层网络围栏；监督进程位于边界的可信一侧，负责按策略审查请求、提供凭据、解析 DNS、建立获准的连接；沙箱与不可信的 agent 处于同一侧，从不自行做策略决定，只报告 agent 想做什么。在当前的 Linux 后端中，工作负载以单一非 root 身份运行且没有任何 Linux capability，用 Landlock 限制文件系统访问，用 seccomp 用户通知拦截 TCP 连接和 DNS 查询并交给监督进程裁决；它根据可信的 /proc 信息识别发起请求的真实程序，而不是 agent 自称的路径。双方通过相互认证的 HTTP/2 连接通信，传输层视运行时而定：Docker/Podman 用 Unix socket，Kubernetes 用 TCP，MicroVM 用 vsock。系统默认「失败即关闭」：监督进程确认边界之前 agent 不会启动，连接断开时 agent 会被冻结。

**策略证明器。** 证明器基于 SMT 求解器，有两种检查：「边界检查」验证一份策略的权限没有超出某个边界策略（比如企业允许 agent 拥有的最大权限），父 agent 可以用它确认自己给子 agent 写的策略没有越界；「提议风险检查」在 agent 每次通过策略顾问提出新网络规则时自动运行，标记诸如访问云元数据地址或给凭据增加新目的地之类的风险，有任何发现就不能自动批准。它覆盖文件系统、进程身份、Landlock、网络连接和 REST 请求；遇到 GraphQL 或 MCP 规则等它无法建模的内容，会明确报告无法检查，而不是悄悄忽略。证明器也作为独立的 `openshell-prover` 命令发布，可以在 CI 中使用。

**上手与生态。** 支持 Linux、Apple Silicon 上的 macOS 和 WSL 2 下的 Windows（实验性），需要 Docker、Podman 或主机虚拟化。一行脚本即可安装 CLI 和本地网关，默认沙箱镜像是不带 agent 的最小化 Ubuntu；入门教程演示用 OpenCode 搭配 OpenRouter 免费模型运行，并在 agent 需要时批准新的访问。网关可以用 Helm 部署到 Kubernetes（CNI 必须支持 NetworkPolicy），支持 GPU，提供中间件、拦截器和计算驱动等扩展点，有 Python、TypeScript、Go、Rust 四种 SDK，还提供一组 agent skills 让编码 agent 学会操作 OpenShell CLI、编写沙箱策略和排查网关问题。项目自称「agent 优先」，本身也是用它所支持的 agent 工作流开发的。需要注意的是，OpenShell 默认收集匿名遥测（只有运营类别和计数，不含沙箱名、主机名、路径、提示词、凭据或用户内容），可以通过环境变量关闭，也可以在编译时彻底去掉。

## HN 评论精华

本帖 433 分、近 200 条评论。讨论主要分为几条线：默认开启遥测引发的反感、「显卡厂商来做 agent 沙箱」的调侃、大家各自的 agent 隔离方案，以及数据外泄这个几乎无解的难题。

**来自 GTC 的背景。** 提交者 niemandhier 说刚从 GTC 欧洲大会回来，很大一部分演示都用了 OpenShell，而他此前从没听说过。RevEng 解释这是 NVIDIA 的惯常做法：与许多公司合作制作使用 NVIDIA 产品的演示，合作方也乐意，因为「被黄仁勋点名能让股价涨三个点」。prennert 介绍了 GTC 上的宣讲内容：OpenShell 不只限制单个 agent，还要确保整个 agent 集群的权限总和不超出预期，因为 agent 可能把任务委托给拥有所需权限的其他 agent，从而合谋突破各自的沙箱。例如一个 agent 负责提取原始数据、一个负责写报告、一个负责发布，如果某个 agent 认为需要发布原始数据，它们可能合力做到，尽管没有任何单个 agent 有此权限。现场还一并展示了一种基于 BlueField 的片上硬件防火墙，位于 agent 运行时和主机之间，使 agent 无法修改自己的 OpenShell 策略。

**默认遥测引发反感。** overflowy 第一时间摘出遥测说明，xyst 指出还可以从源码编译时去掉。f3408fh 问关闭「匿名」遥测的人究竟在担心什么；lcampbell 说 2026 年已经不存在匿名，而且文档用「例如」这种含糊措辞，而不是给出完整清单；Lammy 指出仅仅建立网络连接就会暴露 IP、使用时间和操作系统指纹；rnd0 说没必要有具体的担心，能少给就少给。dangoodmanUT 称默认开启遥测「太疯狂」，sscaryterry 直接称之为间谍软件；thway15269037 担心即使有开关，也难保没有漏掉某个上报函数。

**显卡厂商？** nasretdinov 调侃居然要靠「一家游戏显卡厂商」而不是 AI 公司来做出好的 agent 沙箱。kbenson、LoganDark 纠正说 NVIDIA 早已不是显卡厂商，nickv 给出数据：消费级游戏显卡只占 NVIDIA 营收约 5.5%，《经济学人》甚至称它为「AI 的中央银行」。imtringued 认为 AI 实验室本身没有保护 agent 安全的经济动力，它们只需展示能力上限、卖 token。toddmorey 提醒 OpenShell 本身并不是沙箱，它依赖 Docker 或虚拟化，位于更上一层去编排沙箱及网络和资源访问，整套设置相当重。

**大家怎么隔离 agent。** mfro 说如果你还没在容器里跑 agent，那你疯了；devmor 指出大多数人用的默认 Docker 配置其实隔离不了什么，Reddit_MLP2 也说现代模型逃逸常见本地容器并不难。thway15269037 反问为什么不用虚拟机、放进独立网络并加过滤和日志。各种方案纷纷登场：Twirrim、d2xdy2 推荐 lima 虚拟机，mfro 推荐 OrbStack，binsquare 推荐自己维护的 smolvm（启动不到 200 毫秒，带凭据处理和出口控制）；walrus01 在无头服务器上用专门的 KVM 虚拟机跑 opencode，与个人数据完全隔离，还能 7×24 小时跑慢活；devmor 用两台专用物理机，共享独立 VLAN，凭据由硬件令牌按需提供，他把无人监管的 agent 工作比作在病原体实验室里处理病毒。thehours 推荐 Anthropic 的 sandbox-runtime，配置简单，能让他放心以 YOLO 模式跑 Pi，但 Xcode 和 Docker 等工具在沙箱内受限；zmmmmm 指出这种方案把 harness 本身放在沙箱外，等于完全信任 Claude，他更希望把整个 harness 放进虚拟机。LawrenceKerr 坦言大概 90% 的开发者都在裸机上跑 Claude Code 或 Codex 并依赖自动分类器，他以前用 dev container 加 `--dangerously-skip-permissions`，但为了放行文档访问一天要重建十次防火墙，如今觉得自动分类器足够可靠，「但我知道这仍是玩火」。dingaling911 问为什么不直接用容器，knicholes 回答普通容器不会用 seccomp 和 Landlock 依外部策略审查文件和网络调用，也不会作为中间人注入凭据让 LLM 接触不到密钥；smashed 则说这些用 sidecar 和 Docker 网络技巧也能实现，他就自己做了一个 egzo。

**数据外泄几乎无解。** fennecbutt 说他正在工作中锁定 agent：所有流量走代理、只放行白名单域名、替换并重新签名凭据（他推荐 iron proxy），但还没解决「agent 把代码库片段送到不该去的地方」的问题。marcosscriven 认为只要 agent 能对外界产生哪怕最微小的公开影响就能外泄数据，比如某个冷门论坛帖子的阅读计数；epolanski 说只要能向外发请求，就能把数据编码进 URL 或请求头，想要安全就必须断网并用自己的硬件。smashed 介绍他按 Simon Willison 的「致命三要素」（不可信内容、私有数据、网络访问）设计：单个 agent 最多只拥有其中两项，并给消息打上可信或不可信的标签，这样要外泄数据就得同时欺骗多个 agent。wren6991 建议 harness 可以联网，但它派生的执行命令的 shell 不必联网，再把整体放进私有网络的虚拟机，用 nft 规则只放行推理服务器。fennecbutt 还设想在代理处对比出站数据与私有数据集的匹配程度，长时间累积超过阈值就暂停 agent。

**其他。** crypt1d 说他上周末尝试 vibe coding 一个类似工具，却被 Claude 的安全分类器拦住，不让他构建追踪或审计进程执行的功能；edoceo 也说 Claude 拒绝帮他写一个能申请上千个租约的 DHCP 测试客户端，Gemini 却一次就给了。flohofwoe 嘲笑项目 AGENTS.md 开头那句「我们设计系统并用 agent 实现，这不是 vibe coding」，tomsmeding 认为那是写给 LLM 而非人看的。smallnix 认为宣传语掩盖了一个内在矛盾：给 agent 的权限越多它越能干，不可能鱼与熊掌兼得。Havoc 问为什么要用 AI 的名义重新发明虚拟化和容器。input_sh 吐槽 OpenAI、OpenCode、OpenClaw、Open WebUI 之后又来个 OpenShell，「名词快不够用了」；layer8 提醒别和 Windows 开始菜单替代品 Open-Shell 混淆。
