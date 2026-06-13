---
layout: article
title: "软件是在两次提交之间诞生的"
issue: 797
number: 19
category: code
original_url: "https://zed.dev/blog/introducing-deltadb"
hn_url: "https://news.ycombinator.com/item?id=48492533"
date: 2026-06-12
---

## 文章摘要

Zed 团队推出 DeltaDB，一套为 AI 辅助开发时代设计的全新版本控制系统。其核心观点是：传统 Git 工作流围绕离散的 commit 展开，但现代软件开发——尤其是与 AI agent 协作时——是在持续不断的对话中发生的。团队认为"生成代码的那段对话，正在成为软件真正的源头"，而 Git 却强迫协作只能发生在代码提交、推送之后。

DeltaDB 围绕这一现实重新设计版本控制：它不以 commit 为单位，而是把每一次操作记录为细粒度、带稳定标识的"delta"，让对话与代码在整个开发过程中保持同步。

主要特性：

- **细粒度追踪**：每次操作都获得可寻址的独立标识，可引用任意演化时刻的代码。
- **对话与代码耦合**：消息与对应的编辑被并排记录，避免二者随时间漂移脱节。
- **无冲突协作**：基于复制的工作区（replicated worktrees），多人与多个 agent 可在不同机器上同时编辑同一文件。
- **动态交叉引用**：代码引用在文件变化时依然有效，用户可在对话与其产生的代码之间跳转，甚至追溯每一处由谁修改。

主要用例是与 AI agent 的实时协作：队友可加入进行中的工作，与 agent 讨论、批注，无需等待提交，从而省去 pull request 的繁文缛节。

## HN 评论精华

**Lindby**（311 赞）：主张写小而原子的 commit，并用 rebase 清理历史。认为 commit 应当讲述一个连贯的故事，而不是暴露所有中间过程；PR 评审的痛点源于在分支层面而非 commit 层面审查。

**marliechiller**：指出存在两个对立的开发者阵营——一派把 git 当作"粗糙的自动保存"、合并时 squash，另一派偏好刻意的原子 commit。前者更普遍，因为 GitHub 的设计和 stacked PR 支持更倾向于它。

**isityettime**：质疑"详细审查每个 commit"到底是 GitHub 的局限还是根本问题，认为 Phabricator、Gerrit 等工具处理方式不同。

**danpalmer**：解释 Phabricator/Gerrit 的工作流产生的变更单元大于单个 commit、小于整个 PR；切换平台是权衡取舍而非纯粹的改进。

**jasonkester**：强烈反对 squash commit，认为保留细粒度历史能借助 `git bisect` 定位数周后才显现的、由某一行改动引发的微妙 bug。

**WorldMaker**：建议用 `git merge --no-ff` 配合 `--first-parent` 过滤，在不引入新工具的前提下，同时保留详细历史与干净的顶层叙事。
