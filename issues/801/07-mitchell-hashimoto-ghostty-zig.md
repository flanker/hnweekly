---
layout: article
title: "专访 Mitchell Hashimoto：聊 Ghostty 与 Zig"
issue: 801
number: 7
category: favorites
original_url: "https://alexalejandre.com/programming/interview-with-mitchell-hashimoto/"
hn_url: "https://news.ycombinator.com/item?id=48849292"
date: 2026-07-17
---

## 文章摘要

这是一篇对 Mitchell Hashimoto 的深度访谈。Hashimoto 曾一手打造 Vagrant、Packer、Consul、Terraform、Vault、Nomad、Waypoint 等 HashiCorp 系列工具，如今全职开发终端模拟器 Ghostty 以及 Vouch。访谈围绕终端、Zig 语言和开源哲学展开，几乎没有商业推销意味。

关于为什么做 Ghostty：Hashimoto 说他做了 15 年 CLI 应用却始终不理解终端模拟器内部如何运作，离开 HashiCorp 后想重新磨炼技术，同时满足三个愿望——搞 GPU 编程、做单机系统编程、玩一玩 Zig。最初的目标只是"能跑 vim、能自举编译、然后扔掉"，结果朋友们真的天天在用，Ghostty 才逐渐成型。他反对把终端推向极端（暗讽 Warp 的路线），认为浏览器、桌面和基于等宽网格的文本应用各有所长，终端应用的独特价值在于易实现、易组合、安全模型清晰。他批评 PTY 的带内信令（夹带转义序列的无结构字节流）是根本性问题，Nushell 只是又加了一层，真正值得学习的是 PowerShell 的结构化数据。他还透露了两个正在设计的协议：一个"n-screen API"支持无限后台屏幕并把 Neovim 标签页变成原生窗口标签，一个类似 OSC 8 的"按钮协议"让滚动到历史记录里的可点击元素依然有效。

关于开源哲学，他反复强调维护者对用户"零义务"——开源许可证第一行就是"as is, no warranty"。他鼓励人们多 fork：与其乞求别人改软件，不如自己维护补丁，这才是重获主动权。他把风险投资支持的开源项目视为"把开源当产品"的异类，认为它养出了一代人对"高度打磨、有官网和付费客服"的错误期待。想要保证和问责，就该付费购买软件。

关于 Zig，他说自己是通过写编译器补丁入门、深知其文化的，因此对 Zig 至今没有 1.0、频繁破坏性变更毫不介意。他盛赞 Zig 为提升编译速度甚至删减语言特性，认为 API 越改越好。他坦言在 AI 时代，向后兼容变得没那么重要——他演示几种上下文后让 AI"补完猫头鹰的其余部分"，90% 的迁移在他做饭时自动完成了，这对 Zig 严格的反 AI 政策颇具反讽意味。谈到 Rust，他直言"我不喜欢 Rust 的文化，没有更好的说法"，但强调这不代表 Rust 人是坏人、语言不好，只是他不想身处那个社区——"我也不喜欢足球"。他真正欣赏 Zig 的是"不加辩解地怪"，他愿意为敢做自己的人持续掏钱和用其技术。文末他给学 C 的人建议：语言不重要，重要的是理解计算机如何运作——CPU 调度、内存、缓存层级、文件系统，并去读标准库和高级语言的实现。

## HN 评论精华

评论区几乎被"Rust vs Zig 的社区文化之争"占据，同时也有对终端设计和开源理念的讨论。

- **skhameneh** 把 Hashimoto 那句"我不喜欢 Rust 文化"原封不动地反弹回去：他说自己想探索用 Zig 重写 Rust 项目，还跟 Loris Cro 聊过，结果对方满口都是 Rust 有多烂、完全没理解他的实际需求；他还吐槽 HN 上每有 Rust 帖子上热榜，紧接着就会冒出一篇"吹 Zig"的帖子，这情况持续了整整四年。
- **dbdr** 提供了最有洞见的调和视角：不同的人对 Rust 社区的体验可能截然不同——身处社区之外的人主要在语言口水战、"为什么不用 Rust 重写"这类语境里接触到 Rust 里最爱贬低他人的那一小撮；而他作为社区一员，感受到的是极其友善、包容新手、体贴周到的一群人。
- **rustystump** 拉高视角泼冷水：早在 Rust 出现前，大学里的 C++ 党就在鄙视用 Java 的新手；Rust vs C++ vs Zig vs Odin 之争"蠢透了"，是系统语言精英主义者一贯的老毛病，而绝大多数程序员正开开心心地用 Python、JS、Lua 把活干完。**geraneum** 补了一刀：可悲的是 Rust vs Zig 之争已被裹进"AI 狂热 vs 反 AI"的叙事，人们开始按阵营和信仰站队。
- **Jtsummers** 针对 Hashimoto"应该有更多 fork"的观点提出务实反驳：fork 之后你要么承担与上游同步的负担，要么放弃未来的上游成果；正因为如此人们才不愿 fork。不过他也认可，如果核心工程做得好（libghostty 正是为此），就能只 fork 边缘部分而共享核心。
- **sgarland** 是全文他唯一不同意的地方——Hashimoto 夸 PowerShell 的结构化数据。sgarland 坚持 CLI 程序默认应输出纯文本，好让人随手 grep/awk；他尤其恼火 AWS CLI 默认输出 JSON，因为读它的是人，最后还得再管道进 jq。
- **mowens3** 提供了一条难得的实战反馈：他上周无意间一直在用基于 libghostty 的 iOS 终端 app rootshell，它解决了他在 iPad 上用 Claude Code 的滚动和会话问题，是他第一次觉得 tmux 体验像原生 shell。
