---
layout: article
title: "Ask HN：在 macOS 27 上真的无法彻底关闭 Siri 吗？"
issue: 811
number: 15
category: ask_hn
original_url: "https://news.ycombinator.com/item?id=49786609"
hn_url: "https://news.ycombinator.com/item?id=49786609"
date: 2026-09-25
---

## 文章摘要

这是一则 Ask HN 求助帖，发帖人 semidror 描述了自己在 macOS 27 上试图彻底关闭 Siri 的经历，帖子获得 153 分。

他的操作步骤相当彻底：先在"系统设置"里关闭 Siri；然后在"设置 > 屏幕使用时间"中进一步禁用 Siri 及其扩展；接着参照 GitHub 上的 mac-os-debloat 脚本（OleksandrKrupko/mac-os-debloat），把所有与 Siri、Apple Intelligence 相关的系统服务都禁用了。然而重启之后，活动监视器里依然能看到一个名为"Siri"的进程在运行，进一步检查发现，它对应的是一个叫"Siri AI.app"的程序。

他指出，这些"功能"加上苹果的"个人情境"（Personal Context），听起来和微软饱受争议的 Recall 非常相似。他的问题是：有没有人遇到同样的情况？在 macOS 27 上，真的不可能完全关闭 Siri、Siri AI、Apple Intelligence 以及所有相关服务和功能吗？

在后续回复中，semidror 补充了更多细节：他并没有选择"Siri 经典版"，而是在屏幕使用时间里选了"不允许 Siri"，但无济于事；他手动对 debloat 脚本中所有 Siri 与 AI 相关服务执行了 `launchctl bootout` 和 `launchctl disable`，mediaanalysisd、photoanalysisd 等进程确实都停了，唯独"Siri AI.app"重启后照样运行；这个 app 位于只读系统卷上，无法删除，他在考虑是否要进恢复模式把系统分区挂载为可写再删掉它。他还先把 Siri 语言设成法语、系统语言保持英语（英国）——这是让 Apple Intelligence 无法启用的老办法——但那个进程依然在跑。

讨论最终给出了一个很可能的解释：在 macOS 27 中，显示 Spotlight 搜索界面的进程已经改名叫"Siri"了（此前叫"Spotlight"），也就是说 Spotlight 很可能已被新的 Siri AI 整合或替代。这让发帖人更担心：即便 Siri 和 Apple Intelligence 显示为已关闭，苹果是否仍在后台持续构建"个人情境"。

## HN 评论精华

帖子有 87 条评论。大家并没有给出彻底关闭的方法，但技术性的排查相当有价值，另外还有不少对 macOS 走向的集体吐槽。

**谜底：那个"Siri"进程其实是 Spotlight。** galad87 指出，macOS 27 里负责显示 Spotlight 界面的进程现在就叫"Siri"，以前叫"Spotlight"。kccqzy 验证了这一点：向这个进程发送 SIGKILL，菜单栏里的 Spotlight 界面会闪一下。他此前也确认了发帖人的发现——设置界面显示 Siri 已关闭并提示可以开启，但进程依然在运行；而且这个进程会根据界面认为 Siri 是否开启来改名，可能叫"Siri"也可能叫"Siri AI"，但其实是同一个可执行文件。quesera 补充说，同一个二进制根据启动时的名字表现出不同行为并不罕见，两者可能在行为和依赖上确实不同。cosmotic 讲了一个 Windows 上的前车之鉴：几年前他千方百计卸载了 Cortana，结果开始菜单的搜索索引从此再也不更新了，怎么重建都没用，重装 Cortana 才恢复——暗示强行删除这类组件可能会连带破坏搜索功能。Someone 则问了一个很实际的问题：那个进程到底在干什么，CPU 和内存占用多少？

**可以尝试的办法。** Nata_Elisym 提到在屏幕使用时间的"内容与隐私"中可以把 Siri 切换为"Siri 经典版"，去掉 Apple Intelligence 支撑的行为（发帖人表示他选了更彻底的"不允许 Siri"也没用）。wpm 建议禁用相关的 launchd 任务，并提醒最后很可能要关掉 SIP（系统完整性保护）——他自己就是为了阻止 mediaanalysisd 在 2018 款 Mac mini 上狂吃 CPU 而走到这一步的。alsetmusic 说自己正是公司里负责屏蔽 AI 功能的人之一，企业描述文件（configuration profile）肯定能做到；jonathantf2 补充说无需 MDM，用 Apple Configurator 生成 .mobileconfig 文件直接安装即可。Shuddown 贴出一个 Reddit 帖子，楼主称 Apple 客服给了删除 Apple Intelligence 的终端命令，但 crossroadsguy 说在 27 上没用，所有东西都会重新下载；iAMkenough 讽刺道："我很喜欢苹果设计了一个可选功能，却要你关掉 SIP 才能回收存储空间。"pluc 和 throw0101a 则指向了另一个同期帖子《macOS 27：避免下载 AI 模型、节省存储空间的变通方法》。jodrellblank 开玩笑地建议利用坎宁安定律：与其发 Ask HN 问能不能关，不如发个帖子宣称"不可能关"，自然会有人蜂拥而来纠正你。

**磁盘空间才是很多人更在意的问题。** nottorp 指出，问题不只是隐私，本地 AI 模型占了基础款 Mac 大约 10% 的存储空间，而这些机型的空间本来就很紧张。VCFundedGenYer 说它"正在谋杀我的 Mac Mini"。sklargh 痛斥为一个大多数认真用户根本不会碰的功能占用这么多空间是"绝对的傲慢"，toasty228 回了一句："但 Siri/Apple AI 在苹果设备上有 100% 的采用率啊 ;)"dwaite 则提供了一个反向视角：大家说的"Siri"占空间，其实主要是基础模型；这些模型被系统广泛使用，也作为资源开放给第三方 app，苹果的初衷恰恰是避免每个软件各自捆绑小模型——"讽刺的是，它们的存在本是为了帮大家省空间"。

**Siri 本身的口碑。** chuckadams 说自己用 Mac 十年，只在误触时唤起过 Siri；runjake 分享了一个冷知识："根据分析数据，使用 Siri 最主要的方式是'误触激活'。"embedding-shape 抱怨 CarPlay 必须开启 Siri 才能用，而他从不用 Siri，"我已经把它当作第一方恶意软件了"；gumby 解释说苹果是希望你开车时用语音而不是看屏幕，但鉴于 Siri 一贯的表现，这个目标并不现实。threetonesun 说本地/云端切换并不可靠，设一个两分钟的计时器都花了近 30 秒。lloydatkinson 和 selicos 则抱怨 Siri 和 Gemini 在车上连"收藏这首歌"都做不到了，反而比旧版助手倒退。seviu 真诚地发问：到底有谁在 Mac 上用 Siri？使用场景是什么？

**"我还留在 Sequoia"。** 不少人表示拒绝升级：avazhi 说"要我放弃 Sequoia，除非从我冰冷的手中夺走"；nottorp 说他留在 Sequoia 主要是因为新版低对比度界面和看不见的窗口调整区域，AI 占空间倒是次要的；azalemeth 甚至还在用 Mojave。但发帖人 semidror 指出了现实困境：做 iOS 开发的人被迫升级，因为 iPhone 18 Pro 需要 Xcode 27，而 Xcode 27 无法在 Sequoia 上运行——以前直接执行 Xcode.app 内的二进制还能绕过版本检查运行 Xcode 26.6，到 27 就不行了。hurfdurf 提醒，苹果只保证在最新系统版本中修复所有已知安全问题，旧版本并不会得到全部修补。

**更宏观的不满。** pixl97 调侃"苹果好像雇了几个微软的项目经理"，bigstrat2003 解释说这指的是微软多年来无视用户偏好、只做对自己有利的事，mh- 补充了 Windows 那个只有"现在设置"和"稍后提醒"、没有"别再问我"的经典弹窗。Razengan 和 jimbloat 抱怨 27 正式版仍然充满 bug，jimbloat 形容如今的 Mac 是"一辆刹车吱吱响、链条老掉的思维自行车"；Tistron 说他从 2006 年的 Tiger 用到现在，终于要换 ThinkPad 转投 Linux 了。dime 说这是他第一次做出战略性决定，不升级 macOS 和 iOS。
