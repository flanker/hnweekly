---
layout: article
title: "AI 没有取代 SRE，却可能让他们失去练习"
issue: 817
number: 39
category: working
original_url: "https://newsletter.signoz.io/p/ai-isnt-replacing-sres-its-deskilling"
hn_url: "https://news.ycombinator.com/item?id=50305693"
date: 2026-11-06
---

## 文章摘要

SigNoz 的 Elizabeth 将 AI 运维讨论从「会不会失业」转向技能形成：如果系统承担大量常见故障，人类只处理罕见而复杂的事故，他们还能在哪里获得判断这些事故的经验？文章以 Lisanne Bainbridge 1983 年的《Ironies of Automation》为框架，说明自动化可能让人长时间旁观，却在最困难的时刻突然要求其接管。

作者列举告警归并、已知问题的运行手册执行、日志模式识别和自动修复，描述从已知故障自动处理，到部分理解问题由人验证，再到新型级联故障由人主导的分层方式。隐患是日常读日志、提出假设和理解依赖关系的练习被一并外包。对已有经验的人，这是技能衰退风险；对新人，则可能是从未建立过基本能力，而不是以后再补几次培训就能解决。

文章主张有意识地保留人工练习机会，并让工程师主动参与推理，而非只点击批准。需要把论证的证据边界说清：开头「AI 处理 95%」是设问场景，不是对整个 SRE 行业采用率的测量；医疗和航空例子是跨领域类比，也不能直接证明 SRE 已按同样程度退化。末尾同时介绍 SigNoz 自身的 AI 产品方向，因此本文兼具观点文章与厂商立场，适合用来讨论团队训练设计，不宜当作中立的行业效果评估。

## HN 评论精华

**刻意练习还是把精力移到更高层？** [recroad](https://news.ycombinator.com/item?id=50305937) 建议固定手工编程时间或合并前自测；[okdood64](https://news.ycombinator.com/item?id=50306005) 则说多年工作后，编码已是实现目标的手段，更关心系统如何可靠协作。

**不应据此判断谁热爱技术。** [radlad](https://news.ycombinator.com/item?id=50306224) 提醒兴趣、工作内容与职业疲劳都会变化，有人喜欢底层细节，有人喜欢系统设计。

**老手恢复和新人入门是两回事。** [gtufano](https://news.ycombinator.com/item?id=50306897) 以自己长期离开后仍能恢复编程为例，认为更值得担心的是从一开始就完全依赖模型的人。这仍是经验判断，而非对技能衰退的量化研究。
