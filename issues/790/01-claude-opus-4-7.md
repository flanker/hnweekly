---
layout: article
title: "Claude Opus 4.7 发布"
issue: 790
number: 1
category: favorites
original_url: "https://www.anthropic.com/news/claude-opus-4-7"
hn_url: "https://news.ycombinator.com/item?id=47793411"
date: 2026-04-17
---

## 文章摘要

Anthropic 于 2026 年 4 月 16 日发布 Claude Opus 4.7，作为其最新的通用可用模型。相较于上一代 Opus 4.6，这一版本在软件工程和复杂推理任务上有显著提升，被官方定位为"可以放心交付最困难编码工作"的旗舰模型。在扩展的长任务执行过程中，Opus 4.7 展示出更强的一致性和自我校验能力。

视觉能力方面，Opus 4.7 支持最长边达 2,576 像素（约 3.75 百万像素）的图片，处理能力较上代提升三倍以上，在需要细致视觉分析的计算机视觉任务上表现更好。指令遵循方面也有大幅改进，但老用户需要针对新模型重新调校 Prompt 才能获得最佳效果。

官方内部评测显示，Opus 4.7 在多个领域超越前代：金融分析基准达到 SOTA 水平，文档推理任务错误率较 4.6 降低 21%，对技术图表和化学结构的多模态理解显著增强。定价延续 Opus 4.6 的水平——输入 5 美元/百万 token、输出 25 美元/百万 token，可通过 Claude 网页平台、API（标识符 `claude-opus-4-7`）及 AWS Bedrock、Google Cloud Vertex AI、Microsoft Foundry 使用。

新功能方面，模型引入 `xhigh` 努力级别，支持更细粒度的推理延迟控制；Task budgets 以公共测试版形式上线，用于管理 token 开销；Claude Code 中的 `/ultrareview` 斜杠命令也得到增强，支持更详细的代码分析。安全层面维持与前代相当的档位，新增自动侦测并阻止高风险安全请求的网络安全防护机制，同时为合法安全从业者提供 Cyber Verification Program。

## HN 评论精华

1. **自适应思考被强制且不可控**：最受关注的抱怨是 Claude 新的 adaptive thinking 功能——"它会在不该思考时思考"，反而降低性能。有开发者发现"关闭自适应思考 + 提升 effort 等级"才能恢复基线表现，但 Opus 4.7 直接让自适应思考成为强制项，手动指定 thinking 参数会返回 400 错误，用户失去了控制权。

2. **产品矩阵割裂**：多位用户抱怨 Anthropic 的 Claude 产品线（Chat、Code、Cowork）缺乏整合——在一种模式下创建的 projects 无法在另一种模式中使用或共享，MCP 在不同平台上行为不一致，没有统一的项目抽象。有评论讽刺道"Claude Desktop 本来可以轻松做成放手式 IDE，只要它真的把细节打磨好"。

3. **隐藏 thinking 引发信任危机**：在将自适应思考设为强制的同时，模型还"隐藏了 thinking 内容"。Boris 此前公开承认自适应思考有问题后就没了下文，这种不透明的产品决策让开发者对 Anthropic 的优先级判断产生质疑。

4. **依赖奇技淫巧的配置**：许多开发者分享了恢复功能的隐藏开关——`/effort xhigh` 防止偷懒、环境变量 `CLAUDE_CODE_DISABLE_1M_CONTEXT` 关闭 1M 上下文、未文档化的 CLI 标志 `--thinking-display summarized`，以及在系统 prompt 里强调"直接、批判性分析"等。这些技巧证明默认体验并不理想。

5. **经典"洗车测试"翻车**：有人用"洗车测试"验证 Claude 的推理能力——结果模型让用户步行到洗车房而不是开着脏车过去，被追问时先是用模式匹配编了个解释，最后才承认出错。这个小插曲反映出前沿大模型在朴素常识推理上依然不稳定。
