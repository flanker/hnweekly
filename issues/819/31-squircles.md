---
layout: article
title: "Squircle：圆角方形背后不止一种曲线"
issue: 819
number: 31
category: design
original_url: "https://linebender.org/wiki/curves/squircle/"
hn_url: "https://news.ycombinator.com/item?id=50515151"
date: 2026-11-19
---

## 文章摘要

Linebender 的资料页首先澄清，介于方形与圆形之间的 squircle 并没有唯一数学定义。超椭圆是常见的一族，但把直线段与平滑转角拼接起来，也能产生类似外观。前者通过指数改变形状，后者则便于适应不同长宽比，同时保持角部形状不被拉伸。

文章逐一比较 Apple、Figma、回旋线和超椭圆方案，重点不只是轮廓看起来是否接近，还包括沿着曲线前进时曲率怎样变化。普通圆角矩形的直线与圆弧相接处，切线方向可以连续，曲率却会突然变化；更高阶的几何连续性有助于平滑过渡。不过“更平滑”并不自动等于“每个人都更喜欢”，外观选择仍包含审美判断。

作者指出，把 Apple 的图标轮廓简单称作某个固定指数的超椭圆并不准确，Figma 的近似也有自己的曲率接缝。CSS 的 corner-shape 又采用另一套参数约定，函数参数不能直接当成超椭圆指数。页面给出接近 Apple 外观的实现思路，并讨论浏览器中 Bézier 近似的取舍。本期未运行交互比较或跨浏览器验证；这些形状的标准定义、近似实现和浏览器支持应分别核对，不能因视觉相似就视为完全相同。

## HN 评论精华

- [flowerlad](https://news.ycombinator.com/item?id=50522429) 用圆角矩形的固定圆弧与更渐进的转角作直观比较，同时表达自己仍偏好普通圆角。

- [fasterik](https://news.ycombinator.com/item?id=50522748) 解释图表横轴是弧长、纵轴是曲率，帮助读者理解比较的不是普通轮廓坐标。

- [prideout](https://news.ycombinator.com/item?id=50523708) 提醒 squircle 本身没有统一定义，讨论实现时需要说明具体是哪一种曲线。
