---
layout: article
title: "AI 让写代码更快，为何交付没有同比加速？"
issue: 818
number: 48
category: watching
original_url: "https://cabird.com/talks/shipping-faster/"
hn_url: "https://news.ycombinator.com/item?id=50455332"
date: 2026-11-13
---

## 文章摘要

Christian Bird 的演讲把开发活动与用户最终得到的成果分开观察：更多代码、提交和原型，并不自动意味着更多可靠功能已经上线。本次核读了演讲页面及其[十九页官方讲义](https://cabird.com/talks/shipping-faster/why-arent-we-shipping-faster-now-handout.pdf)，没有观看视频。讲义引用一项覆盖逾五十万 GitHub 开发者的工作论文，以提交增长约 240%、实际发布增长约 30% 的对照提出问题；这些是被引用研究的结果，不是适用于所有团队的固定比例。

另一组材料来自三十次关于长期维护产品的访谈。AI 帮工程师一天修复多年性能问题，也让非维护者更容易修改复杂系统，但评审、测试、集成、协调和部署能力没有同速扩张。作者特别关注资深工程师被大量审查任务挤占时间，以及管理层把快速演示误认为完整生产交付的风险。

讲义并未否认模型进步：它列出代码审查代理检出率提高、人机组合进一步提高的研究，同时把完全无人审批限制在范围窄、后果低的变更。结论是让验证能力与生成能力共同增长，按成果衡量效率，并保护承担上线责任的人。部分研究仍在投稿或预印本阶段，访谈与单公司偏好调查也有样本边界，不能据此断言所有团队都没有提速。

## HN 评论精华

- [jayd16](https://news.ycombinator.com/item?id=50455510) 追问排除代码行数和提交量后，有哪些证据说明用户更快拿到功能。
- [adriand](https://news.ycombinator.com/item?id=50455655) 分享两小时多完成小功能及数周完成复杂解析器的案例，说明不同任务的收益差别很大。
- [9cb14c1ec0](https://news.ycombinator.com/item?id=50455594) 说新增能力用于重构、修漏洞和缩短支持响应，客户未必会把这些改善看作新功能。评论提供个人经验，不是对照实验。
