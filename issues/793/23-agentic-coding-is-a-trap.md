---
layout: article
title: "Agentic Coding 是个陷阱"
issue: 793
number: 23
category: code
original_url: "https://larsfaye.com/articles/agentic-coding-is-a-trap"
hn_url: "https://news.ycombinator.com/item?id=48002442"
date: 2026-05-08
---

## 文章摘要

Lars Faye 在这篇措辞克制但立场鲜明的文章中提出一个反直觉的观点：**完全交给 agent 写代码，本质上是个陷阱**。AI agent 确实能让某些任务变快，但它同时在悄悄侵蚀使用者"有效监督它"所必需的核心技能——结果就是开发者越来越依赖一个自己越来越无法判断对错的工具。这是一种和 FORTRAN→更高级语言、本地→AWS 等历史迁移**根本不同**的转变：以前的工具升级让你的"思考层级"被抬高，但你的动手能力还在；agentic coding 则在直接磨损"动手解决问题"的肌肉。

文章引用了一份 Anthropic 自己的研究结论："Effectively using Claude requires supervision, and supervising Claude requires the very coding skills that may atrophy from AI overuse"。研究里提到一个冲击性的数字：**重度使用 agent 的开发者在调试能力上下降了 47%**。这就是作者所说的 **Supervision Paradox（监督悖论）**：你必须有非常好的代码能力才能审查 agent 输出，但你越依赖 agent，这种能力衰退得越快。同时，agentic 工作流把整个研发的优先级**反转**了——传统开发优先"理解"和"质量"，agentic 优先"速度"和"产出量"，开发者每天 review 的代码量远超人脑可消化的极限。

第三层风险是**供应商依赖**。文章举了一个直接的例子：Claude 出故障时，许多团队整体陷入瘫痪。这种对一家订阅服务、对一种不可预测 token 成本的依赖，是历史上拿固定工资的员工从未给企业带来过的不确定性。Faye 给出的建议不是回到纯手工，而是**把 agent 作为副手而非主力**：根据任务性质，开发者本人保持 20%–100% 的"亲自动手"比例，让大脑和键盘都不脱离代码本身——这样既享受 AI 的速度红利，又不至于把判断力交付出去。

## HN 评论精华

- **fnordpiglet**：站在反方。"我用 agentic coding 这几年学到的东西，比之前 35 年手工写代码还多。"他把 agent 比作"博览群书但缺乏判断力的实习生"——你给它 feedback 它就能改，至少比"大型代码库里没有人真正全懂"这件事要好。

- **byzantinegene**：反驳 fnordpiglet——老兵在被 agent 加速之前，已经掌握了"如何学习"的元能力，所以他们能从 agent 身上薅到知识；但**初级开发者**没有这个底子，过早依赖 agent 只会让他们卡在"看似在做事、实际不理解"的状态。

- **CGamesPlay**：呼吁讨论别走极端。他类比 ChatGPT 让大学论文质量整体下滑：agentic coding 也需要"有品位地使用"，全盘拥抱或全盘抵制都会培养出一代不合格的中间层。

- **keyle（25 年经验工程师）**：现身说法的"认知负债"案例。他在外部集成上重度使用 AI，结果在会议中被问到自己交付的代码细节时一片空白，被迫重新建立"边写边记文档"的习惯，否则连自己的产出都解释不了。

- **larsfaye（作者本人）**：在评论区澄清——文章并不是要求开发者了解整份代码库的每一个细节，而是反对"完全放弃读代码"。他强调"用代码来思考"和"做架构规划"是相互依赖的，不能让 agent 替代任何一边。

- 进一步的延伸讨论指向更深的组织问题：当公司把"产出代码量"作为 KPI、却没人为"被快速生成的代码的长期理解负责"时，agentic coding 就从生产力工具变成了**把技术债转化为人脑认知债**的机器，最终账还是要还。
