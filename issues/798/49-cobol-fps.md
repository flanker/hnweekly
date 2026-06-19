---
layout: article
title: "自虐型 YouTuber：用纯 COBOL 写出一款第一人称射击游戏"
issue: 798
number: 49
category: watching
original_url: "https://gizmodo.com/masochistic-youtuber-punishes-himself-by-writing-a-first-person-shooter-entirely-in-cobol-2000768785"
hn_url: "https://news.ycombinator.com/item?id=48465954"
date: 2026-06-19
---

## 文章摘要

一位名叫 icitry（频道签名是「try now, suffer later」，先上手、后受苦）的 YouTuber，给自己出了一道奇葩难题：用 COBOL 完整写出一款第一人称射击（FPS）游戏。这个念头源于他的一个自问——「写一个小型 FPS 游戏，最蠢但理论上又确实可行的语言会是哪个？」最终他锁定了 COBOL，并决定真的把它做出来。

COBOL 诞生于上世纪 60 年代，是为大型机上的商业事务处理而设计的语言：古老、冗长，连许多现代「烂语言」都有的功能它都缺，更完全不是为游戏开发准备的。计算机科学家 Dijkstra 甚至曾说，使用 COBOL「会让人的思维残废」。

为了绕开 COBOL 完全没有图形能力这一硬伤，icitry 写了一个「帧生成器」：逐帧计算画面并输出为简单的图像格式，再交给 ffplay 渲染显示；游戏控制则通过控制台输入实现。他几乎是从零手写了游戏所需的向量数学函数（处理 2D 移动计算的核心），甚至还实现了源自 DOOM 引擎的可变天花板高度等高级特性。最终成果是一款类似《德军总部 3D》（Wolfenstein 3D）风格、且真的能跑起来的游戏——既是编程巧思的展示，也是一场自找的极致折磨。

## HN 评论精华

- **DiscourseFan**：在承认 COBOL 局限性的同时，把它放进了历史语境里。他类比当下的 AI——认为几十年后，人们回看今天的 ChatGPT、Claude 等大语言模型时，很可能会像我们如今看 COBOL 一样，将其视为缺乏诸多日后习以为常之能力的早期技术。

（注：该提交在 HN 上讨论较少，实质性评论有限。）
