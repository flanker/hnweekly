---
layout: article
title: "Plex 裁掉了整个播放器团队"
issue: 815
number: 55
category: startup_news
original_url: "https://forums.plex.tv/t/2026-20-0-beta-ios-and-tvos-4k-dovi-hdr10-won-t-play-infinite-spinning-circle/943966?page=5"
hn_url: "https://news.ycombinator.com/item?id=50186318"
date: 2026-10-23
---

## 文章摘要

这篇"原文"不是新闻报道，而是 Plex 官方论坛上一个 bug 报告帖。帖子标题是《2026.20.0 测试版 iOS 和 tvOS 无法播放 4K 杜比视界/HDR10，一直转圈》，HN 链接指向它的第 5 页。真正引爆讨论的是其中一条很快被删除的回帖。

**bug 本身。** 10 月 5 日，用户 veeejay 发帖说，把 iOS 和 tvOS 上的 Plex 升级到 2026.20.0 测试版后，所有 4K 杜比视界（DoVi）内容都只显示无限转圈，普通 HDR 和 1080p 正常。随后几位用户把范围缩小到杜比视界 Profile 8，Profile 5 和 7 能放；用转码方式播放可以，直接播放（Direct Play）会卡死。dklein 在帖子里多次附上服务器和客户端日志，并 @ 了 Plex 员工 Atomatth 和 Craig_Holliday。测试版前后出了 6 个版本，问题始终没修好，Menel 9 号就担心"要是这样推到正式版，社区会炸"。结果 10 月 13 日，2026.20.0 原封不动发布到 App Store，普通用户纷纷中招，有人说"半个片库没法看了"。19 号的 2026.21.0 依旧没修，这个版本的更新说明里反倒写着适配了"iPhone Duo"折叠屏，Menel 讽刺地问大家"懂不懂什么叫优先级"。20 号，Plex 员工 drzoidberg33 才回复说已经在修，"很快会有修复"。

**被删除的那条回帖。** 10 月 21 日，第 92 楼出现了 Craig_Holliday 的回复。他是之前被用户 @ 过的 Plex 播放器开发者，发帖时身份标签已经显示为普通的"Plex Pass"用户。根据 Reddit 上流传的截图，他写道："修复这个问题的代码我已经提交了，但不知道会不会被合并、问题能不能修好。播放器团队今天被裁了。换 Jellyfin 和/或 Infuse 吧。"这条帖子随后被彻底删除，论坛页面上 92 楼已经不见了，但从第 93 楼开始的回复可以看出它确实存在过。dklein 说"Craig，很遗憾听到这个消息，一直感谢你帮忙、愿意让我们了解情况"；另一位用户说"我们很多人都在为你和 Plex 加油，听起来 Plex 要撑不下去了"。

**用户的愤怒。** 后面的回帖情绪明显转向。dklein 呼吁 Plex 发一篇博客或者开一次交流会，说明各平台客户端的现状：各个平台的老用户本来都对 Plex 很热情、愿意推荐给亲友，结果所有"新体验"客户端都一塌糊涂，官方几乎一言不发。他还提到，现在每隔几天就有一个 vibe coding 写出来的社区播放器，放片子比官方新版还好，用户为了解决本不该存在的问题去装这些未经审查的应用，本身就是安全隐患。smmacca 的长帖批评"新体验"改版：名义上是"全新打磨的界面"，实际是为了把所有平台统一到一套代码库而牺牲用户体验，因此流失的订阅和修 bug 的成本可能比维护多个原生应用还高。也有人直接给出迁移方案：服务器换 Jellyfin，Apple TV 播放器换 Rivulet，用 JellyPlex-Watched 迁移观看记录。还有终身会员说，再听到一次 Plex 裁员的消息就要求退款；也有人建议先删账号，免得 Plex 把数据卖给别人。

## HN 评论精华

本帖 160 分、114 条评论。值得注意的是，提交时链接到的那条员工回帖已经被删除，不少人点进去一头雾水；讨论主要分成两块：一是吐槽 Plex 新版 Apple TV 客户端和这几年的整体走向，二是大量交流 Jellyfin、Emby、Kodi、Infuse 等替代方案，后者占了大半篇幅。

**那条帖子去哪了。** zzyzxd 给出了原帖楼层链接，说自己所有杜比视界/HDR10 视频都放不了，今天才找到这个帖子。arprocter 贴了 r/plex 上的截图，说那条回复先被编辑删掉了"换 Jellyfin 和/或 Infuse"一句，之后 techjamie、jmuguy、Bluescreenbuddy 都确认整条已经被删；arprocter 还发现这位开发者连 Reddit 账号也删了。joshuat 说，在社区论坛讨论帖里扔下这么一条消息，难以想象怎么会觉得是好主意；maxprimes 则说"在这种帖子里扔炸弹，真够狠的"。

**新版 Apple TV 客户端有多糟。** PierceJoy 说新的 Apple TV 应用彻底重新设计，看着还行，但 bug 多、交互糟糕，他用了十年 Plex 终于"成了那个陈词滥调"，换了 Jellyfin，以后打算拿一个开源播放器让 Claude 按自己的口味改。fetus8 说昨晚看电影时就决定这周末迁移。joshstrange 说新播放器"倒退了十步"，他换成 Plezy 当客户端，但也不满意，Jellyfin 他试过，对家人这类外部用户来说太粗糙。dimmke 倒不讨厌新界面，但遇到按暂停会让播放彻底卡死的 bug，他说一直听人说软件质量在下滑，如果这次更新真是 AI 主导、真人开发被裁，这可能是他第一次亲身体会。gdulli 指出一个讽刺：Xbox 版 Plex 因为早被放弃、没跟上改版，现在反倒可能是最好用的。

**该怪谁。** joshstrange 好奇裁员原因：是因为新版口碑太差，还是因为 AI，还是要转向。tstrimple 认为 Plex 清楚自己的钱从哪来，不是靠帮你播放个人视频，而是靠合作分成和在首页塞别人的内容，他付了 Plex Pass 还要在本地媒体前看广告。halJordan 说不能让一线工程师把责任推给无名的高管，egeozcan 和 watwut 反驳说开发者能影响的范围有限，质量靠的是测试、需求等流程，单个工程师改变不了。oliwarner 话说得更狠：看过去两年的产出，裁掉他们也许是对的，只希望被裁的正是推动这些烂主意的人。georgemcbay 则说他以为 Plex 早已进入"Bending Spoons 模式"，只顾榨钱，惊讶它居然还有一个像样的团队可裁，但仍真心同情被裁的人。joshuat 认为"终身会员"的收费模式从一开始就有缺陷，是把 Plex 推上这条路的重要原因；xaksnTMu2 也认为这是追求全平台统一代码库的后果。

**Jellyfin 准备好了吗。** 很多人推荐替代方案：dudu24 推荐 Apple TV 上的 Jellyfin 客户端 Nautilus，cr125rider 说 Swiftfin 最近也大更新了；cdrnsf 推荐 Jellyfin 搭配苹果生态的 Infuse 和安卓上的 Wholphin。toomuchtodo 说这是转向一个无法被收购、无法被"劣质化"的开放生态的好机会，Jellyfin 不足的地方就去贡献代码，他自己在一个多 PB 的片库上同时跑 Plex 和 Jellyfin。但 Plex 的优势也被反复提到：joshstrange 和 inhumantsar 说，难的是给不懂技术的家人用，Plex 客户端几乎覆盖所有设备、账号体系完善，inhumantsar 的父母和妹妹一家分散各地、设备五花八门，Jellyfin 生态太碎片化。jmuguy 专门问 Jellyfin 对非技术用户的体验，回答两极：asdfqwertzxcv 和 Quick2822 说比 Plex 还简单，PorciiVorbesc 则说他父母更喜欢 Plex，而且 Jellyfin 对文件命名要求严格，很多剧集识别不出来。80fj0348i 让亲友用 Tailscale 加 Jellyfin，唯一抱怨是 Xbox 上不方便。音乐方面，2OEH8eoCRo0 说 11 万首 FLAC 的曲库让 Jellyfin 无法使用，只好换 Navidrome；chneu 说 Jellyfin 10.x 换了数据库后，部分大型音乐库性能严重下降，他把 30 万首的库复制成三份分别升级，才有一份正常。想替代 Plexamp 的人推荐了 MusicAssistant、Gonic 加 Symfonium 等。

**老派方案。** AdmiralAsshat 提醒 Plex 当年就是从 XBMC/Kodi 分叉出来的，他至今还在用 Kodi，不在乎别人嫌界面过时；znkynz 推荐"Kodi 前端加 Emby 后端"，happymellon 则说他用 Emby 多年，也快被逼到要换 Jellyfin 了。thiht 不同意，认为 Kodi 在电视上根本没法用，光虚拟键盘就够劝退。Jhsto 提醒，还有 Infuse、VLC 这类直接对接 Plex 服务端的第三方客户端可用，ctippett 推荐了一个 WebOS 上的原生 Plex 客户端。评论区里还有几位独立开发者趁机推荐自己做的播放器和媒体服务器。
