---
layout: article
title: "用 C 写一个现代 Web 应用"
issue: 814
number: 20
category: code
original_url: "https://prophitt.me/writing/a-modern-web-app-in-c"
hn_url: "https://news.ycombinator.com/item?id=50058528"
date: 2026-10-16
---

## 文章摘要

Corey Prophitt 在 10 月 12 日的博客里讲了一个"逆潮流"的故事：1994 年如果你想做动态网站，就写个 C 程序丢进 `cgi-bin` 目录，让 Apache 每来一个请求就 fork 一次。三十二年后，他用同一套技术上线了一个商业产品。

这个产品叫 marketyour.games，从外面看相当"现代"：渐进增强的网页、用户账号、面向内容创作者的搜索面板、用于整理的文件夹、Stripe 支付、Postmark 事务邮件。可内部完全不现代：全部用 C 写成，以 CGI 方式运行。

**地基：BCHS 技术栈。** 也就是 OpenBSD、C、OpenBSD 自带的 httpd 和 SQLite。没有框架、没有模板语言、没有数据库服务器、没有容器。工作流程简单到一句话就能说完：C 程序读取 CGI 环境变量，把响应写到 stdout、日志写到 stderr，需要数据库就直接用 SQLite 的 C API 打开数据库文件，最后编译成静态链接的二进制丢进 `cgi-bin`。由于每个请求都启动新进程，部署新版本就是替换一个文件，不用重启服务、没有停机，新请求自然会用上新二进制。OpenBSD 默认配置稳妥，还提供 `pledge` 和 `unveil` 这类真正能约束 C 程序的工具，httpd 默认在 chroot 里运行，CGI 程序被关在文件系统的一个角落里。

除此之外他只用了三个库：libcurl 负责调用 Stripe 和 Postmark，libsodium 负责密码哈希、Cookie 加密和 HMAC 校验，libcjson 负责解析 JSON。其中只有前两个是真正的构建依赖，libcjson 只有一个 C 文件，SQLite 官方也提供了单文件"合并版"，两者都直接放进项目一起编译。所有静态资源在编译时烤进二进制，干净构建只需几秒，增量构建不到半秒。

**好处。** 作者列了好几条：

- **基础知识胜过框架。** CGI 1.1 规范 2004 年发布后再没变过，整个接口就是请求头、请求方法、查询字符串和请求体这几个 HTTP 基本概念。懂 HTTP 就懂 CGI，学一次管一辈子。
- **职责分明。** 常见做法是 nginx 在前面反向代理到 Ruby、Python、Go 或 Rust 写的应用服务器，作者一直觉得别扭：等于有两个 Web 服务器，其中一个还要负责业务逻辑。CGI 把第二个服务器去掉了。调试也更简单，应用就是一个普通程序，可以在 shell 里给它喂一个请求、看 stderr、用 gdb 单步。他还说 CGI 很像无服务器架构，只是少了供应商（和意外账单）。
- **供应链风险极小。** 在自主智能体泛滥、包仓库每周都被攻击的今天，依赖少让人安心。要攻破他的供应链，得先攻破 libcurl 这种全互联网都盯着的项目，而不是一个周下载量 11 次、维护者 2019 年就跑路的包。
- **工具简单。** 他用不装任何插件的经典 Vim，没有 LSP、没有自动补全、没有格式化工具，也不用 AI。整个应用就是一个目录的 C 文件加一个 Makefile，打开文件读一读就知道它在做什么。真需要工具时，sanitizer、valgrind、gdb、ctags、cscope 都是打磨了几十年的成熟（他笑称"老迈"）工具，Vim 甚至内置了 gdb 图形前端。
- **用 C 写 HTML 没想象中糟。** 他曾无数次和编辑器较劲，让 Vim 高亮 Go 的 html/template、让 Helix 识别 Go 模板又不破坏 HTML 检测、让 RustRover 认出 askama 用的 Jinja 模板。用 C 就没有第二种语言需要编辑器半猜半认，"不好看，但能干活"。
- **SQLite 让人耳目一新。** 他过去默认用 Postgres，但它要看护一个服务进程、跟进频繁的版本升级。SQLite 没有进程、没有配置、没有依赖，运维负担几乎为零。

**坏处。** CGI 只是接口，开箱几乎什么都不给。Cookie 解析和生成、查询参数解析校验、表单数据解析校验、输出转义，都要自己写。他把这些塞进了一个 `cgi.h` 头文件，但不确定这是不是正确的做法；kcgi 之类的库倒是有，可采用它就得接受它的 API 和安全模型。他希望有一套像 stb 那样可以直接拖进项目的共享工具库。

**结论。** CGI 虽老但未被遗忘。当年在 2000 年代初放弃它的大部分理由如今已站不住，现代内核对 fork 做了大量优化，真成了瓶颈还可以平滑升级到 FastCGI。用 C 写现代 Web 应用完全可行，而且有一种现代技术栈给不了的乐趣，他下次还会用这套组合。

## HN 评论精华

本帖 713 分、300 多条评论，是本期最热的帖子之一。讨论大致分成几条线：为什么偏偏选 C（以及 C、C++、Rust 之争），CGI 的性能与架构是否合理，C 程序暴露在公网上的安全性，产品网站本身的可读性，还有大量 CGI 年代的怀旧。

**为什么是 C？** fmo91 说没看懂作者为什么一开始就选 C。作者 prophittcorey 回答，他故意不解释"为什么是 C 而不是某某语言"，因为评论区一定会变成语言战争，结果当然还是打起来了。做 C 编译器的 kanbankaren 说他觉得大家对 C 的痴迷不可理喻：新项目想要 C 系语言就选 C++，只用结构体、不用虚函数就没有运行时代价，还能获得更好的类型安全、引用和命名空间，"讽刺的是，C 编译器本身过去 15 年都是用 C++ 写的"。作者说他喜欢"C 风格的 C++"，但很难划清界限，总忍不住"再多用一个 C++ 特性"。glouwbug 把这种滑坡描述得活灵活现：从命名空间和结构体开始，然后想要 vector、map、模板，接着某个 `fopen` 拿到的资源在链表弹出时没释放，于是开始写移动语义，最后不知不觉用上了大半个现代 C++，"人们痴迷 C，是因为它的功能上限是硬性的"，而 C 自己也在小步改进，比如 C29 的 `defer`。lelanthran 说要看衡量标准：比功能多 C++ 赢，比坑少 C++ 输得一塌糊涂。uecker 说他用过的软件里，C 写的反而最可靠，vim、bash、coreutils、X11、GIMP、Blender，几十年没崩溃过一次。

**AI 时代该选 C 还是 Rust？** Kuyawa 建议让 AI 生成一个类似 Express 的 C Web 框架。nostrademons 反问：既然用 AI，为什么不让它写 Rust？C 的内存安全问题在公网服务器上是致命的，AI 也写不出零内存错误的代码，而借用检查器恰好给 AI 提供了护栏，编译错误还会反馈给 AI 让它知道自己跑偏了。joshheitzman 认为 Rust 的内存安全抵不上它的供应链风险，C 项目的依赖少、通常由操作系统维护者打包或者直接放在项目里方便审计；zanecodes 回应 Rust 一样可以用 `cargo vendor` 把依赖放进项目；Ygg2 则说 C 也有供应链，"那就是没有报酬的 Linux 发行版维护者"。Rust 核心人物 steveklabnik 纠正了"Rust 难以配置 ASan"的说法，一条命令就能连依赖一起开启，而且 Rust 并不绑定 Cargo，他自己的项目早就不用 Cargo 了。modeless 说，如果用 Fil-C 保证内存安全，这算是合理的选择，否则就是不负责任。

**CGI 的性能与架构。** EGreg 指出这正是 FastCGI 存在的原因，fork 一个进程大约要 8 毫秒，并详细介绍了他在 PHP 服务器里先做"每请求一进程"再改成长驻进程的经历，还提到在加载完框架后再 fork 可以借助写时复制共享内存，进程数能多出 10 到 100 倍。rfgplk 认为这是"半吊子方案"，应该把 Web 服务器和应用打包成一个二进制，"能避开 CGI 就避开"。zelcon 提出第三条路：直接写 nginx 模块，在配置里把路由映射到共享库里的 C 函数。Retr0id 不认同"计算机变快了"可以作为使用低效设计的理由；PaulRobinson 反驳说 C 写的 CGI 占用的内存和 CPU 几乎肯定比其他语言少。macleginn 质疑"两个 Web 服务器"的说法，作者解释他想表达的是层次：常见的是 nginx 到 puma/gunicorn 再到你的代码，CGI 去掉了中间那层，不用再猜是中间服务器吞了还是没转发某个头。gwbas1c、pak9rabid 则提醒反向代理负责 TLS 终止、负载均衡、DDoS 防护，这些本就不该让应用开发者操心，并追问数据库备份、扩容和灾难恢复怎么做。

当年写过《CGI Programming Unleashed》的 DannyBee（他说那本书是他 16 岁时写的，tptacek 惊讶地追问，他确认属实）认为文章的结论大体对：CGI 当年主要死于速度不够，如今这个理由基本已不成立，但这不意味着大家应该回到 CGI。pjmlp 和 collinmanderson 都说，我们其实已经回到了 CGI，只是现在叫"无服务器"；p0w3n3d 也说 CGI 本质上就是 Lambda。

**SQLite 并发和字符串处理。** Panzerschrek、creatonez 担心每个请求都打开 SQLite 会导致锁争用。作者说开启 WAL 模式后读完全并发，写仍需排队，还要设置 busy timeout，否则被阻塞的写会立刻超时；citrin_ru 解释了基于 fcntl 文件锁的机制，ihateolives 说"你严重低估了 SQLite 加 NVMe 的速度"，zelcon 则调侃这种网站一个月就一个访客，还是 Googlebot。Scubabear68 说 C 里的一切字符串处理都很痛苦；作者回应他自己写了基于定长缓冲区的 string view 和 string builder，完全不分配内存，而且相比之下用 C 写的 YouTube 爬虫才真叫痛苦。PhilipRoman 一针见血："输出字符串没问题，解析才要命。"SoftTalker 指出作者自己写的 Cookie 解析、参数校验、表单解析、转义，恰恰是开发者最容易写出漏洞的地方。

**安全性。** UltraSane 说把任何 C 代码接上互联网都是坏主意，我们一次又一次学到了这个教训；assimpleaspossi 和 BirAdam 反驳，你的操作系统、数据库、Web 服务器八成都是 C 写的。benlivengood 说他最担心的是数据库多久备份一次，"今天不可能有没有 RCE 的 C Web 服务器"；HackerThemAll 举出 DJB 的 publicfile，27 年没出过 RCE。ncruces 借机推荐了他用 Wasm 沙箱把 SQLite 翻译成 Go 的驱动。cbrozefsky、aragilar 提醒，部署在 OpenBSD 上配合 `pledge`、`unveil` 能大大缩小 C 程序被利用的面。

**网站可读性与"AI 味"。** 置顶评论来自 paularmstrong，说 marketyour.games 除了标题什么字都看不清，"这就是所谓的现代吗"。作者解释他想尝试单色设计，两种灰度的对比度过了 AAA 标准，会尽快改进，并说网站"100% 人工手搓"。noduerme 从美术指导角度狠批网站像"太平间"，建议至少给按钮加个亮粉色悬停效果，甚至加只粉色北极熊吉祥物；LoganDark 请求别再用 Courier New 做等宽字体。afavour、claiir 等人认为博文本身像 AI 写的（作者自称连 AI 都不用），讽刺味十足；skrrtww 则说它只是有 AI 博文那种小标题和排比结构，读起来并不空洞。

**怀旧。** hughw 说 1990 年代他认识的人往 `cgi-bin` 里丢的是 Perl 而不是 C，引来一片回忆：nkozyra、rjrjrjrj、jim_lawless 都说最早确实是 C，jim_lawless 1995 年还在《Dr. Dobb's》上发过 C CGI 教程；也有人用 bash、Pascal、Delphi 甚至 FORTRAN 77 写过 CGI。PaulDavisThe1st 说 1994 年亚马逊就是这么用 C 写 CGI 的，当时整个站点跑在一台 Solaris 工作站上，几乎没有库存，靠一套叫"almost in time"的系统"现在没货但很快能给你弄到"。assimpleaspossi 说他 2004 年起做的餐馆在线点餐系统全是 C、FastCGI、FreeBSD 和 PostgreSQL，后来被连锁店买走改成微软技术栈，从此问题不断。ramses0、rpdillon 分享了 busybox 内置 `cgi-bin` 支持的冷知识，MomsAVoxell 推荐了 TurboLua。
