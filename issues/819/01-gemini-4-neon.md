---
layout: article
title: "Gemini 4 Neon：在复杂任务中平衡能力、速度与成本"
issue: 819
number: 1
category: favorites
original_url: "https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-neon/"
hn_url: "https://news.ycombinator.com/item?id=50552424"
date: 2026-11-19
---

## 文章摘要

Google 于 11 月 18 日介绍 Gemini 4 Neon，将它定位为面向复杂编程、分析及企业工作的模型，在 Gemini 4 家族中兼顾推理深度与执行效率。官方公布一百万 token 上下文窗口，API 每百万输入 token 为 3 美元、缓存输入为 0.15 美元、输出为 15 美元，并宣布向相应付费开发者和订阅用户陆续开放。具体产品入口与账号资格仍须分别确认，不能把发布日理解为所有账号立即可用。

文章特别强调长时间代理工作，包括跨文件调试、减少重复读取、处理中途纠正和上下文压缩。Google 报告 Neon 在 SWE-sweep 上使用 mini-SWE 框架得到 12.8%，换用 Antigravity 后达到 39.0%。这组对比也说明，代理成绩受模型外部工具和执行框架影响，不能只看模型名称。官方还列出高、极高等思考档位下的任务成本，读者比较时应同时考虑档位、输出量和完整任务开销。

上述性能与安全数字来自发布方及其引用的评测，本期没有独立复现，也未运行页面中的游戏、三维场景或表格演示。内部较低的编造率及特定提示注入测试中的好成绩，不构成真实环境永不出错的保证。对工程团队更有用的验证，是选取已有验收标准的修复任务，比较完成率、审查负担、耗时及总费用。

## HN 评论精华

- [3wolf](https://news.ycombinator.com/item?id=50552517) 调侃 Google 的命名越来越难懂，反映读者难以从名称判断模型定位。
- [maplet](https://news.ycombinator.com/item?id=50552543) 质疑不同排行榜下的价格竞争力；这是评论者的比较，不能替代同一任务、同一设置的成本测试。
- [mrbungie](https://news.ycombinator.com/item?id=50552513) 在编辑中纠正了最初对 Antigravity 可用性的判断，又提到学生账号资格问题。账号体验不宜直接推广为所有订阅用户的情况。
