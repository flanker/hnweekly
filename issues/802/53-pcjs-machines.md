---
layout: article
title: "PCjs Machines：浏览器里的复古计算机博物馆"
issue: 802
number: 53
category: fun
original_url: "https://www.pcjs.org/"
hn_url: "https://news.ycombinator.com/item?id=48992323"
date: 2026-07-24
---

## 文章摘要

PCjs Machines 是一个用纯 JavaScript 实现的复古硬件模拟项目，由 Jeff Parsons 开发维护，源码托管在 GitHub 上、采用 MIT 许可。作者的自述很朴素：用 JavaScript 模拟"我在 1970 和 80 年代长大时接触到的一小批硬件和软件"。这实际上是一项数字保存工程——所有机器都直接在浏览器里运行，桌面和移动端都能访问，不需要安装任何东西。

模拟的硬件谱系相当宽。个人计算机方面覆盖了 IBM PC 家族的多个型号（5150、5160 XT、5170 AT）、COMPAQ 系列（Portable、DeskPro 286/386、LTE/286），以及 AT&T 6300、CDP MPC 1600、Zenith Z-150 等当年的兼容机。小型机与处理器方面有 DEC 的 PDP-10 和多个 PDP-11 变体、VT100 终端、Intel 8080、OSI Challenger 1P。此外还包括 TI-57 / TI-55 / TI-42 可编程计算器、Space Invaders 街机，以及一些基于 LED 显示的小玩意（生命游戏、Lite-Brite、文字滚动屏）。

软件库同样丰厚。操作系统一栏能找到 CP/M-86、各版本 DOS、OS/2、从 1.0 到 95 的 Windows、多种 UNIX、MINIX 和 QNX。应用软件包括 VisiCalc、Lotus 1-2-3、微软的 Word / Excel / Multiplan、dBASE、WordStar。游戏有 Zork 系列、Wolfenstein 3D、Commander Keen、Dune II、俄勒冈之旅（The Oregon Trail）、Rogue。开发工具则涵盖 C、Pascal、COBOL、Fortran、BASIC 的编译器，以及汇编器和调试器。站点还附带大量参考资料：技术手册、数据表、BYTE 杂志存档和编程书籍。

这个项目在 HN 上的热度（214 分）与其说来自技术新颖性，不如说来自它作为"可运行的历史"所激起的集体记忆。评论区里最打动人的部分，是一位用户如何用它完成了一次跨越 34 年的软件往返旅行，以及关于"该继续折腾真硬件还是干脆用模拟器"的忒修斯之船式辩论。

## HN 评论精华

- **mk_stjames** 贡献了全场最精彩的一条实验记录。他在站点上装的 Windows 3.1 里找到了一份 Visual Basic，做了一个带窗口和按钮的小程序，然后直接存成 .exe——"还记得这件事以前有多容易吗？做完程序，然后就……从 VB 里把可执行文件存出来！不需要任何编译器知识。" 接着他把这个 .exe 存进磁盘镜像，把镜像从浏览器下载回自己的 Mac，再连同唯一的依赖 vbrun100.dll 一起拷到一张真软盘上，插进一台还装着 Windows 3.1 的老机器（一台 25 MHz 的 Intel 486 DX 工业电脑），运行成功。"一个真正的 GUI Windows 程序，用 VB 做的，跑在 Windows 3.1 里，而 Windows 3.1 跑在一整台 JavaScript 模拟机里，而这台模拟机跑在 2026 年的 Apple Silicon MacBook 的浏览器窗口里……导出存盘，然后在一台 1992 年制造的硬件上毫无问题地运行了！"

- 后续更妙：**EvanAnderson** 猜那个 exe 在 32 位 Windows 10 上大概也能跑，**Krssst** 补充说用 otvdm/winevdm 连 64 位现代 Windows 都行。**mk_stjames** 亲测确认后感叹："这意味着我用浏览器里模拟的 Win 3.1 中的 VB 造了一个 16 位 Windows GUI 小程序，并让它跑在现代 Windows 上，花的时间还比我今天在 Windows 10 上原生搭同类 demo 更短。"

- **glimshe** 说了一句火药味十足的话："那些把每个小 JS 框架、每代 iPhone 和每次 AI 迭代都当成'革命性'的人，去这个站点上找标着 'Visicalc (1981)' 的那个东西，看看真正的革命长什么样。" **alok-g** 顺手贴了直达链接。

- **CharlesW** 说他等不及要让孩子们全屏体验《俄勒冈之旅》和《国王密使》了。这条引来了一串警告：**trollbridge** 说自己长子的第一句完整句子是"King Graham fell"（出自《国王密使 V》的雪橇场景）；**EvanAnderson** 认为俄勒冈之旅经受住了时间考验，但"《国王密使》那种动辄杀死你、让你重头再来的做法太狠了"；**quietsegfault** 则遗憾地发现自己的孩子对 Sierra 的游戏毫无兴趣。

- **glhaynes** 为这种设计做了一次有意思的辩护：作为一个没见过别的东西的孩子，"及早存档、经常存档"这个机制对他完全合理，他甚至一直希望现实生活里也有类似机制。"这不是'好的游戏设计'，但在我看来它确实以 LucasArts 那类作品做不到的方式提供了赌注和危险感。" **vunderba** 补了第三条守则——"及早存档、经常存档、存多个"，因为有太多方式能让你不知不觉地把游戏卡成不可通关，而几小时后才发现。他还描绘了一个所有人都会心一笑的画面："这就是为什么我们小时候一开始都用 `INNKEEP.SAV`、`BUYSLED.SAV` 这样描述性的存档名，然后玩到一半就变成 `1.SAV`、`2.SAV`、`3.SAV` 了。"

- **drivers99** 分享了他真机与模拟器并行的经历：他为了重读 Peter Norton 的汇编书而架起了一台真 IBM PC，结果那块 20 MB 的 Seagate ST-225 硬盘第一次报读取错误、内存又开始报奇偶校验错。他也试了站点上的《Exploring the IBM PC》——一张可引导的教学盘，教你 Scroll Lock 按键到底是干什么用的——发现 CGA 版本的声音会断，"Fun Writer" 部分那个可以打开的"收音机"播出的音乐似乎漏掉了休止符，"我猜它不完美。这个程序一向给我一种在对硬件做奇怪事情的感觉，也许那就是原因"。

- 由此展开了一段颇有深度的存储现代化讨论。**ssl-3** 确认 ST-225 确实是 MFM 接口，并给出 Lo-tech 的 XT-CF-Lite 方案（一张能直接插 CF 卡的 8 位 ISA 卡）；**hakfoo** 建议用 SD-IDE 适配器而非 CF，因为有些 CF 卡依赖 8 位模式而 XT-IDE 卡不保证支持，SD 适配器通常更稳且更便宜；他还提了另一条路——用 CH375/CH376 芯片的"ISA USB"卡配 DOS 驱动或扩展 BIOS ROM，就能把普通 U 盘当硬盘分区格式化。**ssl-3** 还回忆自己在世纪之交玩过一台带 10BASE2 以太网的老 XT，最实用的成果是连上 Linux 机器的 Samba 共享，"效果很好"。

- **TacticalCoder** 把话题推向哲学层面：他的机器都是"未被现代性污染的原始时间胶囊"，当年的 Amiga 就没有"Vampire 8000 GTX Turbo 16 气门"加速卡。但他承认东西终究会坏，"如果你要用模拟真机一部分的东西，为什么不干脆用一个模拟整台真机的模拟器？" 他自己就有台真古董街机柜，但插在里面的往往不是老 PCB 而是配 Pi2JAMMA 转接卡的树莓派，"游戏一跑起来你分辨不出差别：那基本上是周期精确的模拟。忒修斯之船之类的问题吧。"

- **drivers99** 的选择是保持原样：他最近给那台机器装了块 8 位 VGA 卡写了些 VGA 汇编，然后又把原来的 CGA 卡换回去了，即使这意味着要用两块外接转换板才能在 VGA 显示器上看到 CGA 输出。**anthk** 则站在实用一边："磁盘很容易腐坏。今天给老电脑配个 IDE→CF 适配器几乎是必须的。"

- **bityard** 补了一段被遗忘的软件史：1988 年他家第一台电脑 Tandy 2000 TX 附带的 DeskMate，是最早配 3.5 英寸软驱的主流消费级 PC 之一（"我们后来还得再买个 5.25 寸驱动器，才能跟邻居共享软件——呃我是说数据"）。DeskMate 当年的宣传语是"终于！不用再碰复杂的 MS-DOS 了"，但"你其实没法用它干太多事，因为那个年代的 MS-DOS 程序强大得多也有趣得多，它算个不错的技术演示吧"。

- **danvayn** 留下了一条极其实用的提醒："看几个之前先把音量调小。我没能在 Dune II 启动完之前找到音量旋钮。"

- **ChrisArchitect**、**smusamashah** 分别贴出了相关资源：floooh 的 Tiny Emulators（8 位机模拟集），以及一份浏览器内虚拟机的汇总清单。
