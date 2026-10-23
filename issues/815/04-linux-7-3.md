---
layout: article
title: "Linux 7.3 发布"
issue: 815
number: 4
category: favorites
original_url: "https://lwn.net/Articles/1101143/"
hn_url: "https://news.ycombinator.com/item?id=50149392"
date: 2026-10-23
---

## 文章摘要

2026 年 10 月 18 日，Linus Torvalds 按计划发布了 Linux 7.3 内核，LWN 的 Jonathan Corbet 随即发了一则简讯。Torvalds 在发布邮件里自嘲，这周他总算记住了今天星期几，所以发布没有莫名推迟；最后一周风平浪静，7.4 的合并窗口次日开启，不过他正在年度家庭度假，打算上午出海潜水、下午才回来合并代码，接下来一段时间作息会有点不规律。

Corbet 说这是一个「全方位破纪录」的版本：共有 18765 个非合并提交，来自 3021 名开发者，其中 807 人是首次向内核贡献代码。早在 rc1 时，合并进主线的变更就达到 15267 个，是历史上第二高的 rc1 提交数，仅次于带入近 3000 个 bcachefs 历史提交的 6.7-rc1。LWN 在合并窗口总结中还提到，7.3 预计将成为长期支持版本。

结合 LWN 的两篇合并窗口总结，这个版本的主要变化有：

- **binfmt_misc 大幅扩展。** 这个用来为可执行文件配置解释器的机制新增了 BPF 钩子，可以在运行时决定用哪个解释器，为「封闭式二进制」（hermetic binaries）等用法打开了空间。
- **组调度重做。** 以往一个任务的权重会按其控制组在该 CPU 上运行的比例缩放，在大型多核系统上缩放因子可能小到失真，甚至逼近整数表示的极限。调度器维护者 Peter Zijlstra 为此新增了 `cgroup_mode` 调节项，提供 smp（旧行为）、up、max、concur、tasks 等模式，默认改为 concur，即低争用时接近旧行为、可运行任务数接近 CPU 数时趋近 max。这意味着 7.3 调度控制组的方式与以前不同，提交说明直言「改了默认值，希望帮到的人比伤到的人多」。同时控制组改用单一运行队列，以降低开销和延迟。
- **failfs 和 fchroot()。** 新的 failfs 文件系统会让一切操作失败。进程如果把根目录或工作目录放进 failfs，就只能基于已经打开的文件描述符做路径查找，绝对路径、绝对符号链接和相对当前目录的查找统统失败，从而彻底甩掉文件系统状态，主要用于沙箱。配套新增了接受文件描述符的 `fchroot()` 系统调用，以及可以传给 `fchdir()` 的特殊值 `FD_FAILFS_ROOT`。
- **BPF。** BPF 程序可以使用全局 per-CPU 数据；验证器报错信息更详细；arena 指针传参更方便；新 kfunc 可以读取 socket inode 的扩展属性。
- **sched_ext 子调度器完成。** 根 BPF 调度器现在可以把一个 cgroup 子树连同可撤销的 CPU 授权交给嵌套的子调度器，由后者全权负责这些任务的调度。
- **虚拟机工作集跟踪。** LWN 称这可能是迄今合入的最大一块由 LLM 辅助编写的核心代码。
- **文件系统。** ntfs3 支持备用数据流（alternate data streams）；内核态 SMB 服务器 ksmbd 迎来一长串改进，其中包括可选的 Apple Time Machine 支持；overlayfs 支持 id 映射挂载；移除了 SGI 早期使用的 EFS 和 SCO UnixWare 的 FreeVxFS 两个无人维护的老文件系统。
- **架构与其他。** 一批 32 位 Arm CPU 被标记为弃用，7.3 之后可能移除；PowerPC 获得 Rust 支持；x86 移除了「SMP alternatives」代码（单处理器启动时打补丁去掉锁指令的老功能）；s390 获得新的 cpuidle 子系统和基于 Clang 的内核控制流完整性；内核线程改住在独立的 nullfs 中；iomap 层大改；Rust 支持继续增加 RCU、内存屏障、关中断自旋锁等抽象。

LWN 文末附上了合并窗口两篇总结和 KernelNewbies 的 7.3 页面供查阅细节。文章评论区里有读者打趣：合并窗口开启前，先是 Linus 的「潜水窗口」（submerge window），被提名为本周金句。

## HN 评论精华

本帖获得 632 分、约 250 条评论。讨论主要围绕 failfs 的用途、内核态 SMB 服务器与 Time Machine、创纪录的提交量与 AI 辅助代码，以及 Apple Silicon 支持，其中有相当长一段跑题到给家人选什么操作系统。

**failfs 是做什么的。** mdspan 问 failfs 是不是只用于测试，rustyhancock 和 rwmj 解释它用于沙箱：chroot 进 failfs 的进程无法再打开新文件，只能使用已持有的文件描述符。saint-evan 描绘了典型用法：静态链接的进程只带预分配的描述符进入 failfs，再配合默认拒绝的防火墙，被攻破后也无法读取本地文件或启动 shell，成了一根纯粹的数据管道。rwmj 却不以为然：「我们想要的是 capabilities，结果得到一堆缺乏设计一致性、稍不小心就会被绕过的系统调用」，nesarkvechnep 也感叹 Linux 宁可做什么都不采用 FreeBSD 的 Capsicum；jabl 反问在现有内核里怎么不破坏兼容地引入能力安全模型。setheron 提到这个想法源自 Nix 核心贡献者提出的「空命名空间」，用于让细粒度沙箱构建更廉价。zero_k 希望有个随机失败的「havocfs」来测试数据库的健壮性，rwmj 和 tosti 指出 nbdkit 的 error/evil 过滤器和 device-mapper 的 dm-flakey 已经能在块设备层做到。顺带一提，saint-evan 刻意把 you 写成「yu」，好几位读者说一开始还以为是某个 Linux 命令。

**内核里居然有 SMB 服务器。** 不少人（sippingabonedry、spwa4、franga2000）这才知道 ksmbd 的存在，tecleandor 说它 2021 年就进了内核。为什么放进内核？vbezhenar 说为了性能，kasabali 和 AshamedCaptain 一针见血：因为 Samba 是 GPLv3。ahartmetz 认为用户态守护进程用一个核就能跑满大多数网络链路，而 ksmbd 一旦被攻破就是整机沦陷；loeg 和 14u2c 反驳在 200Gbps 和 RDMA 场景下单核远远不够。theden 关注新增的 Time Machine 支持，并引用 Phoronix 的说法抱怨这得靠逆向工程；drdexebtjl 纠正说 Apple 在 GitHub 上以 BSD 许可公开了 SMB 客户端源码，补丁本身也没说是逆向。关于 Time Machine 走 SMB 好不好用，robin_reala 和 windowsrookie 说在群晖 NAS 上用得不错，tonyedgecombe 则说每次听到有人抱怨 Time Machine，最后都发现是在走 SMB；alt227 解释 Apple 既不愿付微软授权、又因 GPL 弃用 Samba，只好自己实现 SMBX，因此问题不断。

**创纪录的提交数与 AI。** INTPenis 认为这么多提交和新人里有大量 AI 辅助补丁，「不是批评，只是观察」。sedan_baklazhan 讽刺大家曾嘲笑的代码行数指标换成了提交数指标，machomaster 回应这类统计每个版本都会公布，并非因 AI 才开始。leonidasrup 贴出内核的 AI 编码助手政策：可以随意使用、需要披露、提交者本人对质量负责；machomaster 和 shakna 指出开发者已投票取消强制的「Assisted-by」署名，相关补丁讨论仍有争议，LWN 主编 corbet 亲自出来说文档更新已进入 linux-next，几天内就会进主线。pbhjpbhj 觉得取消署名令人意外，也丢失了「哪个模型写的代码 bug 最少」这样的有用信号；xboxnolifes 则认为反正总有人撒谎，这个标签只会引发猎巫。palata 说作为维护者，他只按补丁质量和沟通是否顺畅来评判，不关心代码出自谁之手；Verdex 担心有人可以用几十个马甲慢慢积累信任，最终掌控某个子系统。bityard 解释「对提交负责」在实践中就是：别人说这里不行，你要么修，要么辩护，要么放弃。VCFundedGenYer 和 rvz 则对 Torvalds 近来自己用 LLM 写补丁并引入竞态条件颇有微词。

**第一次提交补丁。** Lanzaa 分享说自己的第一个内核补丁就在这个版本里：罗技 G915 TKL 键盘通过 Lightspeed 接收器连接时电量不显示，AI 帮他理清了「内核到 UPower 到 xfce4 电源管理器」的信息链并定位到问题，根因是这个 USB 设备 ID 没有关联到正确驱动，补丁本身只是几行映射。jonkoops 认为这正是 AI 的长处：快速获得原本要花很长时间积累的系统知识。

**硬件支持。** djfergus 惊叹 Apple M3 Pro/Max/Ultra 的设备树支持进展之快，cromka 认为这些芯片已经好几年了，GPU 和雷雳仍不支持，算不上快；palata 为 Asahi 团队辩护，认为没有任何文档能做到这样已很了不起。在 Morse Micro 工作的 wryun 高兴地指出，7.3 包含第一个使用新 Wi-Fi HaLow（900MHz 频段 802.11ah）支持的驱动，hostapd 和 wpa_supplicant 的支持正在上游化。clhodapp 提到这个版本可能让在 Linux 上开发可重定位的 Nix 包成为可能。skybrian 贴出图表称修复的 CVE 数量创下新高。pdmccormick 问 7.x 什么时候出 LTS，tleb_ 解释惯例是每年最后一个版本成为 LTS，但 Greg Kroah-Hartman 有意不把它写成官方承诺。

**跑题。** 由 Time Machine 引出的家庭 IT 支持话题占了不少篇幅：teekert 说他父亲用 iPad 当电脑问题不断，brnt 认为 Linux 对不懂技术的家人来说是「装好即忘」的系统，jorvi 和 pjmlp 则表示迟早会遇到需要终端的问题，pjmlp 的父母甚至背着他去电脑店重装了 Windows。
