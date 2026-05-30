---
layout: article
title: "Claude Opus 4.8 发布：更诚实、更省钱、能并行驱动上百个子智能体"
issue: 795
number: 22
category: data
original_url: "https://www.anthropic.com/news/claude-opus-4-8"
hn_url: "https://news.ycombinator.com/item?id=48311647"
date: 2026-05-29
---

## 文章摘要

**Claude Opus 4.8** 是 Anthropic 于 **2026 年 5 月 28 日**发布的新旗舰模型，作为 Opus 4.7 的升级版，在**价格不变**的前提下，在编程、推理和智能体（agentic）任务上全面提升。

这次最突出的改进是**诚实度（honesty）**：Opus 4.8 让自己写的代码缺陷被悄悄放过的概率，比前代**降低了约 4 倍**；它也更善于标注不确定性、避免无依据的断言。在对齐（alignment）方面，模型「在支持用户自主、以用户最佳利益行事等亲社会特质的衡量上达到新高」，错位行为的发生率较 4.7 更低。基准测试方面，官方援引了多项成绩：法律智能体基准（**Legal Agent Benchmark**，首个在 all-pass 标准上突破 10% 的模型）、**Online-Mind2Web** 拿到 84%（高于 Opus 4.7 与 GPT-5.5）、**Super-Agent** 基准（唯一端到端完成全部用例的模型）、以及在各档「努力等级」上都超越前代的 **CursorBench**。

价格与可用性：输入 **$5/百万 token**、输出 **$25/百万 token**（均与 4.7 持平）；新增的**快速模式（Fast mode）**为 $10/$50 每百万 token，比此前模型便宜 **3 倍**。模型即刻在各平台上线，API 标识为 `claude-opus-4-8`。新能力还包括：**动态工作流（Dynamic Workflows）**——Claude Code 可并行运行**数百个子智能体（subagents）**，从而完成跨越数十万行代码的代码库级迁移；**努力控制（Effort Control）**——用户可调节模型投入的算力（更高努力 = 更高质量但更慢、更费 token）；以及 Messages API 支持在对话中途插入 system 条目而不破坏提示缓存（prompt caching）。

## HN 评论精华

- **NiloCK**：指出这是首个第三次小版本号迭代的前沿模型，质疑 4.6→4.7→4.8 的提升是否还能被用户明显感知——随着模型逼近饱和，增量收益可能越来越难察觉。
- **onlyrealcuzzo**：预测下一代前沿模型可能是「最后几代」，认为 2–3 年内 60–90B 的小模型会超过当前 SOTA；像 GRAM 这类技术或能让 30B 模型在几天训练内追平今天的最强模型。
- **vlovich123 / knollimar / areweai / sebzim4500**：围绕 GRAM（Generative Recursive reAsoning Models）这个缩写吐槽不断——有人说该叫 GRRM（影射乔治·R·R·马丁「迟迟不出结局」），有人说它和常用词「gram」撞车、无法检索；也有人举 LION 优化器（evoLved sIgn mOmeNtum）反讽缩写硬凑是常态。
- **supern0va**：猜测增量版本的提升是否来自蒸馏（distillation），即用更大模型生成合成数据来训练。
- **mrandish / steveylang**：前者推测前沿实验室更愿意维持昂贵的基础设施以撑住高价，刻意回避会让 AI 商品化的降本研究；后者反驳称在 token 受限下，效率提升对 Anthropic/OpenAI 维持定价权、对抗中国模型反而具有战略价值。
- **sometimelurker / onlyrealcuzzo**：前者称 GRAM 类技术「被反复重新发明」却因可解释性/对齐难题而难以使用，缺乏可读推理轨迹会让模型危险；后者反驳说只要输出正确，不可见的潜在推理无关紧要——「就像很多无害的人脑子里也有疯狂念头」。
- **ericd / haldujai / svachalek**：争论前沿智能的边际回报——有人说在治理/组织决策上回报无穷，小幅提升复利成巨大价值；有人反驳市场规模和智能成本会限制回报；也有人指出专用编程模型已遇边际递减，真正价值来自通用智能帮人「以高效偷懒的方式绕开杂活」。
- **mastazi**：把当下 AI 动态类比互联网泡沫，强调泡沫不等于死路——「互联网用量在崩盘后仍以数量级持续扩张了二十年」。
