---
layout: article
title: "Homebrew 6.0.0 发布"
issue: 797
number: 16
category: show_hn
original_url: "https://brew.sh/2026/06/11/homebrew-6.0.0/"
hn_url: "https://news.ycombinator.com/item?id=48490024"
date: 2026-06-12
---

## 文章摘要

Homebrew 6.0.0 是一次重要的大版本更新，重点强化了安全性与性能。最受关注的几项新特性：

- **Tap 信任机制**：第三方 tap 现在必须被显式信任才能执行其代码，以防范"恶意或被攻陷的 tap"带来的风险；官方 tap 默认仍受信任。
- **内置 JSON API 成为默认**：更快、更小的内置 JSON API 由原来的可选项变为默认，将元数据合并为单次下载，减少网络请求、加快更新速度。
- **Linux 沙箱化**：构建、测试和 postinstall 阶段现在通过 Bubblewrap 在 Linux 上沙箱运行，与 macOS 的安全实践对齐。
- **Install Steps 框架**：新增一套仅含字面量的 DSL，用于表达常见的 postinstall 行为，避免安装时执行 Ruby，提升安全性与性能。

性能与体验改进方面：`brew leaves` 提速约 30%、启动更快、bottle 下载并行化；开发者默认启用"ask 模式"，在变更前展示依赖摘要；`brew bundle` 支持并行安装、npm 与 krew、改进清理逻辑；Cask 可被 pin、Linux 支持 AppImage；新增 `brew exec`（类似 `npx`）和 `brew as-console-user`（用于 MDM 环境）。

此版本还修复了三个严重漏洞：POST 下载策略中 HTTPS 到 HTTP 重定向绕过、macOS 安装器中通过 Git hook 的 root 代码执行、macOS 安装包 plist 提权问题。

重大变更与弃用：Intel macOS 支持即将终结——2026 年 9 月 x86_64 降为 Tier 3（无 CI 与 bottle），2027 年 9 月彻底移除；`HOMEBREW_USE_INTERNAL_API` 等环境变量被弃用；未通过 Gatekeeper 检查的 Cask 将于 2026 年 9 月禁用。平台扩展上新增了 macOS 27（Golden Gate）初步支持、通过 `winget` 的 Windows 支持，以及更完善的 WSL 与 Linux cask 支持。

## HN 评论精华

**hk__2**：感谢 Mike McQuaid 维护 Homebrew 超过 16 年，至今仍持续推出新功能，对这种长期坚守表示真诚敬意。

**maxloh**：指出 Homebrew 解决了 Linux 的一个痛点——大多数包管理器无法区分用户安装与系统自带的包，导致清理困难，而 Homebrew 的做法让工作环境更整洁。

**PufPufPuf**：分享自己改用 Mise，认为它避开了曾困扰其打包体验的依赖复杂度，优势在于版本零滞后、可直接访问上游源。

**klodolph**：从 Nix 回归 Homebrew，原因是后者包维护更好、对 macOS 支持更佳、体验更顺手，尽管承认失去了可复现性。

**broxit**：对 Homebrew 的快速分发提出供应链安全担忧（举例某包在上游发布 85 分钟后即可获取），希望引入冷却期机制，并以 npm 式攻击为前车之鉴。

**0xbadcafebee**：批评强制升级且缺乏 pin 能力，已转向 Mise 和 MacPorts 以获得版本控制的灵活性、避免意外破坏。
