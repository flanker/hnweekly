---
layout: article
title: "Squeak 6.1 发布"
issue: 805
number: 25
category: code
original_url: "https://squeak.org/release_notes/6.1/"
hn_url: "https://news.ycombinator.com/item?id=49242653"
date: 2026-08-14
---

## 文章摘要

在 Squeak 即将迎来三十周年之际，Squeak 团队发布了 6.1 版本，代号「Vanessa」——这个代号是为了纪念 2025 年去世的核心开发者 Vanessa Freudenberg（SqueakJS 的作者）。距离上一个版本已经过去了四年，这四年里项目合并了 1700 多个补丁、9000 多处方法改动，因此这份发布说明本身就是一份相当可观的文档。值得一提的是，这份 release notes 被设计成可以在 Squeak 镜像内部阅读，文中大量的「交互式示例」链接可以直接点开在 SqueakJS（浏览器里的 Smalltalk 虚拟机）中运行，所以它既是变更日志，也是一份动手探索新特性的导览。发布说明本身也从 GitHub 搬进了镜像内部，同时镜像到 squeak.org 上。

官方列出的几个重点：一是**全新的树形浏览器（tree browser）**，用重构过的层级 morph 来导航类和分类，并集成了 Monticello 包管理，还附带五个新的外观配置项；二是**Objectland 回归**，这是对 2003 年那个著名的「Worlds of Squeak」的重建，里面收录了一批展示 Squeak 各种能力的彩色示例，经典的 Etoys 赛车示例被重新加回，还有「消失了 18 年」的 BlobMorph 也被复活并更新；三是对内核基础设施的大量修改与修复，涉及进程的模拟（simulation）、栈展开（unwinding）、调度以及类的 reshape；四是对检视、调试、性能分析和版本管理这一整套工具链与 UI 的杂项改进。

在 GUI 层面，Morphic 做了一轮相当彻底的树 morph 大修：改善配色与键鼠导航、重做「输入即过滤」并高亮搜索词、用 [TAB] 搜索单列、可配置的过滤模式（在当前选中项内 / 所有可见节点 / 整棵树中搜索）、递归的 find/find again（[CMD]+F / [CMD]+G）、树的自动展开开关，以及拖放时悬停一秒自动展开节点。世界（world）里现在支持把类引用、系统与方法分类、检视器字段直接拖进来打开新窗口。文本编辑器给链接加了悬停下划线，选区加引号时会自动转义嵌套引号。此外还有大量高 DPI 支持的改进：按钮、滚动面板、滑块、菜单、多选列表、树、投影、便签、Etoys 图标、游戏、日历等等都在列。

工具链方面的改动尤其密集。检视器现在允许对象贡献自定义字段和菜单项，新增 shift 元菜单用于检视字段或浏览/调试其访问器，检视超大集合时可以关闭截断显示全部元素；对透明代理对象和不可哈希对象的检视变得更安全，因为工具会避免向它们发送任何消息。调试器新增了 "send until…" 命令用于在嵌套消息发送中精确定位条件，"run to here" 可以到达块内的表达式，栈列表菜单新增「select home」用于找到深层嵌套块的 home context，还新增了 **byteCodes 模式**可以逐条查看和单步执行方法的 VM 指令，以及一个「simulate」按钮，用来调试 Squeak 的元循环求值器（即上下文模拟器）是怎么解释下一条字节码的——这是相当典型的 Smalltalk 式「用系统调试系统自己」。

核心语言层面，Kernel 增加了 `Complex>>log`、`Integer>>bitInvert64`、`Integer>>timesCollect:`、`Number>>timesTwoPower:` 等设施，修正了大整数与分数的 `#sin`/`#cos`/`#tan` 行为，修复了若干数字解析问题，并对整数位运算、整型-浮点混合运算、浮点转分数、三角函数做了优化；`BlockClosure>>valueWithExitValue` 支持带值的非局部退出。正则表达式引擎升级幅度不小：支持 Unicode 字面量（`\x41`、`\u{1F388}`）、字符类转义（`\p{N}`、`\P{Lu}`）、命名捕获组、非捕获组，允许可空闭包重复，新增 POSIX 风格的最左最长匹配开关，并修复了捕获组顺序问题（现在一致地按从左到右存储）。FFI 部分正在开发可在后台运行 primitive 与 callout 的多线程 OpenSmalltalk VM，新增了可配置的异常处理模式、C 声明的变长参数支持以及 ARM64 支持。版本控制方面 Monticello 与 Installer 全面转向 HTTPS，Squot 新版本支持 git revert、tag 和 co-author，Squeak Inbox Talk 发布了第一个稳定版，整合了邮件列表的 HyperKitty 归档。

发布说明也如实列出了已知问题：后台进程抛出的调试器通知可能出现「Proceed」按钮不可用（变通办法是先点 Debug 再点 Proceed）；递归的 process-faithful 调试在某些步进下可能罕见地陷入死循环；lookbehind 内的贪婪模式处理不正确；SqueakSSL 在 Linux 和 macOS 上跑 HTTPS 服务器目前不工作；Linux 上镜像可能耗尽文件描述符（需要 `ulimit -n` 提高上限）；Linux + Wayland 下文本 morph 可能每次点击都丢焦点。兼容性方面有若干破坏性变更，比如 `Promise>>value` 现在会等待接收者、`DateAndTime>>offset:` 修正为符合 ANSI 的语义、`BrokenPromise` 被弃用（promise 被拒绝时会重新抛出原始异常）。测试套件目前有超过 5800 个测试，在 CI 上三分钟内跑完。

## HN 评论精华

这条帖子拿到 289 分，但讨论的重心几乎完全不在 6.1 的具体变更上，而是变成了一场关于 Smalltalk 这种编程范式本身的怀旧与辩论，其中夹着一条相当尖锐的「UI 十年没长进」的抱怨线。

- **avaer** 定下了整场讨论的调子：就像学 Lisp 会让你重新思考编程语言、学 Erlang 会让你理解并发的真正威力一样，学 Smalltalk 会让你明白「面向对象」到底是什么意思。他还顺带说 JavaScript 的好东西几乎都来自 Smalltalk——这句话立刻招来一串反驳。**ramesh31** 追问「具体是哪些」：JS 里既没有根对象也没有消息传递，OO 部分是附带装上去的（class 并不真实存在，原型继承是事后补的），并引用 Crockford 的话说 JS 是「披着 C 外衣的 Lisp」。**vanderZwan** 更直接：这是在抹掉 E 和 Self 的贡献。**Jtsummers** 给出了相对权威的答案——Scheme 和 Self 是主要影响，Java 的语法则是「由公司指令决定的」。
- **kitd** 说他的 Smalltalk 「wow 时刻」是那个持续运行的镜像（image）概念：开发者的工作就是不断把这个活着的系统捏成自己想要的形状。这条引出了整场讨论里最长的一条支线。**jlokier** 补充了一个不太为人所知的事实：GNU Emacs 其实就是这么工作的——你启动 Emacs 时加载的是一个构建期通过运行 Lisp 代码填充好的镜像，早年还用一种叫 `unexec` 的巧妙但不可移植的机制把镜像本身做成可执行文件，直到 27.1（2020 年）才为了可移植性把镜像和可执行文件分开。**mike_ivanov** 和 **packetlost** 指出所有 Common Lisp 实现都能做到（`sb-ext:save-lisp-and-die`），Janet、Chibi Scheme、Chez Scheme 也支持镜像式部署。
- **whartung** 提出了一个让不少人心动的幻想：如果 Smalltalk 镜像不是靠 RAM 而是靠**持久化虚拟内存**支撑呢？把镜像 mmap 到一个 100GB 的文件，让操作系统去刷页。他想象把全部历史邮件当作一个全局 `mailbox` 数组里的一等对象，按需建索引，邮件不「在磁盘上」，而是作为对象活着。他自己也承认这大概「行不通」，而且「运行中的镜像」与「已保存的镜像」之间的界线会很危险。**unignorant** 回应说自己做过类似的东西：一个 git 风格的内容寻址镜像，所有东西按哈希标识，是否驻留内存决定了什么在内存里，镜像上还有一个按这种方式工作的文件系统，可以对超大文件的「冷」字节做分页；任何状态都可以以增量而非全量重写的方式保存。**actionfromafar** 指出这听起来有点像 GemStone/S。**dev_dan_2** 分享了自己 2015 年论文里的做法：用一个位于 Nil 之下、其他所有对象之上的 Proxy 重新实现 `doesNotUnderstand:`，先从持久化存储加载真实对象再把消息转发过去，对消息发送方完全透明；难点在集合（一次加载多少？怎么查找特定对象？）。
- 对镜像模型的反面意见同样鲜明。**mkl** 说二十年前跟着入门教程做的时候，这恰恰是他「趁还没把它彻底搞坏赶紧跑路」的时刻，从此再没碰过 Smalltalk：「我理解不了为什么定制化和不可复现会是一个理想属性。」**igouy** 回应说这是个误解——Cuis-Smalltalk 会记录环境中发生的所有动作（System Browser 里编辑的代码、Workspace 里执行的代码），崩溃后重启同一镜像即可恢复未保存的改动，而且一直都有把代码改动存成纯文本文件的各种方式（change set、package）。**xjdjdkdjn** 贡献了本帖最精辟的一句吐槽：「想象一下你继承的不是同事的代码，而是他的整台机器。然后你发现这台机器同时还是生产环境。」**bowsamic** 则说，从 Smalltalk 你主要会学到：操作系统那些静态而僵硬的抽象，在保护系统完整性、防止一切炸掉这件事上其实做了很多工作——很多 Smalltalk VM 依赖恢复功能是有原因的。
- **atemerev** 起了整场讨论里最实际的一条批评线：这么多年过去，高 DPI 显示器还是没修好，UI 依然是像素化的、慢的，「这是我唯一在乎的 bug，至少十年了还没解决」。**sswezey** 说自己同时在盯 Squeak 和 Pharo 的高 DPI 修复。不过 **mparrett** 引用发布说明指出 6.1 里其实有动静了（按钮、滚动面板、滑块、菜单、多选列表、树、投影等等都改进了高 DPI 支持），**sswezey** 随后在 Mac 上打开镜像验证，回复说「我检查的地方看起来都没问题」。**adius** 和 **isr** 推荐了 Cuis Smalltalk（cuis.st）——它的 GUI 完全基于矢量图形，isr 强调「你看到的一切都是硬件加速的矢量图形，连字体都是」，可以单独缩放、旋转任意窗口、按钮、文本，还能保持清晰的全 Unicode 渲染（代码里也能用 Unicode），你甚至可以把标准的系统浏览器（四个面板 + 编辑器）拆开、按自己的喜好拖放重排，然后一切照常工作。**AdmiralAsshat** 总结了很多人的心情：「Smalltalk/Squeak/Pharo 本该是未来，但它感觉像一个卡在 90 年代的 IDE。」而 **segmondy** 的回应则很 HN：「这么多年了，他们一直在等你的贡献。」
- **davexunit** 问 Morphic 架构有什么好的书/论文/博客可读，引出了本帖信息密度最高的一条回复。**DonHopkins** 梳理了完整谱系：Self（1992，Smith & Maloney）→ Squeak 移植（Maloney & Ingalls，Etoys/Scratch 1）→ 两个独立的 JavaScript 重新实现，外加通过 SqueakJS 实现的浏览器内 Squeak。他强调 Self 的 Morphic 不是传统类层次结构，而是并行的 traits 对象（共享行为）加原型（结构），实例通过 `parent*` 槽委托并做「copy-down」差异化原型；自下而上的工作方式是：复制一个 morph、调整它、再把共享行为抽取到 traits 对象里。而 Squeak 的移植版是单继承的 Smalltalk 类，更接近大多数人说的「OOP UI 工具包」。JavaScript 世界里两个常被混淆的 Morphic 实现分别是：Dan Ingalls 的 Lively Kernel / Lively Web（约 2008，Sun）——完整的活系统，带 Halos、序列化、scrubbing、connector、约束，外加 IDE、parts bin 和世界持久化；以及 Jens Mönig 的 morphic.js（约 2010，用于 BYOB4/Snap!）——一个约 1.3 万行的单文件 Canvas 内核，包含 World/Hand/stepping/脏矩形、模板剥离、带惯性平移的 ScrollFrame。他还提到关于多继承：两个 JS 移植版都没用，Self 的 Morphic 其实也基本没用，LK 靠显式的 Trait 组合层「作弊」，Snap 靠 Squeak 风格的复制（`fullCopy`、`isTemplate` 剥离）「作弊」——那种「活」的感觉来自可复制的 morph 树和共享行为，而不是多继承。有意思的是这条长评论一度被 flag 成 dead，**srean** 和 **Jtsummers** 都 vouch 过但无效，Jtsummers 注意到它在只有 4 分钟大的时候就已经是 [flagged][dead] 了，怀疑是用户误操作而非自动审核。
- **uncircle** 把话题推向更宽的范围：Lisp、Forth、Erlang、Smalltalk、Rebol，这些「能倍增你对编程本质的洞察」的语言太容易被遗忘了，可惜很多人在第一个就止步，更多人连 Python 和 JavaScript 都没走出去。**ux266478** 打趣说这种感慨很有意思，因为清单永远不完整——你忘了强大的 Prolog，Tcl 当然也该在里面。**skydhash** 给出了一个很好的概括：这种感觉就是你会意识到计算是符号操纵——道理很显然，但要真正流畅地运用很难；用 C 之类的语言你离机器太近，很难领会符号操纵，而在 Lisp、Prolog、Smalltalk 式 OOP 这些范式里，你开始用符号及其操纵来设计方案，而不是用值和具体动作，一旦开始这样做，你就在解决问题的阶梯上上了一级——解决的是一类问题而不是一个具体实例。**iLemming** 则反驳「Lisp 被遗忘」的说法，举了 Nubank（用 Clojure 做主力，2019 到 2025 年从约 1200 万客户涨到 1.31 亿）以及 Apple、Walmart、Netflix、Cisco、Amazon 用 Clojure、Google 和 Grammarly 用 Common Lisp 的例子，还特意提到 GitHub 语言统计里 Emacs Lisp 排名之高「简直离谱」——一个纯粹为文本编辑器而生的配置语言，新包每天都在发布。
- **Decabytes** 说他最爱 Smalltalk 的一点是能在运行时检视代码，尤其是从 GUI 直接检视：「我好奇这个按钮的代码在哪」，一点就直接跳到代码。**eitland** 回忆说 Java Swing（以及 Visual Basic）当年也能做到类似的事——虽然不是运行时，但你可以在 IDE 里右键对象直接跳到点击/双击/右键处理器；他补充说，现在的 Web 应用更好看也更容易升级，但在开发者体验和 UX 上我们一路丢掉了很多东西。
- **taolson** 现身说自己是 Alan Kay 团队还在 Apple 时期的 Squeak 早期贡献者，并注意到 SameGame——第一个用 Morphic 实现的游戏——至今还在镜像里。**mannycalavera42** 只回了一句：「多讲点故事！」
- **pkphilip** 问新手该从哪开始（Pharo？Squeak？Common Lisp？），**xkriva11** 推荐了 Pharo 的 MOOC 课程；**brabel** 的提问则引出了对 Glamorous Toolkit 的一段讨论——有人第一反应是抗拒它首页扑面而来的 AI 营销，但 brabel 澄清说 GT 本身跟 AI 无关，它是目前最现代的 Smalltalk 浏览器，用起来非常有趣，他不再用它只是因为内存占用偏重（启动就要 1GB 左右）。
