---
layout: article
title: "聊聊我的「码字机」：一台只为写作而生的旧笔记本"
issue: 795
number: 6
category: favorites
original_url: "https://veronicaexplains.net/my-first-writerdeck/"
hn_url: "https://news.ycombinator.com/item?id=48250144"
date: 2026-05-29
---

## 文章摘要

作者饱受数字干扰之苦，于是动手打造了一台「**码字机（writerdeck）**」——一种「专门用于写作、隔绝现代互联网干扰」的设备，理念是让技术「只把一件事做到极好」。她没有买新硬件，而是把一台用了六年的 **System76 Galago Pro** 笔记本翻新利用：键盘手感出色、哑光屏在日光下可读、性能虽老但够用，还自带电池便于携带。

软件栈是整篇文章的精华，走的是极简纯命令行路线。系统装的是 **Debian Trixie**，**完全不装 X11 或 Wayland**，没有任何桌面环境。关键软件包括：用 `network-manager`（配合 `nm-tui`）连 Wi-Fi；用 `kmscon` 替代默认控制台以支持可缩放字体和 16+ 色；主力编辑器是 **Neovim**（搭配 `blue` 配色）；用 `tmux` 做终端复用和状态栏；`vim-vimwiki` 提供个人 wiki 功能；`acpi` 看电量、`light` 调亮度、**Syncthing** 做文件备份与同步。

为了让体验更顺手，她还做了不少定制：通过修改 systemd 服务实现自动登录；自定义 `.tmux.conf`，把绿色状态栏放在顶部；在 tmux 里把 F8/F9 绑定为亮度热键；开机自动进入 `VimwikiIndex`；并在 Neovim 中开启自动换行以提升舒适度。整套方案的动机非常清晰——拥有「一台只把一件事做到极好的设备」，以此培养专注、有意识的写作习惯。

## HN 评论精华

评论区一半在怀旧、一半在安利各自的「无干扰写作」方案：

- **jlundberg**：感叹「一个朴素的 Linux 终端带来的减压效果不该被低估」——不只是写作，连日常 shell 操作也是，他特别钟爱用树莓派来干这事。
- **ramses0**：力荐用 **zellij** 取代 tmux，「好用太多了」，并列举其优势：默认不与 Ctrl-A 冲突、支持鼠标、浮动窗口、可保存布局、堆叠标签页和会话管理器；他强调「用 Rust 写的」反而是个噱头，不是重点。
- **dragonfax**：说这让他想起 80 年代到 90 年代初 DOS 上的文字处理，那种 WYSIWYG 之前的体验。**vidarh** 接话，认为那套配色「非常有 WordPerfect 5.1 的味道」，怀疑是有意或间接的致敬。
- **itrunsdoomguy**：经典 HN 梗——「很棒的机器，可惜没法跑 Doom。」**Shellban** 立刻甩出 `doom-ascii` 的 GitHub 链接接梗。
- **daoboy**：表示自己在「望穿秋水」地等一台完美的电子墨水（eink）写作设备，已有一套基于 Obsidian 和机械键盘的好用配置，只差下一代 eink 屏；Boox Note Max 曾「无限接近」，却几乎一上市就被砍掉了。
- **drakonka**：分享自己用 **Onyx Boox Palma** 做便携 eink 起草设备的经验，并写了博客介绍。她认为小屏只要保持「这是起草工具、而非编辑或结构化工具」的心态就完全够用——她的起草过程几乎是意识流，一次只需看到一两句话。
- **stavros**：吐槽作者抱怨键盘——「也许你该选个更好的？」**drakonka** 回怼：「我选的就是我想要的键盘。」
