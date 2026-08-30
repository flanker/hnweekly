---
layout: article
title: "Show HN：Kobo 现在能装应用了"
issue: 807
number: 17
category: show_hn
original_url: "https://bandarlabs.github.io/Cobalt/"
hn_url: "https://news.ycombinator.com/item?id=49390427"
date: 2026-08-28
---

## 文章摘要

Cobalt 是一个给 Kobo 电子书阅读器做的开源应用平台（AGPL-3.0，仓库在 BandarLabs/Cobalt），由 HN 用户 thepoet 开发。它不是一个应用，而是四样东西的组合：一个启动器（Launcher）、一个带签名验证的应用商店（App Store）、一套 Rust SDK，以及一个把每个应用关在自己的非特权进程里的运行时。

核心体验设计是「一次 USB，之后全走 Wi-Fi」：你只需要用数据线装一次 Cobalt，此后所有应用的安装、更新、卸载都在阅读器上通过 Wi-Fi 完成；Cobalt 平台本身也能通过设置界面在另一条独立通道上自更新。重启则回到 Kobo 原生阅读器。已测试机型包括 Kobo Clara BW（N365）、Clara Colour（N367）、Elipsa 2E（N605）、Clara HD（N249）、Libra 2（N418）、Libra Colour（N428），2025 年的 Clara BW P365 改版在硬件与固件事实比对与 N365 一致后也获得支持。

安全模型是项目最强调的部分。每个应用都是一个静态编译的 ARM（armv7-musl）可执行文件，以独立的非特权进程运行在原厂硬件上；商店从一个固定的 GitHub release 读取签名过的目录，每个包内含一个 ARM 可执行文件和一份签名的规范清单，运行时会在应用启动前依次校验目录、包、已安装清单和二进制本身。安装与目录事务是可恢复的，更新中断只会让设备停留在原有版本。Cobalt 不替换 Kobo 的引导链，正常的面板写入入口需要硬件与固件标识完全匹配才放行——**不匹配的设备是被拒绝，而不是靠猜**；不过首次安装确实会修改用户存储分区上的文件，且不提供任何担保。项目与乐天 Kobo 无关联。

SDK 的口号是「一个应用就是一个 Rust 文件」：实现 KoboApp trait、用声明式方式描述屏幕，运行时负责布局、墨水屏刷新规划、返回导航和生命周期。权限模型是能力门控的——应用不直接打开设备资源，而是「申请」：网络、存储、音频、前光、Wi-Fi 都是能力，被拒绝时返回的是一个应用可以正常处理的值而不是崩溃。SDK 还附带浏览器模拟器和运行时模拟器（带布局诊断）、异步能力（HTTPS、分段下载、可取消任务、定时唤醒）、每应用的原子键值存储。命令行是 `kobo new my-app` 和 `kobo dev`。应用发布与平台发布解耦：合并一个应用 PR 就会为 ARM 构建、签名并更新目录，不需要 Cobalt 版本号变动、不需要重装，应用直接出现在商店里。

已有的应用列表相当能说明作者的取向：arXiv（读 arXiv 自 2023 年 12 月起为每篇论文发布的 HTML 版本，摘要、章节、数学公式和结果表格都按面板分页）、Gutenbird（任意 OPDS 书库：古登堡计划、Standard Ebooks、Open Library 或你自己的）、Hacker News（Top/New/Ask/Show 加完整评论树）、Feeds、Daily Brief（在你用别的应用时后台收集当天的报道）、Audiobook Studio（自己调研、撰写、朗读并播放一本原创有声书）、AI Command Center、Sidekick（离开键盘也能批准或拒绝编码智能体发来的请求）、Terminal、Todo、井字棋、数独、Morse（把你输入的消息用前光以摩尔斯电码逐字母打在整块面板上）、Magnet（定位边框后面的霍尔传感器并报告其变化）。其中数独被标注为「刻意只走商店」——装上它本身就证明了 USB 包里从未包含过的应用确实被投递成功了。

贡献方式是普通的 PR：把应用作为工作区包放进 apps/&lt;app-id&gt;/、在 apps/catalog.json 里注册、写单元测试和布局测试、在两个模拟器里跑通、**在自己的真机上跑**，然后开一个带 gif 或照片的 PR。社区在 r/CobaltForKobo。

## HN 评论精华

这条 Show HN 拿到 660 分、评论数百条，是当周的头条。讨论明显分成四大块，而且其中两块跟技术无关。

**一、「我只想让阅读器好好干一件事」的路线之争**

这是最热的一块。**baal80spam** 定调：「我不羡慕。我要我的 Kindle 把一件事做好，而不是又变成一台多用途设备。」**adezxc** 回敬：「它做得最好的那件事就是给你看广告哈哈（前提是你还没越狱）。」**Artgor** 问得更直白：电子书阅读器的全部意义不就是无干扰阅读吗？

反方给出的理由更结构化。**toomuchtodo**：Kindle 是一台你几乎无法控制的电器，而这个语境下的 Kobo 是一把多功能刀；就设备所有权和控制权而言，选择多总好过选择少。**NewJazz**：有些人看中的是这个组合本身——一块墨水屏、一块性能适中的应用板、一块像样的电池和存储；对某些人来说这就是完美的瘦客户端。**paxys**：重点从来是墨水屏，Kindle 从第一代起就带浏览器，大家照样好好读书。

**AdmiralAsshat** 贡献了本帖最出圈的一段吐槽：「有那么一群爱好者，一旦发现某个设备跑 Linux 或者能跑 Linux，就非要在上面开个终端不可。我不明白为什么有人需要 SSH 进自己的烤面包机，但他们非干不可。我个人只想要一台可靠的烤面包机。」**MYEUHD** 接梗：「当然是为了在上面架个网站，或者跑 DOOM。」**9dev**：「一台不能跑 DOOM 的烤面包机有什么价值？一辈子都不去弄明白这件事，人生又有什么价值？」

**jauntywundrkind** 则被激怒了，写了一段火气很大的话：「这里不是 Normie News！你走错站了吧！街头自会为事物找到用途——令人兴奋的正是我们还不知道什么是可能的，以及我们理应有尝试的自由。HN 如此高声地反黑客、如此保守、如此频繁地跑来宣誓效忠于反可能性、反功能，实在让人难过。」**Apocryphon** 冷静回应：这里的反感更多是针对功能臃肿，而且墨水屏的刷新率真的适合通用计算吗？再说在 Linux 消费设备上拿到 root 然后随便跑东西，本身也早就不新鲜了。**foobarchu** 补了一句：「黑客精神问的不是『我该不该做』，而是『我能不能做』，只要不上升到公司层面，这挺美的。」

**brailsafe** 给出了最有建设性的中间立场：就像徒步不带耳机、露营不带笔记本一样，他不希望阅读器上「存在」游戏这个选项；但他确实想要和 Karakeep、RSS、Google Play Books、Internet Archive 这些托管着他待读内容的服务打通——「只是这件事应该以更窄的方式解决」。**luciana1u** 的比喻传播很广：「阅读器跑应用，感觉像是往图书馆里放跑步机。下一步它就要给你推『你已经 3 天没读书了』的红点了。」

**二、NickelMenu 的先例之争，以及作者的正面回应**

这是全帖信息密度最高的一段。**the-grump** 提醒大家：Kobo 生态里早就有 NickelMenu，维护多年、支持几乎所有型号，「我进 Kobo 生态就是因为 NickelMenu 和 Plato」。他随后直言不讳：「我敬佩并支持你的努力，但这个标题让人以为以前不能跑应用。NickelMenu 值得出现在开篇第一段里，并说清楚这个新东西带来了什么。」

作者 **thepoet** 的回复被顶得很高，也是本帖最值得读的技术说明：NickelMenu 很棒，而且 **Cobalt 正是用 NickelMenu 在 Kobo 原生菜单里显示入口的**，只是两个项目目标不同。NickelMenu 的定位是扩展 Kobo 的原生界面；Cobalt 的定位是像 Android 那样的**应用平台**——他希望应用作者只写应用逻辑，而不必自己造 UI 工具包、应用生命周期和渲染支持（帧缓冲绘制、墨水屏局部刷新、触摸输入处理）、蓝牙、传感器；而通过 NickelMenu 启动的 KOReader、Plato 这些都得自己实现这一整套。安全上他也划了条线：据他所知 NickelMenu 的命令动作会派生任意 shell 命令并继承 Nickel 的 root 权限，Cobalt 则把应用跑成声明了能力（网络、存储、音频、前光等）的独立非特权进程。他还想要 SDK 加模拟器让人先在电脑上测、以及首次 USB 安装后走 Wi-Fi 的商店。他坦承动机很个人：「我有个应用点子，想明天就在我的 Kobo 上用上，多快能上机？」他日常在 Mac 和手机上读的不只是书——arXiv 论文抓下来离线读、Substack、大量时间花在国际象棋残局上、远程盯着自己的 Claude/Codex、生成讲解类有声书然后用蓝牙音箱听——这些用例在长续航和墨水屏上体验会格外好。**cassepipe** 建议：「这段话几乎可以原样贴成 FAQ 的第一条：『这和 NickelMenu 有什么不同？』」

**mplewis** 的批评更尖锐：「在你那份 Claude 写的营销文案里一个字都不提 KOReader，挺侮辱人的。」

**三、Vibe coding 和 LLM 文案的强烈反弹**

这一块的火力超出很多人预料。**BoingBoomTschak** 一句「从 git log 看是 vibecoded」点燃了导火索。**code-blooded** 的立场最完整：他希望所有项目都标注 AI 使用比例；「vibe coding 本身不坏，但它让我担心项目的未来——来得容易去得也容易」。**emerongi** 反问「去哪儿？开源啊，fork 就是了」，他回答：我不能也不想维护我用的所有软件，必须挑；**把钱或时间投进一个建设者社区并在过程中学习，和维护一个 vibe coded 项目，是两回事**；我只是想尽早知道这个信息以便做判断。他还补了一句从维护者角度出发的话：如果大家都 fork，上游整合就成问题了——让原作者去评审、测试、合并 vibe coded 的代码是不公平的。

文案本身成了众矢之的。**Cyph0n**、**gadrev**、**wbxp99**、**xearl**、**mortenjorck**、**5G_activated**、**ricardobeat** 轮番开火。被引用最多的两句原文是「Other models are refused, not guessed at.」和「These are photographs of the device, not simulator captures.」——**wbxp99** 的回复只有一句：「谢谢澄清，Claude。」**mortenjorck** 的分析最有说服力：「『这些是设备照片，不是模拟器截图』是那种人类根本不会停下来考虑要不要写的废话，因为它在整体语境里显而易见。这让文字读起来更累，也给项目留下了糟糕的第一印象。」**5G_activated** 更绝：他有一台 Clara 2E，本来对软件有兴趣，「但这留下的味道太酸了，我十英尺内都不想碰它」——他的论点是，源码用 LLM 写他可以不在意，但网站和 README 是直接面向用户的，用 LLM 生成是一种失礼。**ricardobeat**：「老天，让个真人校对一下产品发布的第一段就这么难吗？」**msephton** 随后补刀：「网站文案好像已经改过了。你这条评论会永远留着。」

也有明确的反方。**Method5440**：「我不在乎这是不是 LLM 辅助做的，我只是很高兴在一个近乎封闭的生态里终于有了可行的替代方案。」他一直想要一个能直连 MangaDex 的漫画阅读器，目前的流程是下到电脑、用 KCC 打包成 cbz、传 Dropbox、再下到设备——「太费时间了」。**ashu1461** 则一句反问：「现在难道不是什么都 vibecoded 吗？」

**四、顺带炸出来的硬件经验帖：黑白屏 vs 彩色屏**

这条支线意外地成了整帖最实用的部分。共识是**问题不在清晰度而在对比度**。**izacus** 同时有 Kobo Libra 2（黑白）、Boox Go 7 II 和 Note Air 3（彩色，与彩色 Kobo 同款屏）：渲染黑白内容时锐度差不多，但彩色屏背景很暗，对他来说不开 80–100% 前光基本没法用；即便开到 100%，页面底色还是比黑白屏开 30% 时更暗；这既更耗电又更不适合读文字；而且深色模式下彩色屏有严重的残影。但他也承认漫画和彩色书（比如编程书）在彩色屏上很好看，那种略微不那么鲜艳的「纸感」很有吸引力。**II2II**（Libra 2 vs Libra Colour）和 **rbits**（Libra Colour 不开背光屏幕太暗）都印证了这点。**mkozlows** 是少数派：对比度是略差，但不并排比根本注意不到；大多数人用彩色会稍微更开心，只是不值得为它多花钱。**leokennis** 的说法很妙：Clara BW 更锐，但 Libra Colour 更像真书/纸——「用十分钟之后，哪一个都会变成『新的正常』」。

其余零散但有用的信息：**the-grump** 提醒想入坑改机的人考虑双核机型（他没做功课买了单核的 Clara BW，开蓝牙/Wi-Fi 时会卡死），他自己在 Kobo 上跑 boringtun 连回家庭网络、跑同步书和订阅源的抓取器、跑 Plato 和它的文章抓取器同步 Wallabag；**lh7777** 抱怨乐天的产品策略——把最好的两款（8 寸 Sage、7 寸 Libra 2）都停产了；**bsammon** 提醒防水型号的 SD 卡藏在防水层下面，想改机要慎选；**iib** 把 Clara Colour 刷砖了，得到的答案是新款 Kobo 没有可拆 SD 卡，基本只能换主板。

替代方案被反复提及：KOReader（配 SimpleUI 插件缓解笨重感）、Plato、NickelHook、fmon，以及 **yoavm** 在 Clara 上跑 postmarketOS 并自己做的 UI「air」——能跑 Firefox、Syncthing、KOReader 和任何 Linux 程序（**BCM43** 补充：Kindle 当年就用过 awesome 窗口管理器）。**adriaanm** 说他用 Claude 把 Plato 移植到了 Paperwhite 3 上。需求方面，**bragr** 想要 Libby 客户端，**GlenTheMachine** 投票要 Zotero 集成，**inatreecrown2** 问能不能支持 Anki，**locusofself** 想要 Obsidian/Markdown 的「活文档」查看器，**mawise** 想要独立的 OPDS 客户端——作者指路 gutenbird 示例。**mlongval** 想把自己给 NickelMenu 写的国际象棋游戏移植过来，但 Claude 分析说 Cobalt 的沙箱让它无法派生 Stockfish 引擎；作者请他开两个 issue（Elipsa Gen1 支持、Stockfish 与 Tailscale 客户端），并表示愿意配合真机测试。同样地，**Method5440** 想要 Libra Colour 移植，作者也回复愿意帮忙，只要对方能在真机上测。最后是最实在的转化证据：**dylandodds** 和 **onemoresoop** 都说「看完这篇文章就下单了一台 Kobo BW」。
