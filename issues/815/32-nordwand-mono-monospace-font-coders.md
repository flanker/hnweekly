---
layout: article
title: "Nordwand Mono：一款献给程序员与冒险者的等宽字体"
issue: 815
number: 32
category: design
original_url: "https://github.com/tywr/Nordwand-Mono"
hn_url: "https://news.ycombinator.com/item?id=50141581"
date: 2026-10-23
---

## 文章摘要

Nordwand Mono 是开发者 tywr 发布在 GitHub 上的一款开源编程字体，自我定位是"为写代码设计的紧凑型新怪诞（neo-grotesk）字体"。作者在 README 里说，自己对终端和 Neovim 用的字体非常执着，却始终找不到"完美"的那一款，于是决定从零开始设计一款，而且整个字体是用 Python 画出来的。

**设计思路。** 这款字体想把 Adrian Frutiger 的经典字体 Univers 的一些元素，装进 JetBrains Mono 的比例里。名字 Nordwand 在德语里意为"北壁"，指向瑞士和瑞士风格字体排印的传统，作者希望把那种"瑞士山峰北壁的冷峻与刚硬"带进字体。README 中的一张示例图就是阿尔卑斯名峰 Les Drus。作者列出的灵感来源还包括苹果的 SF Pro 与 SF Mono、Sharp Type 的 Alpes Mono，以及 Dalton Maag 的 Ubuntu Mono。从示例图看，字形偏方正、圆角矩形的轮廓明显，小写 m 的中间竖笔较短，这一点来自 Ubuntu Mono，也成了讨论中最有争议的细节。

**可调的字形和特性。** 项目提供了一批替代字符，通过 OpenType 特性开启，例如 `cv01` 可把 0 换成斜杠零或 Berkeley Mono 风格的六边形零，另有替代的小写 a（仅斜体）、l、f、m、数字 3 和德语 ß。样式集方面，`ss02` 开启箭头连字，`ss03` 把 `<`、`>` 和箭头换成更宽的角度。连字刻意做得很克制，只有 `->` 和 `=>` 两个；此外连续的 `=`、`-`、`_`、`#` 可以使用连接形态，让横线首尾相连，这些默认关闭，需要开启 `dlig` 特性。

**Nerdwand Mono。** 项目还能生成打了 Nerd Fonts 图标补丁的版本，图标保持单格宽度，确保严格等宽。这个版本不随发布提供，需要用 uv 和 fontforge 在本地执行 `make build` 与 `make build-nerd-font` 构建。

**自定义编译。** 由于每个字形都由一个 Python 类绘制，并可以注入绘图上下文对象，用户可以修改 x 高度、大写字母高度等度量参数：改好 `config.yml` 后，用 `python -m visualize` 预览单个字形，再用 `python -m generate_font` 生成整套字体。仓库的 `configs` 目录里还附有其他参数组合的配置和效果图。作者也提醒，改了度量后不保证好看，部分字形可能需要手动微调。字体支持多种字重，并附有西里尔字母，示例图覆盖了 Python、C++ 和 Haskell 代码。字体以 OFL 开源许可发布。

## HN 评论精华

本帖 568 分、约 160 条评论，整体评价友善，讨论围绕"它和 JetBrains Mono、Berkeley Mono 到底像不像"、小写 m 的争议、连字好恶，以及大家各自心头好的字体推荐展开。作者本人也现身答疑。

**作者自述。** tywr 说早上看到项目上了首页很意外，自己平时只是潜水。他强调这个项目更像一个 Python 沙盒，而不只是字体文件，欢迎 fork 或提 PR 微调字形，绘制和预览字形都不需要外部工具。设计师 agilek 问用 Python 怎么设计字体，tywr 解释：在矢量绘图软件里画字形，本质上也是调用库执行"在这放一个点、把贝塞尔控制柄放到那"之类的命令，他只是绕过图形界面，直接在 Python 里为每个字形调用这些底层命令；工作流是改字形代码里的公式或函数，然后立刻绘图看效果。gorgoiler 称赞字形好看、代码也好，"不必引入字体库就能画出一个 A"的设计很漂亮。ThePhysicist 怀疑如今每天冒出的新字体都是 AI 生成的，mariusor 反问仓库里看不出任何迹象，凭什么这么认为。

**到底像不像 JetBrains Mono？** rootlocus 起初说"99% 就是 JetBrains Mono"，随后自己用 Claude 做了一个逐字对比工具，承认差异比想象的大；dtj1123 指出平均差异有 33%。tasuki 认为按像素面积算差异没有意义，小写 m 那一笔短短的中间竖画来自 Ubuntu Mono，"带来完全不同的气质"。GreenWatermelon 觉得它更方正、大小写高度更接近，小写 l 甚至高过大写 G，虽然漂亮但可读性不如 JetBrains Mono，不适合日常使用。Subdivide8452 和 hoherd 则说用对比工具来回切换后，反而更喜欢 Nordwand。delta_p_delta_x、pookieinc、jan_m_savage 认为它整体更像付费字体 Berkeley Mono，jan_m_savage 形容 Nordwand 更柔和，传达的气质是"我承载着重要数据"，而 Berkeley Mono 是"我只是在干活，别管我"；常年使用 Berkeley Mono 的 bayindirh 说那款字体的哲学是"好到让你感觉不到它的存在"。

**小写 m 与其他细节。** abcd_f 眯眼一看"m 太怪，关掉标签页"，NewsaHackO、moecables、kls0e 也觉得 m 别扭；tasuki、brzz 则认为 m 是点睛之笔，给字体带来了个性。strathos、mariusor 提醒 m 和斜杠零都有替代字形可选。tgv 和 cdelsolar 特别喜欢小写 k，还说方圆角的造型有种 70 年代的味道；Findecanor 和 rbanffy 也联想到父辈用过的 70 年代打字机、老式 Olivetti 打字机。挑刺的意见包括：圆括号和方括号太像（fercircularbuf、Altern4tiveAcc），连字符太长、容易与长破折号混淆，`<` `>` 太高、可能与 Unicode 角括号混淆（kqr、chirayuk），老花眼下 8 和 0 难分（Findecanor）。语言覆盖方面，emsixteen 说挪威语只差一个 å，"叫 Nordwand 还缺这个挺好笑"，bayindirh 则缺土耳其语的 ğ、ş、ı、İ，并顺带讲了土耳其语 i/İ 问题曾让 Kodi 等软件崩溃的旧事。

**连字之争。** traxler 喜欢只保留箭头的克制设计，因为讨厌 `==`、`===` 的连字。maratc 则讨厌一切连字：它们看起来像一个字形，删除时却要按两次退格，"就像 Ö 要删两次，或者光标能停在 Æ 中间"。antiframe 贴出 Emacs 的 ligature 包配置，说明可以按模式逐个开关连字。bronlund 则表示没有连字的字体他一概不用。

**宽窄之辩与字体推荐。** JoshTriplett 喜欢它不像许多字体那样刻意做窄；tarcon 觉得太宽；lxe 和 nrvn 力挺 Iosevka 和 PragmataPro 这类窄字体，"屏幕这么宽，我想多分几栏"。评论区随即变成推荐大会：Pragmasevka 与 IoskeleyMono（shoobiedoo、DiabloD3、hawk_3221）、Commit Mono（nelsonfigueroa，dancek 解释它的"智能字距"会微调字符位置，弥补 i 窄 m 宽带来的疏密不均）、PragmataPro（eddyg 称其由作者手工微调，覆盖 9000 多个字符，"值回票价"）、Operator Mono、IBM 3270 字体等。abainbridge 晒出自己做的位图字体，认为低 DPI 显示器上位图字体最锐利，bbrks 推荐了 Cozette。

**字体该不该花钱。** Geezus_42 说永远不理解为字体花 50 美元。delta_p_delta_x 回应字体不是天上掉下来的，单是拉丁、希腊、西里尔字符集加上变音符、上下文替代和连字就有几千个字形，每个都要针对不同字重和分辨率调整，好的免费字体几乎都出自大公司委托的字体厂；toyg 把它比作好椅子，每天盯八小时文字，50 美元买个让眼睛舒服的字体很划算。

**其他。** throwaway2046 建议作者把 OFL 授权的字体提交到 Google Fonts，herrherrmann 说出于性能和隐私考虑不推荐直接用 Google Fonts 的 CDN，echoangle、jacobalcock 补充可以在那里找到字体后下载自托管。voidUpdate 问"为冒险者"是什么意思，dguest 答"你没看到那座山吗"，bitwize 接梗"天很黑，你可能会被 grue 吃掉"。hebetude 说他写了个脚本，把帖子里提到的所有字体都下载下来在编辑器里挨个试。
