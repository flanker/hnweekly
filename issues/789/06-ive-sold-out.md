---
layout: article
title: "我'卖身'了"
issue: 789
number: 6
category: favorites
original_url: "https://mariozechner.at/posts/2026-04-08-ive-sold-out/"
hn_url: "https://news.ycombinator.com/item?id=47687533"
date: 2026-04-10
---

## 文章摘要

Mario Zechner，pi 编程代理的创建者，在这篇坦诚的博文中宣布他已加入 Earendil 公司，并将 pi 项目带入了这个新家。pi 是一个极简主义的编程代理框架，仅使用 1500 token 的系统提示和 read、edit、bash 三个工具，却展现出了令人惊讶的能力。pi 为 OpenClaw（由 Peter Steinberger 开发的热门编程工具）提供底层技术支持。

文章首先回顾了 Mario 的开源历程以解释他做出这个决定的背景。他从 2009 年开始做开源，第一个成功项目是 libGDX——一个跨平台游戏开发框架，曾是 Android 上最流行的游戏框架，用户包括 Niantic（Ingress）和 Slay the Spire 的开发者。这个项目运作良好，Mario 在 2016 年将管理权交给了核心贡献者团队。

然而，他在 RoboVM（一个 JVM 代码的 iOS 预编译器）的经历则截然不同。Mario 团队将 RoboVM 卖给了 Xamarin，Xamarin 随后将开源核心闭源，接着 Xamarin 被卖给了 Microsoft，Microsoft 立即关闭了 RoboVM。作为负责社区管理的"开源人"，Mario 不得不写那篇"对不起，不再开源了"的博文，尽管他作为非控股股东对此毫无控制权。但最终重要的是社区的力量——libGDX 的贡献者们在几天内就 fork 了旧仓库并让一切重新运转，这个 fork 叫 MobiVM，至今仍在为 libGDX 的 iOS 支持提供动力。

当 Peter Steinberger 基于 pi 构建 OpenClaw 并爆红后，Mario 突然成为了热门人物。过去两个月他每天接 3-5 个电话，收到了各种 VC 投资意向书和梦想工作邀请。但他明确知道自己不想要什么：他不想自己创建一家 VC 资助的创业公司。他有一个四岁的孩子，"爸爸不在"让孩子哭了很多次，他再也不想经历这种情况。

Mario 与 Armin Ronacher（Flask 的创建者、Sentry 的联合创始人）和 Colin 的关系可以追溯到多年前。他们通过 r/austria subreddit 相识，在技术和政治观点上既有分歧又有共鸣。三人在 2025 年 4 月一起在 Peter 的维也纳公寓里构建了第一个协作项目 VibeTunnel，此后围绕"维也纳自主编程学派"的理念紧密合作。

选择 Earendil 的原因包括：Armin 在开源及其商业化方面有成熟的经验和正确的理念；Colin 懂产品和创业管理；团队成员大多有孩子，公司对此很体贴；早期投资者不在他的"黑名单"上；公司的价值观宪章强调软件应服务于人类而非取代人类。

对于 pi 的未来，Mario 做出了明确承诺：pi 的 MIT 许可证永远不变，这是不可协商的。在 MIT 核心之上，将有 Fair Source（公平源码）层的增值功能和专有企业功能，采用延迟开源发布（DOSP）策略。GitHub 仓库将从 badlogic/pi-mono 迁移到 earendil-works/pi，但 Discord 社区保持不变。外部贡献的方式也完全不变——无需 CLA 或 DCO。如果社区觉得他们偏离了方向，GitHub 上的 fork 按钮永远有效。

## HN 评论精华

1. **提供关键背景信息**：一位评论者详细解释了文章的背景——Mario Zechner 是 pi 编程代理的创建者，pi 为 OpenClaw 提供底层技术。pi 本身是一个极简编程代理，仅有 1500 token 的系统提示。他还提供了自己的解读："我'卖身'了"是非常奥地利式的表达，实际上含义恰恰相反，并引用了 Mario 关于 RoboVM 经历的段落来说明他为什么这次做了大量尽职调查。

2. **对信任和信誉的高度评价**：一位评论者表示，在这个行业中能让他完全信任的人不超过两只手能数得过来的，而 Mario 和 Armin 是其中两位。在当今网络时代建立信任异常困难，但"维也纳帮"有一种让人信服的特质。虽然这个消息对一些人来说可能令人失望，但听起来这对他们个人而言是正确的选择。

3. **对文章可读性的吐槽**：一位评论者承认读完大部分文章、访问了链接的项目后仍然不太明白这到底是关于什么的。这反映了 Mario 的写作风格——需要先了解其背景和 pi 生态系统才能充分理解。

4. **关于 Armin 和 Flask 的补充**：一位评论者为不了解背景的读者补充说明 Armin 是 Flask 等众多项目的创建者，并推荐了 pi 作为编程代理的体验，认为它即使与 Claude Code、Codex 等商业产品相比也值得尝试。

5. **对 LOTR 命名惯例的吐槽**：一条简短但获得大量点赞的评论问道："为什么所有东西都要用该死的指环王来命名？"——这引发了关于科技圈命名文化的轻松讨论。
