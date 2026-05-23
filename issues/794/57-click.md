---
layout: article
title: "一个会告诉你它知道你多少事的点击按钮"
issue: 794
number: 57
category: fun
original_url: "https://clickclickclick.click/"
hn_url: "https://news.ycombinator.com/item?id=48187054"
date: 2026-05-22
---

## 文章摘要

**clickclickclick.click** 是荷兰设计工作室 Studio Moniker 在 2016 年做的一个**关于在线画像（online profiling）的浏览器小游戏**——表面上你只是在屏幕中央反复点一个按钮，但页面会用一个**带语音播报的解说员**实时念出它对你的观察："Subject is using Chrome." "Subject has clicked the button five times." "Subject's cursor is moving to the upper left."——越玩越发现解说员能"看见"的细节多到离谱：浏览器版本、操作系统、电池电量、是否插着电源、屏幕尺寸、当前时间、所在国家、鼠标轨迹、点击频率、与历史上其他玩家的对比……

它的**所有"洞察"都来自浏览器原生 JavaScript API**——`navigator.battery`、`navigator.userAgent`、`window.innerWidth`、`screen.orientation`、鼠标事件、性能 API 等等，加上几个简单的统计（"你的点击比 87% 的访客都少"）。技术栈极朴素：纯前端 + Socket.IO（实时同步语音/解说节奏）+ GSAP 做动画，背后没有什么神秘的 AI、也没有指纹追踪 SDK——**它的杀手锏正是"这些东西网站本来就能看见"**。

它在 HN 上被翻出来重新讨论的原因，和它当年红的原因一样：把一件**早就在你身上发生**的事变得**让你毛骨悚然**。脚本小子可以打开 console 写 `for (let i=0; i<1000; i++) document.querySelector('.button').click()`，页面会立刻识破并喊出"Bot"——它还能用 `event.isTrusted` 区分人类点击和脚本点击。这是一个 10 年前的项目，作者们想说的话今天反而**比当年更扎心**。

## HN 评论精华

- **hspeiser**：第一反应——"它知道我光标的精确位置，这很 unnerving"。这条评论下挂了一长串讨论。**ProAm** 回敬一句话："So does every advertiser and data broker in the world（每一个广告商和数据经纪人都是这么干的）。"

- **rolph & nomel**：脑暴**更阴间的版本**——如果鼠标光标根据你**说话**的内容自己动呢？技术上完全做得到。nomel 真做过类似的 demo：本地跑 Whisper 持续监听，定期搜索相关图片/视频显示出来，"像个会读心的网页"。

- **busymom0**：泼冷水派——"这不就是任何网站靠基础 JS 都能拿到的信息吗？我没看出什么名堂。"**layer8** 一句话回他："说明你不是目标受众。"——这个项目的全部价值正是**让普通人意识到"基础 JS 就能拿到"有多吓人**。

- **BudaDude**：在 console 里跑 `for (let i = 0; i < 1000; i++) document.querySelector(".button")?.click()`，页面立刻喊出"Bot!"，并播报"clicks less than most other subjects（点击数比大多数人少）"——说明它**用 `event.isTrusted` 区分**了脚本点击和真人点击。

- **ProAm**：升华到价值观层面——"这是一个非常好的 POC，证明你**只是在用 web**就在不停地出卖隐私。这些数据每天被买、被卖、被用来对付你。"

- **foxfired**：以**广告业内人**身份现身——他自己创业时给网站加上了那种**能像视频一样回放用户会话**的分析工具（heatmap + session replay），当他的朋友亲眼看到自己被录像，第一反应是震惊。底下又被 **htx80nerd** 类比成"超市监控":"每个人都知道商店有监控，但如果店员主动给你打电话说'我看到你拿了薯片'，感觉就完全不一样。"

- **jrowen & singpolyma3**：辨析"为什么人们对**聚合数据**不那么在意、但对**个体被点名**反应剧烈"——一个精彩的提法是**"public restaurant 的隐私"**：人在公共餐厅说话不是因为没人能听见，而是因为**没人在听**。一旦感到"有人在听"，公共空间立刻就变成了监视场。

- **LeoPanthera**：顺手安利同类型的有趣网页 **Pointer Pointer**（pointerpointer.com）——你把鼠标停在网页任意位置，它会调出一张正好"有人指着那个位置"的照片。

- **d4rkp4ttern**：补一个**精神续作**——sinceyouarrived.world/taken，思路类似但更暗黑。

- **10000truths**：Linux + Firefox 用户报告 `PR_END_OF_FILE_ERROR`——这种"实验性艺术站"在小众环境下经常翻车，也是 HN 一直存在的一类典型 bug 报告。
