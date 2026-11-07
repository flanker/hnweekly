---
layout: article
title: "HackerTube：用 HN 推荐记录重做 YouTube 发现页"
issue: 817
number: 45
category: watching
original_url: "https://karpathy.bearblog.dev/hacker-tube/"
hn_url: "https://news.ycombinator.com/item?id=50319510"
date: 2026-11-06
---

## 文章摘要

Andrej Karpathy 不满 YouTube 首页充斥夸张缩略图与重复推荐，于是让 Claude Code 根据 Hacker News 历年评论搭建自己的视频发现站。按他的介绍，流程先下载评论、筛选 YouTube 链接，再用 Sonnet 分类并去除垃圾推荐，最终从约四万条评论中整理出 6,454 个视频，覆盖 2007—2026 年及约 150 个主题。这些数字是作者对项目数据集的描述，并不代表每个视频都经过独立编辑审核。

HackerTube 保留熟悉的视频网站界面，却改变了筛选依据：使用视频中点画面代替上传者设计的封面，提供键盘快捷键，并把个性化状态保存在浏览器本地，不要求用户建立账户。作者也开放基础数据下载，方便别人用不同规则重做站点。由此，社区长期积累的推荐可以脱离原来分散的讨论串，成为一份可重新组织的目录。

作者称整个过程约两小时，自己主要处理 Google API 密钥、服务器和域名，其余部署工作交给模型。但这是一段个人开发记录，不能据此认定安全加固已通过独立审计，或把订阅额度等同于零成本。公开的是数据，并非完整服务器源码；调用 YouTube 元数据也有单独的使用条件。

## HN 评论精华

- [karpathy](https://news.ycombinator.com/item?id=50319793) 回答成本问题时称，统计得到约 5.04 亿输入 token、760 万输出 token，九成输入为缓存，主要消耗来自 Sonnet 分类。订阅使用量与 API 标价不能直接混为一个实际账单。
- [karpathy](https://news.ycombinator.com/item?id=50320084) 说明未公开服务器代码，且不能一并分发 YouTube 元数据；复用者需自行取得相应 API 访问。
- [ssl-3](https://news.ycombinator.com/item?id=50320431) 提供不同体验：长期主动点赞、屏蔽和标记不感兴趣后，他的 YouTube 推荐相当好。争议也涉及用户愿意花多少精力训练推荐系统。
