---
layout: article
title: "Example.com 迎来几十年来最大的一次改版"
issue: 813
number: 32
category: design
original_url: "https://www.debugbear.com/blog/example-dot-com-redesign-history"
hn_url: "https://news.ycombinator.com/item?id=49971921"
date: 2026-10-09
---

## 文章摘要

这篇文章出自网页性能监控公司 DebugBear 的客服工程师 Conor McCarthy，标题带点调侃：example.com 这个 IANA 专门保留给文档示例使用的域名，在 2026 年 9 月 28 日迎来了「几十年来最大的一次改版」。文章一半在拆解这次改版的技术细节，一半在借助 Wayback Machine 回顾 example.com 二十多年来的演变。

**这次改了什么。** 此前 example.com 一直是一个纯静态的英文页面。新版用 JavaScript 加入了多语言支持：页面每 5 秒轮换一次语言，依次用英语、阿拉伯语、中文、法语、俄语和西班牙语（恰好是联合国六种官方语言）说明这个域名的用途，并附带一个 SVG 书本图标。新文案的意思是：本域名可在文档示例中自由使用，无需许可；它不是一项服务，请不要拿它做测试或监控。语言切换时还有一个逐字渐隐渐现的动画——作者打开 DevTools 发现，每个字符都被包进单独的 `<span>`，每个 span 的 `transition-delay` 比前一个稍长，于是形成了逐字过渡的效果。不过到 10 月 3 日，动画就被撤掉了，改为一次性列出所有语言。

**IANA 的解释。** 文章引用了 IANA 的说明：改动通常出于两个目的，要么降低带宽开销，要么提高实用性。这个站点流量很大，而其中大部分是自动化请求，这些请求一般不会去拉取 JS 文件。所以这次把页面拆成一个极简的基础 HTML，再用单独的 JS 文件补充其余内容，就能减少总流量。IANA 还强调，域名的根本用途是写进文档里，很多流量来自有人照抄示例配置却忘了改域名；它并不打算充当可用性检测的端点，在上面跑 HTTP 服务只是出于礼貌。

**二十多年的演变。** 作者按时间梳理了几个节点：2002 年最早的快照只是一张 ICANN/IANA 保留域名列表，用表格布局，左上角放 ICANN 标志，服务器是 Apache 1.3.22；同年 3 月出现了大家熟悉的那句「你通过输入 example.com……来到这里」；2003 年加入 RFC 2606 链接；2010 年把 example.edu 也指了过来；2011 年起一度直接跳转到 IANA 官网；2013 年取消跳转，改由 EdgeCast CDN 提供服务，并换成圆角卡片加「Example Domain」大标题的经典设计；2019 年调整了字体栈和措辞；2025 年 1 月 Server 响应头消失（背景是 EdgeCast 几经转手、母公司 Edgio 破产）；2025 年 10 月去掉卡片、压缩 HTML；2025 年 12 月迁到 Cloudflare；2026 年 6 月加了一行 `<link rel="icon" href="data:,">`，用空 favicon 省掉浏览器对 `/favicon.ico` 的无谓请求。

文章最后提醒开发者，不要拿 example.com 当网络连通性检测目标——DebugBear 自己的可用性监控数据就显示它偶尔会响应异常，用自己掌控的基础设施更可靠。

## HN 评论精华

这条帖子有 350 多分、约 250 条评论。讨论很快从「改版好不好看」转向更实际的问题：大量人其实一直把 example.com 当成「服务」在用；Hyrum 定律；以及该用什么来做联网检测。评论区里还有一位 **kijeda**，从发言看是参与维护该站点的 IANA 方人员（他说「我们用过多家 CDN」），多次出面解释。

**多语言与动画**

- **sajithdilshan** 建议按访客 IP 所在地区显示本地语言，**Cidan**、**zahrevsky** 回应说重点就在于保持静态、低开销；**justinpombrio** 则指出「一个地区只有一种语言、人人都说这种语言」这两个前提都不成立，按 IP 猜语言在实践中效果很差。**lxgr**、**ffaccount2** 借机吐槽 Google 无视浏览器语言设置、总按 IP 给本地语言。
- **kijeda** 说明：轮播版上线一周后就改了，因为有人合理地指出这样对无障碍不友好；现在所有译文一起显示，但会根据浏览器语言偏好把你的语言排到最前面。**ChrisRR** 表示当初就觉得没读完一段就切走，很难受。

**为什么要用 JS？SVG 有必要吗？**

- **Gualdrapo** 质疑用 JS 插入一个静态 SVG 的意义；**afavour** 说关了 JS 也能看到 SVG、认为文章像 AI 写的，但 **ajcp** 贴出 `s.js` 源码证明确实是 JS 注入的，**zamadatix** 关掉 JS 后也只看到灰底英文页面。
- **worg**、**TheCoelacanth** 解释：自动化客户端不执行 JS，把内容移到外部脚本里就能省下大量字节。**creatonez** 给出具体数字：新版全部内容 2540 字节，brotli 压缩后 1713 字节；但只请求 HTML 的话仅 334 字节，对于绝大多数爬虫和健康检查来说，体积反而只有旧版的一半左右。
- **kjellsbells** 对此半信半疑：一个把每个字母都包进 `<span>` 的网站，真的是在省流量？他还想到微软文档用了 25 年的 contoso.com，日志一定很精彩。

**大家其实都把它当服务用**

- **chews**、**ButlerianJihad**、**gblargg**、**wildzzz**、**lxgr** 都说自己用 example.com 来触发酒店、机场、航班 Wi-Fi 的登录门户，因为它支持纯 HTTP、没有 HSTS。有人推荐 neverssl.com，但 **Arainach** 说 NeverSSL 后来也加了 SSL，**DaSHacka** 吐槽「名字里就写着 never，就这一个任务都做不好」，并推荐 http.rip。
- **simoncion** 态度强硬：按 RFC 2119 的说法，系统「绝不能」依赖 example.com 上的 HTTP 服务，对一个出于礼貌提供的小服务猛刷请求很没礼貌，要测就去刷 Cloudflare 状态页之类。**hayleox** 列出了各大厂商专门用于联网检测的地址（Google 的 generate_204、Apple 的 captive.apple.com、微软的 connecttest.txt、Firefox 的 detectportal），认为这些才适合写进测试。**Ajedi32** 提到 RFC 8910 已经规范了门户发现。
- **reincoder**（IPinfo）认为不管维护者怎么想，一些网站实际上已成为互联网的「基石服务」；**kijeda** 同意它已经自然而然地变成了一种服务，但仍然有必要讲清楚它的用途，否则就等于默认签下一份无限期的隐性契约。

**Hyrum 定律与被搞坏的测试**

- **selcuka** 好奇这次改版弄坏了多少自动化测试，**teraflop**、**hanibrel** 半开玩笑地建议干脆定期改版甚至随机化内容，让大家吃一堑长一智。**pimterry** 推出了自己的 testserver.host，原样复刻了经典版页面，开源可自托管。
- **madeofpalk**、**bonesss** 分享了「客户依赖未公开接口」的亲身经历；**datsci_est_2015** 总结：API 的边界是暴露出来的一切，而不只是写进文档的部分。**smashed** 说 Claude Code 里的 Opus 5.5 前一天还给他的项目写了一个用 example.com 检测联网的单元测试，被他在审查时拦了下来，但「成千上万的人不会发现」。
- **xtajv** 的总结很直接：如果这次改版弄坏了你的测试，说明它们本来就是坏的。

**其他**

- 关于 `ping 1.1.1.1` 测网速是否合理，**ButlerianJihad**、**zettabomb**、**brookst** 等人展开了一长串争论；**JdeBP** 回顾了 1.1.1.1 在 Cloudflare 接手前长期承受大量垃圾流量的历史。
- **VVilhelmsen** 开玩笑说自己每周用 example.org 的白底页面擦眼镜，现在变成深色了，十分绝望；**ErroneousBosh** 等人表示学到了空 favicon 的小技巧。
- **frogulis** 指出文中所谓「IANA 声明」其实来自此前 HN 上讨论过的一封 IANA 邮件，只是删掉了开头的客套话。
