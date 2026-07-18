---
layout: article
title: "Hilo：一款基于二分查找的猜词游戏"
issue: 801
number: 65
category: fun
original_url: "https://hilogame.cc/"
hn_url: "https://news.ycombinator.com/item?id=48934092"
date: 2026-07-17
---

## 文章摘要

（说明：原网站抓取时返回 403，以下内容主要基于作者 ludovicianul 的 Show HN 与讨论区还原。）

Hilo 是一款每日更新的猜词游戏，把程序员熟悉的"二分查找"（binary search）思想搬进了文字游戏。谜底是一个 7 个字母的英文单词。它和 Wordle 的核心区别在于反馈机制：Wordle 告诉你某个字母"在不在词里、位置对不对"，而 Hilo 针对每一个位置告诉你——真实字母在字母表里比你猜的"更靠前（lower）"还是"更靠后（higher）"。于是最优策略不再是靠词汇联想，而是对每个位置做二分查找：先猜 M，若提示 higher 就往 N–Z 的区间收缩，如此逐步逼近，颇有算法解题的乐趣。

作者解释了选用 7 个字母的原因：他先试过 5 个字母，发现太简单；6 个字母居中；最终选定 7 个字母以保证足够的策略深度。喜欢它的玩家（如 jszymborski、snarf21、troyvit）正是欣赏这种"确定性策略"——相比 Wordle 有时靠运气蒙，Hilo 更像一道可以严密推理的题。

但首发版本的门槛也劝退了不少人，构成了讨论区的主要反馈。最集中的抱怨是"太难"：7 个字母的单词本身就不好想，很多人抓耳挠腮也拼不出合法词。其次是词库判定过严——像 plaudit 这样的常见词一开始都不被接受；作者随后紧急扩容，把词库放开到约 32,000 个全部 7 字母英文单词。此外还有一批可用性问题：左下角的"Go"（提交）按钮对比度太低、很多人根本没看到，导致以为游戏卡住了；键盘上的颜色提示没有按位置区分、参考价值不大；以及普遍呼吁增加"练习模式"和"往期存档"。作者 ludovicianul 在讨论中持续更新迭代，逐一回应了这些问题。

## HN 评论精华

- **jszymborski** 与 **snarf21**、**troyvit**：代表了叫好的一派。他们盛赞 Hilo 相比 Wordle 有更扎实的策略深度——Wordle 偶尔靠猜，而二分查找机制让每一步都能严密推理。snarf21 还顺带问了长期的词库授权计划。

- **多位玩家**（vibcdingenjoyer、ronbenton、chrisweekly、mostly_harmless）：几乎一致地反映"太难"。vibcdingenjoyer 干脆替大家把话挑明——"就是有点太难了"；mostly_harmless 则点出根源：7 个字母的词实在太长，一时半会儿想不出几个。

- **pimlottc**、**4chandaily** 与 **saberience**：反馈词库判定过严。pimlottc 的第一个猜测 plaudit 就被拒；这直接促使作者把词库扩容到约 3.2 万个 7 字母单词。pimlottc 另外还建议：上下箭头（higher/lower）不够直观，改成左右箭头也许更清晰。

- **jonwinstanley** 与 **eoanermine**：点出了一个致命的可用性问题——提交用的"Go"按钮藏在左下角、对比度又低，很多人（包括 vova_hn2 在手机上）压根没发现要按它，误以为游戏没反应。

- **dec0dedab0de**、**xyzsparetimexyz** 与 **zote**：集中呼吁增加"练习模式"和"往期谜题存档"，好让新手先练手、也能补做错过的每日挑战。

- **rbrodie** 与 **mNovak**：从交互细节提建议——应该把最近一次的猜测显示在最上方而不是最下方，方便对照推理，减少来回滚动。
