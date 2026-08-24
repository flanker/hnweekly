---
layout: article
title: "Tidal Cycles：用算法模式现场编码音乐"
issue: 806
number: 19
category: show_hn
original_url: "https://tidalcycles.org/"
hn_url: "https://news.ycombinator.com/item?id=49378950"
date: 2026-08-21
---

## 文章摘要

Tidal Cycles（社区里通常简称 Tidal）是一个自由/开源的「算法模式现场编码」环境，用 Haskell 编写，由 Alex McLean 大约在 2009 年读博期间发起（受 EPSRC 资助），后续开发部分得到欧盟 Horizon 2020 框架下 PENELOPE 项目（资助协议号 682711）等经费支持。2021 年 Alex 把项目的资金支持模式从个人化的 Ko-fi 迁到 OpenCollective 组织模式，并任命了一支管理团队来做透明的账务。项目官方说明里还特别致谢了 Bernard Bel、Laurie Spiegel 和 Adrian Ward 的工作对 Tidal 的启发。值得注意的是，Tidal 的主仓库已经从 GitHub 迁到了 Codeberg（`codeberg.org/uzu/tidal`），GitHub 上的 README 现在只是一个跳转说明，代码以 GPLv3 授权，并明确声明任何把 Tidal 源码当作算法/类型参考的移植项目都属于衍生作品、受同一许可约束。

架构上，Tidal 本身只负责「模式」的描述与变换，声音默认由 SuperDirt 这个基于 SuperCollider 的合成器/采样器产生，也可以通过 OSC 或 MIDI 去驱动别的合成器；无论走哪条路，每个滤波器和效果器参数都能被独立地「模式化」。官方强调 Tidal 虽然嵌在 Haskell 里，但你不需要先学会 Haskell——大多数 Tidal 演奏者几乎没有软件工程背景。Tidal 提出的「时间模式」模型已经衍生出一整个开源家族，被统称为 Uzulangs，其中最出名的是浏览器端的 Strudel。

Tidal 的核心是 mini-notation（迷你记号法）：把一个循环（cycle）里的事件写在双引号字符串里，内部会被解析成等价的函数组合。常用符号包括：`~` 表示休止；方括号做分组（等价于 `fastcat`）；`.` 是分组的简写；逗号表示多层同时播放（等价于 `stack`）；`*` 重复、`/` 放慢；`|` 等概率随机选一个；尖括号在多个循环之间轮替（等价于 `slow`）；`!` 复制步数、`_` 和 `@` 延长事件时长；`?` 按概率随机丢弃事件（默认 1/2，可写 `?0.8`）；`:` 选择采样文件夹里的第几个样本（`bd:3` 等价于 `s "bd" # n 3`，而且索引越界不会报错，它会在文件夹里循环取）；圆括号写欧几里得节奏（`bd(3,8)` 等价于 `euclid 3 8`，该记谱法由加拿大计算机科学家 Godfried Toussaint 在 2004 年描述，用最大公约数思想覆盖了世界各地大量传统节奏型）；花括号写多重节拍（polymetric）序列，第二个模式会自动 wrap，还可以用 `%` 指定细分数。文档里的写法示例是 `d1 $ s "[bd*2,hh*3,[~ cp]*2, bass]"` 这样把底鼓、踩镲、拍手、贝斯叠成一个完整节奏段。

## HN 评论精华

这条帖子 80 分、16 条评论，讨论量不大，而且几乎没人聊 Tidal 本身的模式语言——**讨论的主线整体跑偏到了「不如直接用 Strudel」和安装难度上**，另一条支线是社区里到底有没有人用它做出好听的音乐。

- **hmokiguess** 开场第一条就是「另见 https://strudel.cc/」，这一条直接定调了整个讨论。
- **blltprfmnk** 泼冷水说 Strudel 本质上就是「给 TidalCycles 换了个 JavaScript 语法的皮」。
- **yoyohello13** 反驳：Strudel 的主要好处是上手门槛低得多；他几年前试 Tidal Cycles 时安装过程「麻烦得要命」。
- **VohuMana** 附议并给出细节：当年要同时搞定 Haskell、TidalCycles 和 SuperCollider 三者版本匹配并互相通信，装好之后运行倒是很稳，但装的过程一路踩坑。
- **whywhywhywhy** 为 Strudel 辩护：把它说成「换皮」是低估了——搬到浏览器让非技术人群也能跑，还换到了一个懂的人多得多的语言上。
- **Shadowmist** 说很喜欢 Strudel，但因为许可证问题没法把曲子嵌进自己的博客。**jchanimal** 回复说自己写过教程，Strudel 发布的是 JS 模块，可以直接 import 而不用嵌入 iframe；**Shadowmist** 回答自己的博客不从外域加载任何资源，不过用子域名自托管 Strudel 倒是个好主意。
- **onion2k** 推荐 YouTube 上的 Switch Angel，会定期直播用 Tidal 做 trance；**lelandbatey** 立刻更正说这位艺术家视频里技术上用的是 Strudel 而不是 Tidal Cycles。
- **plastic-enjoyer** 留下了整场里最尖锐的一句：他觉得 Switch Angel 可能是第一个用 Tidal 做出「真的能听」的音乐的人。
- **andai** 贴了一个演示视频链接，感叹「太酷了，我该去学学 Haskell」，并顺手贴了 Learn You a Haskell。
- **brian-armstrong** 问这个项目和同名的流媒体服务（Tidal）有没有关系，**zimpenfish** 用一个字回答：「没有。」
- **cpill** 收尾时又贴了一遍 Strudel 的 workshop 入门链接，理由是「同样的东西，浏览器里交互 UI 更好，还不用装任何东西」。
