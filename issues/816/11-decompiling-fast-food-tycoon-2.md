---
layout: article
title: "反编译《Fast Food Tycoon 2》：城市如何绘制出来"
issue: 816
number: 11
category: favorites
original_url: "https://healeycodes.com/decompiling-fast-food-tycoon-2-drawing-the-world"
hn_url: "https://news.ycombinator.com/item?id=50223739"
date: 2026-10-30
---

## 文章摘要

Andrew Healey 借助 Ghidra 和编码 Agent 研究 2001 年经营游戏《Fast Food Tycoon 2》，也就是欧洲版《Pizza Connection 2》。他把反编译结果建立为可查询索引，再结合运行中的内存和画面观察，验证某个字段或函数究竟控制什么。文章的重点不是让模型猜测游戏机制，而是用静态代码与可观察行为互相约束。

城市画面由背景、精灵和分层重绘组成，渲染器只修复发生变化的区域，并把颜色与光照分开处理。多次绘制控制遮挡，还能勾勒被建筑挡住的人物。昼夜亮度、灯光阈值、室内外视图和行走动画共同制造了细腻的城市感。作者还研究了市民日程与披萨评分等隐藏规则。文中给出了具体观察和可视化，但这次研究不是对整款游戏的完整重实现，也不意味着游戏素材可以自由分发。它更像一次把童年体验拆成可解释机制的工程调查。

## HN 评论精华

- **旧游戏背后的技术。** cellsstudio 对光照和预绘制精灵的组合印象深刻；fredrikholm 回忆折扣游戏区带来的偶然发现，说明冷门作品也能留下长期影响。
- **AI 用来帮助理解。** vintagedave 赞赏作者用 AI 反编译和调查，而不是让 AI 代替观察后直接生成文章；glimshe 同样强调工程经验与工具配合的价值。
- **翻译会影响对规则的理解。** yayachiken 指出文中对德语「Grufties」的解释可能不合游戏语境，认为这里更像对老年人的俏皮称呼。应把这保留为读者纠正意见，而不是把原文的群体标签当作无争议事实。
- **重实现仍涉及素材问题。** mdtrooper 希望类似开放游戏项目能拥有自由许可素材。代码研究、可运行重制版和完整开放内容是不同层次的工作。
