---
layout: article
title: "crustyBASIC——面向复古机型的 BASIC 交叉编译器"
issue: 810
number: 24
category: code
original_url: "https://telengard.itch.io/crustybasic"
hn_url: "https://news.ycombinator.com/item?id=49675675"
date: 2026-09-18
---

## 文章摘要

crustyBASIC 是开发者 telengard（GitHub 用户 bsturk）用 Rust 写的一个 BASIC 交叉编译器，目标是那些八位复古计算机和游戏主机，顺带也支持现代系统。它不是解释器：它把 BASIC 源码编译成目标机器的原生机器码，为的就是速度。编译器本身在 Windows、macOS 和 Linux 上都能跑，itch.io 页面上按「随意付费」提供各平台二进制，当前版本 1.5.0。

**目标平台**是这个项目最惊人的地方——Apple ][+、//c、//e；Atari 400/800/XL/XE、5200、2600 VCS、7800；Commodore 64（含 64 Ultimate）、128、Plus/4、VIC-20；Tandy/Radio Shack CoCo 1/2/3；任天堂 NES；Sinclair ZX81、ZX Spectrum、Timex Sinclair 1000；16 位 MS-DOS；以及 Windows 64 位（含图形，Win XP 到 Win 11）和 Linux x64（仅文本）。作者说还会继续加。让 Atari 2600 这种只有 128 字节 RAM 的机器成为 BASIC 编译目标，本身就是件相当疯狂的事。

**设计思路**围绕「可移植与原生并存」。一方面它提供一组跨目标的共享 API——图形、声音、输入、文件、tile、sprite，写一次可以在多个系统（包括桌面）上运行；另一方面它也提供目标专属 API，直接暴露原生显示模式、内存、硬件精灵以及 SID、POKEY、PSG 这些声音芯片。图像和音频既支持可移植格式，也支持各机型的原生格式；资源可以内嵌、转换，或从存储加载。

其余特性包括：编译期的灵活语法改写机制 `@REWRITE`；通过 `@INCLUDE` 拉入文件和资源来实现模块化；大量编译选项在速度与体积、卡带布局、数学库、内存布局之间做权衡；以及一个很有考古意味的能力——支持编译老的 BASIC 程序清单，包括已 token 化的格式，甚至能让这些老清单跑在与它原本所属机型不同的系统上。

最小的例子是这样的：源文件里写 `@OPTION TARGET c64` 选定目标机，然后 `CLS`、`POSITION 0, 0`、`PRINT "HELLO, WORLD!"`；命令行执行 `crustybasic hello.cbs` 就直接产出一个可运行的 `.prg`。如果只想生成汇编，用 `crustybasic compile hello.cbs -o hello.s`。构建程序需要目标机的汇编器，文档里有说明。如果把 `@OPTION TARGET` 那行去掉，同一份可移植源码可以一次性构建到多个目标。

项目附带完整的使用指南、语言参考和大量示例代码，itch.io 上还有活跃的开发日志：最近 7 天前发布 1.5.0 加入 ZX Spectrum，29 天前的 1.4.0 加了三个新目标，此前的版本陆续补上 C128、Windows 64 位、Plus/4、MS-DOS、VIC-20 和 Atari 2600。itch.io 页面的「AI 披露」一栏标注为「AI 辅助（代码）」。社区支持通过 itch.io 论坛和新开的 Discord 进行，论坛里能看到一位叫 JavaJack 的用户在持续提交相当具体的问题——1.3 版常量数组访问疑似损坏、VIC-20 上编译出的程序能不能重复 RUN、`rnd` 函数找不到、怎么把数组传进过程并在过程内使用、怎么避免按键重复等等，作者每条都有回复。这些帖子比任何宣传材料都更能说明这是一个真在被人用、也真在被人维护的工具。

## HN 评论精华

必须如实说明：这个 HN 帖子拿到了 11 分，但**评论数为零**，没有产生任何讨论。通过 Algolia 全站搜索 "crustyBASIC" 也只返回这一个条目，不存在被合并或重复提交的其他主帖可供取材。因此本篇没有可供转述的社区观点，也不会虚构任何用户名和发言。

这种「有票数、无讨论」的分布在 HN 上其实很典型：复古计算与 8 位工具链是一个有稳定受众但人数不多的领域，看到的人会点赞表示欣赏，却很少有人有足够的上手经验去展开技术讨论——真正能对 Atari 2600 的内存布局或 VIC-20 的 RUN 语义发表意见的人本来就不多，而这些人更可能直接去了项目的 itch.io 论坛或新建的 Discord。

作为替代材料，上文摘要部分综合了 itch.io 产品页与项目 GitHub 仓库（github.com/bsturk/crustybasic）的 README，后者给出了比 itch.io 更完整的目标机型清单（多出 Commodore 64 Ultimate、Timex Sinclair 1000，以及 `win64`/`linux64` 的明确命名）和更详细的 API 说明。对这个项目感兴趣的读者，真正值得看的是仓库里的 `doc/API.md` 和 `doc/targets/` 目录——跨目标的可移植 API 究竟抽象到了什么程度、在 2600 这种极端受限的机器上又退化成什么样，是这个项目最有技术含量的部分，而这恰恰是 HN 讨论没有触及的。
