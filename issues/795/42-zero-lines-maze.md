---
layout: article
title: "把那行经典的 C64 迷宫代码压到「零行」：8-Bit Guy 还能教我们什么"
issue: 795
number: 42
category: watching
original_url: "https://retrogamecoders.com/zero-lines-maze/"
hn_url: "https://news.ycombinator.com/item?id=48279507"
date: 2026-05-29
---

## 文章摘要

这篇文章从那行家喻户晓的 Commodore 64 BASIC「单行迷宫」程序讲起：`10 PRINT CHR$(205.5+RND(1)); : GOTO 10`。它的原理是 `RND(1)` 产生 0 到 0.999 之间的值，加上 205.5 再截断取整后，得到字符 205 或 206——这两个 PETSCII 字符正是两种方向相反的对角线，**各 50% 概率** 不断打印、随屏幕滚动，便拼出一座无限延伸的随机迷宫。

文章的核心是 **8-Bit Guy** 的一个炫技：他把这行代码做成了「零程序行（zero lines）」——直接在 BASIC 提示符下敲入 `FOR A=0 TO 1 STEP 0:PRINT CHR$(205.5+RND(1));:NEXT`。这利用了两个特性：FOR 循环可以不带行号直接在命令行执行；而 **`STEP 0`** 让计数器永不前进，从而构成死循环。

文章进一步剖析了「加速版」里的三个优化：① **硬件随机数**——读取 SID 芯片的噪声寄存器 `PEEK(54299)` 来替代慢吞吞的 `RND()`；② **查表法**——预先算好全部 256 个「字节→二进制」映射，省掉运行时计算；③ **数据与逻辑解耦**——只换表的内容（换成别的字符）就能改变输出，而无需改动逻辑。最关键的一课是：8-Bit Guy 通过性能剖析发现，真正的瓶颈其实是 **硬件屏幕滚动**，而非 BASIC 解释器——识别真实约束，远比过早优化更重要。

## HN 评论精华

- **madanparas**：补充了文章漏掉的重要背景——MIT 出版社有一本长达 300 页、专门分析这一行代码的书《10 PRINT CHR$(205.5+RND(1)); :GOTO 10》，免费 PDF，涵盖其数学（**Truchet 拼贴**）、C64 硬件与文化史。
- **fortyseven**：泼冷水：把「一行」变成「将近一打行」，恰恰违背了这件事原本的精神。
- **rob74** 与 **pimlottc**：两位都对「零行」的说法较真——在提示符里敲入的一行仍然是一行，去掉行号并不等于「零行」。
- **newmana**：实测指出预计算查找表要花 **40 秒**；Robin（8-Bit Show and Tell）发布了至少两个更快的改进版，几秒钟就能跑起来，关键是减少了 BASIC 里很慢的字符串拼接。**juancn** 补充了同一个回应视频链接。
- **aa-jv**：安利了一年一度的 **BASIC 10 行代码大赛**（basic10liner.com）——有人用 10 行 BASIC 写出动态生成的地牢爬行游戏、完整的登月着陆器、街机游戏，甚至一个 Brainfuck 解释器。
- **jason_s**：怀旧——当年他会把 C64 输出的迷宫用 Star Micronics 点阵打印机打出来，再用涂改液和墨水手动加工，让图案更有趣。
- **ErroneousBosh**：好奇为什么 1980 年代几乎所有家用机的 BASIC 都不内置按位运算。
- **teaearlgraycold**：发问——文章说 8-Bit Guy「有争议」，到底为什么？
- **teddyh**：贴了一行等价的现代 Python 命令，用 `itertools.repeat` 无限打印 ╱ ╲ 两个 Unicode 斜线字符。
- **busfahrer**：推荐了另一个用 10 行 BASIC 写成的 roguelike《The Snake Temple》。
