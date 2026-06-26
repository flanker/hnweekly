---
layout: article
title: "Deno Desktop：把 Deno 项目打包成桌面应用"
issue: 799
number: 24
category: code
original_url: "https://docs.deno.com/runtime/desktop/"
hn_url: "https://news.ycombinator.com/item?id=48626137"
date: 2026-06-26
---

## 文章摘要

Deno Desktop 是 Deno 推出的一项新功能，可以把一个 Deno 项目——从单个 TypeScript 文件到完整的 Next.js 应用——转换成自包含（self-contained）的桌面应用。编译出的每个二进制文件都打包了你的代码、Deno 运行时，以及目标平台的 Web 渲染引擎。

它的主要能力包括：**框架自动集成**——能自动检测并运行 Next.js、Astro、Fresh、Remix、Nuxt、SvelteKit 等主流 Web 框架，无需修改代码；既能以 release 模式运行生产服务器，也能以热重载方式运行开发服务器。**灵活的渲染后端**——开发者可以在操作系统原生 webview（生成更小的二进制文件）和打包的 Chromium（CEF，提供跨平台一致的渲染）之间选择。**直接通信**——与传统桌面框架不同，Deno Desktop 用进程内通道（in-process channels）实现后端与 UI 的通信，而非基于 socket 的进程间通信（IPC），从而降低延迟。**跨平台构建**——单台机器即可同时为 macOS、Windows 和 Linux 编译，无需在本地分别构建各平台后端。**内置更新**——框架自带基于二进制差分（binary-diff）的自动更新和回滚能力，只需一份简单的 manifest 文件。

上手非常简单：用 `Deno.serve()` 写一个基本的 HTTP 服务器，然后对该文件运行 `deno desktop`，编译出的二进制就会自动打开并连接到你的本地服务器。它被普遍视为 Tauri、Electron 的有力竞争者。

## HN 评论精华

- **echelon（高赞）**：尖锐指出系统 WebView 的关键弱点：Linux 上的 WebKitGTK"地狱般难用"，macOS 上不受支持的老机器其 WebKit 内核可能已经有 10 年以上的历史，平台差异造成"持续不断的痛苦"。他主张应当把 CEF 设为默认目标。

- **dfabulich**：进一步解释为什么老旧 WebView 是个问题——古老的 Safari 版本可能占到 macOS 桌面用户的 10-15%（而在 Web 端这类用户占比极小），这意味着开发者仍需做优雅降级（graceful degradation）。

- **leleat**：质疑 CEF 运行时的版本管理——当不同应用需要不同 CEF 版本时该如何处理？这究竟是真正解决了 Electron 的打包臃肿问题，还是只是把它换了个形式重演一遍？

- **jakelazaroff**：反问：既然开发者本来就要支持多种浏览器，那平台间的差异是否真的那么重要。

- 与 Tauri 的对比：**GeneralMaximus** 等人指出，Deno Desktop 提供了 Tauri 所缺乏的灵活性——可以根据需求在轻量的系统 WebView 和打包的 CEF 之间自由选择。**bel8** 则赞赏其 TypeScript 支持的优势，并发问：既然 Deno Desktop 选项更全面，为什么还要选 Tauri？

- **synchrone**：分享了用 Tauri（aptakube）在 Linux 上的正面体验，认为尽管有人对框架有顾虑，但性能其实够用。

整体讨论围绕着原生 UI 一致性 vs. 实用功能的权衡、对各平台 WebView 质量参差不齐的不满，以及"竞争有利于整个生态"的共识展开。
