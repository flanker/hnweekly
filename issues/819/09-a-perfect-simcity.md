---
layout: article
title: "完美模拟城市：高分规则与宜居目标并不相同"
issue: 819
number: 9
category: favorites
original_url: "https://www.jeffquast.com/post/a-perfect-simcity/"
hn_url: "https://news.ycombinator.com/item?id=50504373"
date: 2026-11-19
---

## 文章摘要

Jeff Quast 通过阅读 Micropolis 相关源码并进行算法实验，研究早期 SimCity 的城市评分如何形成。他展示了五座偏向高分、五座偏向人口规模的城市：高分案例大多只有一万多人，人口实验则达到约七十万。两组目标产生很不同的布局，提醒玩家评分、规模和自己心目中的宜居城市并不是同一个优化问题。

文章分析了多个反直觉机制。按作者实验，交通检查并不总要求现实中的完整道路网络，工作岗位与居民比例可能为评分带来额外好处，喷泉会影响某些平均值的统计范围，而过多绿地在特定计算中可能触发数值回绕、降低地价。作者还区分 Micropolis 与 SNES 版本，后者的赠礼机制会改变人口潜力。因此这些结论不能不加区分地套到所有 SimCity 版本或后来的城市模拟游戏。

更值得思考的是模型背后的价值选择：当游戏奖励某些统计指标时，玩家可以找到高效但不符合现实生活直觉的配置。文章把这种现象联系到早期城市动力学模型，属于作者对模拟假设的反思，不应把游戏中的住房策略当作现实政策依据。本期没有运行代码或复现城市实验，数字和机制结论均来自作者报告。

## HN 评论精华

- [robotmay](https://news.ycombinator.com/item?id=50545936) 回忆自己在 Cities: Skylines 用单向螺旋道路解决拥堵，却发现居民绕行体验极差，形象说明指标优化可能背离人的需求。
- [monster_truck](https://news.ycombinator.com/item?id=50545228) 回忆借助一本 SimCity 2000 攻略理解规则的乐趣；该评论谈的是另一版本，不能作为本文机制的验证。
- [bovermyer](https://news.ycombinator.com/item?id=50545554) 开玩笑说自己最喜欢遭遇灾难的城市，也提醒游戏乐趣不必局限于追求最高分。
