---
layout: article
title: "Demo Scene 的用户界面"
issue: 803
number: 35
category: design
original_url: "https://www.datagubbe.se/scenegui/"
hn_url: "https://news.ycombinator.com/item?id=49093434"
date: 2026-08-01
---

## 文章摘要

datagubbe 这篇 2026 年 7 月的文章带着明显的怀旧感，专门考察 demo scene（演示场景，一种起源于 1980 年代家用电脑的数字艺术亚文化）自己造的那些工具的界面长什么样。作者的出发点是：demo scene 有着自建工具的悠久传统——有时从零写，有时偷别家的点子甚至代码——而青少年的经验不足、旧习惯、爱做实验的性格，再加上一心想让东西**看起来很酷**的欲望，共同催生了一批相当古怪的用户界面。文中大部分例子来自 Amiga，也涉及 C64、Atari ST/Falcon 和 MS-DOS。

**开场压轴是 Elite Sinus Producer**（Ipec Elite 出品，Amiga 平台）。这里的 sinus 就是正弦。作者顺带解释了 demo 的"实时"到底是什么意思：demo 几乎从不是动画播放器，效果是逐帧由代码算出来的，但为了在 7 MHz（甚至更慢）的 CPU 上完成看似不可能的技术壮举，大量使用"作弊"手段，最常见的就是预计算查找表（precalc）。这类工具就是专门用来生成查找表的。它的界面细节非常有代表性：按 F 键选中菜单项时会**高声播放一段布谷鸟钟的采样**；选中 "Flower" 选项能生成一条漂亮的运动轨迹，可以存成汇编源码；而它的帮助页面离谱到作者不得不录了段短视频——背景一半是移动的蓝色光栅条，另一半在红青之间闪烁，文字则用一种"完全不考虑可读性"的字体呈现。

**文本界面**一节覆盖了汇编器：Amiga 上有大量汇编器，但 scene 一直偏爱 Seka、Asm-One 及其衍生品，版本和魔改（比如 Trash'm-one）多到家族谱系堪比 Unix。这类汇编器总是先问用户要分配多大工作内存，然后进入一个可以检查 RAM 和 CPU 寄存器、加载源文件的命令行模式。接着是 **ripper**（扒取器）：想从游戏里偷采样、精灵图或整首曲子，就需要一个在退出游戏后翻找内存残留数据的工具，Multi-Ripper 是典型代表。还有一类更特殊的 ripper 专门在死机后从内存里找回 Seka 汇编源码——Amiga 和多数家用电脑一样没有内存保护，而 demo 编码是出了名的容易崩溃，勤存盘是惯例，但老手也会忘，运气好的话热启动后还能把代码捞出来。

**音乐 tracker** 是全文最有分量的一节。demo 音乐历来在 tracker 里制作：它不用传统记谱法，而是让音乐人把音符连同各种修饰和效果输入到一个更像程序编辑器的界面里。Amiga 上基于采样的 tracker 家族比汇编器还要枝繁叶茂，但全部源自 Karsten Obarski 1987 年的商业软件 Ultimate Soundtracker——因为收费，很快被 scener 拆解成 NoiseTracker，再改成 ProTracker，之后版本与魔改多到无从追踪。文章还收录了 C64 上的 SoundMonitor 1.0（Chris Huelsbeck 作品，严格说不算 scene 出品也不叫 tracker，但"每个可用声道一条 track"的界面正是 Ultimate Soundtracker 的灵感来源）、Atari ST/e 的 Digicomposer 1.0、MS-DOS 上标志性的 Fasttracker II（支持 Gravis UltraSound、16 位采样、离谱数量的声道，甚至内置一个简版贪吃蛇）、模仿 C64 SID 音色的 Amiga chiptune tracker Abyss' Highest Experience、Atari ST 的 Megatizer，以及反其道而行、改用多窗口界面的 JamCrackerPro。作者对 ProTracker 文件选择器的描述尤其传神：它对熟悉主流 Amiga 程序的人来说"几乎但又不完全"直观，极易误点、误解或干脆漏看东西；最能说明其古怪设计的是那个**竖排的 EXIT 按钮，恰好夹在用于滚动文件列表的上下箭头之间**。

**磁盘拷贝器**一节解释了为什么需要专门软件：AmigaOS 自带的拷贝工具应付不了那些绕过文件系统、直接往软盘磁道写数据的 demo 和游戏。X-Copy 显眼的网格里每个方块对应磁盘的一条磁道并显示拷贝状态，拷完会贴心地"boing"一声。作者个人更偏爱 D-Copy，纯粹因为界面好看，而且它是货真价实的非商业 scene 产品。

**其他工具**部分蜻蜓点水地扫过：Titanics Cruncher（可执行文件的非对称压缩器，省磁盘空间换解压时间）；BBS 时代催生的一批 ANSI/PETSCII 编辑器（Digital Intelligence 的 Ansi-Editor v2.4 界面尤其怪——底部工具栏显示当前颜色，但你**点它没用**，必须用下拉菜单选）；MS-DOS 上的字符集编辑器 Charedit（因为滚动字幕是 BBS 之前 scene 的主要交流媒介，酷的滚动字幕需要酷的字体）；Atari Falcon 的 Motorola 56001 DSP 专用编辑器兼汇编器 DSPdit（用标准 GEM 工具包，外观干净专业）；Amiga 第一个引导块病毒的作者 SCA 后来又做了第一个杀毒工具（专门对付自己写的病毒）；trackmo 制作套件 TrackmoDOS；1990 年代初最流行的 Amiga 磁盘杂志之一 RAW（界面有光泽和纹理，"远早于 Frutiger Aero 成为一个词"，还内置调色板编辑器）；以及 Atari Falcon 的像素画工具 FuckPaint——作者明说，收录它纯粹因为名字，以及那个同样"优雅"的 Analizer 工具。最后他补充：据他所知 scene 从未为 Amiga 做过像素画工具（后来的 Grafx2 移植除外），大概是因为没人觉得有必要——Deluxe Paint 太过统治性，几乎不存在没有一份拷贝的 Amiga 用户。

## HN 评论精华

这条帖子拿了 438 分、70 条评论，讨论几乎完全是老 scener 的集体回忆，非常热闹。

- **weinzierl** 贡献了全帖最好的一条追加案例：C64 磁盘杂志 "Input" 的菜单是一个小游戏——屏幕上是三行菜单项，你操纵一艘小飞船上下移动并**开炮击中菜单项**来选择。因为菜单项之间有空隙，你会打偏（而且当年家用电脑的操控远不如今天精准）；打偏的炮弹会从对面墙反弹回来，不躲开就会击中自己的飞船。飞船坠地后会出现一个几像素大的小人，一边抱怨一边把坏掉的飞船拖出屏幕。**apples_oranges** 在下面回忆说，正是学校里一次"写个程序"的作业让他写了个 ASCII 卷轴游戏，发现这事有多好玩，从此改行当了程序员。

- **一场关于 "sinus" 的语言学考据**意外地成了热门支线。**userbinator** 打趣说他一开始以为 "Elite Sinus Producer" 和 "The Sinus Creator" 是专门做鼻音音乐的 tracker（sinus 在英语里也指鼻窦），随后推测用 sinus 而非 sine 是因为很多 scener 来自欧洲大陆，母语保留了拉丁词源。评论区立刻变成点名册：**vidarh** 确认挪威语、瑞典语、丹麦语、荷兰语、法语都是 sinus，**profi2000** 报德语，**TeMPOraL** 报波兰语，**windenntw** 报西班牙，**delian66** 报保加利亚语。

- **Fasttracker II 是全帖被最多人怀念的软件**。**wartywhoa23** 称它至今仍是"最令人怀念、最直观、最沉浸的 DAW"，也是整体上设计最周到的软件之一；他 1996-2001 年间用它写的一些曲子上过本地电台和夜店，多年后只有 Ableton 在"不打断创作心流"这个精神上接近了它。**superted** 补了一个绝妙的时代细节：FT2 没有音效功能，所以他手工制造回声——把声音从一个声道复制到另一个可用声道，偏移几个 tick 并降低音量。**bitwize** 提醒众人 Fasttracker 本身就是 demo scene 的人（Triton 小组，后来的 Starbreeze Studios）写的，关于页面里有 demo 效果，甚至内置了一个贪吃蛇；他还加了一句："如果你觉得 Fasttracker 复杂，那你该去试试 ProTracker。"**whizzter** 从格式演化角度补充：XM 格式比 ST3 强、比 Impulse Tracker 的 IT 格式弱，但正因为 XM 的第三方回放器又多又够用，它在 1994 到 1998-99 年间成了 demo scene 的甜蜜点，直到 PC 快到能直接播 MP3。

- **tracker 到底直不直观**引发了一场小争论。**tosti** 完全不能接受："我从没想过有人会把 tracker 这么晦涩的东西叫直观，我想不出更糟的了，Pure Data 勉强接近。"**joeyjojo** 的回应很有说服力：对不擅长现场演奏 MIDI 控制器的人来说，tracker 反而很直观；起头做个循环时 DAW 更快，但过了最初那道坎之后，把一首曲子完整做完的工作流反而更简单——"体验上就像在编辑电子表格"，DAW 与 tracker 的区别类似于在文本编辑器里写代码 vs 使用节点式 UI。**5-0** 讲了本帖最动人的一段：他是靠加载别人的 mod 文件、观察哪些音符和采样在何时被触发、用了什么效果指令来学会做音乐的；几年前他下载了 SchismTracker，**二十多年前 Scream/Impulse Tracker 的肌肉记忆瞬间回来了，"这是我这辈子最美妙的人机交互体验之一"**。他还澄清自己不是靠暴力试错——每个界面都有帮助页，按 ESC 就能看到所有屏幕和快捷键；顺带记录了 90 年代瑞典的现实：能有台家用电脑（通常是 486 66 MHz、8 MB 内存）已属幸运，有 MIDI 的极少，认识一个在 Windows 3.11 上跑 Cubase 配硬件采样器的人就觉得异常奢华；而"扒"别人 mod 里的采样被认为很 lame，买不起合成器的人会买采样 CD。

- **对现代 UI 的批评**是另一条主线。**sph** 怀念 DOS 时代还没有标准化和"人机界面指南"的日子：小时候运行父亲从公司带回的软盘上的 exe，意味着被未知的色彩和声音轰炸，永远不知道会遇到什么。**Lerc** 把话说得最系统：过度追求统一的观感损失了很多东西——标准化让毫无设计的应用也能共享一副体面外观，而做自定义界面需要真的去设计，设计者水平不够就会做出怪物，但解药是 dogfooding，自己用自己做的界面就是把它做好的动力。他认为 demo scene "精确地造出你想要的功能"的哲学明显影响了 Winamp，而这一切随着软件选择的减少而消失："如果你不喜欢 Winamp，你可以不用它。当一个程序被当作某问题的唯一解决方案推销、并且想让所有人都用同一个程序时（往往靠锁定），如果软件想迎合所有人的口味，它就不会合任何人的口味，只会变得平庸。"

- **几条来自当事人的评论**让帖子格外有分量。**fudgie** 直接现身："16 岁时我和一个朋友用 AsmOne 做了 Multi-Ripper，还很机灵地把自己的电话号码写进了它转换出的 mod 文件里。我父母对多年后那些随机打来的陌生人电话非常'感激'。"**jsonc** 说他是澳大利亚的 Amiga demo 编码者，在 asm-one 里熬过无数长夜，但当年完全不知道有现成的预计算查表工具——他一直以为大家都是自己写代码生成的。**amiga386** 则深挖了 X-Copy 那声 "boing"：它是 Paula 声音芯片极少数使用"绑定（attached）模式"的场合之一，即用一个声道的输出去调制另一个声道的音量或音高，他还给出了完整源码的链接。**Schlagbohrer** 提到一个当代观察：现在的前沿模型在生成 ASCII/ANSI 艺术上依然烂得彻底——**spankibalt** 对此的回应非常 scene："这挺好。只有 lamer 才需要那种东西。lamer 和艺术家（scener）不是一路人。"

- **"demoscene 是一个词还是两个词"**引发了一场典型的 HN 式偏题争论。**larodi** 认为应该写作 demoscene，pouet.net、scene.org、assembly.org 都这么写。**amiga386** 的反驳既有理论又有史料：英语复合名词可以分写、连字符或合写，可接受用法随时间变化（file name → filename，foot ball → football），"demo scene" 是 demoscene 被承认的变体；他还搬出了 1991 年 Kefrens 的滚动字幕原文和 90 年代的多份 scene 文献作证，并直言"如果你已经理解了对方的拼法却仍要强迫对方改，那才有点像混蛋"。**vidarh** 猜测这种合写倾向和前面的 sinus 一样，源于 scene 早期由北欧日耳曼语族使用者主导。

- **cbold** 问 demo scene 现在还存在吗，**Sesse__** 的回答略带伤感：还在，但似乎吸引不到多少新人，活动大多回到了 Amiga 这类老平台上；Assembly 2026 恰好就在那个周末。**not_a9** 补了一条微妙的跑题观察：今年的 Assembly 居然有了一个"独家 AI 合作伙伴"。

- 最后，**DonHopkins** 顺着 FuckPaint 的名字写了一整套虚构的 "FuckWork 365" 办公套件恶搞清单（订阅制，"保证不满意"），从 FuckPaint、FuckDraw、FuckWrite 一路排到 FuckerNews（"你会后悔点开的帖子"，由 XXXCombinator 提供）——是那种只有 HN 才会出现、也只有 HN 会把它顶上去的东西。
