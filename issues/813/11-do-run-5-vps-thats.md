---
layout: article
title: "Ask HN：你在 5 美元的 VPS 上跑了什么值得 24 小时在线的东西？"
issue: 813
number: 11
category: ask_hn
original_url: "https://news.ycombinator.com/item?id=49985548"
hn_url: "https://news.ycombinator.com/item?id=49985548"
date: 2026-10-09
---

## 文章摘要

这是一篇 Ask HN 帖子，发帖人 mariocesar 说，DHH 提出的「AI 小棚屋」（AI shed）设想让他动了念头，想弄一台每月 5 到 10 美元的小型 VPS。他自称是「Linux 人」，人生中很多阶段都有过自己的服务器，喜欢有一台永远在线的机器；但他也怀疑维护成本是否值得，毕竟大部分东西在笔记本上就能跑。他也承认自己有个盲点：习惯事事亲力亲为，事后才发现本可以自动化或交给别人。所以他向大家请教：如果你有一台便宜的 VPS 或迷你主机，长期下来真正有用的是什么？有没有哪样东西节省的时间或替代的订阅足以让它留下来？

这个帖子获得了 405 分、约 640 条评论，成了一场大型的「自托管清单」分享会。综合来看，回答大致可以归为几类：

- **网络与隐私基础设施**：WireGuard/Tailscale/NetBird/Headscale 组网、VPN 出口节点（翻越地域封锁或网络审查）、权威 DNS、Pangolin 或 Caddy 反向代理（把家里的服务安全暴露出去，避免暴露家庭 IP，或绕过 CGNAT）、Pi-hole/AdGuard 广告过滤。
- **通信与身份**：自建邮件服务器（Postfix + Dovecot）配合「每个网站一个别名」防垃圾邮件、Matrix/XMPP/Jitsi/IRC 保活、Mumble/TeamSpeak。
- **个人数据与生活工具**：Vaultwarden 密码管理、Immich 照片、Jellyfin/Plex 影音、Audiobookshelf、FreshRSS/Miniflux、Wallabag/Readeck、Actual Budget、Syncthing 中继、Nextcloud、Uptime Kuma 监控、家庭共享购物清单等。
- **开发相关**：Gitea/Forgejo 代码托管与 GitHub 镜像、CI 运行器，以及越来越多人提到的「编程智能体专用机」（Claude Code、Codex、Hermes 等），用手机通过 Tailscale 和 tmux 随时随地 vibecoding。
- **小网站与小生意**：个人主页、博客、游戏服务器（Minecraft、Factorio、CS 1.6），以及若干用 SQLite、Go 跑起来的小型 SaaS 和网店。

讨论中还有两条贯穿性的线索：一是「VPS 还是家用迷你主机」之争，很多高赞回答其实跑在家里的旧台式机或 N100 迷你主机上，VPS 只充当公网入口；二是 AI 编程工具大幅降低了搭建和维护这些服务的门槛，不少人说自己的个人工具是「vibe-coded」出来的，用来替代原本付费订阅的服务。也有少数人泼冷水，认为维护和安全成本并不划算。

## HN 评论精华

**最高赞：家用服务器全家桶，与「Linux ISO」梗**

- 评论区最热的回答来自 **ehe78qhe**：他的家用服务器（一台旧台式机）跑着 qBittorrent 下载「Linux ISO」、Jellyfin 观看「Linux ISO」、Tailscale 让家人朋友在全球各地都能看他的「Linux ISO」、Immich 托管照片（不想把私人照片交给陌生科技公司），以及 Gitea 托管未开源的个人项目。
- 引号里的「Linux ISO」是盗版影视的委婉说法，于是引发一连串玩笑：**jotato** 说自己有了新爱好，在二手店花 1 美元一张买「Linux ISO」，已经收了 300 张；**satvikpendem** 抱怨不想在免费 4K 的世界里看付费的 480p；**bambax** 说法国很多公共图书馆可以免费借来「复制」；**paulcole** 直说「你就说你在盗版呗」，ehe78qhe 回答：「什么？那是违法的！我只是个操作系统爱好者！」
- **jedberg** 提醒自托管照片务必做异地备份：「从我的错误中吸取教训。」ehe78qhe 说自己有加密的异地备份，很多人会和家人约定互相交叉备份；**mrheosuper** 提到 3-2-1 原则，**CountGeek** 用 13 欧元 5TB 的 Hetzner 存储盒配 kopia 备份，并实际测试了恢复。
- **Lammy** 提醒：Tailscale 默认会记录设备行为元数据；**ehe78qhe** 表示完全不在乎，**chrisweekly** 指出文档同一节紧接着就写了如何关闭，这种批评不够厚道；**trollbridge** 反问为什么不直接用 WireGuard。
- **iso1631** 算了一笔账：100W 的家用服务器每月耗电 72 度，电价每度超过 7 美分就不如租 5 美元的 VPS；**user43928** 和 **u8080** 回应 Mac mini 或 N100 待机不到 5W，**ehe78qhe** 则说 5 美元的 VPS 可没有 GPU 给 Jellyfin 转码。

**邮件、DNS 与「数字主权」**

- **spl757** 自建 DNS 和邮件，为每个注册的网站分配唯一别名，收到垃圾邮件就关掉，还能看出是谁卖了或泄露了他的信息（比如 bestbuy@ 那个地址开始收到来自俄罗斯的邮件）。**Sha1rholder** 认为在 VPS 上自建邮件风险大又麻烦；spl757 回应自己搭过日发 1500 万封（合规双重确认）的邮件系统，关键在于会不会管理；**Lammy** 补充主要看 IP 信誉的运气，他的 IP 已经用了 15 年。
- **gspr** 说，对他来说 VPS 的意义是在糟糕的互联网上拥有一个永远可达、属于自己的地方。他在附言中还批评了 DHH 的政治立场，劝大家别听他的建议，引来 **xeornet** 质疑「这跟用不用 VPS 有什么关系」。
- **bawolff** 只放了个人主页，喜欢「有个 Apache 在互联网上跑着」的感觉，临时传文件也方便。**keithnz** 问为什么不用免费的 GitHub + Cloudflare Pages；**ak6te** 和 **zem** 回应不想依赖大公司及其不稳定的自动化服务条款执行，5 美元几乎等于免费；**loloquwowndueo** 吐槽「GitHub 天天宕机」。
- **mjcarden** 惊讶没人提到密码库：pass 工具自带 git 集成，他十多年来都用一台每月几美分的云主机托管加密密码库的副本。**equinumerous** 担心被攻破，**oarfish** 指出密码是 GPG 加密的，但你在哪些网站有账号会暴露。

**VPS 还是迷你主机？**

- **monk_grilla** 本想听听为什么选 VPS 而不是迷你主机：他从树莓派升级到 Beelink 迷你主机后，省下的订阅费很快就回了本。**throwawayk7h** 回应：迷你主机适合私有网络使用，但如果直接暴露在公网，就得担心「漏洞末日」把整个局域网都打穿。**robwayne** 和 **Marsymars** 的理由是可用性：停电断网或者出门两周时家里的服务器掉线了，没法修。
- **cubesnooper** 详细解释了不在家用 IP 上直接托管的原因：通过 IP 和 TLS 证书域名能轻易查到你是谁、通过 ISP 能查到你住哪；被 DDoS 时全家断网；住宅宽带往往没有固定 IP 甚至是 CGNAT。他现在把服务都迁回家中硬件，三台 VPS 只负责通过 WireGuard 转发加密流量。
- **wraptile** 认为 VPS 已不太适合自托管：两台 250 美元的翻新 32GB 联想笔记本，配 Tailscale 和 Cloudflare Tunnel 就够了，全部用 NixOS 管理，由 Claude Code 修改配置，但禁止它直接执行 `nixos-rebuild switch`，每次都人工审查。**trollbridge** 则刚把业务从一台 25 美元的 VPS 上撤下来：稳定运行 6 年后宕机，控制台登不上，客服好几个小时才处理。
- **massysett** 泼了冷水：他也搭过 ThinkCentre 家用服务器，解决了一些问题，也带来了新问题——又多一台要更新、要做安全加固的机器。他最终认为不值得，用 Arq 备份到 B2 等专业服务加 U 盘拷贝反而更省事，「也许你一直以来的想法是对的：在笔记本上跑就行了」。

**省钱与极限压榨**

- **throwaway81523** 说按 lowendspirit 的标准，5 美元一个月已经算高端了：他的个人网站跑在一台每年 4 美元的 VPS 上，tierhive 甚至有每月 10 美分的 128MB 纯 IPv6 小机器。**oefrha** 说去 LowEndTalk 看看，Hetzner 都显得贵，但要能容忍一些不靠谱；**stavros** 补充 Hetzner 是「可信赖选项里最便宜的」。**masfuerte** 用一台每年 15 美元、128MB 内存的 VPS 跑着十个人用的 Matrix 服务器。
- **lelanthran** 说人们低估了 5 美元 VPS 的能力，是因为用的是 Java、C#、Python、PHP 写的东西：他曾在一台上同时跑 Gitea、主页和服务 5000 用户的 B2B SaaS，也曾一夜之间让几十万台设备通过它下载镜像更新，峰值约 1 万并发；可同一台机器后来连一个 Jenkins 都带不动。**assimpleaspossi** 每月 2.03 美元，跑着七个剧团网站（含售票）、五家餐厅的在线点餐和个人邮件。
- **tombert** 用 Oracle「永久免费」实例（4 核 24GB）跑 Minecraft 服务器；**j027** 指出最近免费额度已砍到 2 核 12GB，**rstat1** 说切换成按量付费账户后仍可用到 4 核 24GB。

**有意思的个人用例**

- **mycofunguy** 用 OpenBSD 跑一个无 JS、无账户、无 Cookie 的蘑菇产品网店：购物车存在 URL 里，Go 后端用 pledge/unveil 加固，订单用公钥加密存为 JSON，服务器上不存私钥，再由笔记本 SSH 上去收取。
- **droidjj** 是 1 型糖尿病患者，在 VPS 上跑 Nightscout，把血糖仪数据同步到 Pebble 手表，在编程智能体帮助下一小时就搭好了。
- **chr15m** 列出了一长串个人自动化：通过 VPS 把笔记本、手机、家中推理机组成 WireGuard 网络；家庭购物清单、个人仪表盘；根据 git 活动和 RSS 推算「哪个项目被冷落了」的优先级工具；长按电源键就能语音下指令的手机 AI 助手（调用家里以太阳能为主供电的 RTX 3090 推理机）。他感叹有了 LLM，构建和维护这些东西几乎不费力，「现在当个家庭折腾党真是太疯狂了」。
- **nicbou** 做过柏林市民办公室预约查找器，通过 WebSocket 推送空余时段；如今市政部门把预约系统理顺了，他很高兴这个服务可以关掉了。**kidnoodle** 给孩子的 Yoto 播放器做了对讲功能，喊「来吃早饭了！」不用再冲楼上大吼。**petercooper** 曾给妈妈搭了一个只有一个听众的网络电台，配合她的便携收音机使用。
- **weberer** 专门用一台服务器做 Claude Code 开发机，只能通过 tailnet 访问：起因是 Claude Code 有次卡住后开始翻他笔记本的家目录找资源，他觉得这种行为不可接受且吓人，于是让它待在碰不到密钥的机器上。**mindwork** 所在国家正推行类似中国、伊朗的审查和防火墙，他自建的 Jitsi 实例是全天候可用的通话手段；**andOlga** 则为身处审查国家的朋友提供出口节点。
- **mrngld** 用 Miniflux + Flatnotes 搭了每日新闻摘要流水线：自动抓全文、合并相似文章、生成二三十词的摘要，写成 Markdown 摘要页，通过 Tailscale Funnel 随处阅读。
