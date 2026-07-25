---
layout: article
title: "MoonBASIC：一门用于开发 2D 和 3D 游戏的现代 BASIC"
issue: 802
number: 22
category: code
original_url: "https://github.com/CharmingBlaze/moonbasic"
hn_url: "https://news.ycombinator.com/item?id=48910579"
date: 2026-07-24
---

## 文章摘要

moonBASIC 是一门专为 2D 和 3D 游戏开发设计的现代 BASIC 语言。项目最强调的卖点是可访问性：直接分发预编译的二进制文件，用户不需要预先安装 Go、C 编译器、Node.js 或 Raylib 中的任何一样。你只要从 releases 页面下载对应平台的打包（Windows、Linux、macOS ARM64）即可开始写游戏。其中 IDE 版本把编辑器、编译器、运行时、示例和内置文档全部塞进一个文件夹，开箱可用；另外还有一个只含编译器的分发版，用于 lint 和编译而不启动游戏窗口。

语言本身沿用传统的 BASIC 结构，但 API 按命名空间组织，调用方式接近方法风格。README 里的示例程序展示了这种风格：用 `APP.OPEN(960, 540, "Hello moonBASIC")` 打开窗口，用 `WHILE NOT APP.SHOULDCLOSE()` 作为主循环条件，用 `RENDER.CLEAR(20, 24, 32)` 清屏——`APP`、`RENDER` 这样的命名空间前缀让庞大的命令集变得有条理，同时保留了 BASIC 大写关键字那种一眼可辨的观感（有评论者专门称赞这一点，认为在语法高亮普及之前大写关键字让非代码部分更突出，如今依然好读）。

游戏能力方面，moonBASIC 通过集成多个成熟库来同时覆盖 2D 和 3D：2D 物理用 Box2D；3D 物理用 Jolt 引擎，并提供运动学角色控制器（kinematic character controller）；图形和音频后端是 Raylib；在启用的构建中还支持 ENet 做网络。运行时的工作方式是编译 BASIC 代码后对着 Raylib 执行渲染。项目采用 MIT 许可，引擎源码放在一个独立的贡献者仓库中。

HN 上的讨论呈现出鲜明的两层：上半部分是一场大规模的怀旧，几乎所有资深评论者都在报出自己的启动语言——Amiga 上的 AMOS、Atari ST 上的 STOS、Blitz BASIC / Blitz 3D、DARK Basic、LibertyBASIC，很多人明确说正是这些工具让他们真正入了编程的门，也有人指出因为 Dijkstra 那句著名的评价，人们对 BASIC 往往带有势利的偏见。下半部分则被一个尖锐得多的话题占据：仓库的提交记录显示是「CharmingBlaze and cursoragent committed」，也就是大量 AI 协作产物，这引爆了一场关于"AI 时代还剩下什么乐趣"的坦诚讨论，其中一位 52 岁的老程序员的自白成了整个帖子最高热度的分支。

## HN 评论精华

- **captaincrunch**：全场最引发共鸣的一条自白——"我以前很爱这类帖子，我自己当年还写过 BASIC 运行时。但最近因为 AI，我对这种东西彻底失去了兴趣。我不是轻率地这么说。我 11 岁开始写代码，享受了每一分钟。我现在 52 岁，自从彻底投入使用 AI 之后，我基本上不再自己写代码了。有没有人也觉得自己失去了那种火花？"
- **rnd0**：年长几岁，感受相同，而且不只在编程和极客爱好上，连创造性活动也一样。"这看起来字面意义上失去了意义，因为别人做的一切都涉及 AI。它把意义直接拿走了，因为人们只会说'谁在乎？看我用 AI 做的这玩意儿'。"
- **ofrzeta**：描述了一种撕裂感——一方面现在有超能力去实现更复杂的项目（比如 Gameboy 模拟器），另一方面又失去了做它的欲望，"因为有什么意义呢？"作为软件和博客的消费者也是同理，"一个新的游戏 BASIC 不再那么有趣了，因为我自己就能 vibe code 一个"。他甚至说看到格式清晰漂亮的 README 时，那本该是好事，但也有点让人倒胃口。
- **TonyAlicea10**：完全相反的体验——"对我来说火花被重新点燃了。我可以尝试以前没时间做的想法，做原型、做测试，只在值得投入时才继续。"
- **prologic**：不觉得失去了火花。"当然我不再那么在意'写代码'这件事本身，但你的重心会转移到创造性的一面、内容创作和方向把控上。那依然有趣。AI（还）替代不了这个。"
- **qsera**：连发三条追问，态度带刺但问题真诚——"你在 LLM 之前的乐趣来自哪里？我们有了 3D 打印，人们就不玩乐高了吗？要么你以前并没有真的'享受'做东西，只是把它当作达成目的的手段；要么你只是暂时迷失，过一阵会回来的。"另外他也反驳"AI 抹掉了意义"的说法："这完全说不通。谁在乎你怎么做的。我觉得如果你说你没用 LLM 来做，人们反而会更有共鸣。"
- **whateveracct**：给出最干脆的回答——"1. 没人逼你用 AI。你可以手工慢慢做酷东西，甚至有额外的好处。你可以直接……停下来。2. 这个项目里满是'co-authored with cursoragent'，所以它并不属于第 1 种情况，那还有什么可说的。"他在另一条评论里更直白地表达了发现真相的过程："哦酷，多有意思的项——'CharmingBlaze and cursoragent committed'——哦……"
- **lproven**：58 岁，态度最强硬——"我碰都不碰。对程序员来说它像是海洛因混合可卡因。一次都别试。AI：说不就好。"
- **pjmlp**：同感，除非是自己写的。"在公司我们甚至已经不怎么写代码了，以前是微服务，现在是 MCP 工具，基本上就是跨 SaaS 产品的编排，用低代码无代码工具。谢天谢地在家我可以想干什么就干什么。"
- **wunderwuzzi23**：一个用于游戏开发的 BASIC 让他想起 Commodore Amiga 上的 AMOS。**ColinEberhardt** 回应："我太爱 AMOS 了，正是它让我真正认真投入编程。"
- **lproven / xgkickt / binaryturtle**：补上 Atari ST 阵营的 STOS（还有现代化的 v5.5 alpha 构建和后继者 AOZ Studio），以及 Amiga 上更受欢迎的 Blitz BASIC(2)。xgkickt 顺带指出："遗憾的是，因为 Dijkstra 关于 BASIC 的一句话，人们往往对它相当势利。"
- **synqvest**：Blitz Basic 和 Blitz 3d 后来怎么了？**shakna** 答：变成了 Monkey。**msephton** 补充了令人惋惜的消息：作者 Mark Sibly 已于 2024 年末去世，"但他留下了何等的遗产"。
- **dabbz**：这个项目让他强烈想起 The Game Creators 做的 DARK Basic，"那是我当年开始编程的方式"。
- **monster_truck**：想看一些粗略的基准测试，好奇这种构建方式损失了多少性能（如果有的话）。他最早的应用是近 30 年前用 LibertyBASIC 写的，"我学会盗版是因为要把作品分享给朋友就得买 Borland 编译器，要 299 美元，那在当时是一大笔钱"。
- **klik99**：25 年前用 BASIC 做了第一个游戏，现在依然在做游戏。他一直在找一个好的 BASIC 来教孩子编程，因为 pygame、Scratch、Roblox Lua 各有问题——要么太让人不知所措，要么把重要的部分抹得太平以至于学不到真东西。"希望这就是那个，只要有好的示例或者能把老 BASIC 游戏移植过来。"
- **stuaxo**：作为 Python 开发者，"我能用 Python 给我 9 岁的孩子演示上百万种东西，而这么大的选择空间本身就成了一个问题"，所以这个可能正合适。
- **shakna**：一直在玩 SmallBASIC，它也自带 raylib，还有 nuklear。"BASIC 作为游戏设计语言，感觉像 draw 循环和 tick 循环这些常见的游戏抽象非常契合它。把光线投射这类数学密集的东西下放到 C 再 import 进来，就得到了一个不错的抽象层。"
- **frenzcan**：还挺喜欢老语言的大写关键字，它让非代码部分更突出，"在语法高亮普及之前大概更重要"。
