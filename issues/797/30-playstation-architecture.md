---
layout: article
title: "PlayStation 架构剖析"
issue: 797
number: 30
category: books
original_url: "https://www.copetti.org/writings/consoles/playstation/"
hn_url: "https://news.ycombinator.com/item?id=48382142"
date: 2026-06-12
---

## 文章摘要

这是 Rodrigo Copetti "游戏主机架构剖析"系列中关于初代 PlayStation 的长文（最初发表于 2019 年）。文章以图文并茂的方式，深入解析了这台第五代主机的硬件设计与工作原理。

主要技术内容包括：

- **CPU**：核心是 Sony CXD8530BQ，一颗基于 MIPS R3000A 架构、运行在 33.87 MHz 的片上系统（SoC），拥有 32 个通用寄存器和 5 级流水线，但没有浮点单元（FPU）。
- **图形**：GPU 负责绘制三角形、线段和矩形，采用不带透视校正的"仿射纹理映射"（affine texture mapping），这正是 PS1 远处物体出现标志性"纹理扭曲"的原因。系统配有 1 MB 显存用于帧缓冲和纹理。
- **内存与音频**：主内存 2 MB，外加 512 KB 音频 RAM；DMA 控制器让各子系统能独立于 CPU 访问内存。声音处理单元（SPU）支持 24 个声道、44.1 kHz 的 16 位采样，并用 ADPCM 压缩适配 CD 存储。
- **防盗版**：依靠光盘制造中的"摆动槽"（Wobble Groove）频率，结合 SCEA/SCEE/SCEI 区域识别码；后期游戏还加入校验和与 Libcrypt 保护对抗仿真器和盗版。

文章还澄清了一个常见误解：纹理扭曲并非单纯因为缺少 FPU，而是缺乏子像素精度和透视校正所致。游戏开发者通过预渲染过场动画和"排序表"（手动深度排序）等技巧来弥补硬件限制。总体来看，PlayStation 的成功在于其务实、注重成本与开发者友好度的设计理念。

## HN 评论精华

- **MrDOS**：提醒这篇文章最初发表于 2019 年，并附上 2020、2021 年的往期讨论链接（各有约 114 条评论）。
- **malkia**：分享了自己参与《合金装备》（Metal Gear Solid）从 PSX 移植到 PC 的经历——Konami 程序员用一个巧妙技巧（用 0x80000000 等地址做按位运算）来存储 C4 炸弹是装在墙上还是地面，移植时颇费周折。
- **lowbloodsugar**：回忆 MIPS"分支延迟槽"（在跳转后仍执行下一条指令）初看很反直觉，几天后便成了第二天性；并提到 N64 乘法指令也有类似的硬件陷阱。
- **suprjami**：表示正是 PS1 的架构让他爱上了 RISC（load-store）设计，看清了 x86 的种种问题。
- **busfahrer**：指出这套架构正好解释了为何 PS1 游戏有那种独特、辨识度极高的视觉风格，如今不少游戏还在刻意复刻它。
- **adamddev1 / Forgeties79**：盛赞这个网站设计精美、图表用心，是"数字花园"的优秀范例，即使外行也能被深深吸引。
