---
layout: article
title: "LittleSnitch Linux 版：将 macOS 知名防火墙带到 Linux"
issue: 789
number: 15
category: show_hn
original_url: "https://obdev.at/products/littlesnitch-linux/index.html"
hn_url: "https://news.ycombinator.com/item?id=47697870"
date: 2026-04-10
---

## 文章摘要

Objective Development 公司将其在 macOS 上广受欢迎的网络监控工具 Little Snitch 正式带到了 Linux 平台。Little Snitch for Linux 是一款应用级防火墙工具，能够让用户清楚地看到计算机上每个应用程序正在与哪些服务器通信，并允许用户一键阻止不需要的连接，同时跟踪流量历史和数据使用量。

该工具的核心功能包括：连接视图（Connections View）展示当前和过去的网络活动，按应用程序分类，显示被规则和屏蔽列表阻止的内容，并追踪数据量和流量历史。用户可以通过最近活动、数据量或名称排序和过滤列表，轻松发现任何异常连接。底部的流量图表显示随时间变化的数据量，用户可以拖动选择时间范围来缩放和过滤连接列表。

在技术实现上，Little Snitch 使用 eBPF（Extended Berkeley Packet Filter）技术接入 Linux 网络栈，这是一种允许程序在内核层面观察和拦截网络活动的机制。一个 eBPF 程序负责监视出站连接并将数据提供给守护进程，守护进程跟踪统计信息、预处理规则并提供 Web 界面。值得注意的是，eBPF 程序和 Web 界面的源代码已在 GitHub 上开源。

该工具支持多种格式的屏蔽列表，包括每行一个域名、每行一个主机名、/etc/hosts 格式以及 CIDR 网络范围。推荐的屏蔽列表提供商包括 Hagezi、Peter Lowe、Steven Black 和 oisd.nl。此外，用户还可以编写自定义规则，针对特定进程、端口或协议进行精细控制。

安装后，用户可以在终端运行 `littlesnitch` 命令或直接访问 `http://localhost:3031/` 来打开 Web 界面。该界面还可以作为渐进式 Web 应用（PWA）安装。系统兼容 Linux 内核 6.12 至 6.19.0，需要 BTF 内核支持。默认情况下，Web 界面对本机上的任何进程开放，但可以配置身份验证来增强安全性。高级配置通过纯文本文件（如 web_ui.toml、main.toml、executables.toml）进行管理。

## HN 评论精华

1. **haswell（OpenSnitch 长期用户）**：作为多年的 OpenSnitch 用户进行了对比。他认为 Little Snitch 在展示进程连接的时间线方面确实更优秀，但 OpenSnitch 在弹出式新连接通知和单击允许/拒绝方面做得不错。OpenSnitch 可以进一步自定义规则，例如允许 ntpd 连接但仅限于 pool.ntp.org 的 123 端口。他认为 Little Snitch 真正领先的地方是在允许进程连接后展示其随时间的连接情况。

2. **abeyer**：对闭源安全软件的本质提出了深刻质疑。他指出，个人对商业许可证加上源码可用并没有意见——问题不在于价格，而在于用户被要求对每个网络连接进行中间人监控（MITM），而这个监控是由一个不透明的二进制文件控制的。他认为，选择构建这类需要如此深层访问权限的工具的开发者，应该倾向于保持透明。

3. **littlesnitch（官方账号）**：详细解释了 DNS 查找的技术细节。Little Snitch 守护进程必须在进程启动时就在运行才能可靠地识别它，因此建议安装后重启。关于主机名解析，他们监视离开机器的 DNS 查找来建立域名与 IP 地址的映射，但当应用程序使用 Unix 域套接字或加密 DNS（如 systemd-resolved）时，可能无法捕获这些查找。他们选择不像 OpenSnitch 那样拦截客户端库，而是等待社区反馈。

4. **sgc**：指出 OpenSnitch 可以在多个节点（不同机器）上运行守护进程，并通过一个集中的 UI 查看所有节点的连接情况，这对服务器管理非常有用。同时他提出了一个实际问题：如果不知道机器在和谁通信，信息就没有太大用处，建议 Little Snitch 提供一个模式来使用系统配置的反向 DNS 进行查找。

5. **dizhn（关于开源与付费的辩论）**：在一场关于开源软件是否应该免费的热烈讨论中，他认为不应该试图向那些选择以 MIT 许可证发布软件的开发者"规定条款"——很多开发者只是想纯粹利他地分享他们的工作。他承认靠 FOSS 谋生确实困难，但指出存在数百万个开源项目，有些已经存在了几十年，一定有另一种动机在驱动着这些开发者。
