---
layout: article
title: "Ghostty 即将离开 GitHub"
issue: 792
number: 1
category: favorites
original_url: "https://mitchellh.com/writing/ghostty-leaving-github"
hn_url: "https://news.ycombinator.com/item?id=47939579"
date: 2026-05-01
---

## 文章摘要

作者 Mitchell Hashimoto（GitHub 用户编号 1299，HashiCorp、Vagrant 的联合创始人，自 2008 年起活跃于 GitHub 长达 18 年）在这篇情绪化但克制的博文中宣布：他主导的开源终端项目 Ghostty 将逐步离开 GitHub，迁往他处。Hashimoto 在文章中坦承自己对 GitHub 抱有"超出常人甚至有些不理性"的情感——他从大学时代起就在课间贡献代码，连蜜月期间都会抽空提 PR；当年创建 Vagrant 的目标之一就是希望能够拿到 GitHub 的 offer。正因为爱得深，他对 GitHub 近年来的下滑感到尤其失望。

直接导火索是平台**持续的可靠性恶化**。Hashimoto 表示自己在过去一个月几乎每天都记录到 GitHub 故障：PR 评审无法完成、Actions 频繁失败、UI 卡顿等。文章发表当天，他正因 GitHub Actions 的故障而无法 review PR——这种状况已经从异常变成了常态。他明确指出，问题不在 Git 协议本身，而在于围绕 Git 构建的那一层服务（Issues、Pull Requests、Actions、CI/CD 等），这些"企业级服务层"已经稳定不到能承担"严肃工作"。

迁移策略采取**渐进式**：Ghostty 会逐步移除对 GitHub 各项功能（Issues、Discussions、Actions、Releases）的依赖，并在原 URL 保留一个只读镜像，避免链接腐烂。具体的目标平台还没确定，作者透露自己正与多家供应商（既有商业产品也有开源项目）洽谈，未来几个月会公布更多细节——业内常被提及的候选包括 Codeberg、GitLab、sourcehut、自建 Forgejo 等。Hashimoto 个人的其他项目暂时仍留在 GitHub，因为 Ghostty 是社区影响最大、最值得迁出的项目。

文章还有一个时间细节：本文是提前一周写好的，时间上恰好与 4 月 27 日的大规模 GitHub 故障重合，但他强调这只是巧合——决定酝酿已数月，并非临时反应。他特别为多年来公开批评 GitHub 团队成员的措辞道歉，但坚持自己的愤怒来源是合理的：是被自己最爱的工具一次次阻挡。

## HN 评论精华

1. **idan（GitHub 员工）**：呼吁不要轻易放弃 GitHub。他认为微软的收购并非 Heroku-Salesforce 那样的失败案例，平台问题更多源于规模膨胀而非恶意。希望有责任感的开发者继续推动 GitHub 改进，而不是出走。

2. **margalabargala**：从用户角度反驳——只靠"留下来"无法改变现状。GitHub 的可靠性已经在下滑（PR 评论丢失、Actions 频繁罢工），作为外部用户没有任何杠杆推动改进，唯一能传达的信号就是用脚投票。

3. **jordemort（前 GitHub 员工）**：从内部视角提供了结构性诊断。微软收购后，GitHub 的组织目标从"做一款好产品"变成"满足多元业务诉求"，内部激励机制混乱，许多团队为了 OKR 不在乎产品体验是否变差。这是组织层面的病灶，难以靠个体修复。

4. **amluto**：指出 GitHub 的衰退是**主动设计的**，而不是规模扩张的副作用。新版代码审查界面、越发臃肿的 Actions、推 AI 入口——这些都是产品决策，与用户增长无直接关系，反而拉低了体验。

5. **IanCal**：批评 GitHub 想当"开发者一切工具的入口"而失去焦点。在 AI 辅助编程时代，开发者对 CI/CD 的稳定性需求更高（一次失败的 Action 就让 agent 卡死），GitHub 应该回到核心：把代码托管和基础 CI 做扎实，而不是堆 AI feature。

6. **watwut**：从生态角度看好这件事——GitHub 长期接近垄断对开源不健康，知名项目出走会推动 Forgejo、Codeberg、Sourcehut 等替代方案的成熟，让生态重新出现竞争。

7. 评论区还普遍提到一个矛盾点：Ghostty 的迁移意味着开发者要重新建立 issue、CI、release 流水线，迁出成本高得吓人；这恰恰说明 GitHub 多年来沉淀的网络效应（社交图谱、PR 工作流）有多强大，也是它"摆烂"的底气。Hashimoto 这一步对其他维护者具有示范效应，但能否触发更大规模的迁移仍要看接下来的目标平台是否成熟。
