---
layout: article
title: "一款根据经典 Commodore 64 键帽照片复刻的字体"
issue: 813
number: 31
category: design
original_url: "https://github.com/szabadkai/c64-keyboard-font/"
hn_url: "https://news.ycombinator.com/item?id=49990224"
date: 2026-10-09
---

## 文章摘要

这是匈牙利开发者 Levente Szabadkai 在 GitHub 上发布的一个字体项目，名为 **C64 Keyboard**。它不是 C64 屏幕上那套著名的点阵字符，而是复刻**印在实体键帽上的字样**——也就是那套干净、略带人文气质的无衬线大写字母。作者的参照物是自己手上一台中欧版 Commodore 64，每个字母、数字和符号都以这台机器键帽的近距离照片为底稿描摹，再对笔画形状做清理，统一比例。

字体目前包含：大写字母、数字、标点、方向箭头、英镑符号 £ 和 π；部分带重音的字母（包括匈牙利语字符）；f1 到 f12 功能键标签；以及键帽正面印着的全部 63 个 PETSCII 图形符号。小写输入会自动显示为大写，因为原键帽上本来就只有大写字样。PETSCII 部分有一个值得注意的说明：符号的身份和对应按键来自《Ultimate Commodore 64 Reference》这份资料，而边框粗细、曲线和印刷比例则是从键盘照片中推断出来的，因此这些图形是「几何重建」，并非逐像素的照片描摹。键帽左侧的图例对应 Commodore 键 + 按键，右侧对应 Shift + 按键。

项目提供 TTF、OTF、WOFF2 和完整压缩包下载，还有一个在线预览页，可以点按钮把功能键或 PETSCII 符号插入测试框再复制出去使用。可编辑的字形轮廓放在 `glyphs/` 目录，构建脚本放在 `scripts/`。整个仓库（字体文件、轮廓、脚本、预览页、文档等）以 CC0 公共领域协议发布，可以商用、无需署名；不过用于描摹的原始近照没有收录，CC0 也不涉及 Commodore 对键帽设计或商标的权利，作者强调这是非官方的独立复刻。

从版本记录看，项目在 HN 上走红的这几天迭代很密集：1.108 移除了 Commodore 徽标字形并改用 CC0；1.109 根据 issue 反馈重画了 S（之前的中心对称 S 看起来像倒过来的）和 @，并调整了感叹号等标点的位置与大小；1.110 删掉了 RETURN、SHIFT 等整词键名图例；1.111、1.112 又继续把 $、#、% 放大到正常字号，并重画了 & 和 @。可以看出，不少改动直接来自社区挑出的毛病。

## HN 评论精华

这条帖子获得 400 多分、约 60 条评论。讨论大致分为三块：字体本身的「出身」考据、对字形质量的挑剔（以及由此引出的「AI 生成」争议），还有大家顺手分享的相关字体故事。

**原始字体是哪一款？**

- **Sharlin** 觉得这是一款很耐看的人文无衬线体，好奇当年是专门为键盘设计的，还是直接拿了现成字体。**urbnspacecowboy** 说自己研究过，最接近的是 Akzidenz-Grotesk Condensed，但并不完全一致：G 的右下角不同，0 中间那道斜线像是定制的，Q 更是差得很远，像在 O 上拼了另一个 O 的一段当尾巴。他顺带吐槽这款商业字体贵得离谱。
- **nathell** 补充，后来的 C64C 键帽据 MechBoard64 项目考证用的是 ITC Avant Garde Gothic；**tniemi** 指出早期 VIC-20 用的是 Microgramma，**badc0ffee** 说 PET 系列也用过它；**Sharlin** 和 **queenkjuul** 接着聊到 Microgramma/Eurostile 是科幻电影和老电子设备面板上的常客。
- **exitb** 有个有意思的观察：原版 C64 和后来 64C 的键帽字样差别很大，而出于怀旧滤镜，更老的那套反而显得更现代。

**字形质量与 AI 争议**

- **petecooper** 贴出了仓库的 issue #4，说那条 issue 让他很难过——提交者语气相当冲。**ricardobeat** 认为那条 issue 把字体和「复刻整块键盘」搞混了（抱怨没有 RETURN、SHIFT 键、F 键太多），不过说 @ 字符歪歪扭扭倒是对的。**ryandrake** 认为那种反馈既侮辱人又毫无建设性，**cmrdporcupine** 则说有些人一闻到 LLM 的味道就变得极其粗鲁。
- 另一方面，**BearOso** 列出一串具体问题：S 比例不对、M 的中间应该落到基线、3 和 4 的中间笔画长短不对、标点大小不一。他还认为感叹号被抬高并非忠于原键帽，而是 AI 把照片里的位置当成了设计；该 GitHub 用户有上百个「随手生成」的仓库，像是在刷履历。**gwbas1c**、**exceptione**、**otikik** 也都指出 `!#$%` 这几个字符悬得太高或太小。**queenkjuul** 则表示预览页里那些 Claude 写的说明文字可以去掉。（从更新日志看，作者随后几个版本确实修了 S、@、! 和 $#% 的问题。）

**延伸分享**

- **jankhg** 推荐了 aresluna 那篇讲 Gorton 字体（曼哈顿「最勤劳的字体」）的长文，**gxs** 说那篇比这次的项目还好看；**abrookewood** 分享了一个把字体刻在树上、让树长出变形字母的艺术项目。
- 有人呼吁做 ZX Spectrum、Atari XL、Apple ][ 版本；**bay_baobab_ii** 推荐 8BitDo 出的 C64 外观机械键盘，**yifanl** 说它腔体很空、敲起来回声明显，而 **HeckinCoolDude** 吐槽它其实只是套了 Commodore 皮的普通 PC 键盘。
- **pinkmuffinere** 说这字体很美，但只有大写，而他打的字 95% 都是小写，基本没法用；**dar5hak** 则觉得如果扩充字符，会是一款不错的编程字体。
- 帖子开头 **petesergeant** 反映下载链接坏了，**dekoruotas**、**leptons** 给出了改用 raw 链接的绕法。
