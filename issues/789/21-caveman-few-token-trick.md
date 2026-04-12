---
layout: article
title: "Caveman：何必用多 Token，少 Token 也能行"
issue: 789
number: 21
category: code
original_url: "https://github.com/JuliusBrussee/caveman"
hn_url: "https://news.ycombinator.com/item?id=47647455"
date: 2026-04-10
---

## 文章摘要

Caveman 是由 Julius Brussee 开发的一个 Claude Code 技能插件，它通过让 AI 代理采用"穴居人式"的简洁语言风格来大幅减少输出 token，平均节省约 65-75% 的 token 消耗，同时保持 100% 的技术准确性。项目名称致敬了美剧《The Office》中 Kevin 的经典台词："Why use many word when few word do trick"（何必用多词，少词也能行）。

Caveman 的工作原理很直接：它删除冠词（the、a）、礼貌用语（please、thank you）、填充词和不影响技术含义的客套话，使用更短的同义词，允许句子片段和不完整句，但严格保留技术术语和代码块不变。核心思路是：AI 输出中大量的"人情世故"其实消耗了宝贵的 token，但对技术工作毫无帮助。

该工具提供了四个强度级别：Lite（保留语法的专业简洁风格）、Full（默认穴居人模式，使用句子片段）、Ultra（电报式极限压缩）以及文言文模式（使用古典中文进行最大程度的 token 压缩）。此外还附带了一系列配套工具：`caveman-commit` 生成 50 字符以内的 conventional commit 消息，`caveman-review` 提供单行 PR 反馈（如 "L42: bug: user null. Add guard"），`caveman-compress` 重写记忆文件（如 CLAUDE.md）以减少约 46% 的输入 token。

安装支持多种 AI 代理平台，包括 Claude Code（通过 marketplace 安装）、Gemini CLI、Cursor、Windsurf 和 Cline 等。在 Claude Code 和 Gemini CLI 中可以自动激活，其他代理需要手动触发或在系统提示中集成。

该项目在 GitHub 上发布后迅速走红，星标数在半天内从几十颗飙升到 500，后来突破了 20,000。在 Hackaday 上也获得了报道。一项 2026 年 3 月的研究论文为这一方法提供了理论基础，研究表明"约束大模型给出简短回答在某些基准测试上提高了 26 个百分点的准确率"，甚至逆转了模型性能排名。这说明减少输出 token 不仅是省钱，还可能提升模型表现。

## HN 评论精华

1. **关于思维 token 与输出 token 的区分**：项目作者在讨论中澄清，Caveman 的目标不是减少隐藏的推理/思维 token，而是针对可见的输出文本——减少前言、填充词和虽然精致但非必要的文本。这一澄清很重要，因为思维 token 对模型推理质量至关重要。

2. **"AI 话痨"引发的共鸣**：大量评论者表达了对 AI 工具过度啰嗦的不满，称之为"AI 八股文"。有人指出，AI 总是用"Great question!"开头，然后用三段话说一句话能说完的事情，这种冗余在按 token 计费的场景下尤其令人沮丧。Caveman 精准地击中了这个痛点。

3. **对准确性的担忧**：一些评论者质疑在极端压缩模式下是否真能保持 100% 的技术准确性。有人提出，当上下文被过度压缩时，可能会丢失重要的限定条件和边界情况说明，建议在生产环境中谨慎使用 Ultra 模式。

4. **文言文模式的讨论**：文言文模式引发了有趣的讨论。有评论者指出，中文（尤其是文言文）的信息密度本身就比英文高，用更少的字符表达更多信息，这种语言特性天然适合 token 压缩。也有人认为这更多是一个有趣的概念验证，而非实际可用的功能。

5. **对 LLM 默认行为的反思**：有评论者从更深层次讨论了为什么 LLM 默认就那么啰嗦——这是 RLHF（人类反馈强化学习）训练的结果，因为人类评价者倾向于给更详细的回答更高分。Caveman 本质上是在用系统提示来抵消 RLHF 带来的冗余偏好，这暴露了当前 AI 对齐方法的一个有趣悖论。
