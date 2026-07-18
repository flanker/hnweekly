---
layout: article
title: "极致考据：《侏罗纪公园》里的电脑"
issue: 801
number: 2
category: favorites
original_url: "https://fabiensanglard.net/jurrasic_park_computers/index.html"
hn_url: "https://news.ycombinator.com/item?id=48915709"
date: 2026-07-17
---

## 文章摘要

Fabien Sanglard 以近乎偏执的细致，逐一考证了 1993 年电影《侏罗纪公园》中出现的每一台电脑与每一段软件。据文章统计，SGI、Apple 等厂商为该片出借了价值 172.5 万美元的硬件，换算成 2026 年的币值约合 400 万美元。

**便携设备**：开场戏中出现的是 Apple PowerBook 100（16 MHz 摩托罗拉 68000、2-8 MB 内存、单色 LCD）。此外还有摩托罗拉 Envoy PDA——一台可折叠、带无线能力的设备，其出现之谜由编者注揭晓：frogdesign 创始人曾在飞机上向斯皮尔伯格展示过它的原始模型。

**工作站**：Ray Arnold 的座机是 SGI R4000 Indigo，运行名为 Earthwatch 的 3D 飓风动画软件（原为气象学家开发）。Dennis Nedry 的机器则是强大的 SGI IRIS Crimson（100-150 MHz），因太吵实际拍摄时从未开机。Nedry 桌上还有两台、Arnold 桌上一台 Macintosh Quadra 700。

**超级计算机**：控制室里可见 5 个节点的 Thinking Machines CM-5——1991 年发布，1993 年被视为全球最强。其标志性的红色 LED 面板闪烁其实是纯装饰、随机图案。由于 Cray 拒绝出借设备，Thinking Machines 因这次植入广告受益。片中 Dennis 台词里提到"8 台 connection machines"，准确对应了 CM-5。

**存储**：共 7 台 PLI Mini Array（Nedry 桌上 5 台、Arnold 桌上 2 台），每台 1 GB、售价 3598 美元，在 1993 年堪称海量。

软件方面，机器运行 System 7.0.1 与 IRIX；著名的"这是个 Unix 系统"（It's a Unix system）场景用的是 SGI 实验性文件浏览器 `fsn`；假视频会议用 QuickTime 播放器伪造；IRIX 系统监视器 `gr_osview` 也有出镜。Nedry 的控制系统"Nedryland"由 Michael Backes 的四人图形团队用 Macromedia 动画软件耗时半年制作，源码用 Pascal（MPW 环境）写成并在屏幕上可见；锁定界面"白兔"（whte_rbt.obj）则用 After Effects 制作。为解决 CRT 刷新率与 24fps 胶片相机的同步问题，剧组甚至专设了"24 帧电脑同步工程师"John Monsour。

## HN 评论精华

- **smaili** 引用 PowerBook 100 的规格（2-8 MB 内存）后感慨："如今一首 MP3 就比它的全部内存还大，好好体会一下。"这条评论引出了一大串关于早年硬件的怀旧接龙。

- **decimalenough** 调侃 HN 上很多人是用 Commodore 64 长大的——64KB 内存，约等于 2026 年一个网站 favicon 的大小；而"真正的黑客是亲手在石头上凿出 0 和 1 的"。随后 **Intermernet**、**chasd00**、**jsymolon** 等人接力玩梗（"我们得从稀薄的氢氦云里挑拣 0 和 1""正氢与仲氢的反向自旋会把 1 搞乱"），成为整条讨论最有趣的段落。

- **windenntw** 从技术上纠正了"内存装不下 MP3"的误解：真正的瓶颈不是内存（可以从硬盘或软盘流式读取），而是解码所需的算力——在 22kHz 单声道下解码 MP3 大约相当于 68030@50MHz，约为 68000@16MHz 的 5 倍。**tremon** 补充，当年主流是 128kbps，一首流行歌也就 3.5 MB。

- 一场关于"用 SGI + 老式 Mac 管理主题公园是否合理"的讨论展开。**yjftsjthsd-h** 觉得 Mac 与 UNIX 工作站混搭很怪；**LeoPanthera** 与 **mike_hearn** 则指出，在当年（乃至今天）Mac 连接 UNIX 服务器的混合系统很常见，Mac 当时是"不需要 Unix 但想要好用 GUI 的人"的高端工作站。**RodgerTheGreat** 补充 Quadra 700 可运行 A/UX 3.0，能让 Mac 与 UNIX 工作站较好地互操作。

- **aa-jv** 分享了亲历者视角：早期 Web 就是建立在 Mac 连接 SGI 机器之上的，SGI Indy 是很受欢迎的多用途/建站平台。这引出与 **ahartmetz** 的争论——后者认为 SGI 主要是昂贵的 3D 图形工作站，服务器市场是 Sun、HP-PA、IBM 的天下。aa-jv 坚持己见，并透露自己曾用 SGI 系统为多家大型媒体公司搭建早期网站，还带出了 SGI "WebFORCE"品牌的一段历史。

- **ColdStream** 讲了个生动的段子：他曾在一个被称为"洋葱"的 IT 部门工作——越往房间深处走，系统越老，最老的就是台 SGI，靠一群"灰胡子"老工程师撑着整套系统运转，侧面印证了老硬件在现实中长期服役的真实性。
