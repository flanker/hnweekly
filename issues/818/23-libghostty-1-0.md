---
layout: article
title: "Libghostty 1.0：把终端核心做成稳定的嵌入式库"
issue: 818
number: 23
category: code
original_url: "https://mitchellh.com/writing/libghostty-1-0"
hn_url: "https://news.ycombinator.com/item?id=50399933"
date: 2026-11-13
---

## 文章摘要

Mitchell Hashimoto 发布 libghostty-vt 1.0，将 Ghostty 的终端模拟核心作为可独立使用的库提供。它用 Zig 实现，通过 C ABI 与其他语言和应用连接；这里的「终端核心」主要负责终端状态与控制序列处理，不能直接等同于包含窗口、字体和完整界面的终端应用。对编辑器、IDE 或远程工具而言，价值在于复用经过实际应用打磨的模拟实现，而不必重新处理大量兼容性细节。

公告花了很大篇幅说明稳定性承诺如何落实：不透明句柄、可扩展枚举与结构体设计，为未来扩展留下空间；持续集成检查 ABI 变化，Linux 使用版本化符号。库有独立发布流程，提供包含依赖的源码包、静态或共享链接方式，以及 CMake 和 pkg-config 集成。运行时依赖被尽量压低，也支持调用方提供分配器。不同平台的构建与运行测试范围不能混为一谈，性能示例同样是作者的特定环境测试。

应用进度也要逐项区分。公告介绍 JetBrains 已采用这项技术，Neovim 的替换方案则是获批准、等待该版本发布后合并，KDE 还处于实验阶段。不能把这三种状态统称为「全部已上线」。本文依据发布说明整理，未编译库或复现大规模终端测试；稳定 ABI 的设计和检查机制值得关注，但实际集成仍需测试调用方的线程、内存与渲染边界。

## HN 评论精华

- [nektro](https://news.ycombinator.com/item?id=50400677)：澄清库由 Zig 编写但暴露 C ABI，回答了读者将实现语言与调用接口混淆的问题。

- [AnggaSP](https://news.ycombinator.com/item?id=50400799)：以个人 JetBrains 使用体验指出终端输出和转义序列处理有所改善，并非对所有集成场景的统一测评。

- [Conscat](https://news.ycombinator.com/item?id=50401755)：分享 Ghostel 在 Emacs 中的使用感受，显示这类终端核心也能服务于 IDE 之外的编辑器生态。
