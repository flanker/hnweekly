---
layout: article
title: "古早桌面操作系统的截图博物馆"
issue: 794
number: 30
category: design
original_url: "http://www.typewritten.org/Media/"
hn_url: "https://news.ycombinator.com/item?id=48104428"
date: 2026-05-22
---

## 文章摘要

这是 **R. Stricklin（Typewritten Software）**维护的一个怀旧站点的图像分馆，专门收录 1983–1990 年代各种**早期图形桌面操作系统的截图**，而且每张截图都标注了原始机型、操作系统版本、分辨率、文件大小，并配一段简短的历史注释。和那种猎奇向的"老 GUI 大全"网站不同，这里的截图是 Stricklin **亲自在真机上**捕获的——VGA、CGA、Sun 工作站、Acorn Archimedes、HP Integral PC、Amiga 2000 这些机器他都有实物或在用同期硬件运行。

收录非常硬核。开篇就是 **VisiCorp Visi On 1.0（1983）**——比 Mac 还早的 PC 图形界面，640×400 在 Turbo XT 克隆机上跑，配文还提到这张截图做过 line-doubling 修正长宽比；**SunTools on SunOS 1.1（1984）**带着 spheres demo 和象棋；**HP-UX 5.0 在 HP Integral PC** 上跑的画面；**GEM Desktop 1.2（1985）**——文中特意指出"这是 DRI 输掉苹果'look and feel'诉讼前的最后一版 GEM"；**Acorn Archimedes 上的 Arthur 0.30**（红色窗框不代表激活窗口，而是 Note-Pad 桌面附件里有未保存数据）；**Amiga 2000 上的 NewTek Digi-Paint**，演示了那台机器标志性的 4096 色 HAM 模式。

**注释的细密程度是这个站的灵魂**。每张截图旁边都会写一句"为什么这一刻重要"——可能是某个版本的最后一次发布、某场官司的时间节点、某个硬件 trick 的首次实现。看起来是图册，本质是**用图像写的早期个人计算史**。

设计本身也很 retrotechnology——XHTML 1.0 Strict、iso-8859-15 编码、自己写的小 CSS、img 缩略图链接到原始 PNG。整站打开就像走进一间用打字机字体装修的小博物馆，没有滚动 hero、没有动画、加载在 100 毫秒内完成。

## HN 评论精华

- **adunk**（关于现代滚动条的吐槽）：发起整条最长的 thread——现代系统的隐形滚动条让他抓狂，移窗口都要点好几次才找得到边界。下面一群人贡献了**各操作系统的"始终显示滚动条"开关**：Linux 上 `gsettings set org.gnome.desktop.interface overlay-scrolling false`，Windows 在"设置 → 辅助功能"里，macOS 在"系统设置 → 外观 → 显示滚动条 → 始终"。**hulitu** 补刀："大部分程序根本不理这个设置。"

- **Quitschquat**（清醒派）：你们怀念这些老 UNIX GUI，可能忘了**它们在当年也是垃圾**。**cryo32** 立刻举例：被锁在地下室对着 Solaris CDE 桌面工作的几年差点把他逼疯——配色、响应、整体设计全是噩梦，回家用 RISC OS 才算救命。

- **Sophira**（RISC OS 老用户）：竟然在 HN 上碰到另一个家里跑过 RISC OS 的——讨论快速变成 Acorn 老用户内部聚会，互相安利 Arculator、RPCemu 两个模拟器，以及 Stardot 这个仍然活跃的 Acorn 社群论坛，**40 MB 硬盘**当年算是改变命运的升级。

- **hermitcrab**（实用主义者）：自己长期用 Windows 和 macOS 不是因为爱它们，而是因为客户都在那里——做产品视频和截图时电脑必须看起来"和用户的一样"，没空玩个性。

- **共识**：站点本身被普遍称赞——**注释的密度和准确性是这种怀旧项目里少见的**，不少人收藏后准备慢慢翻；多位评论者推荐了类似的 toastytech.com（GUI Gallery）和 Web 上的 PCem/86Box 截图合集，但 typewritten.org 的特点是"所有截图都来自真机而非模拟器"，质感不同。
