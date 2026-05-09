---
layout: article
title: "WhatCable：状态栏小工具，一秒看懂你手里的 USB-C 线"
issue: 793
number: 16
category: show_hn
original_url: "https://github.com/darrylmorley/whatcable"
hn_url: "https://news.ycombinator.com/item?id=47972511"
date: 2026-05-08
---

## 文章摘要

USB-C 是当代最让人困惑的接口：同一种长相的线，可能是 USB 2.0 480Mbps、可能是 USB 3.2 10Gbps、可能是 Thunderbolt 4 40Gbps、还可能是 USB4 80Gbps；功率从 15W 到 240W 都有，DisplayPort、PD、e-marker 各有讲究。**WhatCable** 是开发者 darrylmorley（HN 用户名 sleepingNomad）做的 macOS 状态栏小工具，目标就是把 USB-C 接口背后真正在发生什么"翻译"成人话——你只要瞄一眼菜单栏，就知道现在这根线是什么规格、正在以什么协议工作、是不是限制了你的充电速度。

工具能读出每个 USB-C 端口的：**线缆身份**（速率档位 USB 2.0 到 80Gbps、电流 3-60A、e-marker 厂商信息）；**电源协议商**（5V/9V/12V/20V 等 PDO 列表与实时协商状态）；**充电诊断**（显式提示"线缆限制了充电速度"或当前实际功率）；**已连接设备列表**（按物理端口分组）；**信任信号**（e-marker 数值异常时弹橙色警告，提示可能是假货）；**当前激活的传输协议**（USB 2/3、Thunderbolt、DisplayPort 是否在用）。

技术实现上，作者只用了 4 个 IOKit 服务族（端口控制器、电源源、PD Identity、XHCI 控制器），不需要任何特殊权限、私有 API 或后台守护进程，也因此 App Store 上架不可能。要求 **macOS 14 Sonoma 以上 + Apple Silicon**——Intel Mac 走不通是因为 Intel 南桥不暴露所需信息（GitHub issue #12 已"wontfix"）。除菜单栏 GUI 外还附带一个 `whatcable` CLI，支持 JSON 输出和 watch 模式，方便集成到脚本里。安装可下载 zip 拖入 Applications，或 `brew install --cask whatcable`。所有处理都在本地完成，更新检查只访问 GitHub Releases API。

局限也很明确：没有 e-marker 芯片的线（多数低于 60W 的便宜线）显示信息很有限；软件只能读取芯片自报的数据，无法验证线径或屏蔽是否真符合规格；少数线必须接上对端设备才会暴露 e-marker。

## HN 评论精华

- **"我的线被插反了？"**（**MrBuddyCasino**）：最高赞评论提到工具把 Thunderbolt 线显示成"上下颠倒"，但物理上 USB-C 本来就可以正反插。作者后来修复了这个误导性提示。**justusthane** 解释这其实是物理事实——USB-C 内部有专门的"通道反转"芯片（lane swapping IC）来处理两种插入方向，工具本意是把这一层暴露出来，但对普通用户没意义。**globular-toast** 评价："这是 USB-C 的实现细节，作者本不该秀给用户看。"

- **Linux 等价物**（**n3storm**）：有人问能不能给 Linux 做一个，**WillAdams** 推荐了 [doug-gilbert/lsucpd](https://github.com/doug-gilbert/lsucpd)，是基于 lsusb 的扩展，能解析 PD 信息。

- **Intel Mac 不支持**（**brk / bkummel / avidiax**）：多名用户报告 Intel MBP 显示 "No USB-C Ports Detected"。作者承认 Intel 南桥不暴露所需数据，issue #12 标 wontfix。社区里有评论调侃："...And claude fixed it already"，链接到一个由 LLM 生成的修复 commit。

- **该不该是状态栏 App**（**kmmbvnr_ / Someone**）：很多人觉得这种"偶尔查一次"的工具不该常驻菜单栏，应该做成普通 App 或 Spotlight 可启动的命令。**sagacity / poisonborz** 围绕这个争论了很多——也引出了 Mac 用户长期的菜单栏拥挤问题，推荐了 [Ice](https://github.com/jordanbaird/Ice) 和它的活跃 fork [Thaw](https://github.com/stonerl/Thaw) 来折叠图标。作者 sleepingNomad 表示"会考虑加普通窗口模式"，并在评论里就上线了独立窗口模式。

- **Homebrew 支持**（**Alifatisk**）：发帖几小时后就被加上 cask，作者回复"已加"。这是 Show HN 高效迭代的好例子。

- **e-marker 能否查假货**（**ricardobeat**）：HN 上不少人记得"亚马逊大多数 USB-C 线都谎报规格"那篇分析。**avidiax** 指出工具只能读到芯片自报的内容，真正的线径、屏蔽必须破坏性测试或用温度探针才能验。**thenthenthen** 分享 SDR 工程师视角：测试过 6 个品牌的 USB-C 线，最干净的信号反而是 20 美元的苹果原线。

- **LTT 自家线**（**BiteCode_dev**）：评论里多次提到 Linus Tech Tips 出的 USB-C 线，规格直接印在线身两端，被认为"应该立法强制"。
