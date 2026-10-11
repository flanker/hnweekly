---
layout: article
title: "Bigwords.page：把任何屏幕变成一块告示牌，网址本身就是应用"
issue: 813
number: 13
category: show_hn
original_url: "https://bigwords.page/"
hn_url: "https://news.ycombinator.com/item?id=49994443"
date: 2026-10-09
---

## 文章摘要

BIGWORDS.PAGE 是开发者 Brad（HN 账号 SpeakingOfBrad）做的一个小工具：打开一个链接，屏幕上就以尽可能大的字号显示一段文字，可以当告示牌、提示牌、倒计时器用。这条 Show HN 拿到 700 多分、160 多条评论，是本周最受欢迎的 Show HN 之一。

**起因。** 作者在 HN 自述里说，他想在一台平板上显示一条消息，这台平板他有远程管理权限，但人不在旁边。把浏览器设成 kiosk（全屏信息亭）模式很容易，问题是怎样让浏览器显示他想说的话。他的办法是：让 URL 本身承载消息内容。

**原理。** 所有信息都放在 URL 的片段（fragment，即 `#` 之后的部分）里。浏览器不会把片段发给服务器，所以作者永远看不到用户写了什么。整个站点是一个静态页面，没有后端、没有数据库、不需要账号，一个页面就能渲染所有可能的告示。改变片段时页面无需刷新就会更新。`#` 后面第一个未编码的 `&` 之前是消息正文，之后是一串 `key=value` 参数。例如作者给 HN 的演示链接 `https://bigwords.page/#Hello,%20HN!&bg=ff6600&fg=000000&font=4`，就是在 HN 橙色背景上用黑色大字写出「Hello, HN!」。

**功能。** 首页和文档列出的能力比「显示几个大字」要多：
- **自动适配字号**：文字会自动放大到填满屏幕，从手机到体育馆大屏都能用。
- **Markdown 子集**：支持粗体、斜体、删除线、行内代码、一级和二级标题、换行（`%0A`）。列表、链接、图片等不支持，会按原样显示。
- **幻灯片**：用 `||` 分隔多段内容，按 `interval` 参数设定的秒数轮播，有 slide、fade、zoom 等过渡效果。
- **倒计时与计时器**：在正文里写 `{countdown}`，配合 `until=`（指定日期时间）或 `timer=`（如 30s、1h30m）使用，`zero=` 可以设定归零后显示的内容，比如「Pencils down!」。
- **动画**：pulse、flash、shake、bounce、滚动字幕、打字机、彩虹等。
- **二维码和图片**：二维码在浏览器本地生成，不依赖第三方服务，支持网址、Wi-Fi 连接、电话、短信、邮件和地理位置；也可以加背景图。
- **其他参数**：前景色和背景色、7 种字体、字号上下限、内边距、固定宽高比、定时刷新，以及默认开启的屏幕常亮（wake）。参数写错时会静默回退到默认值，查看页永远不会报错。

首页给了一组可以直接打开的用例：接机牌「WELCOME HOME, GRANDMA!」、课堂测验计时、咖啡馆 Wi-Fi 密码加二维码、新年倒计时、「ON AIR」录音提示、登机口指示牌、新闻滚动条、考场剩余时间、当日议程轮播、扫码看菜单等。另有一个带实时预览的编辑器（`/editor`），它和查看页使用同一套片段格式。

作者自己也承认用途比较小众，但他觉得这可能是那种平时用不上、偶尔需要时会让事情方便一点的工具。项目以 MIT 协议开源，可以自行托管。HN 讨论期间，作者根据评论修了 Firefox 下的排版 bug，删掉了文档里多余的说明，还加上了 PWA 和离线支持。

## HN 评论精华

需要先说明：评论区很大一部分篇幅没有讨论工具本身，而是在争论标题和文案是不是 AI 写的。这条支线排在最前面，也最长。工具本身的评价则几乎一边倒地正面。

**「The URL is the app」是不是 LLM 腔。** **notpushkin** 的置顶评论直言，标题里的「The URL is the app」是典型的 LLM 说法，令人反感，希望作者用自己的话写文案。他还给了一个替代标题：「把任何屏幕变成告示牌，由 URL 参数控制」。**dylanhouli** 问这句话哪里像 LLM。**rpdillon** 的解释是：这句话简短好记，但说得不对。应用是服务器上的那个页面，URL 只装着告示内容。他认为这个项目基本上是 antonmedv/textarea 换了个形态。**bryanrasmussen** 和 **onion2k** 认为这就是营销话术，LLM 只是从无数营销文案里学来了这种写法。**rdiddly**、**ape4** 也提醒，人类早就这么写，比如 Sun 的口号「The Network is the Computer」。**slacktivism123** 翻出仓库首次提交里的文档，列举了「The URL is the whole display: no accounts, no login, no backend」这类句子，认为明显是 Claude 的文风。他的长评论一度被 HN 新上线的低质内容过滤器自动隐藏，后来又恢复了。**efilife** 也从首页找出了「No app. No login. Just a URL.」这类排比。**camillomiller** 说网页设计一看就是 Claude 的风格，但他觉得没问题：点子好、做得也好，作者显然懂得怎么组织一个 Web 应用，这是 AI 编程的正当用法。

反方同样不少。**2III7** 说 HN 上的「LLM 幻视」已经泛滥到令人厌烦。**chrisweekly** 认为这句话简洁准确，为「可能是 AI 写的」而咬牙切齿太过头了。**okaleniuk** 引用 Knuth 的话：会编程的人和会写作的人都是少数，不能要求人人两样都会；不是每个人的母语都是英语，如果 LLM 能帮程序员被听见，那是好事。**notpushkin**、**bmacho**、**ShinyLeftPad** 则回应说，LLM 会把每个人的声音抹平成同一种公司腔，反而让项目淹没在大量长得一样的作品里，他们宁可读蹩脚的英语。作者本人只回了一句：「你说得对，但标题只有 80 个字符。」这条支线中间还跑偏到 en dash 和 em dash 的区别（**xoxxala** 解释两者分别是字母 n 和 M 的宽度）。

**同类项目一大串。** 很多人说自己做过类似的东西：**abejfehr** 的 bigtext、**xd1936** 的 parameter.page、**Zarel** 的 bigtext.k55.io、**okaleniuk** 用于教学的「URL 即幻灯片」页面。还有人把整个状态塞进 URL 做别的事：**arpakuutio** 的 linkify.ink 用 zstd 压缩后把文件和文章编码进链接；**below43** 的 urlfile.app 用 URL 分享文件；**samschooler** 的 keep.recipes 让 AI 代理生成食谱链接；**purple-leafy** 把一整局游戏的状态塞进 URL 或二维码，可以无服务器精确回放；**Wittie** 在手游的异步对决模式里用了同一招。**paulsmith** 想起了老网站 bigassmessage.com，**JoshTriplett** 一直用 screenmessage.com，**trvz** 指出 iOS 上早就有同名应用 Big Words。**IshKebab** 泼冷水：这个点子非常老，2009 年就有流行的同类应用。

**真实用途。** **zie** 是听障人士，经常需要把字写大给人看，他手机上有一个这类应用，但还是打算自托管这个网页版，以免给作者的服务器添负担。作者回复说托管版走 Cloudflare，静态站点缓存效果很好，尽管放心用。**saaaaaam** 经常主持论坛和圆桌，会场往往没有倒计时器，去年他只好用 ffmpeg 临时生成一个。现在只要在桌上放一台 iPad，让台上的人看到倒计时、观众看不到，就能更好地控制节奏。**PBnFlash** 以前组织跨时区的游戏之夜时，用过带 Unix 时间戳和倒计时的链接。**chrisra** 用类似方法在手机上提醒别的车辆有故障。**kinduff** 在酒吧人多时用它点酒。**EricBetts** 认为这很适合写进 NFC 标签。**ceejayoz** 开玩笑说想接上语音控制，把 iPad 放在车后窗对别的司机「说点什么」，**trallnag** 说他想过用 LED 做同样的事，但在德国大概不合法。

**数字标牌与电视。** **KingMachiavelli** 认为数字标牌行业完全可以用一个 Chromium kiosk 加一个自动刷新的网页来替代。**w0rd-driven** 回忆 2014 年给 kiosk 做应用时，要用 SiteKiosk 锁住 IE，还得配昂贵的定制机箱，而现在一块树莓派就绰绰有余。**simlevesque** 建议做成 Google TV 或 Apple TV 应用。作者回复说，它更适合搭配电视上的浏览器使用，但 Apple TV 上一直找不到好用的浏览器。**wiradikusuma** 补充说 Apple TV 不支持 WebView，并顺带介绍了自己给咖啡馆做的 signagesync.app。**michaelsalim** 推荐了自己开源的 TheOpenPresenter。

**技术建议与作者的修复。** **pvillano** 发现，Firefox 中 `scrollWidth` 会把延伸到边距里的空格也算进去，导致文字明明放得下却被判定溢出，他自己的 screaming.saej.in 遇到过同样的 bug，并附上了修复方法。作者感谢说自己只在一个浏览器里测过，借此又修了几个 Firefox 下才暴露的问题。**Leftium** 建议加 PWA 清单，作者起初觉得用处不大，看了反馈后改了主意并加上了 PWA 支持，还提到这是在手机上全屏显示的唯一办法。**arikwald** 问能否离线使用，作者随后加了离线支持。**jluxenberg** 指出 data URL 也能做到同样的事，建议增加「复制为 data URL」的选项。**wateralien** 希望能用 base64 编码正文，免得链接文字提前泄露笑点；**hunter2_** 补充说要注意 URL 安全的 base64 变体。**arshxyz** 提到 Wake Lock API 正适合这种场景（文档显示该站默认已开启屏幕常亮）。**frankthecobbler** 嫌在 iMessage 里分享时的预览图太大，作者解释那是 Open Graph 图片，服务器看不到消息内容，所以没法生成对应的预览，只能把默认图做得更低调。**hidelooktropic** 指出文档里不必教人「%20 是空格」，浏览器会自动编码，作者随即删掉了这句。

**一句话评价。** **T0mSIlver** 说，URL 片段基本上就是这个静态 Web 应用的 argv。**pimlottc** 说 URL 是状态，但这里显然还是有个应用的。**1-6** 认为它体现了「只做一件事并做好」。**Retr0id** 则调侃：「URL 就是应用」？我们过去管这个叫网站。
