---
layout: article
title: "Memo：一种只记住最后 12 行代码的编程语言"
issue: 788
number: 39
category: books
original_url: "https://danieltemkin.com/Esolangs/Memo/"
hn_url: "https://news.ycombinator.com/item?id=47620933"
date: 2026-04-03
---

## 文章摘要

Memo（写作 []memo）是由 Daniel Temkin 创造的一种奇趣编程语言（esoteric programming language），其核心设计理念极为独特：它是一个"意识流式"的编程环境，程序只能记住最后若干行代码（标题称 12 行，但实际实现可能为 10 行），当代码行数超过记忆限制后，最早的行会被"遗忘"，从屏幕上滚走并永久丢失。

这种设计创造了一种独特的编程体验：你只有一个程序，它始终在变化，每次回来时都从上次离开的地方继续。程序的状态通过 cookies 来存储，这意味着每次返回时，程序会恢复到上次的状态。

Memo 采用函数式编程范式，使用接近自然语言的语法。定义函数的方式是："Remember function-name with arguments as body"（记住某个函数名，参数是什么，函数体是什么）。输出则使用 "Tell me about name" 命令。语言还有一个有趣的设计——它忽略阿拉伯数字（如 4），要求用户拼写出英文单词（如 four）。

Memo 属于 Daniel Temkin 创作的 44 种奇趣编程语言系列之一。Temkin 同时也是一位作家，曾在 MIT Press Reader 上发表过关于奇趣编程作为"黑客民间艺术"的文章。这些语言不以实用性为目标，而是通过颠覆编程的基本假设来探索计算的本质、语言的边界以及人机交互的可能性。Memo 的"遗忘"机制可以被理解为对人类记忆有限性的一种隐喻，也挑战了程序员对代码"永久性"和"可回溯性"的默认假设。

## HN 评论精华

**实际行数之争**：omoikane 通过实际测试发现，Memo 记忆的实际上是最后 10 行代码而非标题所说的 12 行，并提供了工作代码示例来展示变量在超出记忆限制后如何丢失依赖关系。

**与交互式终端的类比**：dlcarrier 风趣地评论道："在我的国家，我们称之为交互式 shell。"moron4hire 进一步解释说大多数脚本语言都实现了 REPL（读取-求值-打印循环）来实现类似功能，不过 Memo 的核心区别在于它的"遗忘"是强制性的且不可恢复的。

**语言学讨论**：围绕语言名称中"stream-of-conscious"还是"stream-of-consciousness"的正确用法，dcre 指出了语法修正。thaumasiotes 利用当代美国英语语料库进行了详尽的语言学分析，证明 "conscious" 在实际使用中不充当名词。psychoslave 则从英语形态简化的趋势角度为这一用法辩护。

**HN 首页机制**：stitched2gethr 对这类小众帖子如何登上首页表示好奇。tomhow 解释说，帖子只需要在提交后短时间内获得"少量有机投票"就能获得曝光。
