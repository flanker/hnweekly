---
layout: article
title: "Racket 友好入门"
issue: 807
number: 24
category: code
original_url: "https://geometridae.bearblog.dev/a-friendly-introduction-to-racket/"
hn_url: "https://news.ycombinator.com/item?id=49399898"
date: 2026-08-28
---

## 文章摘要

这是 geometridae 于 2026 年 8 月 11 日发在 Bear Blog 上的一篇 Racket 速成教程，开篇引用了 Eric S. Raymond 那句名言：「Lisp 值得一学，因为你终于领悟它时会获得的那种深刻的启蒙体验。」作者给全文定的目标很明确：读完之后，你将写出属于你自己的语法。

**第一部分是历史。** Lisp 诞生于 1958 年，由 John McCarthy 在 MIT 发明，是目前仍在使用的第二古老的高级语言（只比 1957 年的 Fortran 晚一年），比 Python（1991）和 JavaScript（1995）早了三十多年。作者列举了一串「我们今天视为现代的东西其实生于 Lisp」：垃圾回收、一等函数、REPL（Python、Node、Julia 今天都有的交互式读取-求值-打印循环）、把条件语句当作有返回值的表达式，以及最关键的**同像性（homoiconicity）**——代码本身就是这门语言的一种数据结构。文章还提到 70、80 年代 Symbolics 和 LMI 制造过专门跑 Lisp 的物理机器（Lisp Machine），AI 寒冬后经费枯竭，Lisp 从明星沦为小众信仰。作者的评论是：「有趣的想法不会死，它们只是变异了——这也是 Lisp 系语言之美的一部分。」

**第二部分是从 Lisp 到 Scheme 再到 Racket 的谱系。** 1975 年 Gerald Sussman 与 Guy Steele 创造了极简、优雅、近乎数学化的 Scheme，成为学界最爱的教学语言（SICP 就是用 Scheme 写的）。1995 年 Matthias Felleisen 的团队做出面向教育与语言研究的 PLT Scheme，2010 年更名为 Racket。今天的 Racket 已经远不止是一个 Scheme，而是**一门用来造语言的语言**，其非官方口号是「面向语言的编程」（language-oriented programming）：如果你的问题需要一门自己的语言，Racket 让你在一个下午之内造出来。作者附了一张时间线表格（1958 Lisp / 1975 Scheme / 1984 Common Lisp 标准化 / 1995 PLT Scheme / 2007 Clojure / 2010 更名 Racket / 今天：正准备写括号的你）。

**第三部分回答「今天谁还在用 Lisp」：** Clojure 跑在银行、航空公司和创业公司的生产环境（拉美最大数字银行 Nubank 就建在 Clojure 上）；Common Lisp（SBCL）仍活在专家系统、航班规划（被 Google 收购的 ITA Software 驱动了 Google Flights）和科学计算里；Emacs Lisp 让数百万人每天都在跑 Lisp 而不自知；Guile/Guix 是一整个 100% 用 Scheme 配置的 Linux 发行版；Racket 自己有年度大会 RacketCon，被用于语言研究、形式化验证（Rosette）、排版与出版（Pollen）和教育。此外新的 Lisp 还在不断出现：编译到 Lua 的 Fennel、Janet、构建在 Python 之上的 Hy。作者还埋了个彩蛋：动画《The Amazing Digital Circus》第 8 集里 Kinger 打开终端想重置 Caine 时，可以看到这个 1996 年造出来的创意 AI 是用 Lisp 写的，文件名就叫 `Caine-core.lisp`。

**第四部分是实操。** 安装只需五分钟：去 racket-lang.org 下载安装包，打开自带的 DrRacket（上半区写定义、下半区是 REPL），在第一行写 `#lang racket` 告诉 Racket 你用哪门语言——因为 Racket 是个语言工厂，你必须挑一门。命令行党用 `racket` 进 REPL、用 `raco` 做包管理。

作者随后用几组极短的例子铺完了核心：

{% raw %}
```racket
> (+ 1 2)                      ; => 3
> (* 3 (+ 2 2))                ; => 12
> (string-append "hello " "world")
```
{% endraw %}

他把 Lisp 的规则浓缩成一行：**永远是 `(操作符 参数1 参数2 ...)`，没有例外。** 没有运算符优先级要背，也没有任何东西的特殊语法。那些一开始吓人的括号，其实恰恰是「完全没有任意规则」的体现——一周之后你就看不见它们了。

接下来是定义与函数（`define` 带名字创建常量，带 `(名字 参数...)` 创建函数，`;` 开始注释，`lambda` 造匿名函数，作者顺手点出这就是 1930 年代 Church 的 lambda 演算），列表（`list` / `'(1 2 3)` / `first` / `rest` / `cons` / `length`，并特别提醒记住引号 `'` 的含义：**别求值，这是数据**），高阶函数（`map` 变换、`filter` 筛选、`foldl` 累积——「有这三个函数，你不写一个 for 循环也能解决大部分列表问题」），以及递归。递归的思维方式被描述成「不是想『重复 N 次』，而是想『基线条件是什么，我怎么向它靠近』」。为了让它可视，作者给了个用 Racket 自带 `2htdp/image` 图形库画谢尔宾斯基三角形的例子，贴进 DrRacket 按 Run 就能看到图形出现。

**大结局是「写代码的代码」。** 引号把代码变成数据：`'(+ 1 2)` 得到的是列表而不是 3，`(first '(+ 1 2))` 得到符号 `+`，而 `(eval '(+ 1 2))` 又把数据当代码求值回 3。你的程序是一个列表，你能构造列表，因此**你能用程序构造程序**——这就是同像性，也是 Lisp 拥有真正宏（不是 C 那种文本宏，而是接收代码、返回代码、在一切运行之前执行的函数）的原因。作者用一个例子收尾：Racket 没有 `while` 循环？那就自己发明一个。

{% raw %}
```racket
(define-syntax-rule (while condition body ...)
  (let loop ()
    (when condition
      body ...
      (loop))))
```
{% endraw %}

「你刚刚扩展了这门语言——在 Lisp 里，语法是你的。」文末引用 Alan Kay 把 Lisp 称为「软件的麦克斯韦方程组」：一个极小的内核，其余一切都能从中推导出来。推荐读物为 *How to Design Programs*、Racket Guide、*Beautiful Racket* 和 SICP。

## HN 评论精华

这条帖子拿到 271 分、146 条评论。有意思的是，讨论几乎没有停留在教程内容上，而是迅速分裂成三个方向：**Lisp 到底好在哪、Racket 的「无特殊语法」是不是谎话、以及为什么现实中没人用它**。

- 讨论量最大的一楼来自 **vatsachak** 的一句挑衅：「除了热重载，我从没看出 Scheme 系有什么吸引力。」**WalterGR** 只回了一个词：「同像性。」**so-cal-schemer** 则给出了全场最完整的辩护：Lisp/Scheme/Racket 是一门极简却极具可塑性的语言，能精确、简洁、直觉地表达想法，而且**没有边界**——想要中缀数学表达式或者借用检查器？那是个宏。想要惰性求值？在元循环求值器里改一行。想要一门最贴合你问题域的语言？写个 DSL。他还提到这种语法早在 LSP 出现之前就天然支持结构化编辑，并附了 Beautiful Racket 的《Why Racket? Why Lisp?》《Why language-oriented programming?》、ACM Queue 上 Felleisen 的《Creating Languages in Racket》以及 FOSDEM 2026 的演讲《Lisp is clay: the power of composable DSLs》。
- 第二大楼是对原文那句「没有任何东西的特殊语法」的当场打脸。**GregBuchholz** 贴了一段合法但令人眼晕的 Racket 字面量：包含 `-5/6+7.s-8i`、`1@1`、`10#`、`#i+1`、`#e-1e10i`、`#()` 等等，末尾还挖苦一句「意外吧？」。**soegaard**（Racket 社区的常客）耐心地把它拆解开：`#i` 是不精确前缀、`#e` 是精确前缀，所以 `#e0.1` 会被读成精确分数 1/10 而 `#i0.1` 是浮点数；`8i` 里的 `i` 是虚数单位；`1@1` 是复数的极坐标写法；`10#` 是历史遗留——Scheme 作者想表达「已知有效数字有几位」，`#` 代表「某个数字」，多数实现读成 0；`#` 跟一个列表就是向量。他的辩护是：既然要同时支持精确数（大整数与分数）和不精确数（浮点），有 `#i` / `#e` 来显式选择是合理的。
- 这条底下的反应很有代表性。**Syntonicles** 说他一开始被这条评论惹恼——他有相当的 Lisp 经验却读不懂这些例子；然后他在一分钟内下载安装了最新 Racket 进了 REPL，「真的没有借口」。玩过之后他的评价是矛盾的：「我不确定这算 reader 臃肿/被污染，还是我用过最酷的数值解析器」，只能希望这一切底下有某种优雅。**liendolucas** 更直接：这条评论恰好说清了他一直无法被 Racket 吸引的原因，「至少对我来说它的语法看起来不连贯」。**BoingBoomTschak** 反问：「它怎么做到比 Common Lisp 还巴洛克的？」**shawn_w** 补刀：「你还漏了哈希表和正则表达式字面量。」**fweimer** 指出其中至少有一个是未定义行为（`` `(1 ,@2) ``），并感慨 Scheme 留下的未定义之处出奇地多。
- 第三条主线针对「友好」二字。**fn-mote** 说得很不客气：「虽然我是 Racket 粉，但这不是友好的入门，这是速通。当一篇入门自称『友好』时，我不指望它默认我知道 lambda 是什么；也不指望里面出现 syntax rules——一次都不该出现。」**allthetime** 强烈反对：这篇文章信息密度高、切中要害，一个有动力、有能力的读者读完就能对 Racket 有实感并动手用它——他自己此前完全不懂 Racket，「什么样的语言入门会不讲语法规则？我们都是有见识、有经验、有技术倾向的人，不是笨小孩。」
- 「为什么没人用」这一楼由 **zerr** 挑起：语言很有趣，可惜野外无人使用，也许是部署选项太麻烦，能产出原生独立可执行文件的话应该会拉高使用率。这个前提立刻被多人纠正：Racket 早就能打包独立可执行文件（有人贴了官方文档），**velcrovan** 更进一步——他不但产出独立可执行文件，还能在 macOS 上交叉编译出 Windows 的 GUI 应用。**zelphirkalt** 给出了他认为的真实原因，也是全场最扎心的一段：这跟部署无关，人们每天把各种东西塞进 Docker 容器；真正的原因是**很少有人愿意花力气学一门 Lisp，愿意学 Racket 的更少**。少数几所用 Racket 教基础课的大学撼动不了大局，你的同事没人懂这套东西——你会成为公司里那个决定采用「除你之外没人会」的技术的人吗？不可能。他说自己在德国从没遇到过哪怕开始学这些东西的同事，即使他在某个周五下午展示了 Scheme、秀了个很酷的管道式 `syntax-rules` 宏、并演示它如何消除 Python 里无法消除的样板代码。不过他也感激当年那位教授的信条：「我不是来教你 C、Java 或 Python 的，我是来教你计算机编程的。」
- 关于「野外用例」，有几条硬货：**hyperbolablabla** 指出顽皮狗（Naughty Dog）的全部脚本都用 Racket；**CoreformGreg** 说他们用 Racket 的 Scribble 包定义自文档化的文件格式，并自动生成 C++ 和 Python 源码；**mark_l_watson** 用 Racket 写了自己的 AI 编码框架，此前用 Common Lisp 和 Python 做过更早的版本，但 Racket 版功能最完整。**em-bee** 抱怨每次 Racket 上 HN 他都想找些有意思的应用来玩，结果 awesome-racket.com 上全是库和开发工具。**Zambyte** 的回复堪称本楼最佳：「最著名的最初用 Racket 写的应用之一是 news.ycombinator.com」（Arc 语言最初实现于 MzScheme/PLT Scheme 之上）。
- **ux266478** 提出了一条严肃的历史更正：原文说「AI 寒冬导致 Lisp 陨落」并不准确——Lisp 在那之前就已经失去相关性了，只有美国还在用，而且多半是出于技术债和不肯转向的固执；Prolog 在 70 年代末就取代了它，即便在美国，Lisp 那部分也只是「为了得到一个 Prolog 形状的东西」的不幸实现细节。他认为 80 年代美国在这个领域大幅落后日本并不意外，「任何声称宁要 Lisp Machine 不要 PIM 的人，要么对符号计算没兴趣只是喜欢 Lisp，要么完全不了解 PIM 在符号 AI 上先进多少」。**so-cal-schemer** 给了另一种解释：跑 Unix 的小型机/微机变得便宜太多，1989 年柏林墙倒塌后国防部对昂贵 Lisp 工作站的资助迅速枯竭，而当时刚起步的自由软件 Lisp 实现还很不成熟；他附了一份语言流行度可视化数据，显示 1984 年 Q1 Lisp 是第三流行的通用语言（在 Pascal 和 C 之后，Prolog 排第 12 然后跌出榜外），以及 Richard Gabriel 1990 年那篇著名的《Lisp: Good News, Bad News, How to Win Big》。
- 最后是两条轻松的：**perrygeo** 接住了文章里的 TADC 彩蛋——「这解释了 Caine 为什么能在第 9 集回来：Lisp 的续延（continuation）允许优雅的错误恢复。」而 **usxr1515** 提了个正经的技术问题：他在用 Rust 写自己的小语言运行时（VM + JIT + AOT），一直拿 Racket 的宏和面向语言编程当参照，好奇这种灵活性有多少是真实的运行时代价、多少只是编译期抽象。**brabel** 以 SBCL 为例回答：Common Lisp 会把每个函数编译成原生代码（可以用 `disassemble` 看汇编），而宏在代码编译前就已展开执行完毕，**函数编译完成后宏就消失了**——它唯一的职责就是生成那些真正会被编译和执行的表达式。他还顺手演示了一个用 `unwind-protect`（相当于 try/finally）实现 Go/Zig 风格 `defer` 的宏，并解释了反引号（准引用）、逗号（反引用）和 `,@`（展开）这三个符号为什么在宏里满天飞。
