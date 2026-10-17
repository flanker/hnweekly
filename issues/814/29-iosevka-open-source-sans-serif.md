---
layout: article
title: "Iosevka：开源的无衬线与粗衬线等宽字体"
issue: 814
number: 29
category: design
original_url: "https://typeof.net/Iosevka/"
hn_url: "https://news.ycombinator.com/item?id=50084240"
date: 2026-10-16
---

## 文章摘要

这次上榜的是 Iosevka 字体的官方展示页（当前版本 34.9.0），而不是一篇文章。Iosevka 自我介绍为一个开源的字体家族，同时提供无衬线和粗衬线（slab-serif）两种风格、等宽和「准比例」两种间距，用于写代码、在终端里使用以及排版技术文档。它由开发者 Renzhi Li（GitHub 用户名 be5invis）维护，项目已有十多年历史，按 HN 评论里的回顾，最早一次 Show HN 是在 2015 年。

**家族构成。** 官方包里有 6 个等宽子家族：无衬线和粗衬线各分三种间距，即默认的 Iosevka、面向终端的 Iosevka Term 和关闭连字的 Iosevka Fixed（粗衬线版本对应 Iosevka Slab、Term Slab、Fixed Slab）。每个等宽子家族都有从 Thin 到 Heavy 的 9 种字重、Normal 和 Extended 两种宽度，以及正体、意大利斜体（Italic）和倾斜体（Oblique）三种倾斜方式。另外还有两个「准比例」子家族：无衬线的 Iosevka Aile 和粗衬线的 Iosevka Etoile，宽度只有一种。

**省空间是核心卖点。** 展示页第一部分叫「空间高效」：Iosevka 的等宽字形默认非常瘦长，每个字形正好半个 em 宽。和竞争对手相比，同样的屏幕宽度能放下更多列。页面上直接放了与 Fira Code、IBM Plex 的对比示例。如果觉得太挤，可以选 Extended 宽度，「让字符之间多透点气」。

**为终端做的专门版本。** 终端模拟器对字体的兼容性要求更严格，所以 Term 和 Fixed 两个家族会把各种符号做得更窄，以遵守终端「按列计数」的规则；Fixed 家族还关闭了连字，保证在某些环境下的兼容性。

**高度可定制。** 「按需点菜」一节介绍，Iosevka 有 146 个可配置字符和 19 套风格集（stylistic set）。风格集基本是在模仿各家经典等宽字体的特征，包括 Andale Mono、Anonymous Pro、Consolas、Menlo、Fira Mono、Liberation Mono、Monaco、Pragmata Pro、Source Code Pro、Envy Code R、X Window、Ubuntu Mono、Lucida、JetBrains Mono、IBM Plex Mono、PT Mono、Recursive Mono、Input Mono 以及一个「卷曲」风格。你可以直接选一套，也可以逐个字符挑选变体，比如数字 0 要不要加点或加斜线、小写 l 尾巴怎么弯。网站还提供在线定制器，生成配置后用仓库里的构建系统自行编译。

**编程连字。** Iosevka 的等宽家族不仅有一套通用连字，还支持针对特定编程语言的连字，只需在编辑器里开启对应的 OpenType 特性。借助较高级的 OpenType 技术，它还支持任意长度的长箭头、由等号拼成的长横线等「长连字」。（页面上一直把连字写作 ligation 而不是常见的 ligature，HN 上不止一人觉得别扭。）

**为写作准备的准比例字体。** Aile 和 Etoile 面向文档和写作，字符宽度有更多变化，比如 w 和 m 略宽、i 和 l 更窄，读长文时更舒服。页面最后列出了支持的语言数量：248 种，覆盖拉丁字母、希腊字母（包括多调希腊文）、部分西里尔字母、国际音标以及常用标点和符号。

从 GitHub 仓库的 README 可以补充几点：字体以 SIL 开源字体许可证发布；Homebrew、Arch、Void、Fedora COPR、openSUSE、FreeBSD、OpenBSD 等都有现成的包；字体本身用作者自创的参数化字形描述语言 PatEL 以代码生成，这也是它长期被称为「为代码而生、由代码生成的字体」的原因。

## HN 评论精华

本帖 743 分、310 条评论。作为一个十多年的老项目，Iosevka 再次冲上首页，评论区几乎变成了一场等宽字体大会：老用户表白、劝退者吐槽太窄、连字之争，以及各种衍生版本和竞品推荐。

**展示页本身先挨了批。** 置顶最高的评论来自 oefrha：字体展示站可能是最不该用 `font-display: swap` 的地方，他还纳闷怎么看起来和 Courier 没区别，结果看到的就是 Courier。embedding-shape 解释了这个 CSS 属性：在字体加载完成前先用备用字体显示，所以你会一直看到 Courier，直到 Iosevka 加载完，或者加载失败就永远是 Courier。kqp 认为这种教条来自谷歌，谷歌把它当成无条件的建议，还在自家评分工具里把不这么做标成错误，并暗示评分低会减少流量。jrmg 发现在 iPad 上，示例框里的 Fira Code 和 IBM Plex 也显示成了 Courier，两者完全一样；pimlottc 甚至以为字体来回变化是故意展示不同字重的动画。

**一旦习惯就回不去了。** 大量用户表示自己用了多年。TwoFerMaggie 以前用 Fira Code，习惯了紧凑的 Iosevka 后，其他字体在 IDE 和终端里都显得「松散、不够省空间」。IceDane 说他时不时尝试寻找比 Iosevka 更好的字体，但没有一个能留下来。cik 说这个字体真心为他的生活增色。GCUMstlyHarmls 讲了一个有意思的经历：第一次用 Iosevka 时他感到恶心，有点像晕车，但在小屏幕上能多塞几个字符实在诱人，就又试了一次，现在用了六七八年，反而觉得其他等宽字体「宽得奇怪」。lemontheme 说他第三次回头才「开窍」，同样字号下终端能放下更多文字，而每个终端窗口都跑着编程智能体，这也是原因之一。insensible 和 huffer 都说，这种窄字体让他们想起小时候 Apple II 的 80 列模式、或者装在苏联仿制 ZX Spectrum 上的 CP/M 里那种瘦高的字体。

**也有人怎么都看不惯。** goatking 说他总是惊讶于这个字体受到的赞誉，每次试都觉得完全没法读，刚刚又把终端从 JetBrains Mono 换成 Iosevka Term 试了一下，还是不明白大家怎么读得下去。bowsamic 用了之后眼睛疼，drmr 说太伤眼。code-blooded 的观点很有代表性：在相同的整体文字密度下，他觉得宽一些的字体浏览起来更快。不少人建议这类用户试试 Extended 宽度，或者自定义一个中间宽度，needfish 和 frazer2669 分别说他们找到了 576 和 548 这两个「刚刚好」的自定义宽度值。nvme0n1p1 认为它的流行部分源于先发优势：它是最早包含大量编程连字的字体之一，早早赢得了心智份额，而字体选择非常个人化，用的人越多就越多人喜欢和晒图。

**编程连字之争。** jve 问「是不是只有我讨厌代码连字」，结果引来一大片附和与反驳。他抱怨要数清连续几个等号得先选中文字，`<>` 被画成菱形还得复制出来才知道是什么。wildrhythms 说他在所有编辑器里都关掉连字，不想猜是双等号还是三等号；doodpants 认为如果字体把 `>=` 显示成 ≥，就是在对文件里实际有哪些字符「撒谎」；kps 说 Unicode 都发布 35 年了，想要 ≠ 就直接用 ≠。支持者也不少：arialdomartini 说写程序和数学时，他把 `->`、`>>=`、`<*>` 看成单一的、有语义的符号，甚至给它们分配了单独的按键；throwuxiytayq 说有些字体把三等号画成三条线、长度也是三倍，根本不会认错。mkesper 和 sicktriple 提醒，Fixed 版本本来就是关闭连字的。cloverich 把它归入「Tab 与空格」之争：选好你的偏好，然后默默走开。

**字体本身的工程。** wartywhoa23 特别致谢作者为生成字体专门创造了参数化字形描述语言 PatEL，gorgoiler 称其为「真正的黑客字体」，janOla 说它的制作方式受到了高德纳 Metafont 的启发。weinzierl 喜欢它字重丰富，可以把字重当作语法高亮的另一个维度。JdeBP 注意到它在拉丁文和西里尔文中都区分了意大利斜体和倾斜体，等宽字体很少这么做，还包含 MouseText 和七段数码管数字等字符。mcookly 指出它是少数对多调希腊文支持很好的等宽字体之一。est 提到 Iosevka 可以让含中日韩文字和 emoji 的等宽表格、流程图完美垂直对齐；remark5396 和 aleyan 推荐了同一作者的 Sarasa Gothic（更纱黑体），把 Iosevka 和思源黑体合在一起，aleyan 还说两个 Iosevka 半角字形正好拼成一个正方形，他借此在终端里模拟六边形网格，做了一款名为 HexWalker 的 Roguelike 游戏。starquake 补充，从 GNOME 48 开始，默认等宽字体 Adwaita Mono 就是一个修改版的 Iosevka。

**体积与构建门槛。** HackerThemAll 抱怨单个变体就有半个 G，einpoklum 说下载整个发布包要 2.4 GB，每个文件都超过 50 MB；c-hendricks 说他自己定制构建的版本只有约 16 MB，antiframe 解释体积主要来自码位覆盖范围。Daneel_ 抱怨在网站上能把字体调得很完美，却不能直接下载，还得折腾 npm 自己构建，「这简直是犯罪」。paradox460 和 nektro 希望官方能提供可变字体版本。nylonstrung 说 Nix 包经常重新构建而不走二进制缓存，拖慢了他的系统更新，于是干脆卸载了。

**衍生版本与竞品。** Iosevka 的可定制性催生了一批「模仿版」：模仿 Pragmata Pro 的 Pragmasevka 和 Iosvmata，模仿 Berkeley Mono 的 Ioskeley Mono，以及 Emacs 圈知名人物 Prot 的 Aporetic（原名 Iosevka Comfy），allumik 说他现在全面改用 Aporetic，wartywhoa23 则提醒「那其实还是 Iosevka，只是定制构建」。竞品推荐包括 Berkeley Mono（不少人说用过就回不去，但要付费，farlight 指出它字符覆盖很差）、Commit Mono（bowsamic 称之为免费版的 Berkeley Mono）、Maple Mono、JuliaMono、Victor Mono、Input、Comic Code 和 Consolas 等。围绕 Pragmata Pro 也有争议：Diti 说 Iosevka 是基于 Pragmata Pro 的，7e 直言其他都是抄袭、应该去买原版支持原创者；Zizizizz 回应说只有 SS08 风格集深受其影响，Pragmata 在水平方向更挤，Iosevka 的行距也更宽松。climech 说他多年前就爱上了 PragmataPro，但实在买不起，后来 Iosevka 出现了，他再也没回头。

最后几个小插曲：好几个人说自己是从程序员主播 tsoding 的直播里认识这个字体的，cschep 说每次读到这个名字，脑子里都会响起 tsoding 念它的声音；有人问为什么它在 HN 首页，drob518 回答：「因为这里是 Hacker News。」
