---
layout: article
title: "LocalSend：开源跨平台的 AirDrop 替代品"
issue: 792
number: 16
category: show_hn
original_url: "https://github.com/localsend/localsend"
hn_url: "https://news.ycombinator.com/item?id=47933208"
date: 2026-05-01
---

## 文章摘要

LocalSend 是一款免费、开源、完全跨平台的本地文件与消息共享工具，定位是"对标 Apple AirDrop 和 Google Nearby Share / Quick Share"。截至本次 HN 讨论时项目已经在 GitHub 上累积了 80.2k stars，是开源界少数真正能解决"在不同生态系统设备间快速传文件"这一痛点的方案。

**核心技术栈**：
- 主体使用 **Flutter** 构建，UI 层一套代码覆盖移动 + 桌面全平台
- 关键性能路径用 **Rust** 实现
- 通信协议是基于 HTTPS 的自研 REST API，每台设备启动时**动态生成 TLS/SSL 证书**完成端到端加密
- 网络发现完全基于本地局域网（mDNS / 多播），**不依赖任何云端服务器**，整个传输过程不出 LAN

**支持平台**（覆盖度远超 AirDrop / Quick Share）：
- 移动端：Android 5.0+、iOS 12.0+、Fire OS
- 桌面端：macOS 11 Big Sur+、Windows 10+、Linux（需要相应的 portal 依赖）
- 旧版兼容：Windows 7 支持已在 v1.15.4 终止

**分发渠道**也极其丰富：除了官方应用商店（Google Play、App Store、Amazon Appstore），几乎覆盖了所有主流包管理器 —— Winget、Homebrew、Flathub、F-Droid、Snap、AUR、Chocolatey、Nixpkgs；以及直接下载形式的 DMG、AppImage、DEB、TAR、便携 ZIP。

**网络配置要点**：
- 必须放行 TCP/UDP 端口 **53317**
- 路由器需要关闭 AP isolation（部分商用 / 公共 WiFi 默认开启此功能会让设备互相不可见）
- 支持便携模式：在可执行文件同目录放一个 `settings.json` 即可
- 支持 `--hidden` 启动参数实现后台静默启动

**协议透明度**：项目在另一个独立仓库中维护了完整的协议规范文档，社区可以独立实现兼容客户端。这一点是相对 AirDrop（封闭、私有、苹果生态锁定）的重要差异化卖点。

**与竞品的本质差异**：
- AirDrop 把用户锁在苹果生态
- Quick Share 依赖 Google 服务，且部分体验需要联网或同账号
- LocalSend 完全离线、跨任意操作系统、源代码可审计、社区可定制

**社区运作**：翻译走 Weblate 平台，开发通过常规 GitHub PR 流程；Discord 与 GitHub Discussions 双线社区。

## HN 评论精华

- **Unicironic**：体现了 LocalSend 的群众基础 —— "切换到 Linux 之后，这是我装的第一批应用之一。它真的让我对开源 app 的可能性有了非常好的印象。"

- **viktorcode**：抛出 AirDrop 最核心的优势之一 —— "AirDrop 最方便的是它会自动选择两台设备之间最快的可用通道，而且不需要双方都连在同一个 WiFi 上。我好奇有没有替代品做到这一点。"

- **gonzalohm / subscribed / burner35534** 这条线讨论了 Google Quick Share：**subscribed** 表示 Quick Share "完全没用过，从来没成功过一次。"试过三台手机其中两台同账号，确认操作都没问题，也只是无限等待。最后他说："我用红外、蓝牙都比 Quick Share 顺；甚至 `python -m http.server` 起一个本地 HTTP server 都比 Quick Share 靠谱。" **burner35534** 也表示 AirDrop 在他这里 "成功率只有 25%"。

- **lxgr**：对 AirDrop 替代品提出了一份隐性的"反垃圾标准" —— "我们需要一份 AirDrop 替代品的 spamsolutions.txt 清单。这个项目就在 '不能要求双方接入同一个 WiFi' 这一条上失败了。"（指 LocalSend 仍依赖局域网，不像 AirDrop 可以走 P2P WiFi Direct）。

- **mikae1**：分享了另一个跨平台方案 —— "好东西，但被 KDE Connect 替代了。Connect 同时支持 iOS、macOS、Android、Linux，全都行。" **tryptophan** 补充："我喜欢 KDE Connect，但它每个月左右就莫名其妙坏一次，怎么也查不出原因，过一周自己又好了。"

- **dmak / OGWhales / tonyedgecombe** 这条 AirDrop 故障吐槽支线很有代表性 —— OGWhales 描述了"能看到对方设备但发起传输时另一端就是不弹窗"的痼疾，发现率约 50%；toggle AirDrop 开关大概能修好 70% 的情况。tonyedgecombe 推测这是因为 AirDrop 的初始化握手走蓝牙，蓝牙信号差就完蛋。

- **d3Xt3r / michaelscott / inquirerGeneral** 这条则导向真实使用场景：**michaelscott** 强调 LocalSend 在跨生态传 100MB-3GB 视频时优势巨大，"从苹果设备传到安卓常常要 2FA 登录某网盘，或者翻出一年用一次的硬盘。LocalSend 两三次点击搞定，而且非常稳定。" **internet_points** 表示曾经孩子从 iPad 绘画 app 传图到 Windows 笔记本踩坑半天，"早知道有 LocalSend 就好了。"

- **Scarbutt** 调侃苹果："傻苹果，他们应该直接把 AirDrop 砍掉，告诉用户你就该走 WhatsApp 或邮件传文件。"

- **gumboshoes** 提醒大家这并非新项目："这玩意已经在 HN 上发过很多次了。" 附上了 LocalSend GitHub 域过往出现的链接列表。

- **gonzalohm** 顺手澄清术语："Quick Share 就是 Google 那个对标 AirDrop 的东西。"
