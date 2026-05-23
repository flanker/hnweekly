---
layout: article
title: "2000 多个 DOS 游戏直接在浏览器里跑"
issue: 794
number: 58
category: fun
original_url: "https://dos.zone/"
hn_url: "https://news.ycombinator.com/item?id=48215418"
date: 2026-05-22
---

## 文章摘要

**dos.zone** 是 **js-dos** 作者维护的一个**网页 DOS 游戏聚合站**——打开就能玩，**不用下载、不用安装、不用模拟器**。库里有 2000+ 款 MS-DOS 时代的经典：DOOM、Duke Nukem 3D、Commander Keen、SimCity 系列、Need For Speed、Pinball Space Cadet……按字母、按类型（动作、策略、RPG、赛车、解谜）、按年代（Windows 3.1、95/98、支持 3Dfx 加速的）分类得井井有条。

技术底座是 **js-dos**——一个把 DOSBox 通过 **Emscripten** 编译到 WebAssembly 的浏览器端模拟器，等价于把整台 PC（CPU/显卡/声卡/键鼠）虚拟在你的 tab 里。js-dos 同一作者顺手做了这个聚合站，**没有广告**，还附带了多人对战大厅、移动端支持、离线缓存、用户上传/打包工具（Game Studio）等功能。

值得注意的是，HN 评论区有几位敏锐用户指出，库里其实**夹了一些严格来说不是"DOS 游戏"的内容**——比如需要 Windows 98 + DX9 的 GTA: Vice City（HN 帖发出后已经被 DMCA 下架）。这引出了一个有意思的语义问题：**DOS Extender（DOS/4GW 之类）的程序算不算 DOS 程序？**——讨论涉及 Windows 1/2/3/95/98/ME 与 DOS 的关系、Win9x 是不是"运行在 DOS 之上"、什么时候 Windows 真正接管了硬件等一连串老电脑历史题。

## HN 评论精华

- **HeavyStorm**：第一条质疑——"这些根本不是 DOS 游戏啊，里面好多是 Windows DirectX 游戏"。**hungryhobbit** 神回复一句："小知识：早期 Windows 本来就跑在 DOS 之上。"——一个**巨型回复串**就此展开。

- **masfuerte**（深度科普）：纠正"Windows 9x 跑在 DOS 之上"是不准确的——Win 95/98/ME 启动时确实加载了 DOS，但**DOS 只是 bootloader**，一旦 Windows 启动完就接管了所有 I/O。Windows for Workgroups 3.11 引入了 **32-bit Disk Access** 和 **32-bit File Access** 已经在绕过 DOS，95 之后 DOS 基本只剩仪式作用。

- **userbinator**：精辟比喻——Win9x 其实是**裸金属 hypervisor**，最少量地虚拟化硬件以便多个 DOS VM 共享。对比 Win9x 的 DOS box 和 NT/2K/XP 的 NTVDM 跑同样的 EDIT.COM，前者明显更响应——因为后者**全模拟了所有硬件除了 CPU**。

- **selcuka**：换个角度——"无论怎么算，**你不能在 DOS 上运行它就不能叫 DOS 游戏**"。但 **yjftsjthsd-h** 反问："需要 DOS Extender 的程序算不算 DOS 程序？这条线我画不出来。" **alberto-m** 给出一个工程派定义：**Extender 由游戏自带（DOS/4GW）就算；由用户提供（Win 3.11）就不算**。

- **vunderba**：指认作者身份——dos.zone 站长就是 **js-dos** 模拟器的作者。js-dos 这两年质量已经做到**可以用来给老 DOS 游戏出"官方续作"的网页 demo**，浏览器端 DOSBox 的体验已经接近原生。

- **lorecore**：安利更硬核的离线方案——**eXoDOS**（retro-exo.com/exodos.html），"字面意义上的每一个 DOS 游戏"，还能买到实体礼盒版。**AnotherGoodName** 反驳："网页一点即玩比本地装一坨更适合解馋。" **mrandish** 补上 eXoDOS 的省心装法——只下元数据和截图，点哪个游戏才下哪个的资源。

- **xerox13ster**：典型 HN 怀旧——上来就开 Sim City 3000 看"模拟速度会不会像在他当年的 Windows ME Compaq 上一样疯狂飙"——这种 bug 在 XP 以后就被速度限制器修了，他想再体验一次。

- **kimixa**：泼冷水派——"把**Steam 和 GOG 上还在卖**的游戏放出来玩，已经超出 'abandonware' 的边界了。"评论区还有一波关于版权年限是否合理、原开发者究竟有没有从老游戏的二次销售拿到分成的争论。

- **eapriv**：眼尖发现某些游戏的**封面图片明显是 AI 生成的**——给一个怀旧站做 AI slop 封面是一个奇怪的选择。

- **rhema**（隐藏的彩蛋）：一段精彩的童年回忆——"9 岁时我从这游戏（Scorched Earth）学到了第一次'hacking'：shareware 版不让真人选 ultra tank，但 AI 玩家可以用——开局存档、用记事本打开存档、把玩家/AI 字段对调，再读档进去就拿到了 ultra tank。"
