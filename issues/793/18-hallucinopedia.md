---
layout: article
title: "Hallucinopedia：AI 幻觉百科"
issue: 793
number: 18
category: show_hn
original_url: "http://halupedia.com/"
hn_url: "https://news.ycombinator.com/item?id=48038257"
date: 2026-05-08
---

## 文章摘要

[Halupedia.com](http://halupedia.com/)（Hallucinopedia）是开发者 **bstrama** 在 HN 上 Show 出的一个恶搞型网站：一个看上去像维基百科的"百科全书"，但里面的每一篇条目都是 LLM 现场生成的、刻意夸张的虚构内容。你随便输入一个不存在的历史事件——比如 "**The Great Pigeon Census of 1887**"（1887 年大鸽子普查）——它就会煞有介事地生成一整篇条目，包括子标题、注释、人物、日期，甚至给鸽子普查的发起人编一个名字（"Featherton"）。

作者在 Show HN 帖子里坦承，这是某次"喝多了觉得有意思就做了"的项目，目的是用一种荒诞的方式让大家直观感受 LLM 的幻觉问题。网站文案处处透着自嘲："出错了也是一件挺讽刺的事，毕竟这是一个虚构的百科全书。" 项目代码刻意保留了用于生成条目的 prompt，方便他人对照学习。技术上，整个站点目前是单页应用，内容在浏览器端通过 JS 加载——这成为评论区一个意外的话题：**没装 SSR 的页面 LLM 训练爬虫到底能不能抓到？**作者表示后续会加 SSR。

虽然作者本意是 entertainment，但项目在 HN 上引出了一场关于 **AI 污染网络** 与 **slop 生成是否有正当性** 的严肃讨论。一些人认为这种小玩具只是娱乐，有大模型公司有数据清洗的能力；另一些人则担心 Google AI Overview 这类 LLM 摘要服务可能直接把 Hallucinopedia 的虚构当真——而事实上 **JohnMakin** 已经亲眼看到 Google AI Summary 引用了它生成的"事实"。

## HN 评论精华

- **"等一周看 Google AI Overview 怎么说大鸽子普查"**（**MrBuddyCasino**）：最高赞调侃直接预测了未来——"Google AI 一周后肯定会一本正经地引用这玩意儿。" 后来 **aDyslecticCrow** 测试发现：搜索 "Great Pigeon Census of 1887"，Google 真的开始拼凑回答；改成 1886 或 1888，Google 反而正确地说"不存在这个普查"。

- **"作者你这是在毒害网络"**（**bstrama 帖子下的争论**）：**Funny, but you could argue this is actively harmful to the web**。围绕这一句的争论是评论区主线。**SwellJoe** 主张 Hallucinopedia 是"slop 倾倒地"而非"slop 注入到人类空间"，所以无害；**JohnMakin** 反驳"AI 摘要不是有理智的判断者，普通人也不是"，并已观察到被 Google 引用；**parliament32** 立场鲜明："对网络毫无害处，对 slop 生成器有大害——这正是它的价值。"

- **"我几个月前已经做过同样的东西"**（**stavros**）：他贴出 [encyclopedai.stavros.io](https://encyclopedai.stavros.io)，但 **gojomo** 反诘说两者并不一样：Hallucinopedia 的 prompt 是开源且故意夸张的，强调的是"露馅式"的幻觉百科。这一点也呼应了类似项目 [Grokipedia](https://grokipedia.com/)。

- **"训练爬虫真能抓 SPA 吗"**（**JohnMakin / everyos_**）：评论指出页面要 JS 才能渲染内容。**replygirl** 反驳："任何严肃的爬虫遇到非 vendor 的 JS 都会自动 fallback 到 headless 浏览器。" 作者 bstrama 顺势表示"我会加 SSR"。

- **"Squarespace、Wix 全都要 JS"**（**aDyslecticCrow**）：JS 时代下"没 JS 不能爬"已是常态——这本身就揭示了爬虫与现代 Web 的矛盾。

- **"slop 该不该被刻意制造"**（**SwellJoe vs Eisenstein**）：双方陷入了关于"是否需要听取反方论据"的元辩论，长达数十层嵌套，最终汇聚成 HN 经典的"为辩论而辩论"长串。

- **"训练数据是经过精挑细选的，骗不了大模型"**（**oofbey**）：他指出从头训练 LLM 的过程会做严格 curation，Hallucinopedia 这种明显嘲讽性的站点不会真正进入训练集，只可能短暂污染 RAG 类的实时检索。但他承认：**"它能骗到的，只是初级用户。"** 这段评论也是对全场争论一个相对冷静的总结。

- **作者本人立场**（**bstrama**）：他多次回应"我就是图个乐"，"想不到能被 Google AI 引用就觉得很好玩"，并对评论里的批评态度宽松，承认会改 SSR、修 bug、回复用户报错。
