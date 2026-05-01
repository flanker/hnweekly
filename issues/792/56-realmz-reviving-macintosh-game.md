---
layout: article
title: "Realmz 复活记：一位开发者用 SDL3 让经典 Macintosh RPG 重新跑起来"
issue: 792
number: 56
category: fun
original_url: "https://danapplegate.com/projects/2025/11/24/reviving-realmz.html"
hn_url: "https://news.ycombinator.com/item?id=47875457"
date: 2026-05-01
---

## 文章摘要

Realmz 是 1990 年代经典 Macintosh 平台上的一款共享软件 RPG，由 Tim Phillips 开发，以宏大的奇幻世界观、丰富的剧本工具和强烈的"自制冒险模组"社区文化而闻名。开发者 Dan Applegate 童年时看着父亲玩 Realmz 长大，并将其视为自己走上编程之路的启蒙。多年之后，他注意到这款游戏正逐渐落入"abandonware"（弃置软件）的命运——Windows 移植版本 bug 缠身，模拟器对 Classic Mac OS 的支持时灵时不灵，老硬件越来越难寻——于是决定亲自把它救活，并把成果开源。

文章详细记录了整个反向工程与现代化重构的过程，技术含量很高：

1. **取得授权**：Applegate 通过多方打听联系到了原作者 Tim Phillips，说服他以 Creative Commons NonCommercial 许可证开源原始源码。这是整个项目能够"光明正大"推进的法律前提。
2. **桩化 System Calls**：Classic Mac OS 时代的程序大量调用 Toolbox API（QuickDraw、Window Manager、Sound Manager、Resource Manager 等），这些 API 在现代 macOS 上早已不存在。作者参考归档版本的《Inside Macintosh》文档，把所有缺失的系统调用都先桩化（stub）成空函数，让代码能编译通过。
3. **解析 Resource Fork**：Mac 经典文件系统独有的资源叉（resource fork）是个大坑——图像、音效、对话文本、地图都打包在里面。作者借助 Martin Michelsen 开源的 `resource_dasm` 库，写了 parser 把这些资源还原成跨平台可读的格式。
4. **基于 SDL3 的适配层**：把原本对 Window Manager、QuickDraw 的所有调用，替换成 SDL3 的窗口、绘制与输入封装，使得游戏既能跑在 macOS 上，也能跑在 Windows 上。
5. **大小端字节序问题**：原版 Realmz 在 PowerPC（big-endian）上构建，存档与资源文件中的多字节整数都是大端序。作者在加载层加上字节序转换，让 little-endian 的现代 x86_64 / ARM64 平台也能正确解析旧存档与原版的剧本文件。

最终成果是一份首个公开 beta，玩家可以在现代 macOS 与 Windows 上完整体验 Realmz，包括加载老玩家用 Realmz 自带剧本编辑器创作的自制 scenario——这在游戏遗产保护层面意义尤其重大。整篇文章对于任何对"老 Mac 软件复活"或"反向工程 + 现代图形栈接管"感兴趣的开发者都是非常实用的实战参考。

## HN 评论精华

- **danapplegate**（作者本人）：在评论区分享了类似的另一段经历——他还做过一款早期 90 年代 MS-DOS 海事冷知识游戏 RedHook's Revenge 的官方续作，同样是通过 LinkedIn 找到原作者本人取得授权后才动工。他强调"直接联系原作者"是关键，往往原作者乐于看到自己的旧作品被新一代继续延续。他也讲到，向二进制注入新内容时最大的难点是不能破坏原版图形/音频资源的内存偏移量。
- **raroapp**：把这种工作上升到游戏保护（preservation）的高度——随着 macOS、Windows 不断淘汰老 ABI/API，越来越多 1990 年代的标志性作品正在永久从可玩性中消失。社区驱动的反向工程与开源是少数能把它们留住的方式。
- **sea9**：原版 Realmz 老粉表达兴奋。他特别期待能再次进入"City of Bywater"以外的剧本，重新探索由社区创作的扩展场景。这反映出 Realmz 的真正生命力其实是它的 UGC 生态——核心引擎只是入口。
- **延伸讨论**：评论中也涉及一些更广泛的话题——经典 Mac 软件的 Resource Fork 在跨平台移植里的"双刃剑"特性、SDL3 相比 SDL2 在窗口/输入抽象上的进步、以及 `resource_dasm` 这类 Mac 反编译工具链的成熟度。许多评论者把这次 Realmz 的复活与近年 OpenLoco、OpenRCT2、ScummVM 等开源重制项目并列，认为它们共同构成了游戏文化遗产保护的"民间力量"。
