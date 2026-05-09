---
layout: article
title: "Red Squares：把 GitHub 故障当作 contribution 画在草地上"
issue: 793
number: 21
category: code
original_url: "https://red-squares.cian.lol/"
hn_url: "https://news.ycombinator.com/item?id=48034587"
date: 2026-05-08
---

## 文章摘要

Red Squares 是一个带有讽刺意味的小项目：它把 GitHub 那张所有人都熟悉的"绿色 contribution 草地图"反过来画——不再统计某位用户每天提交了多少 commit，而是统计 **GitHub.com 平台本身每天故障了多少次**。颜色越深、方块越红，代表那天 GitHub 平台经历的故障越严重。作者 cianmm 用 React + Vite 把它做成单页应用，托管在 cian.lol 子域上，整个站点除了"红色草地"之外没有多余文字，自嘲式地与 GitHub Profile 上的绿色 contribution 墙形成镜像。

数据本身来自 GitHub 官方公开的 status / incidents 时间线——把每次官方公告中标记的 "incident" 按日期聚合，再映射到 7×N 网格。作者没有特意美化数据，把 2024–2026 年的故障史一股脑铺出来，**周末明显偏少、工作日尤其周二到周四集中**的图样跳了出来：这既是因为周末用量小，也强烈暗示故障与"工作日的发布与变更"高度相关。

这个项目在 Mitchell Hashimoto 宣布 Ghostty 出走 GitHub 之后流传开来，被许多开发者当作"GitHub 不稳定"的可视化证据钉在屏幕上。它的存在也是一种行为艺术：开发者多年来用绿色草地图秀勤奋，现在轮到平台自己来一张"勤奋翻车"的版本。

## HN 评论精华

- **cyanydeez**：这张图是双关的——"周末故障少"既可以解释为流量小、负载低，也可以解释为 GitHub 员工周末不上班、不发布变更。两个因素都在发挥作用。

- **globular-toast**：GitHub 自己曾把可靠性下滑甩锅给"AI 流量增长"，但 AI 用量在工作日和周末并没有那么大差别。从图上反而是周五故障明显更少——这正好对应业内常见的"周五不发布"约定。

- **lnenad / renegade-otter**：评论区半开玩笑地猜测内部场景——管理层在背后吼"YOU NEED TO USE MOAR AI！"，工程师只能加班把 AI feature 塞进产品，自然引入更多事故。

- **rufo（前 GitHub 员工）**：补充内幕。GitHub 此前是 60–90 分钟一次 deploy train 的高频发布节奏，24 小时不停跑（工作日尤其密），所以工作日故障多并不奇怪。还有个反直觉细节：早年间如果**没人发布**，GitHub 反而更容易挂——因为存在内存泄漏类问题，重启发布反而能续命，假期还得安排志愿者每天部署一次。

- **bharxhav / hosteur**：把图理解为"人为变更密度图"——周末故障少强烈暗示**绝大多数事故由人触发**（部署、配置变更），而不是被动负载导致。embedding-shape 也补充：业界数据一直显示绝大多数 outage 来自变更，少有"用户太爱用我们以致挂掉"的情况。

- **figmert**：调侃式总结——周末几乎不挂，完美契合开发者的作息：周末本来就不打算干活。

- **sd9 / skor / atrettel**：质疑数据本身——周末"故障少"也可能是因为周末没人去上报或留意问题，而不是真的没故障。这种"被动数据集"天然带偏差。

- 整体共识：图本身略带恶搞，但工程师们把它读出了门道——它直观印证了开发者多年来的体感：GitHub 的可靠性曲线，更像一张"工作日发布频率"的镜像，而不是"全球用户压力"的镜像。
