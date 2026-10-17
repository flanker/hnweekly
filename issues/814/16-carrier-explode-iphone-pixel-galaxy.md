---
layout: article
title: "Show HN：Carrier-Explode，解码 iPhone、Pixel 和 Galaxy 的运营商设置"
issue: 814
number: 16
category: show_hn
original_url: "https://carrierexplode.com/"
hn_url: "https://news.ycombinator.com/item?id=50024499"
date: 2026-10-16
---

## 文章摘要

carrier-explode 是开发者 Alec Dusheck（HN 用户名 simplyalec）的业余项目。他在 Show HN 帖子里介绍说，这个工具持续归档各大手机品牌的运营商设置，还包含对常见基带配置的解码器和解释；虽然很多假设还有待验证，但它已经在几个发烧友群体里派上了用场。网站首页的定位是：从 iPhone、Pixel 和 Galaxy 固件中提取运营商设置，解码并对比。

**项目缘起。** 根据 GitHub 仓库的 README，作者最初是想弄清楚 iOS 如何处理印度运营商的小区广播（cell broadcast），以及这些运营商添加了哪些紧急警报。要回答这个问题，他得手动翻 CommCenter 和运营商配置包（carrier bundle），很快就烦了，于是写了一个小查看器，一件事引出另一件事，最后变成了 carrier-explode。

**能做什么。**
- 选出你需要的功能（5G SA、VoNR、Wi-Fi 通话等），看看在你的手机上哪些运营商提供这些功能。功能页面列出了从 iPhone 15 到 iPhone 18 Pro Max、Pixel 4a 5G 到 Pixel 11 系列、Galaxy S25 到 Z Fold8 等机型，可选的条件包括 5G SA、VoNR、VoLTE、EVS 编解码、Wi-Fi 通话及漫游时的 Wi-Fi 通话、RCS 消息、运营商卫星功能、eSIM 快速转移、Apple Watch 蜂窝号码共享等，例如勾选 5G、VoLTE、Wi-Fi 通话三项后，792 家运营商中有 214 家在 iPhone 18 Pro Max 上全部满足。
- 查询某家运营商在各平台上的设置：APN、VoLTE、Wi-Fi 通话、5G 等，精确到每款手机。
- 对比某家运营商两个版本的设置，或者两家运营商之间的设置，看某次系统更新改了什么。
- 通过 API，根据 SIM 卡的 MCC/MNC、GID1/GID2、SPN、IMSI 或 ICCID 查出它会选中哪套配置。
- 阅读每个固件版本里的基带配置和载波聚合频段组合。
- 通过免费的 JSON API 获取所有数据，或者下载每日更新、以 CC0 协议发布的完整数据集。

**数据来源。** 所有数据都来自厂商自己公开发布的下载：iPhone 的运营商和国家配置包取自 iOS 恢复镜像（IPSW）和苹果的无线运营商配置更新；Pixel 的 CarrierSettings 和基带配置取自谷歌的完整 OTA 镜像及版本之间推送的运营商设置更新；Galaxy 的 CSC/OMC 运营商包、IMS 设置和基带固件取自三星固件服务器。网站和 API 只发布解码后的数据（以及苹果配置包里内嵌的图片和音频），不直接分发厂商原始文件。覆盖范围是 iPhone 15 及以后机型、iOS 18 及以后版本（含最新大版本的测试版）、2020 年 10 月以来的 Pixel，以及 2025 年以来发布的美版 Galaxy S、Z Fold、Z Flip。

**技术架构。** 整个系统跑在 Cloudflare 上：每个固件版本由一个 Workflow 负责抓取和解码，只有在 Worker 干不了的地方（解开 iOS 根文件系统或 Galaxy 系统分区）才启动容器；R2 按内容哈希存储每个文件及其规范化形式；新记录经过队列，由消费者推导出运营商、时间线和功能状态写入 D1 数据库；网站（SvelteKit）和 API 从 D1、R2 读取数据并在边缘缓存，索引变化时清除缓存。作者说运行成本非常低，因为容器只在有新版本时才启动。代码按 pnpm workspace 组织，解码器覆盖 iOS、Android、三星，以及高通、三星 Shannon 和联发科基带，项目以 MIT 协议开源。

**Wiki。** 网站附带一个相当详细的 wiki，解释 iOS 运营商配置包里有什么、iOS 如何从一张 SIM 卡选到对应的配置包、MVNO 如何共用配置、Default.bundle 里 18 MB 的提示音、查找表和区域基带表，以及 .der.pri、.der.gri 这些基带覆盖文件的格式。还有针对几家大运营商的"怪异之处"专题，比如 AT&T 配置包里一个自 2009 年以来从未改过的 Wi-Fi 密钥、T-Mobile 生产环境里出现的实验室服务器、乐天移动的 XCAP 走明文 HTTP、Jio 配置里一个拼错的键名；以及中国运营商"终端自注册"在 iOS 运营商配置包和高通基带固件中的体现。

这个帖子在 HN 上获得 432 分、约 55 条评论。

## HN 评论精华

评论区规模不大但信息量不小：有人单纯欣赏界面，有人追问这些数据有什么用，懂行的人则补充了如何修改这些配置、哪些字段控制热点、以及最近 iPhone 18 Pro Max 基带故障的背景。

**"这些信息能拿来干嘛？"** mrweasel 说不太清楚这些信息能做什么，但很喜欢这个界面，mjhagen 评价"很有 2000 年代初的味道"，作者回答界面是纯 CSS 加 Svelte（及其较新的 remote functions）写的。ahmedfromtunis 问这些配置在手机上是只读的，还是可以用这里的数据开关某些服务。flotzam 说取决于系统，GrapheneOS 可以覆盖其中很多项；作者补充说，直到最新的测试版之前（刚刚修复），都可以骗 iOS 为你的运营商加载不同的配置包，这在 Wi-Fi 通话不可用的俄罗斯很常见，并给出了相关项目链接。doublerabbit 则提醒：越狱加改 plist 是可以，但要做好被运营商封号的准备。

**iPhone 18 Pro Max 基带故障。** isomorphic 说他是在 MacRumors 关于 AT&T 版 iPhone 18 Pro Max 卡死问题的讨论里看到这个网站的，从数据可以看出 AT&T 和苹果试图抢先应对：目前看来他们关闭了 5G 独立组网（SA）模式，可能是为了防止某个 bug 真的损坏硬件（也许只影响一批手机），而苹果和 AT&T 除了更换受影响的硬件外还没有发表任何声明。kridsdale3 说他上周直接退掉了 iPhone 18 Pro Max：在 Verizon 上基带频繁锁死、掉到 SOS 模式，他花钱办了 AT&T 的 eSIM，一天照样断网五次需要重启，又换 T-Mobile 的 eSIM，还是一样；后来他读到只有 Pro Max 用的是高通基带，Pro 用的是苹果 C2，他太太用 C2 的 iPhone Air 信号很好，于是他换成了 Pro，现在在以前只有 0 到 1 格信号的楼里信号也很好，"去他的高通"。iririririr 和 subscribed 反问他为什么一点不怪苹果：设计 bug 难免，明知元件有问题还卖的应该是手机厂商。

**热点被禁用和信号"注水"。** jakobdabo 说大约一年前他手机界面里的个人热点突然被禁用了，此后只有在旅行用旅游 eSIM 时才重新出现，他想知道是哪个字段控制的，"手机厂商允许运营商做这种反用户的事真让人难过"。dawnerd 和 Melatonic 猜是 iOS 的 bug，FabHK 则指出很多套餐（通常是不限流量的）确实会禁用热点。remix2000 给出了具体答案：相关的 plist 键是 CarrierEntitlements（需从 plist 中移除）和 APN 类型（需要和网络共享的启用掩码做"或"运算），并附上了 The Apple Wiki 的链接。ac29 觉得好笑的是，配置里有一个 inflate_signal_strength_bool（夸大信号强度）选项，而很多运营商把它设成了 true。flyinghamster 说这让他想起 2010 年代美国 AT&T 和 T-Mobile 把 3G 冒充成"4G"的时代，teknolog 补充说，AT&T 在旧金山真正推出 LTE 时，反倒不得不改名叫"4G LTE"。

**审查与限制性政权。** m463 好奇"限制性政权"会用哪些设置，比如把语音信箱的 IMAP 设成不使用 SSL。作者随即甩出了 wiki 里关于中国终端自注册的文章链接。rustcleaner 则感谢这项"公共服务"，认为手机厂商和运营商应该为把如此重要的配置藏在专有格式里感到羞耻。

**本地化与怀旧。** etatester 很欣赏网站直接显示了他所在国家的运营商，而不是常见的"美国优先"，esperent 说在 HN 上看到一个以越南开头的列表时先是震惊、随后是感激，pftburger 也说正因为看到相关信息，他才留下来探索而不是直接关掉。有人问如何做 IP 地理定位，etatester 指出 Cloudflare 会在请求头里带上国家信息。tylergetsay 和 rootsudo 想起了 CDMA 时代高通软件泄露和 PRL 文件的往事。

**数据贡献与相关项目。** seba_dos1 建议把适用的数据贡献给 GNOME 的 mobile-broadband-provider-info 项目，作者表示会研究；KetoManx64 提醒如果在获取数据时用过 AI，那边一闻到 AI 的味道就会封禁，mitxela 说只管贡献准确的信息，"他们要因为有人贡献了准确信息而封号，那就随他们，GNOME 就是个马戏团"。denysvitali 说更有用的是载波聚合组合列表，他以前写过一个只支持高通的解码器，帮他改善了设备对运营商的支持。The_SamminAter 问怎么贡献数据，作者请大家在 GitHub 开 issue，并说正在考虑接入社区数据源，比如 open-carrier-data 项目。
