---
layout: article
title: "让《模拟城市 3000》跑在 4K：一份给老游戏续命的折腾指南"
issue: 795
number: 52
category: fun
original_url: "https://thran.uk/writ/hdid/2025/12/simcity-3k-in-4k.html"
hn_url: "https://news.ycombinator.com/item?id=48297645"
date: 2026-05-29
---

## 文章摘要

作者 Thran 开篇就抛出暴论：**《模拟城市 3000》（SimCity 3000）是最好的一代 SimCity**。它构建在升级版的 SC2K 引擎之上，采用逐像素手工绘制的等距视角美术，在功能与复杂度之间拿捏得恰到好处，还有一套高品质的爵士/新世纪风格 OST。但要在现代机器（作者环境：Windows 10 LTSC 2021、Ryzen 5 3600、RX 7600、48GB 内存、4K 显示器）上跑顺它，得动几处手术。

作者用一张自己原版 Scholastic Edition 光盘装好游戏后，列出了一堆毛病：没有宽屏支持、分辨率只有 800x600 和 1024x768 这类古董档位、滚动时卡顿又会异常加速、读图后大幅掉速、不插光盘没音乐且部分曲目缺失、地块加载时先显示几秒空白。文章随后给出一套**逐项修复清单**：（1）替换 **GOG 提供的免费补丁 EXE**，启用宽屏并免光盘；（2）编辑 `SC3U.ini` 把 `ScrollMarginFactor` 改为 `0.005787` 修正鼠标加速；（3）安装 **D3D Wrapper（DirectX 包装层）** 解决「分辨率为 8 的倍数时画面错位」的诡异问题，从而真正支持原生 3840x2160，并在 `dxwrapper.ini` 中关掉无边框窗口、强制真全屏（`EnableWindowMode=0`、`FullScreen=1`），顺手开了垂直同步。

清单后半段还包括：（4）用 **NTCore 的 4GB 补丁**给游戏放开更大内存以缓解地块延迟加载；（5）替换 `UpdateSettings.ini` 禁用那个还在徒劳联系「早已死掉的更新服务器」的自动更新器，解决开图卡顿；（6）针对 SimCity 3000 Unlimited 缺失音乐的问题打补丁，并手动把光盘 `APPS/RES/SOUND/MUSIC` 下的 `.xa` 文件拷到硬盘。全部搞定后，就能在 Windows 10 上以「华丽的 4K 分辨率」继续打发请愿者、给自己建金色雕像、把安静农田规划出去然后看它瞬间变成一片烟囱工厂。作者还附了一句吐槽：这些步骤只在 Win10 上验证过，因为他「拒绝让 Win11 那个怪物靠近自己的家」。

## HN 评论精华

- **trynumber9**：同样偏爱 SC3000 胜过 SC4，但担心自己「没放大镜可能扛不住 4K 的 UI」。**tracker1** 附议，因视力问题更想要「半分辨率下 2 倍缩放」。
- **wifipunk**：盛赞这套补丁方法（尤其音频部分）比他多年来用的 HD Patch 好得多，回忆当年在学校金工课的老 Mac 上玩 SimCity，「它至今比大多数新游戏都更耐玩」。
- **squeedles**：惊讶于 SC3K 被描述为像 2K 一样的等距视角，原以为 2K 之后就是「自由视角」3D 了；他至今保留一套 Basilisk II 的 System 7.5 Mac 环境专门用来重温 SC2K。这条引出一段关于「哪几代是 3D」的考据。
- **Sohcahtoa82**：纠正道——唯一真正 3D 的 SimCity 是 2013 年那款简单命名为「SimCity」的（常被称作 SimCity 2013）；2K、3K 和 SimCity 4 全都是 2D 游戏。**TylerE** 则指出 squeedles 可能记混了全 3D 的 SimCopter。
- **DanielHB / oneeyedpigeon**：补充技术细节——3K 可旋转到 3 到 4 个固定的等距角度，每个角度都是预渲染好的，所以美术要做多套角度资源；oneeyedpigeon 甚至贴图证明 2K 也能用两个蓝色箭头旋转视角。
- **Waterluvian**：道出老游戏玩家的普遍遗憾——GOG/Steam 上卖的总是 PC 原版，但当年 **Mac 版往往有更好的音乐、音效甚至画面**。
- **MrDOS**：进一步解释「PC 原版」常常其实是「DOSBox 包着的 DOS 版」，因为这样在 Windows 和 Mac 上都好分发。他感慨缺少一个 Rosetta 式、能在新机器上跑老 Mac 应用的「应用级」经典 Mac OS 模拟器；**phs2501** 推荐了 Wine 风格的 MacOS 重实现项目 `executor`，MrDOS 表示「正是我一直在找的东西」。
- **mrpippy**：提供反例——他认为「Mac 版更好」主要适用于 90 年代早中期那些 PC 版要兼容五花八门 DOS 硬件的游戏；但 SC3000 是 1999 年的正常 Win9x 游戏，他手上那张 Software MacKiev 移植的 Mac 版「并不好」：配置要求高、卡顿、不稳定，连开/存盘对话框都沿用了 Windows 风格，很别扭。
- **ndiddy**：推荐 `sc2kfix.net` 补丁，可在现代电脑上跑接近 Mac 版的 Windows 95 版本，并吐槽「GOG 不分发那个版本，所以我得留着一台 CD 光驱」。
