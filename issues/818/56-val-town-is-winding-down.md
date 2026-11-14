---
layout: article
title: "Val Town 宣布分阶段结束服务，并计划开源"
issue: 818
number: 56
category: startup_news
original_url: "https://blog.val.town/winding-down"
hn_url: "https://news.ycombinator.com/item?id=50443427"
date: 2026-11-13
---

## 文章摘要

Val Town 创始人 Steve Krouse 在 11 月 10 日公告中宣布结束业务，坦言未能在云计算市场保持竞争力。公告感谢用户信任，并给出迁移到 Cloudflare、Vercel、Netlify 和 Render 的指南入口。这里是公司正式公布的关停计划，不是对论坛传闻的转述；本次核读了完整公告，但没有验证每一种迁移路径或用户数据导出的完整性。

时间表分为几步：11 月 10 日停止新注册和套餐升级，同时暂停计费，让现有订阅免费继续运行，预付年费者将获部分退款；12 月 10 日进入只读阶段，公告注明这一阶段不包括 SQLite 与 blob 存储；2027 年 1 月 10 日结束其余服务；2 月 1 日计划开放源码。只读、完全关闭和开源是不同日期，不能把最后的开源计划误解为托管服务会一直运行到二月。

对用户来说，脚本源码、定时任务、环境变量、数据库与文件存储需要分别规划去向。即使代码开放，原有云环境与运营依赖能否低成本自托管仍待确认。评论者把 Val Town 视作方便运行小型 TypeScript 程序和自动化任务的空间，其退出也提醒人们：开发体验出色与商业可持续性是两回事。本文仅记录公告时点的安排，退款与开源是否实际兑现需以后核查。

## HN 评论精华

- [simonw](https://news.ycombinator.com/item?id=50444320) 喜欢项目的想法，却理解在这种产品形态上维持业务很难，并期待看到源码。
- [rpmisms](https://news.ycombinator.com/item?id=50445184) 提醒企业服务的源码未必容易缩小为个人自托管部署。
- [dkmar](https://news.ycombinator.com/item?id=50447806) 描述自己的用途是定时运行小脚本、检查网页变化并发通知，帮助解释产品为何受到个人开发者欢迎。
- [Onavo](https://news.ycombinator.com/item?id=50444305) 提醒不要把其他开发工具公司被收购也一概写成产品消失。
