---
layout: article
title: "Show HN：Hubble——给你和你的 Agent 用的开源笔记应用"
issue: 803
number: 20
category: show_hn
original_url: "https://www.hubble.md/"
hn_url: "https://news.ycombinator.com/item?id=49091730"
date: 2026-08-01
---

## 文章摘要

Hubble.md 是一款免费开源的桌面笔记应用，标语是"给你和你的 Agent 用的最好的记事本"（The best notepad for you and your agents），底层完全建立在 Markdown 和 HTML 之上。作者是 Ben Holmes（GitHub/X 上的 @bholmesdev，目前在 Warp.dev 工作）。需要说明的是，这条 Show HN 是由用户 handfuloflight 提交的，Ben 本人在评论区现身并逐条回复了反馈。

按 README 的说法，Hubble 有三个卖点：

**熟悉的书写体验。** 它试图提供 Notion 或 Apple Notes 那样的编辑手感，但底层文件就是 Markdown。支持 `/` 命令菜单、Markdown 快捷输入，以及文件属性（front matter）。

**Agent 就绪。** 把你的 Agent 指向笔记文件夹就可以开始协作，Agent 修改文件时 Hubble 会实时热重载。近期还加了内嵌终端（可以在笔记旁边直接和 Agent 对话）和一套类似 Google Docs 的批注系统——批注元数据写在文件内，Agent 也能读到。

**可以构建任意视图。** 除了 Markdown，还能构建和查看基于 HTML 的小应用。作者提供了一套配套 skills，让编码 Agent 直接生成这些页面：把一个笔记文件夹变成表格、书架、地图，或任何你想得到的东西。这里最有意思的设计是——**这些 HTML 页面可以访问同一文件夹里的 Markdown 文件，也就是把 Markdown 当成一个迷你数据库，在它上面搭应用。**

技术栈方面，Hubble 是 Electron 桌面应用（pnpm workspace，包含 desktop / web 落地页 / www 三个 app，以及 editor、ui、runtime、sync、convex-client、sync-backend、cli 等 package），编辑器核心基于 Tiptap 加 Markdown 转换层。macOS、Windows、Linux 都有构建产物，macOS 版已签名和公证。MIT 许可证。

README 里还有一段值得注意：**Hubble 仓库的自动化流程本身跑在 Warp Factory agents 上**——新 issue 由 Agent 打标签并留言解释判断依据，标记为 `ready-to-implement` 的 issue 由 Agent 产出草稿 PR，PR 在人类看之前先由 Agent 过一遍 code review。作者强调每条 Agent 产出的评论和 PR 都带有回链 Warp 的签名，以便随时分清哪些是人写的、哪些是 Agent 写的。

## HN 评论精华

151 分、77 条评论。**这条帖子的讨论基调是相当怀疑甚至敌意的**，核心质疑集中在两点：一是"这跟 Obsidian / 一个装满 .md 的文件夹到底有什么区别"，二是一条完全跑题但异常热闹的"该不该用 Rust 重写"口水战。

**"这和 Obsidian / 一个文件夹有什么区别？"**

- **flippyhead** 开了第一炮：落地页信息太少，没法说服我为什么需要又一个笔记应用，在这个赛道里举证责任很高。
- **xn--yt9h** 提出了最认真的版本的质疑：比起磁盘上的 Obsidian .md 文件、甚至 .org 文件，这凭什么更"agent-ready"？我一样可以让 Agent 编辑那些文件，而且生态和现成知识都更成熟；我甚至能让 Agent 建带反向链接和 DataView 查询的完整笔记本。"在我看来 Hubble 只是一个 Markdown 编辑器，前提假设是 Agent 能编辑本地文件。"
- 作者 **bholmesdev** 的回应也是全帖最实质的内容：他承认这是公道的批评——Hubble 和 Obsidian 都用磁盘上的 Markdown，所以在 agent 友好度上**完全等价**。他给出自己从用了多年的 Obsidian 转出来的三个理由：(1) 他想要更接近 Apple Notes / Notion 的编辑体验，超链接是弹窗而不是行内字符串，格式化是所见即所得而不是行内的 `*_` 符号，有 `/` 菜单帮助发现格式选项；(2) 他想要一个能把 HTML 页面和 Markdown 并排展示的应用，更进一步是让这些 HTML 页面能访问同文件夹的 Markdown——**把 Markdown 当迷你数据库来搭应用**，这在 Obsidian 里靠插件大概也能做（Obsidian 什么都能做），但他想要一等公民级的支持；(3) 更近期他想让"Markdown → Agent 交接"这件事更顺畅，于是加了内嵌终端和批注系统，而这些功能全部是社区驱动的。
- **dewey** 追问："我不理解为什么这不能就是磁盘上的一个文件夹，Agent 会用 ls 和 cat 啊。"**rpunkfu** 半开玩笑地说："你没理解错，唯一让它 agent-friendly 的是那个复制路径按钮。"dewey 反问：那不是比让 Agent 直接看文件系统路径还更不友好吗？作者答：它就是你文件系统上的一个文件夹，没有花哨的云端设置，和 Obsidian 一样。
- **ernsheong**：我的 Agent 需要的只是项目目录里的纯 .md 文件。**Ycros** 用中间派 meme 总结："两端其实都是'某个同步目录里的一堆 .md 文件'。"
- **0gs** 提出了一个被忽视的观点：大多数（非技术）用户并没有一个形成肌肉记忆的 Markdown 编辑器，"我当初也很讨厌 Obsidian——有趣的是，一旦你学会喜欢 Markdown，Obsidian 就好用一千倍"。**dewey** 反驳说 Markdown 的价值主张恰恰是**不需要专门的编辑器**，用系统自带的任何文本编辑器打开就已经是人类可读的。
- **iLoveOncall** 说得最直白："我比其他人更不客气一点：这东西没用。Obsidian 和一堆别的笔记应用已经做了同样的事甚至更多。再加上它很可能完全是 vibe-coded 的，价值主张为零。"**crimsoneer**：这到底是什么，为什么会上首页？"Agent ready"？意思是它像别的一切一样用 Markdown？这里根本没有任何信息。**pickleglitch**：又一个笔记应用，一定是星期三了。**lhd1**：我们能不能给笔记应用来个暂停令？**Sha1rholder**：ANOTHER.md（又一个 .md）。
- **Nekorosu**：Obsidian 已经存在，更成熟功能更多。**embedding-shape** 反击：至少读一下标语再留这种低质量评论吧，我自己每天用 Obsidian 也很喜欢，但如果我要找的是"backed by Markdown and HTML"的东西，Obsidian 根本不构成替代品。**smallerize** 一句反问："那你的 Obsidian 是被什么 backed 的？"

**跑题最远也最热闹的分支：为什么不用 Rust 重写**

- **thewhitetulip** 抛出："为什么这是个 Node.js 应用？在 LLM 爆发、生产力疯涨的今天，难道不该是个 Rust 应用吗？"随后他一路展开：LLM 之前 Node 是一次部署所有平台的便利选择，LLM 之后这个限制不存在了，"多 prompt 几次就能用原生语言写出来"；而且因为 LLM 本身，内存条一天比一天贵。
- **filcuk** 一针见血地调侃："被骂 LLM 用太多，又被骂用太少，开发者根本赢不了嘛。"
- **dewey** 认真回应："如果你真干过，你会知道那和'一次编写到处运行'一样不靠谱，总会有各种调整和迭代，即使它确实省了你很多时间。"thewhitetulip 表示自己在讽刺。
- **throwthrowuknow** 把梗推到极致："干嘛只限于 Rust？交叉编译是给人类用的。直接让 LLM 用每个 OS 集成最好的语言和 SDK 各写一份代码库。"**danielmeskin** 加码："干嘛限于语言和 SDK？让 LLM 直接写机器码。"**mianos** 收尾："既然 token 用量都爆炸了，为什么不用 z80 汇编写，然后跑在一个用 WebAssembly 写的 z80 模拟器里？"
- 中间也有几条正经的：**mhluongo** 认为 Tauri 是好的折中——Rust 后端加原生 webview，JS 更少、二进制更轻。**mianos** 说"但是 Rust"这种声音在"LLM 用太多"和"用太少"之间仍然有它的位置。

**其他值得一提的**

- **shreddude** 是全场最热情的用户：他一直在等这样的应用——.md 文件能从 Finder 直接打开、树状文件视图和 Obsidian 一样、支持 front matter、还有编辑器工具栏，几乎命中了他 Markdown 编辑器愿望清单上的所有条目，只差 Mermaid 渲染。而**因为这个项目的 agentic 工程实践做得好，他用 Claude Code 花约 30 分钟就自己加上了 Mermaid 支持并在 Mac 上出了本地构建**。这条评论立刻引来了怀疑：**darkwater** 说"你对这个很新、很不知名的软件似乎相当热情啊……"；**mirekrusin** 辩护说"它已经有 680 个 commit 了"，darkwater 回："在这个勇敢的 agentic 新世界里，680 个 commit 可能是两天？"；**e12e** 则指出，既然仓库里已经有一个关于 Mermaid 的 issue（#176），你不去那里留言或发 PR 就很奇怪。**nerptastic** 追问："请问什么叫'好的 agentic 工程实践'？据我所知我们还在探索各种在代码库里用 Agent 的技术，并没有形成公认的最佳实践。你是指'好代码'和'好文档'吗？"作者本人则友好回复，欢迎他开 issue 和原型 PR。
- **firasd** 给了少见的正面评价：从一开始就做双界面（人用 React UI，Agent 直接编辑 .md 配合 skills）是个有意思的想法，很可能是今后很多新软件项目的形态。他还顺带指出，在这波以 .md 为中心的 AI 生态里，表格被严重低估了。
- **jitl**（Notion 员工）在回应"这和我的 Notion 有什么区别"时说了一句耐人寻味的话："磁盘上的本地文件。我们也应该让 Notion 像这样真正支持磁盘上的真实 Markdown 文件。"
- **Brajeshwar** 分享了自己的替代玩法：用 tldraw offline 配合 Claude Code，把它当作 Excalidraw + Draw.io + 自由记事本的无限画布。作者回："今天才知道 Agent 能操作 tldraw 文档，谢谢分享。"
- **mianos** 推荐了 Joplin 及其 MCP agent 生态，理由是他跑 WebDAV 服务，能在多台 Mac 桌面和 Android 之间加密同步，且原生 Markdown。
- **4b11b4** 表达了一种根本性的不信任："我不让 Agent 编辑我正在写的文件——那是精神错乱的来源。我通过在工具调用前后打快照来强制这一点。"**effnorwood** 冷冷补了一句："互联网正在从第一性原理重新发明 vi 的锁文件。"
- **jkwang** 问了一个真正的技术问题：人和 Agent 同时编辑同一篇笔记时如何处理冲突？（**harmonious** 直接怀疑这条是机器人发的。）作者未在帖中作答。
- **monegator** 吐槽了审美："现在所有 vibecoded 网站都用的这种深褐色（sepia）配色方案，实在太、太累了。而且不用问，下一个'潮流'也会一样累人。"
- **FailMore** 借机推销自己的 smalldocs.org，被 **yencabulator** 直接点名："请别再刷你的项目了。"
