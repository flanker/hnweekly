---
layout: article
title: "Bonsai：Jane Street 用 OCaml 写的 UI 框架"
issue: 804
number: 23
category: code
original_url: "https://github.com/janestreet/bonsai"
hn_url: "https://news.ycombinator.com/item?id=49152842"
date: 2026-08-07
---

## 文章摘要

Bonsai 是 Jane Street 开源的 UI 框架，用 OCaml 编写，部分灵感来自 Elm，用于构建高性能的响应式 Web 应用。这不是一个玩具项目：Jane Street 内部几乎所有 Web 应用都由它构建，从公司通讯录一直到监控和操作交易系统的工具。README 里的入门示例是一个掷骰子组件——用 `Bonsai.state` 声明状态和 setter，用类似 JSX 的 {% raw %}`{%html|...|}`{% endraw %} 模板语法写视图，点击按钮时随机换一个骰子面。

框架的核心主张是「拆开」而非「打包」。README 认为，其他 Web 框架倾向于把状态、增量计算和渲染三件事揉进同一个抽象——UI 组件；Bonsai 则允许你按需自由组合状态原语和增量计算原语。同一套用于避免用户交互时整页重渲染的原语，也可以用来对一个实时更新数据集上的昂贵业务逻辑做增量化。README 给了 React 用户一个类比：想象一切都用非常类似 hooks 的东西，而状态管理在组件层级之外。因为状态不与具体组件绑定，框架提供了一整套管理状态生命周期和作用域的 API：比如你想把一组有状态的 UI 组件嵌进另一个组件（例如做标签页界面），Bonsai 会替你处理状态管理，而不是要求你把每个内部组件的状态手动提升到应用顶层的 model。组件本身被实现为纯函数式状态机，易于组合；框架内部的增量化意味着值在必要之前不会被重算，而且这适用于每一个值，不只是视图。

用 OCaml 写还带来了前后端同语言同类型的好处。README 特别强调，当你大量利用 OCaml 类型系统来减少错误时，这对大型 Web 应用代码库的可读性和可管理性影响之大很难被高估；在 Jane Street，很多以前只有终端界面的内部系统，因此可以很容易地把已有的类型和业务逻辑移植到 Web 上。此外框架还自带强大的模板语言、组件级样式表支持，以及全应用自动化测试系统。

README 用相当篇幅展示测试能力，这被称为 Bonsai 最强的特性之一：你可以写出很真实的测试，用程序操作 UI 元素并观察 DOM 的演变。示例是测试一个用户选择器：先创建 handle 并展示初始 DOM，然后用 `Handle.input_text` 往输入框里打字，再用 `Handle.show_diff` 展示 DOM 的差异——输出直接是一个带加减号的 HTML diff，显示 span 里的文本从「hello」变成了「hello Bob」。这套 expect test 甚至能显示 HTML 属性和 class 名如何随用户输入变化，测试里也可以 mock 服务端调用、可以断言驱动 UI 的状态变化。README 的结论是：有这样的测试，你可以在不打开浏览器的情况下写完一整个组件。

最后 README 澄清了一个「褶皱」：Bonsai 本身比上面听起来的更通用——它其实是一个构建通用的增量式、可组合状态机的库。`Bonsai_web` 是在其之上为浏览器 UI 做的特化，此外还有用于构建交互式终端 UI 的 `Bonsai_term`，甚至有过一个愚人节的 `Bonsai_vr` 原型用于响应式 VR 界面。整个家族包括测试库、组件库、示例库、benchmark 库，以及两个预处理器：类 JSX 的 `ppx_html` 和写 CSS 的 `ppx_css`。文档方面有 Quick Start、Thinking in Bonsai、系列 how-to 文章、关于框架历史的博文，以及 Signals & Threads 播客里专门讲「构建一个 UI 框架」的一期。

## HN 评论精华

390 分、145 条评论。讨论真正的重心并不在框架的技术设计上，而是分散在三处：对「又一个用自家语言重造轮子」的调侃、一场关于交易员 UI 信息密度的相当精彩的辩论，以及对 Jane Street 这家公司本身的好奇与吐槽。

**「因为他们就是爱 CAML」**：Traster 贡献了全场最高赞的玩笑，他自告奋勇重写了 README 的「Why Bonsai?」一节：「在 Jane Street，我们超级喜欢函数式编程尤其是 CAML，所以需要低延迟软件时我们用 OCaml，需要硬件时我们自己写语言叫 HardCaml，需要 Web UI 时我们就用 CAML 造一个 Web UI 框架。因为我们他妈的爱 CAML。」troupo 认为这完全正当：「很多语言都在尝试同一件事。既然一种语言就能搞定，何必折腾 15 种？参见 Elixir 的 LiveView 和 Hologram。」bofeiw 则借机唱衰整个框架赛道：「看起来是在重造轮子。框架今天已经不重要了，反正代码是 AI 写的。」ForHackernews 反驳：「更好的框架依然能帮 AI 避免愚蠢错误。框架更聪明，你就能用更笨更便宜的 AI 保持生产力。」KolmogorovComp 补充说 AI 从严格的编译器中受益极大。ramesh31 站在唱衰一边：「UI 库现在基本没意义了，我说这话的身份是过去十年都在造 UI 库的人。只要有像样的数据 API，你离一个测试完备、无依赖、更好维护的定制库只有一条 prompt 的距离。」bobjansen 的一句「这要是 2014 年会很酷」也引出了不少附和，不过 keepupnow 反手一句：「我得纠正你，现在就很酷，因为千禧年初的东西又回潮了宝贝。」

**「终于可以前后端同语言了！」的反讽支线**：flufluflufluffy 引用 README 里「因为 Bonsai 用 OCaml 写，所以前后端可以用同一种语言和类型」，然后来了句「终于！我一直在等这成为可能！」——显然是在拿 JavaScript 早已在后端流行多年开玩笑。这条引出 6 条回复的技术梳理：philipwhiuk 列举了 Scala.js 等类似尝试，并点出真正的难点，「用后端语言写的那部分前端代码编译成 JS 之后，如何与 JS 生态的其余部分整合。Jane Street 喜欢一切自己从头写所以不受影响，但你可能受影响。因此大多数人最后选的是 frontend-as-backend 而不是 backend-as-frontend」。其他人补充了 Ocsigen、WebSharper、ClojureScript、Kotlin/JS、F# 的 Fable，emmelaich 一路追溯到 2006 年的 GWT。lbourdages 问 WASM 能不能一劳永逸解决这个问题，得到两条扎实的回答：danielheath 指出你仍需要一个 JS trampoline 来调用 WASM 并把浏览器原语暴露给它，而且高级语言的源码通常比编译产物小得多，简单场景下用户可能要多下载十倍的代码；applfanboysbgon 则说 WASM 目前还不能直接访问 DOM 和 Web API，每次与浏览器交互都得先过 JS interop 并付出性能代价，「有提案，也许 2035 年能看到」，现阶段 WASM 更适合性能收益能盖过 interop 成本的重负载场景。这条线还意外岔出一段关于「TypeScript 算不算另一种语言」的争论：warpech 说「编译 TS 只是把类型剥掉」，Stratoscope 指出 enum 和 namespace 这类构造需要真正的编译，igl 补充说 TS 团队早已把它们视为错误、去年还加了 `--erasableSyntaxOnly`；gleenn 坚持「需要转译才能在浏览器里跑，那它就是另一种语言」，jitl 反驳说 98% 的 TS 专有语法可以直接替换成空格就得到可运行的 JS，两者运行时语义是一致的，「据我所知没有别的语言能这么说」。

**信息密度之争（评论区最有营养的部分）**：rw2 说「性能大概很好，但对我来说太丑了，边距肯定能修一下还不影响性能」，pgwhalen 追问「边距怎么了」，由此引出一场大讨论。pgwhalen 自己给出了经验之谈：「交易员偏好 UI 里有极少的留白。」OneDeuxTriSeiGo 引用 Jane Street 的 Signals and Threads 播客说他们有专职 UX 设计师，几乎不写代码、只专注于为交易员优化体验，「大部分常规 UX 规则在这里都不适用，因为宽客和交易员的需求和『普通软件』太不一样」。jackcarter 顺势提出一个尖锐的问题：「有哪种专业软件是不会从高信息密度中受益的？大多数软件设计是为那些必须批准它但不使用它的非用户优化的。」RugnirViking 的回答让很多人共鸣：「我们公司真正重要的内部软件都非常密集，有键盘导航和快捷键；那些为了在管理层面前表现自己、顺便谋求晋升而做出来的软件都很漂亮，但从来没人用。可我们自己给客户做的软件同样美丽而空洞，因为 B2B 卖的对象不是真正使用软件的人。」ambicapter 提出反面：非用户也是重要目标，你想让没用过你软件的人开始用，这时可发现性胜过信息密度；tjoff 反驳说那只是「偷懒的用户友好」——尽量少做，逼用户适应你。OneDeuxTriSeiGo 补充了一个重要例外：安全关键软件的「操作空间」必须保持低密度并尽量做物理分隔（实体按钮和显示器而不是一块大触摸屏），因为人在压力、缺觉、警报响个不停的非理想条件下，信息和操作的清晰度要优先于密度。jacobolus 则站在中间：「你可以既有高信息密度又不让文字挤在一起。把文本和信息图周围的空间全抽走会降低易读性，哪怕对熟练的高级用户也一样。我觉得这里的目标更像是一种『仅限严肃交易员』的美学，而不是为速度和清晰度优化的 UI。」troupo 借题发挥吐槽当下的设计风气：「我现在愿意用命换紧凑的 UI。所有东西都坚持要一万五千英里宽的边距，8K 屏幕上最多显示两个条目。我们已经到了这样一个地步：1990 年代跑在 12 寸、最高 480x240 分辨率屏幕上的 TUI，显示的信息比现在几乎任何东西都多。」eej71 点名 JIRA 是「留白荒原」的典型；steve_adams_86 抱怨近几年 macOS 的 UI 倒退让他那块又大又漂亮的显示器越来越没用。yuanBuilds 直接说 UI 元素「像 90 年代一个聪明高中生在 Windows 上给自己副业项目做的界面」，pianoben 的回击颇有分量：「90 年代末到 00 年代初的 Windows 尤其是 Macintosh UI，是桌面时代以来功能性最强的界面——用户研究是门严肃学科，无障碍被内建进框架，人机界面指南给了我们可以依赖的统一约定。如果对 Bonsai 最大的批评是『不漂亮但好用』，那 Bonsai 万岁，再多来点。」rixed 则提醒：「你是不是把 Bonsai 和 CSS 库搞混了？怎么构建 Web UI 和怎么给 HTML 上样式是两个问题。」

**框架定位与实用性问题**：strongly-typed 转述了播客里一个帮他理解 Bonsai 的框架性说法——「它其实是一个构建增量式分布式状态机的框架」；frutiger 指出 README 里就是这么写的；HolyLampshade 接了一句妙语：「整个现代金融市场本质上就是增量式分布式状态机。」lenkite 确认其价值主张在于用了 Jane Street 自家的 incremental 增量计算库。avsm 澄清了 xvilka 的疑问（看起来只支持 Web）：Bonsai 有完整的终端实现，他自己就用它管理个人通讯录数据库；顺带他的个人网站也在评论区收获了一堆赞美——那站点多年作为第一个 MirageOS unikernel 运行，最近跑在一个零分配的 OxCaml Web 服务器上。Schlagbohrer 问这个库适不适合让本地 agent 用来生成 HTML 报表，antonvs 认为不合适因为训练集里数据太少，但 disconcision 以维护一个使用 Bonsai 的代码库（Hazel）的经验反驳「基于 agent 的开发完全没问题，可能只是多烧点 token」；derdi 还翻出两个月前的一篇 Jane Street 博文，里面提到 Bonsai_term「特别适合 AI 辅助，考虑到它相对冷门，模型写 Bonsai_term 代码的水平之好对我们来说甚至有点神秘」。tikhonj 给了最中肯的建议：如果你主要产出静态报表，Bonsai 相比直接生成 HTML+JS 带来的好处不大；它更适合 UI 里有复杂交互逻辑、很多组件需要控制和展示共享状态的应用。也有多条实际抱怨：README 里 docs 目录缺失导致 Quick Start 和 Thinking in Bonsai 链接 404（adastra22、rixed 都提到），pcan77 吐槽「一个 Web UI 库居然没有任何在线示例页面」，rixed 还想知道 DOM 更新是直接修改变化元素还是走 diff（他自己从源码猜是前者）。rew0rk 问有没有人在生产环境的内部应用里用过，得到的回答清一色是「有啊，Jane Street」；jitl 补了一句实话：「我觉得『我引入了 OCaml』这件事花掉的创新额度，比样式问题大得多。」gosub100 从人才角度看这件事：「这也是一种留人手段。如果你是 OCaml 专家，到竞争对手那里不是即插即用的替代品，所以尽管我很想去那儿工作，离开之后很多知识的市场价值不高。」

**关于 Jane Street 本身**：cachius 半开玩笑地问「Jane Street 除了赞助书呆子 YouTube 频道和写 UI 库之外还做什么」，引出了一连串回答。andor 纠正 morkalork 的「印钞」说法：「政府才印钞，高频交易商是抽取；他们自己会说是给市场提供流动性。」markoman 提到当天《华尔街日报》报道了这家公司的量化实力和数学博士招聘，「最惊人的是他们第二季度利润达到创纪录的 103 亿美元，几乎是规模大得多的高盛和摩根士丹利的两倍」。j_walter 声称这来自对印度股市的操纵，erikig 反驳「在一个高度投机且流动性差的市场里做市，跟操纵市场不是一回事」。tecoholic 问「为什么这个库感觉每个月都被发一次」，jere 猜测：「HN 对任何用冷门编程语言做的东西都着迷。」bmitc 补充：「而且还是 Jane Street。」
