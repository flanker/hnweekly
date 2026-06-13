---
layout: article
title: "Claude Fable 5"
issue: 797
number: 1
category: favorites
original_url: "https://www.anthropic.com/news/claude-fable-5-mythos-5"
hn_url: "https://news.ycombinator.com/item?id=48463808"
date: 2026-06-12
---

## 文章摘要

Anthropic 发布了迄今为止最强的模型：**Claude Fable 5** 与 **Claude Mythos 5**。两者底层为同一个 Mythos 级模型，区别在于面向不同用户：Fable 5 面向通用场景并内置安全防护；Mythos 5 则移除部分防护，仅向经过授权的网络安全专业人员与研究人员开放（通过 Project Glasswing 计划）。Fable 5 在几乎所有基准测试上都超越了此前的版本。

在核心能力上，新模型表现突出：

- **软件工程**：Stripe 称 Fable 5 "把数月的工程量压缩到了几天"，并在一天内完成了一个 5000 万行代码库的迁移。
- **长上下文推理**：可在数百万 token 范围内保持专注，胜任长时间的自主任务。
- **视觉任务**：在从科研图表中提取数据、根据截图重建 Web 应用等方面达到业界领先水平。
- **科学研究**：Mythos 5 生成的分子生物学新假设，在盲测对比中有 80% 被科学家更青睐。
- **知识工作**：在金融类基准与资深分析推理任务上取得最高分。

定价方面，输入每百万 token 10 美元、输出每百万 token 50 美元，不到 Mythos 预览版价格的一半。API 与企业版即刻可用；订阅用户（Pro/Max/Team/Enterprise）在 6 月 22 日前可免费体验，之后需消耗额度并视产能扩展而定。安全方面，新的分类器会检测滥用意图，被拦截的请求会回退到 Claude Opus 4.8，覆盖网络安全、生物/化学及防蒸馏等领域；Mythos 级流量数据保留 30 天且不用于训练。

## HN 评论精华

**simonw**（2613 赞）分享了实际体验：他此前构建了一个将 MicroPython 编译为 WASM 并打包的 Python 库，借助 Fable 5 完成了一个搁置数月的完整 Python 编译为 WASM 的版本，展示了模型处理复杂真实编程难题的能力。

**josephg** 报告称在为协同编辑实现一种新的 CRDT 时，Fable 5 在设计推理上表现出色，甚至自主"为原型写了一个 fuzzer"，发现了他自己漏掉的 bug，显示出在基于不变量的正确性验证上的进步。

**kansface** 描述了一次数据库迁移：Fable 5 将内存分配减少了 46 倍，并找出了 Opus 与 GPT-5.5 引入的 bug，感叹"它真的要来抢我的饭碗了"。

**teiferer** 对模型对比的主观性提出批评，认为"这些比较全凭直觉"，缺乏跨代模型、跨提示工程方式的客观无偏测量。

**garciasn** 反映 Fable 5 的安全过滤误拦了正当的商业潜客挖掘工作，不过重启会话后可以继续，提醒过度激进的内容过滤可能影响实际应用。

**1dom** 同样指出定性对比缺乏量化严谨性，认为 AI 打磨得很精致的输出可能掩盖底层质量问题，质疑这类评估究竟是在衡量技术价值还是视觉呈现。
