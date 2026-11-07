---
layout: article
title: "如果你喜欢一个网站，就把它备份下来"
issue: 817
number: 6
category: favorites
original_url: "https://blog.mistys-internet.website/post/2026/10/29/if-you-love-a-website-back-it-up/"
hn_url: "https://news.ycombinator.com/item?id=50336518"
date: 2026-11-06
---

## 文章摘要

Misty 从一个消失的茶犬爱好者网站谈起：曾经多年未改、随时可以回访的页面，某天变成了赌博广告域名。即便自己从事网页归档工作，也错过了主动保存它的时机。文章因此鼓励普通读者参与保存，而不是假定互联网档案馆或别的大机构一定已经留下完整副本。个人在意的小站、帖子和资料，往往正是机构未必及时覆盖的部分。

文章按规模介绍工具选择。ArchiveWeb.page 浏览器扩展适合手动访问并记录页面，独立应用提供类似操作而不要求安装 Chrome 扩展；对交互内容，需要实际展开或点击，让相关请求加载进来。较大网站可以用 Browsertrix 设置起始地址和范围，自动遍历链接。作者说明这些工具来自自己任职的公司，并披露 Browsertrix 托管服务试用后收费，而开源版本可自行部署。

归档的重点不只是截屏：WARC 保存请求、响应及相关信息，WACZ 将归档和元数据打包，ReplayWeb.page 可在浏览器回放，也能嵌入网页。保存后应检查页面、图片和交互是否真正可用，再保留多份副本；只有下载动作完成，并不代表所有内容都被捕获。作者希望形成大量由个人和社区维护的小型档案。文章倡议的是主动保存与检查，本期没有替读者抓取网站，也不把可归档等同于拥有公开转载他人内容的权利。

## HN 评论精华

- **不同保存需求有不同工具。** [drpfenderson](https://news.ycombinator.com/item?id=50338928) 分享书签不断失效的经历，并推荐 SingleFile 用于单页保存、ArchiveBox 用于更系统的归档和检索，属于个人使用推荐。

- **静态网站可以更简单。** [zabriel_goss](https://news.ycombinator.com/item?id=50339215) 说自己备份喜欢的博客时使用 wget 已经够用，提醒读者不必为简单内容一开始就搭建完整爬虫平台。

- **检索历史不等于完整视觉归档。** [hagbard_c](https://news.ycombinator.com/item?id=50339353) 介绍 Hister 会保留页面文字供搜索，但不保存图片，还可能记录不需要的临时页面，选择工具时要看保存范围。
