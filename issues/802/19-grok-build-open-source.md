---
layout: article
title: "Grok Build 开源了"
issue: 802
number: 19
category: code
original_url: "https://github.com/xai-org/grok-build"
hn_url: "https://news.ycombinator.com/item?id=48926590"
date: 2026-07-24
---

## 文章摘要

xAI 把自家的终端编码智能体 Grok Build 开源了，采用 Apache License 2.0 协议，首发即冲上两万多 star。这是一个全屏 TUI（终端用户界面）程序，宣称能"理解你的代码库、编辑文件、执行 shell 命令、搜索网络并管理长时间运行的任务"，支持交互式、无头（headless）和嵌入编辑器三种运行模式。

技术架构上，整个项目是一个 Rust 编写的 monorepo，按职责拆分成若干 crate：TUI 层负责回滚缓冲、提示符与渲染；agent runtime 是核心执行引擎，提供多个入口点；tools 层实现终端、文件编辑、搜索等具体能力；workspace 层处理文件系统、版本控制和检查点（checkpoint）。根目录的 `Cargo.toml` 是自动生成的，开发者需要改动各个 crate 自己的 manifest。预编译二进制覆盖 macOS、Linux 和 Windows，通过 bash 或 PowerShell 脚本安装；从源码构建需要 Rust（版本由 `rust-toolchain.toml` 锁定）、用于管理工具链的 DotSlash 以及 protoc。编译产物名为 `xai-grok-pager`，首次运行会拉起浏览器完成认证流程。文档涵盖快捷键、斜杠命令、配置、主题、MCP 服务器和无头模式，放在 pager crate 的文档目录下，线上版本在 docs.x.ai。

不过这次开源的时机相当微妙。就在几天前，有研究者曝出 Grok Build 会把用户的整个工作目录（不只是 git 仓库，还包括完整的提交历史）上传到云存储桶，引发轩然大波。据 The Register 报道，xAI 已通过服务端变更停止了这类传输，马斯克也承诺删除此前上传的全部用户数据。因此 HN 上的主流解读是：开源既是对这次事故的危机公关，也是让社区能自行审计代码的必要之举。值得一提的是，仓库禁用了 Issues 和 Discussions，且每次同步都会重写全部提交历史（只保留一个 commit），代码量约 130 万行 Rust——有评论者尝试自行审计后表示"这个量级实在没法怪任何人看不完"。

## HN 评论精华

- **loufe**：怀疑开源本来就在路线图上，但被提前了——很可能是几天前那场"用了这个工具就等于交出整个工作目录"风波的应激反应。
- **dmix**：xAI 昨天就砍掉了代码上传功能，说明他们确实在意舆论反弹。援引 The Register 报道称传输已停止，马斯克承诺删除已上传数据。
- **choppaface**：但基于 DOGE 处理社保数据的先例，我们真的能相信数据会被删除吗？
- **lynndotpy**：为质疑者辩护——"它是不是也会把你的代码全传到服务器上"这个问题是（1）切题的（2）可回答的（3）如果用户自己踩坑发现会是灾难性的。README 里没有答案，而 xAI 又关掉了 Issues 和 Discussions，所以在 HN 问这个问题完全正常。他自己尝试查源码，发现 `b189869` 版本有 130 万行 Rust，"这实在不能怪任何人"，而且 xAI 每次上传都会抹掉整个提交日志。
- **tomhow**（版主）：反复强调不是护着马斯克——"批评随便批，只是别发那种最显而易见的挖苦式评论，因为那会让 HN 显得重复、痛苦又低级"，希望讨论有实质内容。
- **jdiff**：这个代码库对它实际做的事情来说臃肿得像个庞然大物，"绝对是 LLM 写的，而且几乎肯定需要 LLM 才能读得动"。
- **petesergeant**：之前逆向它的具体行为非常痛苦，开源之后会容易得多。第一天写脚本驱动 Grok Build 就提了 6 个行为怪异的 bug，现在可以让 agent 直接查源码判断是设计如此还是 bug。
- **simianwords**：不解为什么整个行业都收敛到 TUI，是不是形式大于功能？他觉得 Codex 的图形界面在各个维度都胜过 CLI。
- **maipen**：反驳——大家其实正在回归 GUI。SpaceX 买了 Cursor 所以有了自己的 agent UI，Anthropic 有自己的界面，Z.ai 上个月也发了。"终端只是个原型，所有人都知道。"
- **lynndotpy**（回应 TUI 争论）：从 2000 年代起就偏爱 TUI，因为任何能开终端的地方都能用、能走 SSH、界面到哪都一样，而且极快、没有那些没完没了的动画。"如果你用 macOS，终端是你电脑上唯一没有到处设路障的部分。"
- **christophilus / thomasjb**：TUI 更容易容器化，SSH 进 VM 时也最好用。
- **kamikazechaser**：泄露私有数据实在可惜，因为模型本身很好（个人认为强于 Opus 4.8），harness 本身也顺滑如丝，有潜力成为最好的那个。
- **bakies**：不同意，"完全没有 Opus 的感觉"，他老是要切回 Opus 去修补或收尾 Grok 生成的东西，"感觉像 Sonnet 3"。
- **canadiantim**：偏向喜欢它却还是失望——比 Opus 4.8、5.6-sol:medium 和 GLM 5.2 差得多，只用 Grok 4.5 跑探索型 agent、commit agent 和琐碎任务。讽刺的是它们常常"grok 不了"工具调用，做编排时容易犯糊涂、重复、逻辑打结。
- **maxloh**：提交信息写的是"initial sync from the monorepo"，没有其余源码真能编译吗？
- **skp1995**（xAI 员工）：能编译，发帖前测过所有功能都正常。（追问：能不能别 force-push 重写历史，并且打上版本 tag。）
- **GodelNumbering**：这不是"正确的事"，而是"战术上的事"——当你的 LLM 市占率不到 1%、口碑糟糕又被抓到上传用户数据时，开源是为数不多能爬出坑的战术动作之一。
- **CobrastanJorji**：还有一个战术动作是"干脆停手"。没人逼你继续往炉子里扔钱，xAI 创始人都走光了，品牌名声已经烂了，"就别干这个了，去造飞船"。
- **mlindner**：那是个失误，数据已经全部删除了。**andai** 反问："你怎么会'不小心'把用户的仓库上传到存储桶？"**jdiff** 补充：上传整个目录和全部仓库历史、无视任何 secret 或相反的指令，对"加速缓存访问"来说根本不必要——不必假设有阴谋动机，光是这个想法本身就很糟，实现更是骇人。
