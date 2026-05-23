---
layout: article
title: "Show HN：Rmux——给 Agent 用的 tmux，自带 Playwright 风格 SDK"
issue: 794
number: 20
category: show_hn
original_url: "https://github.com/helvesec/rmux"
hn_url: "https://news.ycombinator.com/item?id=48219918"
date: 2026-05-22
---

## 文章摘要

**Rmux** 是 helvesec 用纯 Rust 写的一个**可编程终端多路复用器**。它对 tmux 完全兼容——90 条 tmux 命令全实现、tmux.conf 可以直接迁移过来——但在此之上加了一层**类型化的 Rust SDK**，让 agent 或者自动化脚本能像 Playwright 操控浏览器那样去操控终端。

差异最简单的概括是：**tmux 是给人用的；Rmux 是给人和 agent 都能用的**。SDK 模型抄了 Playwright 的范式——你拿到一个 session，从 session 拿到 pane，对 pane 调 `send_text`、`snapshot`、`wait_for`，全部异步。下面是官方示例：

```rust
let rmux = Rmux::builder().connect_or_start().await?;
let session = rmux.ensure_session(
    EnsureSession::named("work").detached(true)
).await?;
let pane = session.pane(0, 0);
pane.send_text("command\n").await?;
let snapshot = pane.snapshot().await?;
```

**架构上**是 daemon 模型——客户端断开连接 session 依然存活（这是 tmux 的核心价值），底层用 Tokio 异步运行时，PTY 在 Linux/macOS 上走 Unix PTY、在 Windows 上走 ConPTY，传输层用 Unix sockets 或 Windows Named Pipes。安全策略上较高层 crate 写了 `#![forbid(unsafe_code)]`。还集成了 Ratatui widget，方便嵌入 TUI 应用。

定位非常直白——"**agent、headless CLI workflow 和人，都是一等公民**"——目标场景包括多 agent 编排、Playwright 式的终端测试、SSH 中长寿 agent 管理、人机混合 workflow。目前版本 0.2.5，作者自述是 fresh public preview，预期有 bug。

## HN 评论精华

- **SwellJoe / luizfwolf / kosolam**（共同质疑）：三个不同评论者抛出了同一个问题——"**这跟现有 tmux + send-keys + capture-pane 比，到底多了什么？**"作者主要的回答是类型化 SDK 和结构化快照——但对人类用户来说，确实 tmux 的 CLI 已经够用了。

- **btown**（替这个项目背书）：他认为当前关于"agent 强到不需要看终端"的炒作错了——"**很多重要用例是长期运行的进程、需要在多个 agent 间共享、需要人随时介入**"。Rmux 这种设计天然就服务于这些场景。

- **kloud**（架构设计的反思）：很喜欢"agent 工具继续把 tmux 当事实标准用、人类用更好的 UI"这个方向。但他抛出一个有趣的批判："**tmux/rmux 的设计本身可能就是次优的**——它把会话持久化和窗口管理耦合到了一起。"作为对比他举了 **abduco**（先驱）和基于 libghostty 的 **zmx**——把这两件事解耦。

- **pama**（emacs 派打过招呼）：他自己用 emacs daemon 做所有 shell 复用——"**agent 不需要解释，知道怎么用 emacsclient 创建、读取、给命名 buffer 发输入。**"Elisp 操控窗口是举手之劳。给老派用户的建议很犀利：与其再造 tmux，不如让 agent 学 emacs。

- **Sirental**（视觉警察）：第一眼就吐槽——"**网站太明显是 Claude 写的**——经典的'脉动绿点的 pill 表示活跃中'是个 claudism。"AI 写的 landing page 正在变得有指纹。

- **florianist**（事实校对）：网站上写"切换到 Rmux 因为 tmux 是 C++ 写的"——**tmux 实际是 C 写的**，不是 C++。一个小但显眼的失误。

- **cultofmetatron**（终端多路复用器一周三换党）："**一周前我还在用 cmux，但只在 macOS 上能用、不支持远程；然后换 herdr 还不错但不太会管 pane；现在又一个新的终端多路复用器——我已经晕车了。**"这条评论概括了 2026 年这个赛道的现状——所有方案都不完美，rust 重写中。

- **chaoticbeard**（关键问题）："**支持 sixel 或图片吗？**"作者没在主页提，这对开发场景体验差别很大。

- **0x1ceb00da**：从 git bash 装失败——`rmux install: unsupported OS: MINGW64_NT-10.0-26200`。Windows 支持的边角还需要打磨。

- **cat-whisperer**（同方向探索）：之前用 ghostty 的 AppleScript 把它封成一个 Claude skill，工作得很顺；但 Rmux 这种纯可编程接口在跨平台上更有未来感。
