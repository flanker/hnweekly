---
layout: article
title: "Astroterm：在终端里看星空"
issue: 817
number: 60
category: fun
original_url: "https://github.com/da-luce/astroterm"
hn_url: "https://news.ycombinator.com/item?id=50270651"
date: 2026-11-06
---

## 文章摘要

Astroterm 是一个用 C 编写的终端星图程序，可以用字符显示恒星、行星、月相与星座。README 允许用户设定时间、经纬度或城市，查看不同地点与日期的天空，也可以改变动画速度、帧率、颜色和亮度筛选阈值。它把通常属于图形天文馆的软件体验放进终端，重点是轻量、可配置与探索乐趣，而不是宣称替代专业观测工具。

项目特别解释了几处容易误用的参数：观测时间采用 UTC，不指定时间时使用系统当前时间；位置仍需自行提供，不能假定程序自动知道所在地。星等数字越大表示越暗，若想显示更多星星，应增大阈值。星座线只有相关恒星都通过筛选时才会绘制，因此显示不完整不一定意味着数据库缺少星座。终端字符单元的长宽比也可能影响投影外观。

资料来源包括耶鲁亮星星表、IAU 星名、Stellarium 星座连线、GeoNames 城市数据及 NASA JPL 的行星轨道元素。README 还提供多个包管理器与平台的安装路径，以及颜色、Unicode、盲文线条等选项。本次只核读文档，没有安装或运行程序；「精确」应理解为项目目标与算法描述，不是本文对星位误差或性能所做的独立认证。

## HN 评论精华

- [dspillett](https://news.ycombinator.com/item?id=50302301) 赞赏 README 在用途、功能、安装和示例之间保持平衡，认为文档清楚不必依靠夸张措辞。
- [dotancohen](https://news.ycombinator.com/item?id=50300527) 认为示例中不容易辨认猎户座，说明终端图形的表达能力仍有限。
- [superkickstart](https://news.ycombinator.com/item?id=50301354) 想把它用作动态登录提示；dspillett 回复说当时选项表没有输出单帧后退出的模式。创意用途还需要相应接口支持。
- [jubalizer](https://news.ycombinator.com/item?id=50302625) 则报告已将它设为 Omarchy 屏保，用来显示自家位置的天空。
