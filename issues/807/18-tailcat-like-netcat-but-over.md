---
layout: article
title: "Show HN: Tailcat —— 像 netcat，但跑在 Tailscale 的数据平面上"
issue: 807
number: 18
category: show_hn
original_url: "https://github.com/tailscale/tailcat"
hn_url: "https://news.ycombinator.com/item?id=49452990"
date: 2026-08-28
---

## 文章摘要

Tailcat 是 Tailscale 在 2026 年 8 月的 TailscaleUp 大会上开源的一个小工具，自我介绍是一句很妙的口号：「Tailscale without Tailscale, by Tailscale」（由 Tailscale 出品的、不需要 Tailscale 的 Tailscale）。它把 Tailscale 的开源组件重新组装成一个类似 netcat 的命令行工具，但底层跑的是 Tailscale 的**数据平面**（内部代号 magicsock）——两台机器之间建立点对点的 WireGuard 加密隧道，用 DERP 中继作为 NAT 打洞的旁路信道，并在打洞失败时充当最后的兜底转发。区别在于它完全**不使用 Tailscale 的控制平面**：所有连接元数据由你自己带外（out of band）交换，怎么传都行。

用法模型极简：一端跑 tailcat 服务端（监听方），它会打印出一个短短的连接令牌；另一端把这个令牌交给客户端就连上了。全程 WireGuard 端到端加密。你不需要 Tailscale 账号，不需要 root/管理员权限，它不改动路由表、不碰 DNS——纯粹是一个用户态的 Go 库加 CLI。默认可以用官方提供的免费限速 DERP 中继（默认 DERP map 在 tailcat.dev/derpmap.json），也可以完全自己跑 derper。仓库里还有一个把 tailcat 编译成 WebAssembly 的浏览器实验版，可以和 CLI 互传文件和文本（浏览器侧目前只走 DERP 中继，直连要等 WebRTC 支持）。

功能面比 netcat 宽得多。除了最基本的 stdin/stdout 对拷，还能用 `--serve=8080,8443`（或 `--serve=all`）把本地 TCP 端口透过隧道暴露出去；用 `--serve=no-auth-ssh` 直接跑一个免认证 SSH 服务端（想要认证就 `--serve=22` 反代系统 SSH）；`ping --until-direct` 探测连通性并报告每个响应是走 DERP 还是直连路径；`socks` 子命令通过隧道起一个 SOCKS5 代理跑任意命令，而且令牌本身可以当 URL 主机名用；`--serve=exit-node` 让客户端把服务端所在网络当出口节点用。`parse` 和 `resolve` 两个子命令则用于查看和展开令牌内容。

密钥管理的设计尤其值得一看，README 把安全语义讲得很清楚。默认是**临时密钥**：每次启动生成一把内存里的新密钥，打印一个从未有人见过的地址，进程退出后密钥丢弃、地址永久作废——所以分享这个地址只会指向那一次运行，这是安全的默认值。另一种是 `tailcat genkey` 生成的**保存密钥**：地址跨重启稳定，代价是任何你曾经分享过这个地址的人都能连上未来任何一次用同一把密钥起的服务，除非用 `--allow` 限制允许的客户端。CLI 启动时会明确告诉你用的是哪一种。令牌还可以发布成 DNS TXT 记录，之后凡是接受令牌的地方都能直接写域名。

README 里最漂亮的例子是「通过 DNS 暴露的受保护 SSH 服务」：客户端先 `genkey --client` 生成身份密钥对并把公钥给服务端，服务端用 `genkey --fixed-region` 生成绑定到最近 DERP 区域的密钥，然后 `--serve=22 --allow=nodekey:...` 只对那一个客户端开放，令牌发布成 TXT 记录，客户端只需 `tailcat ssh my-server.example.com`。效果是：服务器上没有任何对外开放的入站端口，不需要端口转发也不需要端口敲门，WireGuard 在 SSH 服务看到任何一个包之前就完成了客户端认证；其他人的握手会被静默忽略，他们甚至无法得知这里跑着一个 SSH 服务。

技术栈层面，它复用了 Tailscale 客户端的四块组件：用户态 WireGuard（不用内核 TUN/TAP，所以不需要 root）、magicsock（基于 STUN 的端点发现和 UDP 打洞，在直连 UDP 和 DERP 之间多路复用）、gVisor 的用户态 TCP/IP 协议栈 Netstack（这是它能在不做任何操作系统网络配置的前提下收发 TCP 连接的关键）、以及 DERP 中继协议。连接流程被拆成六步：服务端起来、连 DERP、打印令牌；客户端解析令牌拿到公钥和区域，生成临时密钥对连同一个 DERP；客户端通过 DERP 发一条名为「**Meow**」的发现握手消息（携带自己的节点公钥），服务端把它加进 WireGuard peer 列表并重配引擎，回一条「**Meowed**」；WireGuard 标准握手完成、隧道打通；与此同时双方通过 DERP 互相通告 UDP 端点（STUN 学到的公网地址加本地接口地址）跑 disco 打洞协议，成功就升级为直连，失败就继续走 DERP；最后由 gVisor 在两端处理 TCP 连接建立，服务端按端口把入站连接分派给不同的处理器。连接令牌本身是 `tc` 前缀加 base64 编码的 CBOR，装着服务端的 Curve25519 公钥和 DERP 信息，只引用区域 ID 时约 50 字节。

作者对稳定性的态度很坦白：Tailcat 免费，但不承诺任何 API 或 CLI 稳定性，Go API、CLI 参数、输出格式和线格式都可能改；公共 DERP 中继没有 SLA 也没有吞吐目标，可以随时被撤销。README 结尾有一段项目史：它 2023 年 9 月诞生于一趟长途航班上，最初叫「derpcat」，第一次跑通的那条提交信息写着「UA 605 PDX-ORD 飞往爱尔兰途中，爽，没买 wifi」。后来它随 Tailscale 内部演进多次腐烂，最近才被复活并重构成 tailscale.com 仓库的普通 Go module 使用者，而不再是一个 fork。

## HN 评论精华

这条帖子拿到 662 分、129 条评论。作者 Brad Fitzpatrick（memcached、OpenID、Perkeep 的作者，Go 团队老兵，Tailscale 联合创始人之一）全程在场答疑，几条关键澄清都出自他手。讨论主要沿三条线展开：这算不算厂商锁定、和 iroh/magic-wormhole/netbird 等既有方案怎么比、以及一场从 Nix 意外岔到 Minecraft Java 版与基岩版之争的超长离题支线。

- **TZubiri** 抛出了全场最有争议的一条：「现在一半的软件供给都是在零附加价值地兜售厂商锁定然后赚钱」，他认为 Tailscale 的附加价值不过是「省得你去路由器里开端口转发」，在他看来是负价值。**bradfitz** 亲自回应：这里没有任何厂商锁定，不需要付费也不需要账号，「就算 Tailscale 这家公司倒了，只要你跑自己的 DERP 服务器，tailcat 照样能用。它只是开源代码，不是托管服务」。**parasyte** 补充了更技术性的澄清：这不是因为 Tailscale 跟 netcat 不兼容（你完全可以对 tailnet 上的地址 nc），而是把 Tailscale 为别的目的搭起来的基础设施拿来复用——不需要 tailnet 甚至不需要任何账号。**MattCruikshank** 在对方暗示这类项目是「初级工程师刷履历的镜像生态」时给出了那条被顶得很高的回复：作者是 Brad Fitzpatrick，在 Google 干了 12 年以上，创造了 memcached、WebSub、OpenID 和 Perkeep，「如果他算初级工程师，那我完全不知道什么算资深了」。
- 「为什么不直接用 WireGuard」这个问题被 **gonzalohm** 提出后，**bradfitz** 一句话说清了产品定位：「WireGuard 不做 NAT 穿透，这才是这东西的主要增量。另外它还提供了一个 CLI 加库，让你在不装内核路由、不要 root 的前提下在 WireGuard 上跑数据流。」**fodkodrasz** 附议说 WireGuard 本身配置极简，麻烦的是 NAT，「我们真该用上 IPv6 了，那样这个工具就基本多余了」。**derkades** 反驳：即便没有 NAT，IPv6 通常也有防火墙挡住入站，同样需要打洞技术。**jcgl** 再反驳：普通 NAT 和 CGNAT 场景下 IPv6 确实帮助很大，打洞在没有 NAT 时简单得多——「你不再需要一个会合服务器来确定端口映射」。
- NAT 穿透到底值不值一个工具，**petcat** 表示怀疑（「会用这类工具的人本来就有一堆绕开 NAT 的办法」），三条回复给了很具体的场景。**9dev** 的画面感最强：靠 Tailscale，他能坐在马拉喀什老城的天台酒吧、用公共 WiFi 连到城市另一头酒店里的笔记本，或者地球另一端戒备森严的生产环境，安全性不打折，所有设备自动选最短的物理链路。**dannyw** 补充：只要你会用手机热点，就需要 NAT 穿透；想要「在各种网络环境下都能直接用」，就必须有 NAT 穿透。
- 大量替代品被拉出来对比。**cpuguy83** 说他读完 Tailscale 那篇 NAT 穿透原理博客后就想做这个，被朋友以 magic-wormhole 已存在为由劝退。**tptacek** 给了最精炼的定位：「这很聪明。它是 Magic Wormhole，但面向通用连接而不只是文件传输。」**doomrobo** 指出 MW 有一个重要区别：它用人类可读的短会话标识符，所以带外信道可以就是一通电话，这也是它需要 PAKE 而 tailcat 只需更简单的密码学的原因。**tptacek** 认为把 PAKE 塞进这个协议不难但收益存疑：Wormhole 的 PAKE 加号牌系统之所以合理，是因为它只做「把文件从 A 挪到 B」这一件很具体的事；而这里你是在起服务，几乎总有非语音信道可用。**MajesticHobo2** 补了一条安全考量：长连接不该让攻击者靠猜错密码就能中断会话。iroh 生态也被反复提及（**megamorf**、**genpfault**、**Arqu** 分别点了 iroh、dumbpipe、pigeons），**colinmarc** 作为 iroh 拥趸做了个公道的澄清：iroh 的打洞算法本身至少部分基于 Tailscale 的，所以说「iroh 先做出来」并不完全准确，准确的只是「作为不带控制平面的库」这一点。另外还有人提到 wush、bitbang-cli、netbird、openziti、zerotier、Nebula。
- 关于公共 DERP 中继的政策，**ipdashc** 问 Tailscale 是否介意非客户用它们的中继。**bradfitz** 的回答透露了一条公司层面的立场：「这是我们 CEO Avery 六年半以来一贯的主张——我们应当为公共利益在互联网上运行 DERP 服务器（限速的）。」但他强调这是一支独立的机群（tailcat.dev/derpmap.json），DNS 和 SNI 里都不出现 tailscale.com。**gz5** 追问既然目标是去专有化，为何不连 DERP 也彻底交出去，bradfitz 回复 derper 本来就是开源的、tailcat 可以用你自己跑的任何 DERP 服务器，并当场往 README 里补了一节更明确的文档。
- **1vuio0pswjnm7** 提了一条有针对性的批评：Tailscale 的 derper 文档实际上在**劝退**用户自建（原文写着「绝大多数用户不需要也不会想运行这份代码」，还强调运行 DERP 需要多层网络与应用诊断的专业能力、没有明文或开放模式），而 Nebula 这类项目反过来鼓励用户自建会合节点（lighthouse）。他还认为「像 netcat」的类比站不住：原版 netcat 没有任何第三方依赖，也没有复杂到能和一家卖 SaaS 的公司挂上钩。**sfllaw**（Tailscale 员工）的回应值得一读：「在 Tailscale，我们并不*想要*流量走我们的服务器。我们想要的是 Tailscale 无需你搭建任何基础设施就能直接用。」他解释 DERP 是最后兜底，客户端会拼命尝试直连；确实需要自建的用户可以用负担轻得多的 peer relay 而不是完整 DERP 服务器；那段劝退文字之所以存在，是因为总有人误以为自建 DERP 是使用 Tailscale 的硬性前提。
- **spockz** 和 **forrestthewoods** 都被术语绕晕了——后者很坦率地问「什么叫数据平面？什么叫控制平面？我是真的不懂这两个词的意思」。**zrail** 给了最清晰的技术解释：网络层包在 Tailscale 守护进程的 magicsock 外面，也就是负责 DERP 和 NAT 打洞的那一层；tailcat 搭了一个假的控制平面，通过 DERP 做单向密钥交换（就是 Meow 消息），然后双方跑常规的 CallMeMaybe 流程建连。**tomxor** 的概括更精炼：「这最好被理解成一次性的控制平面」——单次使用的地址即密钥，带外分享，比 Tailscale 真正的控制平面精简太多；而 Tailscale 卖的是带认证和 ACL 管理的完整控制平面，所以这东西并不构成竞争。
- **aseipp** 说他前一天还在抱怨想从办公室的 tailnet SSH 回家里的网络，为此写了个基于 iroh 的一次性工具，bradfitz 直接回了句「README 里就有一个例子做的正是这件事 :)」。这条支线最有价值的产出是 **maisem**（Tailscale 员工）掏出的 [tailmix](https://github.com/maisem/tailmix)：它起两个 tsnet 服务连到两个不同的 tailnet，通过一个共享 TUN 设备暴露出去，自动给两边主机分配不冲突的 IP 并做租约防止误重用，MagicDNS 也会返回「有效 IP」——因为要支持 macOS 所以不能依赖网络命名空间。**linsomniac** 说他前一天刚好在想这个，并直接开了一个新帖来讨论。
- **mrsssnake** 提出了一个更高层的观察：这类工具暴露出当今互联网形态的大问题——「一切本该用纯 netcat 加 IP 协议栈就能做到。有人要问 NAT 打洞、加密、静态标识、权限，是的，这正是互联网所缺的，也是每个 P2P 应用一遍遍重新发明它的原因。」**MajesticHobo2** 的回复引用了端到端原则：「notabug wontfix；这就是端到端原则在起作用，这些东西请自备。」**pbohun** 说得更直白：「如果我们有 100% 的 IPv6（且没有 CGNAT），本来根本不需要这东西……我觉得人们低估了如果 P2P 变得轻而易举会带来多少创新。」
- **LoganDark** 提了个安全担忧：恶意软件会不会拿这个做 C&C，黑客最喜欢难以逐个封杀的通信信道。**MajesticHobo2** 认为限速至少会让它难以支撑大型僵尸网络。
- bradfitz 自己贡献了一个有趣用例：同事用 tailcat 当传输层做了一个 Minecraft mod（tailscale/tailcat-for-minecraft），「只是个可爱的演示，不打算发布或维护」。这条评论意外引爆了全场最长的离题支线——从「没有联机订阅的主机玩家能不能靠这个连自建服务器」一路吵到 Java 版与基岩版的优劣、GeyserMC、用 Rust 写的 Minecraft 服务端实现，以及 **petterroea** 那段关于「微软本可以用 .NET 化的基岩版打赢操作系统军备竞赛却没做成」的长篇分析。
- 另有一支由「为什么仓库带 Nix flake」引发的支线，**bradfitz** 澄清 Nix 不是 Tailscale 的标准开发环境（只有部分人用），公司基本也不怎么用 Docker，「大多数时候就是 go test」。**mikepurvis** 借机写了一段颇有说服力的 Nix flake 推销：flake 引用直接继承 VCS ref，所以 `nix run github:tailscale/tailcat/my-fancy-branch` 就能让同事跑上你的分支，依赖也能同样灵活地钉住，不必像 Debian 那样搞出 `1.2.3~actually.1.4.6` 这种扭曲版本号。
- 最后是命名彩蛋。**1970-01-01** 说既然 `cattail`（香蒲）这个名字还没被占，只能认为他们错过了一次玩梗机会。**kemotep** 认为这很符合传统：powercat、socat、cryptcat 都是这么来的。
