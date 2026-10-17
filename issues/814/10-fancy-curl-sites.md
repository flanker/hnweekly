---
layout: article
title: "花式 curl 网站"
issue: 814
number: 10
category: favorites
original_url: "https://nytpu.com/gemlog/2026-03-18"
hn_url: "https://news.ycombinator.com/item?id=50093612"
date: 2026-10-16
---

## 文章摘要

博主 nytpu（Alex）在个人博客上写了一篇短文，讲他如何给自己的网站做了一个“curl 版”：在终端里运行 `curl nytpu.com` 或 `curl nytpu.com/gemlog/2026-03-18`，返回的不是一堆 HTML，而是用 ANSI 转义码排版、带颜色和粗体的纯文本页面，专门给终端看。文章写于 2026 年 3 月 18 日，10 月 14 日更新过。他顺便吐槽了 Windows：cmd.exe 默认禁用 ANSI 转义码，PowerShell 又莫名其妙地把 curl 设成了 Invoke-WebRequest 的别名，只能用 curl.exe，“这都不怪我”。

**缘起。** 他很早就加了这个功能，在联邦宇宙（Fediverse）上提过几句，以为不言自明，但陆续有人问怎么做的，于是晚了大约七个月才写下这篇。创意来自 trinkey（curl trinkey.com）和 strongsand（curl proot.party）的 curl 站点，他强调功劳归他们，只是自己的做法很不一样。（更新里补充说，截至 2026 年 10 月 14 日，这两个站点的 curl 版都已失效。）

**第一步：生成带 ANSI 标记的文件。** 首页和关于页完全是手写的。他猜 trinkey 和 strongsand 是用 ratatui 之类的库实时生成，可能更方便，但他觉得手写更省事：在 Vim 寄存器里存好最常用的转义序列开头 `ESC[`（即 `\x1b[`），随时粘贴，再开着维基百科上图形控制码（SGR）的页面查具体代码，就这么简单。他反复试验确定了一套统一的样式，并参照自己网站的 Gopher 版来编写 curl 版页面。

**全站 curl 化的“巧合”。** 他的特别之处在于整个博客都有 curl 版，而不只是一两个页面。这完全得益于一个方便的巧合：四年多前，他就在自己写博客用的自制标记语言 Gemroff 里实现了输出带 ANSI 标记纯文本的功能，最初只是为了写作时能在终端里方便地预览草稿，后来接进自制的静态站点生成器，立刻就有了全站的 curl 版本。他只需手写一个博文包装模板和一个文章列表模板，再在生成器里加几行代码。他坦言这正是不好说“这很简单”的原因：对他来说简单，是因为他用的是完全自制的标记语言和生成器，而且多年前恰好做对了设计选择。例如他的网站同时托管在 HTTP、Gemini 和 Gopher 三种协议上，所以生成器本来就支持多种模块化输出格式，而不像其他生成器那样只输出 HTML。不过他也说，只手写几个页面来玩玩完全可以，这本来就是个小噱头，没人会真的用它浏览超过几分钟。

**第二步：识别 curl 并返回对应版本。** 最后一步是对 User-Agent 中包含 curl 的请求返回 curl 版，他只改了几行 Nginx 配置，其他服务器想必同样简单。他想要的主要特性是一个覆盖用的请求头 `X-Original: true`，这样随手用 curl 抓原始 HTML 的人依然能拿到未经修改的响应；同时所有内容仍然是静态文件。

**结尾。** 他说这种做法和人们平常使用 Web 服务器或 ANSI 转义码的方式差得很远，所以能理解大家的好奇和困惑，希望看到更多网站玩这个把戏。文末还有一段自嘲式脚注，回应“用了破折号就是 AI 写的”这种指控：他的标记语言从 2022 年 11 月起就有专门的破折号元素；大模型用破折号，恰恰是因为它们学的是人类作者的文字；何况他一段话里有二十个括号插入语，总得换换花样吧。

## HN 评论精华

本帖 145 分、约 30 条评论，讨论轻松友好，主要是大家晒自己的 curl 站点，以及关于“怎么做才更规范”的技术建议。

**用 Accept 头代替自定义头。** jayknight 认为用 `Accept: text/html` 做内容协商更符合标准，curl 版则可以用 `text/x-ansi` 这个 MIME 类型，这样 wget 等其他文本客户端也能请求到它；lightandlight 赞同，希望 curl/wget 默认拿到规范版本。ximm 回应说什么是“规范版本”由作者决定，他的博客默认输出 Markdown，只有明确请求时才返回 HTML。basilikum 惊讶 `text/x-ansi` 竟然真的存在，bingo-bongo 解释 `x-` 前缀表示实验性类型，虽然有 RFC 不鼓励这种命名，但实际仍广泛使用。tinthedev 的做法则是 curl 默认仍返回 HTML，但在页面里用 `<link rel="alternate" type="text/markdown">` 提示可以用 `Accept: text/markdown` 拿到 Markdown 版本，兼顾屏幕阅读器和 LLM 代理。

**纯静态的妙招。** colewilson 分享了一个不需要检查 User-Agent 的技巧：把真正的 HTML 包在终端的“备用屏幕缓冲区”里（开头用 HTML 注释包住进入备用缓冲区的转义序列，结尾再切回来），curl 时 HTML 仍会输出到终端，但会立刻被清掉，随后在真正的屏幕上显示附加在末尾的 ANSI 版本；浏览器里这些转义码都藏在 HTML 注释中。这对托管在 GitHub Pages 之类的纯静态网站很有用。G_o_D 联想到邮件可以同时带纯文本和 HTML 两种正文，gfody 说邮件客户端支持 multipart，浏览器从没觉得有必要，bandie91 则提醒 Accept 头本来就是用来为同一资源选择表现格式的。

**大家的 curl 站点。** classictraffic 做了 `curl listening.cailinpitt.com` 显示最近在听的音乐、`curl reading.cailinpitt.com` 显示最近在读的书和文章；jtbaker 推荐经典的天气服务 `curl wttr.in`，abound 仿照它做了输出更简洁的 wthr.dev（跑在自家地下室，限流非常严格）；jesse_dot_id 做了以纯文本返回 IP 信息的 addr.fyi；treykeown 的多语种网站还带 `curl | sh` 模式。reaperducer 感叹“Finger 协议还活着”。zanderwohl 正在搭建跑在家庭服务器上的家务、日历和气象站面板，考虑做纯文本版好让很笨的设备也能显示。

**安全与其他。** ____tom____ 担心：如果网站能给 curl 返回 ANSI 控制码，那用 curl 查看可疑网站是否反而有风险？历史上确实出过与 ANSI 控制码相关的漏洞。IncRnd 顺带提到 curl/libcurl 最近刚发布了一批修复，主要漏洞正是利用恶意服务器攻击客户端。Rohansi 补充说，在 Windows Terminal 里（非 PowerShell 模式）这个 curl 站点其实可以正常显示。djriley 开玩笑说终于可以像 RMS 一样浏览网页了，pizzaiolo 澄清 RMS 其实用的是 GNU IceCat 浏览器加 LibreJS 扩展。bitbasher 调侃作者引以为豪的“专门的破折号元素”，不就是 HTML 里的 `&mdash;` 吗。
