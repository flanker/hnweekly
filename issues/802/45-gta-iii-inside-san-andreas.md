---
layout: article
title: "模改作者让《GTA III》跑在《GTA: 圣安地列斯》的游戏内电视上"
issue: 802
number: 45
category: watching
original_url: "https://videocardz.com/newz/modder-runs-gta-iii-inside-gta-san-andreas-on-an-in-game-tv"
hn_url: "https://news.ycombinator.com/item?id=48970991"
date: 2026-07-24
---

## 文章摘要

模改作者 Dryxio 演示了一件听起来不太可能的事：《GTA III》正在《GTA: 圣安地列斯》里的一台电视机上运行，而且是可玩的。视频里 CJ 站在电视机前，屏幕上《GTA III》的世界照常运转，玩家可以随时在「操控圣安地列斯里的 CJ」和「操控 GTA III 里的 Claude」之间切换。

关键在于，这不是把预录视频或外部窗口贴到贴图上。按 Dryxio 的说明，《GTA III》是在《圣安地列斯》的同一个进程内运行的，两个游戏共享同一个 Direct3D 设备，《GTA III》直接把画面渲染进电视机那块实时 GPU 贴图里。更重要的是，《圣安地列斯》的世界在这期间并没有暂停或被全屏界面覆盖——两个游戏世界是同时在被模拟的。

这次演示建立在 Dryxio 早前一个更大的项目之上：2026 年 2 月发布的一个 mod，把《GTA III》和《GTA: 罪恶都市》一并移植进了《圣安地列斯》，每个游戏都作为一个独立引擎在《圣安地列斯》的进程中运行，各自保留独立的剧情进度、存档、设置和玩法。技术底座是社区逆向工程项目 re3 / reVC（《GTA III》和《罪恶都市》的源码级重制）以及 librw（RenderWare 图形层的开源重实现）的修改版。发布的公开包不包含《GTA III》和《罪恶都市》的游戏文件，玩家必须自己拥有这两款游戏并提供资源文件。

而 Dryxio 后来又把这个实验推进了一层：第二段演示里，《罪恶都市》跑在《GTA III》的一块贴图上，而《GTA III》本身又跑在《圣安地列斯》的电视机上——三款游戏在同一个进程里同时活着，每一层都可以实时游玩。作者本人在说明里用全大写强调：这不是视频贴图，全部游戏是活的、在同一进程内运行，没有任何东西是从别的程序流过来的。

## HN 评论精华

评论区一半在惊叹技术实现，一半在怀念《圣安地列斯》的 mod 黄金年代。

- **joenot443**：帮大家把技术要点摘了出来——两个游戏共享同一个 Direct3D 设备，《GTA III》直接渲染进电视机的实时 GPU 贴图，「这就是基本的管道。很酷的项目，游戏 modder 们太棒了」。
- **samxli**：进一步指出真正的魔法在于共享 D3D 设备这一步——让两个渲染循环在同一个 GPU 上下文里互不踩踏，对于两款设计之初完全没考虑过这件事的游戏来说非常不简单；而且两个世界是同时被模拟的，不是渲染一个、暂停另一个。他调侃道：「过去十年，GTA 系列的创新基本是靠 modder 们扛着走的。」
- **randomtoast** 引述了作者本人的说明：《圣安地列斯》《GTA III》《罪恶都市》全部活在同一个进程里，罪恶都市渲染进 GTA III 的贴图，GTA III 再渲染进圣安地列斯的贴图，每一层都能实时游玩，基于修改版 re3/reVC 和 librw 构建。
- **ciefa**：回忆自己当年沉迷《圣安地列斯》尤其是 SA-MP 联机，一直惊讶于 modder 们能从这款游戏里挖出多少东西。**zahrc** 更进一步——用 PAWN 写脚本、后来做 CLEO 的 mod 和外挂，是他进入编程的入门通道，动机则是「大多数 RP 服务器都很排外，这是我对抗那套体系的方式」。
- **AviationAtom** 给出了最精炼的动机总结：「你为什么要做这个？」「因为我能。」**cadamsdotcom** 则贡献了当期最佳玩笑：「GTA 3 + GTA 3 = GTA 6！」**Griffinsauce** 接了一句梗图台词「Yo dawg..」（出自「我在你车里放了辆车」的 Xzibit meme）。
- **marginalia_nu**：把这个项目和另一类「套娃 mod」并列——Nexus Mods 上的「Morrowind: Pipboy Edition」（在《辐射 4》的 Pip-Boy 上跑《晨风》），并顺带提到一个正在做的「用《艾尔登法环》引擎跑《晨风》」的项目。
- **jamesfinlayson** 从引擎角度讨论了这类做法的可移植性：GoldSource 有个 VGUI surface 系统，可以在游戏内的平面上执行类 OpenGL 命令，但似乎没有完整实现；Source 引擎新版有更高级的 UI 框架，理论上可能做到，但都没有开放给 modder。
- 一条支线跑偏成了 Steam Deck 争论：**newsomix9xl** 抱怨 Rockstar 不给《GTA V》做 Steam Deck 适配，**MYEUHD** 和 **striking** 都表示《GTA V》（连增强版）在 Deck 上跑得挺好、自己完整通了剧情模式；**newsomix9xl** 澄清说他指的是线上模式，且该游戏如今被官方标为「不支持」，并反问「Arc Raiders 那种小工作室都能让线上游戏在 Deck 上跑，Rockstar 有几十亿却做不到」。**striking** 顺带推荐了一句：不如去玩初版《圣安地列斯》，它的重玩价值比《GTA V》高得多。
