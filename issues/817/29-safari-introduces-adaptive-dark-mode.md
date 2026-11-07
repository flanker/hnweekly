---
layout: article
title: "Safari 27.3 测试版引入自适应深色模式"
issue: 817
number: 29
category: design
original_url: "https://webkit.org/blog/18541/introducing-adaptive-dark-mode/"
hn_url: "https://news.ycombinator.com/item?id=50358562"
date: 2026-11-06
---

## 文章摘要

WebKit 宣布在 Safari 27.3 beta 中加入自适应深色模式，由用户开启后，把尚未提供深色主题的浅色网页转换为深色。团队引用的页面加载统计显示，使用深色系统外观的访问接近四成，支持深色显示的页面加载却只有约一成。功能试图填补这个缺口，而不是覆盖网站已经实现的主题：声明支持浅色和深色的网站，以及本来已经较暗的页面，通常不会被改动。

转换并非对整页简单反色。引擎用颜色矩阵改变亮度、保留色相，调整文本时以 4.5:1 对比度为目标；多数照片保持原样，SVG 标识及简单黑白位图则可能按对比度需要转换。遮罩和混合模式在特定情形会被关闭，canvas 内容不做转换。这些区别意味着图表、品牌标志和特殊视觉效果仍值得开发者实际检查。

网站可以主动提供完整主题，或为局部元素声明自己的配色方案。原文也介绍 light only 退出机制，但明确指出 Safari 27.3 beta 1 尚不支持其中的 only 关键字，不能把文档中的未来行为当作当前已可用功能。为避免增加浏览器指纹，JavaScript 读取到的仍是作者原始颜色，而非转换后的屏幕颜色；这也会影响开发者通过脚本排查显示问题的方式。

## HN 评论精华

**谁决定最终外观？** [guessmyname](https://news.ycombinator.com/item?id=50358660) 一方面欢迎用户获得更多选择，一方面担忧开发测试组合增加。[Spivak](https://news.ycombinator.com/item?id=50358722) 则支持浏览器替用户掌握更多显示控制权。

**网页本就不是固定画布。** [isityettime](https://news.ycombinator.com/item?id=50359015) 列举字体、缩放、高对比度和读屏等差异，认为跨用户显示不一致并不是新问题。

**隐瞒转换状态有隐私理由。** 对脚本读取不到真实显示颜色的质疑，[kccqzy](https://news.ycombinator.com/item?id=50358801) 用浏览器限制已访问链接颜色读取作类比；[neilalexander](https://news.ycombinator.com/item?id=50358779) 则概括为减少一个指纹信号。
