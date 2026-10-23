---
layout: article
title: "CSS 的未来：我为 Interop 2027 投票的十大特性"
issue: 815
number: 31
category: design
original_url: "https://www.joshwcomeau.com/css/interop-2027/"
hn_url: "https://news.ycombinator.com/item?id=50175070"
date: 2026-10-23
---

## 文章摘要

前端教育者 Josh W. Comeau 借 Interop 2027 规划之机，盘点了自己最希望进入明年 Interop 的十个 CSS 特性。Interop 是 Google、Apple、微软、Mozilla 联合 Igalia、Bocoup 推动的跨浏览器协作项目，效果显著：一个特性一旦被列为重点，往往一年内就能在所有主流浏览器达到近乎 100% 的支持。作者举的例子是 CSS Subgrid——Firefox 在 2019 年就实现了，其他浏览器多年按兵不动，2023 年进入 Interop 后当年就全面可用。如今 Mozilla 做了一个投票排名系统，开发者可以为心仪的提案点赞，作为挑选明年重点的参考。

作者的十个选择如下（除第 5 项外，目前都只在 Chrome 155 / Edge 154 中支持，Firefox 157 与 Safari 27 尚未支持）：

1. **CSS 自定义函数。** 用 `@function` 定义、用 `result` 声明返回值，比如封装一个"某颜色渐变到透明"的函数。作者最想用它做极坐标到直角坐标的换算，让粒子"沿 45 度方向移动 100px"只需一行。
2. **`corner-shape`：斜角与超椭圆。** 在 `border-radius` 的基础上提供 squircle（苹果常用的超椭圆圆角）、bevel（斜切）、notch、scoop 等形状。作者原本最期待 squircle，实际用下来却大量使用 bevel 做游戏或科幻风界面。它和 clip-path、遮罩不同，会真正改变边框盒形状，因此边框、轮廓和阴影都会跟着走，是完美的渐进增强。
3. **`border-shape`。** 更底层的版本，可用直线、圆弧和贝塞尔曲线自己画边框，同样改变真实的边框盒，还能在不同形状间做动画。
4. **锚点容器查询。** 锚点定位已在 Interop 2026 中普及，可以把提示框固定在触发元素旁，空间不够时自动翻转到另一侧。缺的是"知道它是否翻转了"，Level 2 新增一种容器查询，可以判断目标是否在用后备位置，从而让提示框的小箭头指向正确方向。
5. **`inherit()` 函数。** 作者坦言此前没听说过，目前没有任何浏览器实现，但一看就想要：它能取出父元素的继承值再用 `calc()` 加工，例如让嵌套圆角的内层半径比外层少 4px。他估计这个太早期，进入 Interop 2027 的希望不大。
6. **滚动触发动画。** 与已在各浏览器落地的"滚动驱动动画"（随滚动进度擦洗关键帧）不同，它像接近传感器，滚到某处时触发一段固定时长的动画，可以替代用 IntersectionObserver 写的 JS。
7. **`reading-flow` 与 `reading-order`。** 响应式布局下视觉顺序与 DOM 顺序可能完全不同，而 Tab 键焦点顺序永远跟随 DOM，对辅助技术用户很不友好。`reading-flow: grid-rows` 之类的值可以让焦点顺序跟随视觉顺序，`reading-order` 则可自定义次序。这个提案由 Rachel Andrew 推动。
8. **`interpolate-size` 与 `calc-size()`。** 终于可以在 `height: 0` 和 `height: auto` 之间做过渡动画，手风琴折叠面板不再需要 JS。Chrome 2024 年就已实现，现在就能配合 `@supports` 作为渐进增强使用。
9. **间隙装饰（gap decorations）。** Grid 的单元格没有对应元素，空格子没法加边框。新的 `rule`、`row-rule`、`column-rule` 属性允许直接在网格线上画线。作者也分享了沿用多年的变通办法：给网格设背景色和 1px 间隙，但需要大量空 DOM 节点，且只适用于纯色背景。
10. **滚动状态容器查询。** 可以判断 sticky 元素是否已"吸顶"，从而改变吸顶头部的样式；也能判断滚动容器是否还能向上或向下滚动，用来显示提示性的渐变遮罩。

结尾，作者回应了一个时代性的疑问：既然越来越多工作交给大模型，还有必要追新 CSS 吗？他认为更有必要，因为模型的训练数据大多是几年前的，不会主动用新特性，人得知道这些能力才能引导 AI 做出更好的体验。他鼓励读者去投出自己的排名，并提到这篇文章的灵感来自 Declan Chidlow 等人分享的榜单。

## HN 评论精华

本帖 621 分、约 275 条评论，讨论热烈且发散：一部分人为现代 CSS 的进步欢呼，另一部分人质疑 CSS 越来越像编程语言、功能越堆越多；中途还跑题成一场"原生 JS 还是框架"的长篇争论。

**"Interop" 却全是 Chrome 独占？** herpdyderp、NooneAtAll3、account42 都觉得讽刺：文章开头赞美浏览器协作，列出的特性却几乎都只有 Chrome 支持。paulhebert、afavour 解释这正是 Interop 投票的意义——选出要推动到所有浏览器的特性。troupo 则认为 Interop 本是为遏制 Chrome 单方面推进自家 API 而设，结果也被 Chrome 主导了，就像 OpenUI 一样。boxed 针对文中 Subgrid 的例子说"直说吧，就是 Chrome 不动"。Rapzid 调侃"Baseline 2031 冲啊"，Safari 要两年实现，再等系统更新慢慢铺开。

**CSS 该不该有函数？** 这是最集中的争议。lioeters 问"我们真的想在 CSS 里写函数吗"，觉得这像"用 YAML 编程"，为什么不改善与 JS 的集成；afavour 回答 JS 需要虚拟机和脚本解析，把它作为页面显示的前提是倒退，CSS 别扭的声明式语法正是为了能快速解析，并坦言当年也觉得 CSS 变量别扭，用了才觉得好，"也许叫'模板'会更好"。longor1996、Izkata 也认为它更像卫生宏或模板，文章用"熟悉 JS 的话会觉得很亲切"来介绍反而加深误解。agstudio 认为 CSS 已是"困在过时样式表语法里的编程语言"，缺少静态类型检查。chuckadams 指出 CSS 的前身 DSSSL 本就是 Scheme 方言，又引用 Tim Berners-Lee 的"最小权力原则"说明为何当初刻意不让它成为编程语言。bgarbiak 则认为 Sass 里早有函数，实践中反而降低复杂度。roblh 戏谑地预告了未来的新闻标题："我用纯 CSS 函数跑起了 Doom"、"CSS 函数被用于指纹追踪违反欧盟法律"。hexasquid 一句话总结："几年前大家说 CSS 不算编程语言，因为没有函数和控制流；现在它们来了，大家又喊'不要！'"

**现代 CSS 真香派。** CharlesW 抢在负面评论之前表达对 CSS 的喜爱。afavour、techpression、paczki 都说如今的 CSS 和几年前截然不同：CSS modules、变量配合暗色模式、`light-dark()`、`@scope`、`@layer`、`:has()`、嵌套，正在大块替换掉原先的 JS。Theoleff 举例说手风琴就是几个 `<details>`，阅读进度条就是 `animation-timeline: scroll()`；abanana 则提醒 `scroll()` 这类新东西对生产环境还太新。randfur 看到 `interpolate-size: allow-keywords` 直呼没想到能等到这一天。dsego 举了 Subgrid 的实际用例：多张价格卡片中折行的功能条目需要跨卡片对齐，没有 Subgrid 只能写 JS。

**反对派：功能蔓延与"眼部糖果"。** fodkodrasz 讽刺又多了花哨的边框样式，而工业触屏场景需要的日期选择器、软键盘控制至今靠自定义组件；silvestrov、pseudosavant、user43928 回应日期选择器涉及禁用日期、区间、夏令时、时区、本地化，本就极难标准化；afavour 指出这些不归 CSS 工作组管，软键盘问题可以看 `window.visualViewport`；btown 提到 Open UI 正在做可定制的 select。PennRobotics 吐槽十项里三项与圆角有关，现在的网站全是"logo、四个链接、圆角大按钮、三四列圆角卡片、大量留白"的同一副面孔，他干脆用 Stylus 全局把 `border-radius` 设为 0。thisislife2 认为功能蔓延会让任何人都无法再造新浏览器引擎，hollowturtle、KPGv2 等进一步发展成"大厂故意把标准做复杂"的阴谋论，jorisw 斥之为狭隘的阴谋论，引发一番争执。shevy-java 说自己已回归 HTML 表格，OuterVale 详细解释了表格布局对屏幕阅读器的无障碍问题。

**跑题：原生 JS 还是框架。** 由 paczki"坚持写原生 HTML/CSS/JS 是种享受"引发，appplication 问状态管理和响应式怎么办。abanana 认为自己写的是针对站点需求的小代码，而不是重造臃肿的 React；satvikpendem 以亲身经历反驳，公司里的原生 JS 项目最终都长成了自造框架，换成 React 后加载速度反而提升；reef_sh 也说大型 ERP 后台要么自造框架、要么一团意大利面。tancop 认为 React 慢只代表 React，Svelte、Solid、Vue Vapor 等基于 signals 的框架几乎和手写事件处理一样快。

**AI 时代还要学 CSS 吗？** piro0919 说用 Claude Code 写网页从不看代码，效果符合预期；louiskottmann 反驳正是因为有 CSS 这门语言 Claude 才能写出来，dsego 则说"CSS 就像 Rust，只要你永远不用亲眼看它，它就是门好语言"。markasoftware 认为前端恰恰最不该随意交给 AI 生成，因为它是用户的第一印象，dwayne_dibley 说自己昨天就因为一个网站"太像 Claude 做的"而离开。kwar13 和 christophilus 则感慨这种扎实的技术文章在 AI 话题占据半个首页的今天已经很少见了。
