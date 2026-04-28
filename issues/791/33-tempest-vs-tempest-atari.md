---
layout: article
title: "Tempest vs. Tempest：Atari 标志性街机的诞生与重制"
issue: 791
number: 33
category: books
original_url: "https://tempest.homemade.systems"
hn_url: "https://news.ycombinator.com/item?id=47871195"
date: 2026-04-27
---

## 文章摘要

这是一本面向程序员的免费技术书，把街机史上两款相隔十几年的同名经典——Dave Theurer 在 1981 年为 Atari 写的初代《Tempest》（基于矢量显示器的射击游戏）和 Jeff Minter 1994 年在 Atari Jaguar 上重制的《Tempest 2000》——拆开来对照阅读，重点不在玩法或文化影响，而是它们各自是怎么被"实现"出来的。

作者用并排的双页 PDF 排版，把两份不同硬件、不同时代的源码摆在一起：初代 Tempest 跑在 Atari 的 6502 汇编上，使用矢量显示器和 X-Y 坐标输出；Tempest 2000 则跑在 Jaguar 的 68K 摩托罗拉汇编上，需要面对完全不同的位图输出和音频系统。每一章都聚焦一个具体的机制：怎样在 6502 上画出旋转的矢量管道、怎样实现敌人沿着管壁攀爬的运动学、怎样在更现代的处理器上重新组织粒子和音乐节奏，等等。

这种"一边读旧代码、一边读新代码"的对照法，让读者既能看到一段代码在硬件极度受限时是如何被压榨出每一个字节、每一个时钟周期的；也能看到同样的设计目标在十几年后随着硬件松绑后被如何重新表达。书中附有大量原始一手文档（设计稿、内存布局、寄存器使用图）和可视化解析，对游戏开发史和复古计算感兴趣的读者来说是一份难得的资料。作者本人通过自愿捐赠维持项目，免费 PDF 任意下载。

## HN 评论精华

- **jnaina**：作为深度粉丝晒出了"信仰"——他买了两台 Atari Jaguar 主机专门用来玩 Tempest 2000，还有多个模拟器，甚至在新加坡见过 Jeff "Yak" Minter 本人并听过他演讲，认为 Minter 与初代作者 Dave Theurer 是同一段神坛。

- **ndiddy**：补充了一份非常有用的资料——Tempest 2000 的 MS-DOS 移植版源代码在 archive.org 上是公开的（用 Borland Turbo Assembler 的 ideal mode 写的），可作为本书的姊妹阅读材料。同时盛赞作者把不同版本之间的代码并排呈现、并附上一手设计文档的做法。

- **bandrami**：唤起了一段苦涩的记忆——他当年在街机上爱上 Tempest，后来把游戏卡带搬回 Atari 2600 时被迫又买了一只 paddle 旋钮控制器，结果这只手柄"在之后的整个 2600 生涯里只为这一款游戏服务过"。

- **robin_reala**：给想合法支持 Jeff Minter 的人指了一条路——Digital Eclipse 出了一套叫 *Llamasoft* 的"互动纪录片"游戏合集，几乎登陆所有现代主机，里面收录了官方版的 Tempest 2000，购买能让一部分钱进到 Minter 口袋。

- **stuart78**：说他对这款游戏爱到为 EYESY 平台开发了一款叫 *Tempestuous* 的 Tempest 风格音频可视化器作为入门项目，是这本书引发再创作的一个生动案例。

- **zimpenfish / faxuss**：评论里也有理性提醒——书的某些章节过于硬核，对汇编不熟悉的读者门槛偏高；如果作者后续愿意稍微"翻译"一下，会让更多游戏爱好者读得动。
