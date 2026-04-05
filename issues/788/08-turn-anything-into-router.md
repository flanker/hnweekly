---
layout: article
title: "如何把任何东西变成路由器"
issue: 788
number: 8
category: favorites
original_url: "https://nbailey.ca/post/router/"
hn_url: "https://news.ycombinator.com/item?id=47574034"
date: 2026-04-03
---

## 文章摘要

本文由 Noah Bailey 撰写，背景是美国政府刚刚宣布了一项令人费解的政策，实质上禁止进口新的消费级路由器型号。作者借此机会撰写了一篇详细的教程，展示如何将几乎任何类似计算机的设备变成一台功能完备的路由器。作者本人多年来一直使用一台基于 Linux 的迷你 PC 作为自家路由器，运行极其稳定，唯一的问题只是磨损了一块 20 美元的 mSATA 硬盘。

在硬件选择方面，作者展示了多种可行方案：从专用的被动散热迷你 PC，到用废旧零件拼凑的"垃圾堆"路由器（一台 Celeron 3205U 双核 1.5GHz 的旧笔记本配 USB 网卡和 WiFi 适配器），甚至连一台从垃圾桶捡来的 ThinkPad T60 配上 ExpressCard 转 PCIe 桥接器、一块捡来的无名网卡、一台 10 美元的思科 2960 交换机和一台坏了 WAN 口的 D-Link 路由器（仅当 AP 用），都能完美胜任路由器的工作。关键要求只有两点：能运行 Linux，有至少两个网络接口（USB 网卡也行）。

软件配置方面，作者使用 Debian 系统（Alpine Linux 也可以），整个系统只需约 250 个软件包。核心组件包括：hostapd（创建 WiFi 网络）、dnsmasq（DNS 和 DHCP）、bridge-utils（桥接网口）。文章详细介绍了完整的配置步骤：首先通过 systemd 的 link 文件为网络接口设置持久化的简短名称；然后配置 hostapd 创建 WiFi 接入点；接着配置网络接口，将有线 LAN 口和无线网卡桥接到同一个 br0 桥接设备上；开启 IPv4 转发；使用 nftables 配置防火墙规则和 NAT（网络地址转换），包括拒绝所有外部入站流量、允许内部到外部的新连接、以及 masquerade 伪装；最后配置 dnsmasq 提供 DHCP 和 DNS 缓存服务。

作者还介绍了一个"加分项"：如果设备有串口，可以配置串口控制台用于远程管理，这在企业网络设备中很常见。文章最后列举了该路由器可以进一步扩展的功能，包括 VLAN 分段、监控告警、IPv6、VPN、动态路由协议（BGP 等）、入侵检测等，但建议不要在路由器上安装过多软件，而是将流量转发到 DMZ 或 VLAN 中的其他设备处理。整篇文章的核心理念是：路由器没有什么特别的，它们本质上都只是计算机。

## HN 评论精华

**关于实用工具推荐：** 用户 dlenski 推荐了 `create_ap` 这个 shell 脚本，它能以极少的依赖（hostapd、iw、dnsmasq）让任何 Linux 电脑变成 WiFi 路由器，非常简洁实用。

**路由器的本质：** smashed 指出，这篇文章展示的基本路由概念其实正是 Docker 网络、虚拟机 NAT、Android 热点等背后使用的完全相同的 Linux 内核特性。doubled112 则观察到，把防火墙、路由器、交换机和无线接入点的功能打包进一个"路由器"设备中，让很多用户对每个组件的实际功能产生了混淆。

**怀旧与实践：** bluedino 分享了用废旧电脑搭建路由器的怀旧经历——一台 100MHz 的奔腾处理器加两块网卡就能为整栋楼提供路由服务，使用的是早期 Linux 文档中的 IP-masquerading 技术。

**教育价值 vs 成熟方案：** LatticeAnimal 建议使用 OPNsense/pfSense 这类成熟方案，它们提供自动更新和内置的 Wireguard、Suricata 等功能。但 lucasay 反驳说，从零开始搭建的方式在教育价值上远超推荐现成工具，能真正理解路由原理。jasonjayr 也指出，GUI 抽象层往往与底层 Linux 概念不一致，各平台的术语和配置方式也五花八门。

**WiFi 芯片推荐：** eptcyka 询问可靠的 AP 模式 WiFi 芯片，dlenski 推荐了较老的 Atheros ath10k 系列网卡（约 10 美元，固件稳定），同时警告 Intel 故意限制了 5GHz AP 功能，尽管它支持 2.4GHz。

**高级调优：** Bender 强调了路由器专用的内核调优参数，如禁用 `ip_early_demux` 以及使用 CAKE 进行流量整形，可以显著改善游戏场景下的延迟和抖动。

**可复现配置需求：** solarkraft 寻求"牲口而非宠物"式的路由方案，希望能用版本控制管理配置。社区建议了 NixOS、VyOS 和 OPNsense 作为可复现配置的选择。
