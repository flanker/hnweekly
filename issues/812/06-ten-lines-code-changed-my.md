---
layout: article
title: "改变了我的世界的十行代码"
issue: 812
number: 6
category: favorites
original_url: "https://pixelambacht.nl/2026/ten-lines-of-code/"
hn_url: "https://news.ycombinator.com/item?id=49866534"
date: 2026-10-02
---

## 文章摘要

荷兰前端开发者、字体黑客 Roel Nieskens 在 2026 年 9 月 25 日写了一篇轻松的怀旧小文，列出十段对他有特殊意义的代码："要么是我写的，要么是我大量抄过的，要么让我笑过，或者哭过。"每段代码配几句回忆，合起来像一部个人的编程成长史，从八位机时代的 BASIC 一路走到 CSS 炫技。

**Hello World。** 第一段是几乎所有八十年代孩子都敲过的两行 BASIC：

```basic
10 PRINT "HELLO, WORLD!"
20 GOTO 10
```

他已经记不清第一次敲的是不是这个版本，也许没有逗号，也许感叹号更多。但电脑很配合，欢快地向世界问好，"尤其是向那个第一次和电脑对话、睁大眼睛的孩子问好"。第二行让他明白了一件事：电脑会照你说的做任何事。Hello world 直到永远。

**JavaScript 的怪癖。** 第二段是一句网上流传很广的笑话代码 `Array(16).join('wat' - 1) + ' Batman'`。字符串减 1 得到 `NaN`，拼接 15 次，再接上 " Batman"，正好是老版蝙蝠侠电视剧的主题曲"NaNaNaNa… Batman"。他说这是老派流行文化和程序员冷笑话的跨界，再加上一大份"JavaScript 真蠢"，第一次运行时真的笑出了声。

**会自我修改的 6502 汇编。** 第三段是一段 10 行的 6502 汇编：循环把 `$2000` 开始的一页内存复制到 `$0400`，复制完一页后，用 `INC` 指令直接改写循环里源地址和目标地址的高字节，于是下一轮就复制下一页，直到第 7 页为止。因为 6502 是小端序，所以改的是 `.loop+2` 和 `.loop+5`，而不是 +1 和 +4。他写道：机器语言离硬件最近，没有警告、没有日志、没有护栏，甚至允许程序在运行时覆写自己的字节。"硬核，还有点危险。"意识到这一点，让他真正明白贴着硬件写代码时"什么都可以"。

**touch.bat。** 一行批处理 `type nul > .\"%*"`，是 Windows 上的"穷人版 touch"。从 Windows NT 用到 Windows 7，他常在 Total Commander 的迷你命令行里敲 `touch filename.txt` 快速新建空文件，"离不开它"。

**CSS 调试器。** `border: 10px solid hotpink;`，他称之为"层叠样式表界的 console.log、样式界的 var_dump()、样式表界的 printf()"。而且永远是 hotpink（亮粉色）。

**无限生命。** `POKE 9450,173`。往内存里正确位置写一个字节，游戏就有了无限生命、能量或金钱。这不只是作弊，更让他第一次意识到：只要知道该去哪里 PEEK 和 POKE，游戏里的一切都可以被改写。

**加速循环。** 一个什么都不做、空转一百万次的 `for` 循环，出自程序员圈子里流传已久的段子"The Speed-up Loop"。故事里的 Wayne 解释说：每逢没修 bug、什么都没干的清闲周，就从循环里删掉一个零，然后告诉经理"上次的需求遇到了性能问题，经过一番深度优化，速度大幅提升了"。作者说，对于那些在无聊、没有方向的项目里熬过的人来说，这个点子颇有诱惑力。

**RTFM 或 rm -rf /。** 2000 年代在 IRC 上求助 Linux 问题时，总会有人给出同一个"万能解药"：以 root 登录，然后运行 `rm -rf /`。他调侃说：屡试不爽，所有 Linux 问题都消失了。

**SKAN2.PAS。** 这是全文唯一一段他自己写的"黑历史"：九十年代初学校终于装了局域网，他和一帮自封"1337 h4x0rz"的朋友用 Borland Pascal 写了个非常粗糙的小工具，去试那些一直没人用、还停留在默认密码的闲置账号。他自嘲"bug 比代码行数还多"，但"毕竟已经是 2.00 版了"。他们闯进了一堆闲置账号，然后……就去玩别的了。

**CSS280。** 最后一段是他得意的作品：只用 280 个字符的纯 CSS 写成的动画小诗，不需要 JavaScript，甚至不需要 HTML，放进一个只有 `<style>` 的空文档就能运行。诗句是 "Roses are #F00 / Violets are #00F / All my base / Are belong to you"，混进了 "All your base are belong to us" 这个老梗。280 字符的限制来自当年 Twitter 从 140 字扩到 280 字之后的代码小品潮流，大家把这种片段贴进空白 CodePen 看效果。他特别得意的是把动画时长写成 `1e+9`：比 `infinite` 更短，无论是持续时间还是字符数。

## HN 评论精华

这篇文章拿到 148 分、46 条评论。讨论气氛轻松，基本是大家各自晒"改变了我的那几行代码"，外加一场关于 CSS 调试该用 border 还是 outline 的小争论。

**Hello World 到底是谁的传统。** dahart 说 BASIC 的 PRINT+GOTO 循环可能也是他在八十年代写的第一个程序，但打印的绝不是 "Hello, World"，而是各种傻话、笑话和幼稚的句子；"Hello, World!" 对他来说是 C 的东西，高中 C 课上才第一次见到。他还回忆在 Atari 800 上用 POKE 把键盘重复延迟调成零、重复速率调到最大，谁来打字都会连按出一串字符，只能断电重启，"11 岁的我觉得这太好笑了"；以及在 IBM PCjr 上用 debug.com 改《创世纪 III》的存档，给自己加满金钱和生命。mcherm 解释 "Hello, World" 的传统来自 K&R 的《C 程序设计语言》，gabrielsroka 纠正说其实更早出现在 B 语言的教程里。guelo 从书架上翻出 K&R 原书，发现书里介绍 hello world 时写的是"第一个程序对所有语言都一样"，说明 1978 年时这已经是既有传统。yencabulator 补充：C 的 hello world 能顺带演示编译和函数定义，而 BASIC 的一行 print 两样都演示不了，所以两边的入门教材自然分法不同。andsoitis 则说，从 BASIC 起步的黑客们有一种更有趣、更有创造力的气质。

**最好笑的回忆。** whartung 讲了个故事：高中时他和朋友在 IMSAI 8080 上跑 `PRINT "FRED IS A GEEK"` 无限循环，对着 Fred 咯咯笑。Fred 不动声色地四下看了看，找到电源开关，把机器关了，就在那块"请勿关机"的牌子旁边。两人目瞪口呆，只好手动输入磁带加载程序的十六进制代码，从磁带重新载入 BASIC，"第二次就成功了"。animal531 在 Spectrum 上写了 `10 GOTO 20 / 20 GOTO 10`，让边框无限滚动；后来他拆开机器把键盘弄坏了，从此每次玩游戏都得用一根导线手动短接键盘矩阵里对应的行列来输入加载命令。tc43 说他在 C64 上学编程时，自己的 "Hello world" 总是 "Frodo lives"。ghoul2 六岁时在 ZX Spectrum 上只写了一行打印自己名字的代码，连循环都没有，但看着它出现在家里新买的彩色电视上，"就此沦陷"。analog8374 说自己第一份"发布"的代码是 C64 BASIC 写的艺术小程序，在屏幕随机位置画随机字符和颜色，被当地商场的 Radio Shack 用在所有展示用的 C64 上。

**自我修改代码。** irregularbowels 讲了九十年代中期上汇编课的经历：他在作业里用了自修改代码，输出完全正确。系主任教授把他叫到办公室说"你的代码会修改自己，这里不允许"。他回答作业要求里没说不行。教授说"好吧，但下不为例"，下节课就宣布任何修改自身代码的作业一律作废。

**CSS 调试：border 还是 outline。** 这是评论区唯一的"争议"。throw34889、chris_engel、bigbezet 都指出 border 会改变盒子尺寸、导致重排，应该用 `outline: 1px solid hotpink`，bigbezet 甚至说这是"很糟糕的建议"。chias 补充 Firefox 里按住 Ctrl 点击页面就能给子元素画轮廓而不重排。yencabulator 不服气地说 border 比 Firefox 和 Chrome 都老，outline 要到 2021–2023 年左右才真正好用；chris_engel 反驳说 outline 自 1998 年的 CSS2 就有了，他从 2000 年代初就一直在用，"也许你该想想到底谁才是新手"。至于颜色，probably_wrong 更喜欢 red，oliyoung 则投票给 `rebeccapurple`，并附上 Eric Meyer 2014 年那篇纪念文章的链接。

**touch 的语义。** layer8 觉得那个 touch.bat 很好笑：他一直把 touch 理解为"更新已有文件的时间戳"，而这个脚本面对已有文件会直接把内容清空。xp84 也说新建文件的用途可以理解，但如果习惯了 touch 的两种用法，起这个别名就有点吓人。

**大家晒的其他代码。** tptacek 认真列了一串对自己意义重大的单行代码：曾经放在他 Usenet 签名里的一个 fork 循环；FreeBSD crt0 里调用 `main()` 的那一行，让他第一次意识到 main 下面还有 C 代码；Yacc 文法里一条递归规则，让他明白用解析器生成器其实就是在建一棵树；以及一行中序遍历二叉树的递归函数。他还提到 Vern Paxson 曾告诉他，自己实现 TCP 重组的方法就是打开一个文件，用 seek 把收到的分段放到正确位置。andsoitis 晒了三行 Lisp 宏 `twice`，用来说明"代码可以生成代码"。readthenotes1 想起 Smalltalk 里可以在运行时改变一个对象的类，这让他意识到对象并不永远绑定在创建它的那个类上。metanonsense 总用一段 BASIC 画圆来唬朋友，perilunar 说自己也是先写成 y 关于 x 的函数，后来才"发现"了参数方程。beanaroo 贴出 `#define true ((rand()&15)!=15)` 这种恶作剧宏，drfloyd51 接着感慨：年轻时他看不起 `#define true 1` 的人，因为只有 `#define false 0` 才是对的，"如今我对什么都没有当年对那些 #define 那么自信了"。drpython 回忆 2000 年代初给《战地 1942》之类的游戏写 DLL 注入器练手，每次都会先注入一句最短的 x86 死循环 `jmp $`（机器码 EB FE），再在 OllyDbg 里观察。

**零碎吐槽。** awwaiid 指出这其实是十段代码而不是十行，"不过很酷，很有启发"。0x000xca0xfe 看到加速循环的第一反应是："作者就是这么发现优化编译器的吧。"bhavaniravi 说，在 AI 写代码的时代读这种文章，就像现代人的"出门摸摸草"。
