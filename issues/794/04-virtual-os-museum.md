---
layout: article
title: "一个人用 20 年，建了一座 1700 个操作系统的虚拟博物馆"
issue: 794
number: 4
category: favorites
original_url: "https://virtualosmuseum.org/"
hn_url: "https://news.ycombinator.com/item?id=48195009"
date: 2026-05-22
---

## 文章摘要

**Virtual OS Museum** 是一座规模惊人的数字档案馆——里面收藏了**超过 1700 个预装好的操作系统**，全部跑在模拟器里。整个项目以一个 Linux VM 形式打包，可以在 QEMU、VirtualBox 或 UTM 中启动，覆盖了从 1948 年到今天的整部计算机史。

收藏目录是一部"操作系统活化石"清单：早期大型机（Manchester Baby、EDSAC、Mark 1 上跑的软件）、迷你机和大型机（CTSS、MVS、VM/370、TOPS-10/20）、Unix 变种与工作站（SunOS、IRIX、NeXTSTEP、Plan 9 以及各种 BSD）、家用电脑（Apple II、Commodore、Atari、ZX Spectrum、BBC Micro）、PC 时代（DOS 各种变种、OS/2、BeOS、Windows 1.0 到早期 NT、经典 Mac OS）、移动与嵌入式（PalmOS、Windows CE、早期 Android/iOS、QNX）、以及一堆冷门到出圈的研究型系统（ZetaLisp、Smalltalk、Oberon）。统计下来，**570+ 个独立操作系统、覆盖 250+ 个硬件平台**。

整个项目还附带了一个自制的 launcher，跨 Linux/Windows/macOS 运行，并支持**快照回滚**（玩坏了可以一键恢复）。提供两个版本：**完整版**全部内置（离线即用），**lite 版**按需下载。许多老系统不光是空的安装盘，连同时代的周边软件也一并预装好——你打开 Windows 3.1 进去就能看到那个年代的字处理器、自带音乐播放器，做到了真正意义上的"**还原一台 1995 年的电脑**"。

最让人服气的是：这是**一个人用 20 多年时间**做出来的项目。作者长期从各种归档、原始安装介质里挖素材，至今仍在持续更新，通过 YouTube、博客、BlueSky、Discord 跟社区互动。在大学计算机系档案馆陆续关闭、老介质迅速消失的今天，这种保育级项目几乎是单枪匹马在和时间赛跑。

## HN 评论精华

1. **AnimalMuppet**：一句话感叹——"哇，光是把那些名字读一遍，就已经够怀旧了。"

2. **simonh**：分享亲历——他整个 1990 年代都在为 **Pick 操作系统**做开发，那是当时绝大部分企业网络服务器跑的东西，今天的开发者几乎没听过这个名字。

3. **MisterTea**：Packard Bell 的老用户，记得当年随机附送的拟物化音响 UI 音乐软件，运行在 Windows 3.1 上；他至今遗憾这玩意儿在 Windows 95 上跑不起来。

4. **Keyframe**：哲学吐槽——"成熟带来了同质化的体验，**90 年代的 UI 真是另一种生物**。"

5. **jzer0cool**：盛赞 Apollo 工作站的 OS 设计——多任务、可定制、4MB 内存就能扛起多个 Unix 个性化，"UI 不挡你的路"是那个年代很多人怀念的特质。

6. **semireg**：童年第一个 GUI 是 Commodore 64 上的 GEOS——评论里很多人都被这条勾起了"**第一台电脑**"的回忆。

7. **cortesoft**：纯情感发言——"我就是喜欢这种纯热爱驱动的项目。一个人扛起所有活，仅仅因为他在乎。"

8. **Postosuchus**：满足了多年的心愿——一直想有一个"按需调出任何重要 Unix-like 系统"的工具箱，现在终于有了。

9. **nonamenoslogan**：自惭形秽——"我自己做这事做了好几年，原以为收齐 70 多个 OS 已经很牛了……"

10. **xbar**：呼应保育意义——很多大学档案馆和老硬件正在加速消失，这种项目意味着"**那些冷门系统不会就这样从历史里被擦掉**"。
