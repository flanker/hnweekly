---
layout: article
title: "让小旅鼠在网页上行走的动画实验"
issue: 817
number: 30
category: design
original_url: "https://markblah.github.io/lemmings/"
hn_url: "https://news.ycombinator.com/item?id=50375354"
date: 2026-11-06
---

## 文章摘要

这个向《Lemmings》致敬的网页实验把普通页面变成了小角色的活动场。按钮可以增加行走者，也可拖起角色、甩动后抛出。页面依次摆放楼梯、过高的台阶、悬空平台、坑洞、围墙、卡片和正文，让读者直接观察它们如何与不同网页元素发生碰撞。它更接近一个互动游乐场，而不是复刻原游戏关卡与完整规则。

有趣之处在于布局变化本身也是玩法。卡片会在窄窗口中换行，站在卡片上的角色随之被推走；改变窗口大小时，角色会跟随承载它的表面移动，快速调整甚至能把它弹射出去。边框、按钮、图片和输入控件是实体障碍，文字行则被当作能落脚、但不会挡住横向行走的平台，以免角色卡在过密的行间。底部还有一条长平台接住掉落者。

页面说明提供了这些交互规则，但标题中的「CSS 动画」不应被扩展成「整个项目只用 CSS」的技术结论：页面还公开了添加角色的脚本接口。本摘要依据可读取的演示说明和作者评论，没有执行项目代码，也没有把某一设备上的显示效果当作已经跨浏览器验证的事实。

## HN 评论精华

**致敬对象有历史细节。** 作者 [MarkBlah](https://news.ycombinator.com/item?id=50375355) 说这是给 Psygnosis 的趣味致敬；[msephton](https://news.ycombinator.com/item?id=50377630) 补充 Psygnosis 是发行商，开发者是 DMA Design。

**移动端暴露边界问题。** [ggsp](https://news.ycombinator.com/item?id=50375949) 提醒缩放交互存在问题，并提出 CSS 调整建议；作者回复准备加入。另有 [tosti](https://news.ycombinator.com/item?id=50380025) 报告 Android 最大显示尺寸设置下，角色会卡入楼梯和墙面。

**普通网页可以成为场景。** [Waterluvian](https://news.ycombinator.com/item?id=50375689) 分享自己让机器人绕开网页文字的实验，特别提到测量单个字符边界比预想容易。
