---
layout: article
title: "CodingFont：帮你选编程字体的小游戏"
issue: 788
number: 31
category: design
original_url: "https://www.codingfont.com/"
hn_url: "https://news.ycombinator.com/item?id=47575403"
date: 2026-04-03
---

## 文章摘要

CodingFont 是一个交互式网页游戏，旨在通过锦标赛淘汰赛的方式帮助程序员找到最适合自己的编程字体。这个工具解决了一个程序员群体中经久不衰的痛点：面对数十种等宽编程字体，如何找到最适合自己的那一款？

游戏采用**单淘汰赛制**（bracket tournament）的形式运行。每轮向用户展示两款字体的代码预览，用户在不知道字体名称的盲选状态下选择更偏好的一款。经过多轮对比淘汰后，最终产生一个"冠军"字体作为推荐结果。这种盲选机制消除了品牌偏见，让用户纯粹基于视觉感受做出选择。

网站收录了大量流行的编程等宽字体，包括但不限于：Fira Code、JetBrains Mono、Source Code Pro、Ubuntu Mono、Inconsolata、IBM Plex Mono、Roboto Mono、Hack、Victor Mono、Courier Prime、Red Hat Mono、Overpass Mono、Noto Sans Mono 等。用户可以选择启用或禁用连字（ligatures）功能进行对比。

游戏的代码预览展示了实际的编程代码片段，让用户可以评估字体在真实编码场景中的表现，包括：字符辨识度（如数字 0 和字母 O、数字 1 和小写 l 的区分）、等宽对齐效果、括号和运算符的清晰度等。左侧边栏显示赛程进度，标注当前所在的淘汰轮次。

这个项目的设计思路类似于"Hot or Not"式的配对比较，但采用锦标赛结构来逐步缩小选择范围。虽然单淘汰赛制有一定局限性（一次误选可能淘汰最佳字体），但它提供了一个快速、有趣的字体探索方式，尤其适合对编程字体尚无明确偏好的开发者。

## HN 评论精华

**最受欢迎的字体**：评论区呈现了极其丰富的字体偏好讨论。Berkeley Mono 获得了最多热情拥护（pmarreck、aquir、sevg、TymekDev），被称为"完美"字体，但因为是付费字体未被收录。Iosevka 同样有大量粉丝（joch、KronisLV、quibono），以极窄且可高度定制著称。JetBrains Mono、Source Code Pro、Ubuntu Mono、Fira Code 也各有拥趸。

**连字功能的激烈争论**：dylan604 直言连字"立即取消资格"，认为不应篡改代码字符。adrian_b 则反驳说好的编程字体应该同时支持两种偏好。多人讨论了如何在不同编辑器和终端中精细控制连字开关（wezterm、ghostty、Emacs 都支持按模式配置）。pwdisswordfishy 批评 ASCII 连字的"恐怖谷"效应，认为不如直接使用 Unicode 字符。

**游戏机制的改进建议**：spankalee 建议采用循环赛（round-robin）而非淘汰赛制，crazygringo 同意并指出偏好可能不一致。jordanscales 建议使用 Elo 评分系统允许多轮比较。manbitesdog 希望能看到全局统计数据显示哪些字体最受欢迎。hysan 花了 10 多分钟认真对比却得到不满意的推荐结果。

**渲染环境差异**：DiabloD3 批评浏览器渲染与实际应用（Freetype、DirectWrite）中的字体表现差异很大，"有些字体在大号下很惊艳，小号下却无法阅读"。bool3max 也提到字体在不同环境间差异显著。

**小众字体推荐**：comic-shanns-mono（Comic Sans 风格的等宽字体）引发了有趣讨论——nvahalik 说开始是开玩笑用，结果"越用越喜欢"。ksymph 推荐了可爱的 Lotion 字体。dlvhdr 为 Commit Mono 的缺席感到遗憾。TacticalCoder 分享了几十年前自制像素字体的经历。rbanffy 怀念经典的 IBM 3270 终端字体。

**缺失字体的遗憾**：Berkeley Mono、Iosevka、Consolas、Cascadia Code、Menlo、Monaco、DejaVu Sans Mono 等常用字体的缺席引发了广泛讨论。sedatk 推测 Berkeley Mono 因为是付费字体而未收录。
