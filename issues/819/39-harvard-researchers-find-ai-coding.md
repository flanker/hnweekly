---
layout: article
title: "代码写得更多之后，评审为什么仍可能成为瓶颈"
issue: 819
number: 39
category: working
original_url: "https://complexdiscovery.com/harvard-researchers-find-ai-coding-agents-shifted-the-bottleneck-to-code-review/"
hn_url: "https://news.ycombinator.com/item?id=50538303"
date: 2026-11-19
---

## 文章摘要

ComplexDiscovery 根据一篇 Harvard 工作论文讨论 AI 编码代理与评审瓶颈。报道援引的是较早版本，写到代码增加而最终完成工作没有可检测增长。本次打开[作者论文](https://fion.ac/jellyfish.pdf)时，文件已更新为 2026 年 11 月 19 日版，因此不能把旧报道数字与新版结论混用。

新版覆盖 655 家公司的 720,542 名工作人员，数据至 2026 年 8 月，利用企业采用工具时间的差异估计影响。作者报告代理采用后代码行增加约 44%，完成的 Jira issue 增加约 13.7%，后者处于边缘显著水平；提交评审到合并的时间增加约 56.1%。这比“产出完全没有增加”更细致：代码活动增长明显，但下游产出增长较小。论文也分别观察排队与开始评审后的时间，避免把整个等待区间都理解为人在逐行审查。

研究不是随机试验，依赖采用前趋势等识别假设，专有平台客户也未必代表全部软件团队。Jira 完成量是产出代理指标，不直接等于业务价值；评审意见增加也不能独自证明所有新增代码质量更差。未来就业和产出的预测来自另建的模型与情景模拟，并非已经观察到的裁员。本期核对新版摘要与关键方法、结果，未复现专有数据分析。报道向法律工作作的类比同样不能当成法律行业已测得的效果。

## HN 评论精华

- [judahmeek](https://news.ycombinator.com/item?id=50538304) 提供原论文链接，帮助读者绕过新闻标题直接检查研究方法和版本。

- [u1hcw9nx](https://news.ycombinator.com/item?id=50538349) 认为这会提高阅读、理解和分析代码能力的价值，而不只是增加写代码的需求。

- [gzell](https://news.ycombinator.com/item?id=50538791) 根据个人经验提出另一种解释：瓶颈可能在上游需求规格，而非评审本身；这提示不同团队需要分别测量。
