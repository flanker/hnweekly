---
layout: article
title: "Defcon 上最酷的反监控工具（视频）"
issue: 806
number: 44
category: watching
original_url: "https://www.youtube.com/watch?v=-2uAsJ5EPAw"
hn_url: "https://news.ycombinator.com/item?id=49346444"
date: 2026-08-21
---

## 文章摘要

这是 **Naomi Brockwell TV**（NBTV）频道在 2026 年 8 月 14 日发布的一条 **10 分 56 秒**（656 秒）视频，截至抓取时播放量约 **45 万次**。NBTV 是 Ludlow Institute（一家 501(c)(3) 非营利组织，宗旨是「通过技术推进自由」）的项目。视频的主题是：DEF CON 是世界上规模最大、历时最久的黑客大会之一，主持人在现场走访了那些正在打造工具对抗监控国家（surveillance state）的黑客们。

官方简介给出了完整的**章节列表**，构成了视频的骨架：

- **0:00 DEF CON Is Wild** — 会场概况
- **0:38 Ludlow's IoT Village Hackathon** — Ludlow 在 IoT Village 办的黑客马拉松
- **2:08 Colonel Panic — Flock Camera Detector** — Colonel Panic 的 Flock 摄像头探测器
- **4:27 EFF's Rayhunter — Stingray Detection** — 电子前哨基金会的 Rayhunter，用于检测 Stingray（伪基站）
- **6:53 The Biscuit Ultra — Anti-Surveillance Gadget** — Biscuit Ultra 反监控设备
- **7:40 Meshtastic on the LilyGo T-Deck** — 在 LilyGo T-Deck 上跑 Meshtastic
- **8:15 Exploitee.rs Meshtastic Pager** — Exploitee.rs 的 Meshtastic 寻呼机
- **8:38 Shenanigans** — 会场趣闻
- **8:48 Em3ritus' simulacra** — Em3ritus 的 simulacra
- **9:20 No One Is Coming to Save Us** — 收尾观点

简介里点名给出的项目链接依次是：colonelpanic.tech、eff.org/rayhunter、biscuitshop.us/products/biscuit-ultra、lilygo.cc 的 T-Deck、Exploitee.rs、以及 github.com/Em3ritus/simulacra。

视频的**核心论点**在简介末尾说得很明确，也就是最后一章的标题所指：**没有人会来救我们**（No one is coming to save us）。保护我们隐私的工具，是由一个「拮据的、由黑客和业余爱好者组成的散兵游勇社区」用极其有限的预算做出来的——而任何人都可以加入这场战斗。

结合 HN 评论中几位观众补出的技术细节，这几件工具的具体作用是：

**Flock 摄像头探测器（OUI Spy）** —— 一块基于 ESP32 的板子，通过扫描蓝牙/MAC 地址（OUI 是 MAC 地址前缀所标识的厂商代码）来识别 Flock Safety 的车牌识别摄像头，检测到就发出蜂鸣。项目在 GitHub 上是 colonelpanichacks/oui-spy。评论者 **NDlurker** 提到它甚至有做成**耳环**的版本，他几个月前买了一对送给女友的孩子当生日礼物——「开车到处找到那些我以前从没注意到的 Flock 摄像头，既有趣又令人失望」。

**Rayhunter** —— EFF 的项目，用于检测 IMSI catcher，也就是伪基站/蜂窝站模拟器（cell-site simulator），俗称 stingray。它最初被设计成跑在一个**便宜的二手移动热点**上。

**Biscuit Ultra** —— 官方描述是一个「wardriving 平台：完整的 WiFi 与 BLE 安全工具包」，一个无头（headless）的无线安全研究平台，完全通过蓝牙从手机控制，支持双频 WiFi（2.4GHz + 5GHz）、低功耗蓝牙扫描与攻击、带 GPS 制图的 wardriving、抓包等。

**simulacra** —— 这是评论区争论最多的一件。项目自述是：simulacra 持续在你周围**伪造出一群不断变化的、看起来可信的假无线设备**，用噪声淹没你真实的设备，让被动追踪器、ALPR（车牌自动识别）附加模块和「同行者关联」（co-travel correlator）无法可靠地从人群中挑出你的信号；同时它被动地监视那些跟着你走的追踪器。**staplung** 的概括版本是：一个会提醒你「有东西正在大致跟着你移动」（AirTag、SSID 等）的设备。

**Meshtastic 相关** —— 视频里 Meshtastic（基于 LoRa 的去中心化 mesh 通信）反复出现，包括跑在 LilyGo T-Deck 掌上设备上的版本，以及 Exploitee.rs 做的 Meshtastic 寻呼机。

视频简介的其余部分是频道的常规内容：捐赠链接（LudlowInstitute.org/donate，美国境内可抵税）、周边商店、电子书《Beginner's Introduction To Privacy》、在 Odysee 上的镜像，以及一长串隐私相关的推荐书目（Snowden 的《Permanent Record》、Michael Bazzell 的《Extreme Privacy》、Greenwald 的《No Place to Hide》等）和产品（法拉第袋、USB 数据阻断器、摄像头贴纸、防窥屏等）。简介里还有一条防诈骗声明：作者从不给出电话号码、从不主动联系你提供投资建议。

## HN 评论精华

这条帖子拿到 240 分、46 条评论。**讨论的主线其实并不在这些工具本身**，而是迅速转向了两个更尖锐的话题：一是 DEF CON 这个会议本身是否早已失去黑客精神（这是全帖最激烈的一支），二是 simulacra 这类「噪声淹没」方案在技术上是否真的有效。另有一条支线在讨论伪基站的法律地位，以及一条关于 YouTube 广告质量的吐槽。

- **staplung** 贴出了全帖第一条摘要（也是最高票之一），把视频内容压缩成三行：基于 ESP32 的 Flock 摄像头蓝牙/MAC 探测器；跑在二手移动热点上的 Stingray 检测器；以及一个提醒你有东西在跟着你移动的设备。**pavel_lishin** 和 **addandsubtract** 补出了具体项目（OUI Spy 板），**em3ritus**（项目作者本人现身）指出最后那个是 simulacra，而且它做的事比摘要说的多得多。
- **leetrout** 贴了一条更完整的清单，逐个引用了 simulacra、Biscuit Ultra、Rayhunter 的官方自述，并说很高兴这条帖子拿到了「第二次机会」被顶回首页——内容好且及时。
- **_blk** 提出了一个务实的工程质疑：ESP 的想法很酷，但用默认的 ESP 天线你不是得走得相当慢吗？高增益天线可能更好。他半开玩笑地建议，如果你戴着帽子，可能不太在乎从上方被看到（相对于从正面），干脆在帽子上的天线顶端再装个小风速计。
- **Joel_Mckay** 是最尖锐的怀疑者，也引出了全帖一条重要的价值观辩论：「唉，研究者以前是发表前沿东西的。那个清单读起来像大一的实验课表。」**embedding-shape** 反驳说，就公民行动主义和个人反追踪而言（至少在美国），这确实是前沿，这总该算点什么吧？**Joel_Mckay** 的回应把分歧摆到了台面上：**投票、以及在本地参与政策讨论，牵引力要大得多**。事实是随着卫星直连手机服务上线，无论人们在地面上想什么、做什么，汽车与民用遥测元数据被采集的风险都要大得多。「相信个人能靠小玩意影响政治政策，是天真的。」
- **lrvick** 引爆了全帖最长的一条支线，指控相当具体：DEF CON 的「Crypto and Privacy Village」要求你同意 Salesforce、Google、Microsoft 和 Discord 的隐私政策才能与他们互动——这是一种耻辱。「DEF CON 很久以前就不再关心隐私、黑客精神和数字主权了。想去就去，你可以跟他们的军方招募人员聊天，或者买卖专有的安全 SaaS 万灵油。现在它基本上就是另一个 corpocon（企业大会）。黑客除了当作在拉斯维加斯用公司信用卡跟朋友聚会的机会，大概应该跳过它。真正的黑客现在多在 HOPE 和 CCC。」被 **Cider9986** 追问实际情况后，他解释说：与该 village 互动只能通过 Slack、Google Groups、GitHub 或 Discord，全是中心化的专有监控资本主义产品，没有任何去中心化/FOSS 选项，而 cryptovillage.github.io 显示这一点至今没变。他还批评他们打着「crypto 指的是密码学！」的牌子作道德姿态，却不实际推广让人掌握自己密钥的工具，「它真的是美国隐私倡导一切问题的象征，也是我们为什么远远落后于欧洲的原因」。**kayfox** 要求引证，说自己在现场时网站上没看到、也没人上来要他签任何东西。
- 由此衍生的会议地图讨论：**nerdsniper** 认为 DEF CON 一向是最亲政府的会议，历史上 BlackHat 才是它略偏反主流文化的对照物，而如今反建制黑客根本没有实体聚会；**kayfox** 直接纠正说他搞反了，DEF CON 才是反主流文化的那个、BlackHat 是企业会议；**lrvick** 折中说那是原本的意图，但现代 DEF CON 一样企业化，只是服务于付不起 BlackHat 的人，欧洲的 CCC 才是 DEF CON 曾被传说的样子。补充的替代选项包括 **margalabargala** 提到的 HOPE、**Cider9986** 提到的 MoneroKon 和 Monerotopia、**secabeen** 提到的 BSidesLV（更偏安全从业者而非黑客圈）、以及 **Teknomadix** 贴的 toorcamp.org。**dylan604** 抛出一个反讽：参加隐私中心的会议本身不就等于自我暴露（doxxing）吗？**lrvick** 说隐私向的会议允许现场付现金，这大概是 DEF CON 唯一还做对的一件事；**dylan604** 补刀说付现金相比会场内外遍布的摄像头根本不算什么，而且酒店绝对不收现金，除非你愿意住那种有站街女和毒贩的旅馆。
- **fooqux** 提出了对 simulacra 最有力的技术质疑：他理解这个想法——就是在周围刷出大量设备，希望自己被淹没在人群里；但任何为追踪而设计的东西都是为了处理大量移动设备而专门建造的。「也许如果人人兜里都有一个，我们能压垮这些监控设备，但充其量你只是在把它们的日志撑大。」**bewareofscams** 附议并补上更致命的一点：**原本那个设备对追踪者始终是可见的**——你的射程内只有你一台设备，和你的设备加一大堆噪声，真的有区别吗？
- **gruez** 用 xkcd 1105 的方式吐槽了同一件事的社会层面：「哦，就是那个带蓝牙刷屏器的家伙。」**dylan604** 反而由此想到一个更有意思的部署思路：如果它们便宜到一定程度，你可以把它们「永久」留在城里——挂在装 Flock 摄像头的同一根杆子上，或者塞在杆子附近那种假石头形状的藏钥匙盒里，开车经过时随手扔一个；反正全是固态器件，被扔来扔去应该没问题。
- **kamranjon** 问了一个法律问题：警方基本上运营自己的伪蜂窝基站，相关法律是怎样的？他假定这对个人来说是高度违法的，执法机构需要跳过什么圈子才拿到批准，FCC 需要盖章吗？**Joel_Mckay** 回答说一般而言干扰无线电链路可以是数十万美元罚款加数年监禁，几乎所有消费者拥有的合法 RF 通信设备在检测到冲突或干扰时都会退避广播；「过去几十年里许多政府觉得自己有权对本国公民开展情报行动，这往往指向一种退化的专制趋势。最好的计划根本不需要保密，而且所有人都玩得开心。」**PaulRobinson** 补充说 John Oliver 几周前做过一期相关内容（美国以外不易看到，但《卫报》有 2026 年 8 月 3 日的报道），**TL;DR 是：警方想做就做，基本如此**。
- 元层面的两条短评：**geokon** 说「用一个 Google 的产品来分享这个，很讽刺」，**poopbutt16** 用那句名梗回击「『可你还在参与社会。真有意思！』」。**xyzsparetimexyz** 给出最悲观的一句：DEF CON 多年乃至数十年来除了联邦特工和三字母机构什么都没有，假定这些工具全都已被绕过。
- **gourneau** 借视频里反复出现的 Meshtastic 提到，去 Burning Man 的人可以用一个专门为该活动搭的 mesh 网络（burningmesh.org），他自己会跑几个节点。**willis936** 说他很想知道效果如何：meshcore 的一个宣称是它比 Meshtastic 扩展性更好；900 MHz 频段有限，而在没有牧羊人的情况下放羊是个难题——音乐节是很好的压力测试。**sitzkrieg** 指出两者都用 CSS（chirp spread spectrum 啁啾扩频），所以差别很可能在软件。**warrenmiller** 顺带感叹「手机联网就是 Burning Man 终结的开始」。
- **burger_moon** 的吐槽意外获得共鸣：他不确定这算不算跑题，但视频前那条不可跳过的广告是「快速清出卡住的大便」，来自一个 AI 医生——「这就是我不用带广告屏蔽的浏览器打开的报应。现在没有 ad block 的 YouTube 就是这样吗？」**pudgywalsh** 确认「是的，真的很糟」，他经常看到那条诈骗广告，有些甚至用名人深度伪造；未登录状态的 YouTube 广告 90% 是这种垃圾，举报毫无用处，Google 不在乎。
