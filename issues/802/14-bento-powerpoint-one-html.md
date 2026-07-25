---
layout: article
title: "Bento：把一整套 PowerPoint 塞进单个 HTML 文件（编辑+演示+数据+协作）"
issue: 802
number: 14
category: show_hn
original_url: "https://bento.page/slides/"
hn_url: "https://news.ycombinator.com/item?id=49008211"
date: 2026-07-24
---

## 文章摘要

Bento 是本期 Show HN 的爆款（1000+ 分、235 条评论），作者 starfallg 把一整套幻灯片工具——编辑、演示、打印、导出、图表、动画、实时协作——全部压进了**一个 HTML 文件**里。

起因很具体：作者团队最近几个月越来越多地用 Web 前端技术配合 Claude Code 之类的编码工具做演示文稿，效果很漂亮，但有个共同痛点——哪怕改一个小地方，也得去手动改代码或者再跑一遍 agent。为了跳出这个循环，他做了 Bento：不需要安装、不需要云端登录，完全离线可用；默认演示文稿大约 560 KB，拿到手后运行时不再抓取任何外部资源。用浏览器打开就能编辑、演示、打印、保存；通过邮件或 AirDrop 发给别人，对方只要有浏览器就能编辑、演示甚至实时协作。你还可以把 pptx 丢给 Claude 或 ChatGPT，让它转成 Bento 幻灯片。全部代码 MIT 许可，仓库在 `nyblnet/bento`。

作者在评论区补充了实现细节，这部分比 README 更有料。文件大致分两段：靠前的位置是一整块**明文 JSON**，即幻灯片数据，可以直接读、grep，或者把 agent 指过去；应用本体则是一个 base64 blob，通过一个小 shim 加载，在浏览器里用 `DecompressionStream` 解压——这样既压小了体积，又保证运行时零外部请求。文件回写用的是 File System Access API，把 JSON 写回同一个文件，不支持时降级为普通下载，所有更新都经过 ECDSA 签名。技术栈以 reveal.js 为底，动画用 GSAP/Flip，图表最初用 echarts，但因为体积和行为问题最后两者都自己重新实现了一遍。

作者最得意的是 CRDT 协作有多无缝：所谓「盲中继」（blind relay）是一个跑在 Cloudflare Durable Objects 上的小文件，它看到的只有客户端发来的加密数据。协作默认关闭，需要在应用里通过分享邀请文件启动会话，此时生成一组密钥；权限按用户密钥管理，可以设只读用户，也可以按用户吊销实时协作权限。文件还内置了自动更新——检查签名清单、下载、重写文件结构、以新文件重载。

## HN 评论精华

- **starfallg（作者）**：见上文技术自述。他还透露这是自己潜水阅读、评论 HN 七年后的第一次投稿。

- **inanutshellus**：指出 reveal.js 里最「魔法」的特性是**纵向幻灯片**——做一套高度抽象的轻量主线，看观众是谁再决定要不要按下方向键钻进技术细节：没有技术宅就别在第 3 页往下按；对着市场部的人就在第 5 页往下钻。他提醒作者为了模仿 PowerPoint 把这个砍掉了，但别忘了它的存在。作者回应说自己走的是「状态幻灯片」路线来处理类似场景，会考虑把这个更优雅的实现方式并进来。

- **georgeburdell**：认为这填补了企业世界一个极其常见的空白——他公司里有几个团队从现成的云端或桌面演示/可视化软件转向自己写的 HTML/JS 方案，就因为受不了 bug 不修、定制需求不做。他还留下一句金句：「到处的内部 IT 封建领主们都该害怕了。」作者回应说这正是他们公司的情形：技术人做出惊艳的 Web 演示，但某个只有 Claude 而没有 Claude Code 的同事（公司认定那是给工程师用的）在下午的重要委员会前发现数据有误，就完全束手无策。

- **calebm**：一直在推广「单文件 Web 应用」（Single-File Web Apps）这一概念，邀请作者把 Bento 加进他提的维基百科草稿页。但 **yreg** 反对：草稿读起来不像维基百科条目（比如用「你」称呼读者），而且维基是三级来源，不该用来推广自己想要被确立的概念。**Chilko** 进一步指出草稿里有 4 条引用指向作者自己的博客、YouTube 或项目，难怪被拒。**nashashmi** 建议改用 GitHub 列表，calebm 表示已经建好（`calebmadrigal/Single-File-Web-Apps`）。

- **aag**：一个重要的事实核查——主页写着「Nothing phones home（不回传任何数据）」，但他检查过的 Bento 文件里包含 cloudflareinsights.com 的信标。作者先猜是 Cloudflare Durable Objects SDK 带的，承诺如果关不掉就换掉 DO，随后又连续更新说源码里查不到、Chrome 检查也没有对应连接，怀疑是流量激增触发的 Cloudflare 挑战或站点前置代理导致，会继续追查。**Willamin** 补了一句黑色幽默：好在用 Bento 这种工具，你可以本地把底部那个 script 标签删掉，从此一了百了。

- **pembrook**：质疑「所谓 ambitious and impressive，是指对 Claude 来说吗？这明显是重度 vibe coded 的产物。」**jasonkester** 的反驳成为该子线最受关注的观点：对他来说「由 LLM 生成」反而让这个项目更让人印象深刻，因为这是他见过的第一个真正用 AI 造出来的现实产品——「六年来我一直在读这类工具带来惊人生产力提升的宣传，但预期中的酷炫新产品洪流从未到来，只有一堆工具、harness 和 swarm 协调层，帮公司在炒作周期里花更多钱。终于有人真的做出一个应用并发布了。我认为这是一个里程碑。」**aag** 本人则回了句「我不在乎。好就是好。」

- **notpushkin**：玩 guestbook 玩到 M1 Mac 冻死、不得不硬重启，但「太有意思了」。他提了两条：别人改动无关内容时不该重置自己的焦点；落地页和示例文稿的文案「LLM 味太重」，反倒是 HN 投稿正文读起来更舒服。作者坦承文案确实不行，「但我不擅长写作，而且在正职和当爸爸之间没时间，会慢慢改」。

- **jimmar**：加图片时发现没有 alt text 选项，判断无障碍不是优先项，「除非通过无障碍检查算是一个特性，否则我用不了」。作者承认这确实是有效意见，下个版本至少会加上基础的无障碍能力。

- **maxloh**：从架构上提出异议——HTML 和 CSS 本质上比 JSON 更适合幻灯片，绝大多数页面归根结底就是简单的 flexbox 结构，这类布局天然该用 CSS 和布局树表达，而不是硬编码坐标。作者回应：对于必须固定页面尺寸的静态版式（像 PowerPoint 那样），具体定位更好，否则你会一直和布局引擎打架，这只是权衡取舍。

- **TeeWEE**：实际用后的对比——Bento 用预定义 schema，所以没法引入任意 HTML 库（比如 Mermaid.js），reveal.js 在原始能力上更强；但他喜欢 Bento 的单文件设计和可手工编辑，这是 reveal.js 做不到的。

- **PhilippGille**：注意到作者一周前才为这个项目新建了 GitHub 账号，询问有没有其他账号可以看到维护和安全方面的历史记录。**ericyd** 善意反问：拿到这个信息你会怎么做？你有一套在用别人项目前系统审计 GitHub 账号的流程吗？

- **arttaboi**：提出一个天真但要紧的安全顾虑——这种形态是不是让恶意软件传播变得非常容易？有人可以往压缩的应用体里塞恶意代码，然后把幻灯片分享出去自我扩散。

- **luckydata**：「这个项目是 Google Workspace 团队在支持 agent 编辑上巨大失败的纪念碑。」作为 Google Cloud 前员工，他说自己对 Google 在生成式 AI 上处处掉链子毫不惊讶，只是很难过。

- 相关项目串烧：**bryanhogan** 列了 Marp、Slidev、reveal.js、Animotion；**d4rkp4ttern** 推荐 slidev 和 typst（并吐槽标题里用「PowerPoint」当「演示文稿」的同义词有点讽刺，毕竟很多人讨厌 PowerPoint）；**hdrz** 提到 impress.js；**llagerlof** 和 **mbreese** 都想起了 TiddlyWiki 这个单文件 wiki；**littlecranky67** 说自己 2013 年发布的 mdwiki 大概是最早的「真正单页应用」。
