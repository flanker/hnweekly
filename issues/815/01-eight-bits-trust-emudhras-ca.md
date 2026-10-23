---
layout: article
title: "八比特的信任：eMudhra 的 CA 私钥几毫秒就被破解"
issue: 815
number: 1
category: favorites
original_url: "https://scotthelme.co.uk/eight-bits-of-trust-emudhras-ca-keys-cracked-in-milliseconds/"
hn_url: "https://news.ycombinator.com/item?id=50177916"
date: 2026-10-23
---

## 文章摘要

安全研究员 Scott Helme 开篇就抛出一个让人难以置信的事实：印度证书颁发机构 eMudhra 的一把 CA 私钥就是数字 4。不是以 4 开头的长串，也不是 4 后面跟着几百位随机数，就是 4。另外几把分别是 56、115 等，他在自己的笔记本上从公开证书里把它们全部恢复出来，用时不到一秒。

**事件经过。** 10 月 17 日（周六），一位安全研究员向 Apple、Google、微软、Mozilla 和 eMudhra 报告：eMudhra 的 8 张中间证书私钥已经泄露。Chrome 当晚通过 CRLSet 更新封禁了它们，Mozilla 几小时后通过 OneCRL 在 Firefox 中封禁。20 日（周二），Mozilla 公开宣布此事并发布 NSS 更新移除 eMudhra 根证书，Chrome 也确认将在 Chrome Root Store v43 中移除该根。这 8 张中间证书包括 6 张 TLS 签发 CA（DV、OV、EV 各两代）和 2 张代码签名 CA，都创建于 2025 年 4 月，并在 2025 年 5 月全部获得 Sectigo「AAA Certificate Services」根证书的交叉签名。Ian Carroll 在邮件列表中一句话概括了问题：这些是任何人都能破解的单字节弱密钥，而且问题无法被控制住。

**私钥到底是什么。** 这些是 P-384 曲线上的椭圆曲线密钥。私钥就是一个数 d，公钥是 d × G，G 是所有人共用的曲线基点。从 d 算公钥很容易，反过来几乎不可能，但前提是 d 足够大：正常的 P-384 私钥应是一个随机的 384 位数，约有 10¹¹⁵ 种可能。而 eMudhra 的 8 把密钥都是 47 个字节的零加上最后一个字节，只有 256 种可能，"搜索"就是依次计算 1×G、2×G……直到与证书里的公钥匹配，最多 255 步。作者从 Mozilla 附上的 16 张证书（8 把密钥，每把分别由 eMudhra 和 Sectigo 各签一次）中逐一暴力恢复出 d 值：156、230、115、56、155、4、66、237。值得注意的是，这些证书在结构上毫无异常，d=4 对应的公钥是曲线上合法的点，所有格式检查都能通过，唯一的发现方法就是去试。

**为什么"无法控制"。** 通常 CA 私钥泄露意味着被人偷了，有窃贼、有时间点，可以推断谁拿到了密钥。而这次没人需要偷，从证书存在的第一天起密钥实际上就是公开的。自 2025 年 4 月以来任何看过这些证书的人都可能在普通硬件上几毫秒内恢复密钥，这个窗口长达 17 个月，而且无从知道是否有人这么做过。Chrome 指出，由于它强制要求证书透明度（CT），而 CT 日志中从未出现过链到这些 CA 的 TLS 证书，所以伪造证书不可能默认被 Chrome 信任。但 CT 只能发现误签而无法阻止，大量软件根本不检查 CT。

**谁仍然暴露在风险中。** 更新后的主流浏览器基本没问题：Chrome 和 Firefox 直接封禁了这些中间证书且都要求 CT，Safari 也要求 CT。但绝大多数非浏览器 TLS 客户端，如 curl、Python、Node、Go、Java、git、各种包管理器和后台脚本，默认信任磁盘上的 CA 包、不检查吊销、不检查 CT，所以 eMudhra 和 Sectigo 吊销中间证书对它们毫无作用。作者列举了高风险场景：`curl | sh` 安装脚本和自更新程序（伪造证书意味着以 root 运行攻击者代码）、持有云密钥和签名密钥的 CI/CD 流水线、支付和 OAuth 等服务器间 API 调用、根证书包被固化到 2039 年的 IoT/嵌入式/工控设备，以及 Windows 代码签名（用这两张代码签名 CA 签的恶意软件在不检查吊销的环境中看起来就是正规软件）。他在本地用伪造的 api.stripe.com 证书测试：curl 显示验证通过，Python 完成握手，Node 返回 authorized: true，所有测试的 Linux 和 macOS 机器均如此；Windows 的 CryptoAPI 在未开启吊销检查时同样接受。

**该怎么办。** 能更新的设备就更新：系统 ca-certificates、certifi、Node、Homebrew，并在基础镜像跟进后重建容器镜像。真正的难题是无法更新的设备，路由器、打印机、电视等固件冻结的设备会一直信任这些证书直到 2039 年 7 月过期。

**后续更新。** 文章发布后 Mozilla 公开了事件工单，eMudhra 给出解释：密钥生成仪式所用的脚本有缺陷，在 HSM 上额外生成了一对弱密钥，由于名称相似，人为失误导致用错了密钥去生成 CSR。作者还补充说，他用 AI 代理一条提示词就让 Claude 恢复出了这些 CA 私钥。

## HN 评论精华

本帖 934 分、约 450 条评论。前一天已有一个同主题帖子（「eMudhra intermediate CA compromise」，947 分、400 多条评论），本帖讨论主要集中在密钥为何如此之弱、是蠢还是坏、CA 体系的结构性缺陷，以及非浏览器客户端和 IoT 的长尾风险，夹杂大量玩笑。

**密钥是怎么变成这样的。** muvlon 猜测要么是序列号被误当成私钥，要么是安全生成的密钥被意外截断成 8 位（比如往清零的缓冲区里只写了一个 char）；他做了蒙特卡罗模拟，8 个随机字节里出现两个连续数（G3B 中 155、156 就是连续的）的概率只有约 18%。account42 发现所有数字的各位数字在键盘上都是从左到右排列的，怀疑是人手"随机"输入，muvlon 算出 256 个值中只有 146 个满足这一性质，连续 8 次都满足的概率约 1%。mminer237 引用了 eMudhra 在 Bugzilla 上的说明：当天为测试密钥封装/解封（wrap/unwrap）引入了一个经批准的流程偏差，脚本缺陷在 HSM 上又生成了第二对较弱的密钥，因标签相似而被误用于生成 CSR，并发给了 Sectigo 交叉签名。benmmurphy 认为这说明 HSM 竟能生成小范围的密钥，"这种功能根本不该存在"。karel-3d 补充说这些密钥原本要导出到美国和欧洲（出于主权原因），HSM 本身没问题，出问题的是封装/解封流程；consp 说他吃过苦头，某些 HSM 允许多个槽位存同名密钥，只给名字时默认选第一个。

**是愚蠢还是恶意。** unsupp0rted、sscaryterry 等人怀疑这是故意留下的、带有"可推诿性"的后门。反方占多数：jopsen、swiftcoder、yoavm、hannob 都指出 CA 本来就持有私钥，想给人留后门直接共享一把强密钥即可，"如果我要逼你把密码告诉我，没理由让你把密码设成 4"；edent 说若真是故意的，会选一个看似随机但自己可控的值。bombcar 则反驳：泄露真实密钥很明显，弄个"失误"更容易推脱。missinglugnut 说这家公司事发前估值约 5 亿美元，现在可能一文不值，本来雇一个称职的人就能避免。

**是怎么被发现的。** john_strinlai 转述原报告人 Wayne 的邮件：用的就是开箱即用的 Google 开源工具 paranoid_crypto，原本只想先对 CCADB 语料做个快速测试，他又全面检查了 CCADB 中所有现存证书，没有更多命中，"大部分时间都花在检查那些大家都以为别人早就做过的基础项上，这在安全领域是致命错误"。renhanxue 调侃该仓库今年 8 月刚被归档。kyleomalley 说自己已测试了大部分历史根证书，并对 CT 中新出现的 CA 证书做自动检测，他把这项检查称为"行李箱密码熵"。atanasi 提到报告中说可以用小步大步（baby-step giant-step）离散对数算法几乎瞬间恢复。

**CT 能不能兜底。** krackers 问私钥泄露后 CT 有什么用，作者 Scott_Helme_ 本人解释：攻击者自签的证书若没提交 CT 日志就拿不到 SCT，现代浏览器不会接受；"我们不能说攻击者没签发过证书，只能说没有现代浏览器会接受的那种"。akerl_ 认为在有 CT 和 CAA 的世界里单个 CA 失守影响有限，Scott_Helme_ 回应很难想象所有依赖 TLS 的设备都能做 SCT 验证，而且"没有群体免疫，只要你这个客户端不检查，就会中招"。他还指出 CAA 只在签发时由 CA 检查，客户端并不使用。

**CA 体系本身的问题。** jlmorton 说"回头看，信任 100 个 CA 可能太多了"，Scott_Helme_ 纠正：被至少一家主要运营方信任的根证书近 350 个，算上中间证书超过 3600 个。josephg 主张把证书放进 DNS、取消 CA，引出一长串关于 DNSSEC 的争论：muvlon 引用 APNIC 数据称只有约 1% 的 DNS 查询经过 DNSSEC 验证，tptacek 认为 DANE 的前提是信任 .COM 等 TLD 运营方，而这并不可靠，许多国家的政府对热门顶级域拥有法定控制权。amluto 建议让 curl、Python 像 Chrome 一样检查 CT，并对 CA 强制名称约束；Scott_Helme_ 认为名称约束不现实，tptacek 指出 eMudhra 并非单纯的印度主权 CA，其子公司为大量美国大学签发证书。oofbey 认为问题出在审计：CA 只需证明用 FIPS 模块生成了密钥，之后脚本怎么处理却没人严格检查。

**长尾风险和"意外收获"。** panarky 提醒 pip、Node、Bun 自带证书包，用 Bun 编译的 Claude Code 之类二进制也打包了证书，Scott_Helme_ 确认只能等这些工具自己发布更新。supernes 提到 Flatpak 和容器不继承宿主机证书库。mschuster91 说许多 Java 应用自带 keystore.jks。另一方面，irusensei、reisse、kristofferR 等人发现这对越狱 IoT 设备是天赐良机：已有人借此 MITM 了 Yandex 智能音箱，kristofferR 说他抓到了两款此前越狱失败的 ESP32 设备的固件，cromka 说自家 Google 设备还没更新证书链，已经能嗅探全部流量。closetheloopdev 反过来提议在本地网络架设代理保护 IoT 设备，Scott_Helme_ 认为可行。mnx 做了测试站点 test.key4.lol 供大家检查设备是否仍受影响。

**关于文风。** TazeTSchnitzel 抱怨文章有 AI 腔，stratos123 用 Pangram 检测认为部分段落是 AI 生成。Scott_Helme_ 回应全文由他本人撰写，最后用 Claude 做了一遍修改，"我愿意为文中每个说法负责，有错请指出"。hypfer 则认为前一天的讨论更有料，这篇只是在说"数字很小"。

**段子。** 评论区充满了 xkcd 221（"随机数是 4，经过公平掷骰选出"）的引用；bombcar 说"当然好破，4 连质数都不是"，DavidSJ 接梗说他们可能本想用梅森素数 2²−1 结果打错了；walrus01 引用《太空炮弹》"这和我行李箱的密码一样"；PalmPilotProMax 称之为"手工精酿密码学，手挑私钥、有机采摘的随机数"。
