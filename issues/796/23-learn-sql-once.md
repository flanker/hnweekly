---
layout: article
title: "学一次 SQL，用三十年"
issue: 796
number: 23
category: data
original_url: "https://fagnerbrack.com/learn-sql-once-use-it-for-30-years-9aceb0bdee03"
hn_url: "https://news.ycombinator.com/item?id=48347483"
date: 2026-06-05
---

## 文章摘要

Fagner Brack 主张，在所有编程语言中，SQL 拥有独一无二的持久生命力，因为它建立在关系代数——也就是数学——之上，而非建立在行业潮流之上。一条写于 1995 年的 SQL 查询，在 2026 年依然能原样运行；而 JavaScript 框架则每隔十年就要彻底重写一遍。

作者强调："关系代数是数学，而数学没有发布周期。"自 Edgar Codd 于 1970 年形式化关系代数以来，SQL 的声明式本质始终如一。数据库引擎每年都在进步，但用户写的查询却能跨越不同公司、贯穿整个职业生涯而保持不变。对于初级开发者，作者建议投入 40 个小时把 SQL 基本功练扎实——join、子查询、窗口函数、查询计划——这些知识会在长达数十年的职业生涯中持续产生回报。

文章用了一个鲜明的对比例子：一条 1995 年还能正常工作的 SQL 查询，对比一个 2015 年用了 `React.createClass` 等已废弃方法的 React 组件。那段老 React 代码如今会直接报错、需要彻底重写，由此说明框架们在不断更迭心智模型，而 SQL 却保持着一致性。

作者也坦诚 SQL 背负的历史包袱：NULL 的三值逻辑、GROUP BY 的冗余、各家厂商不一致的日期处理、以及一份长达 4000 页、几乎没有实现能完全遵守的标准。这种"固执"在保留向后兼容的同时，也让技术债得以长期延续。

## HN 评论精华

**SQL 不会被取代**：`molf` 套用 Lindy 效应——经历了半个世纪的 SQL，按理还能再活至少同样长的时间；想颠覆 SQL 就像想取代电子邮件，根本不会发生，顶多某个替代技术能切出一小块利基市场。这条评论还引发了一串关于 Lindy 效应的有趣闲聊。

**SQL 强大的真正原因**：`democracy` 提出了与作者不同的看法——SQL 的威力不在于"基于数学"，而在于任何人（哪怕只有最基础的英语能力）都能很快上手并产出价值。业务分析师、各类管理者、手工 QA 人员几分钟就能掌握基础，几小时就能写出较复杂的查询。这种用户友好的工具总能胜过其他一切。

**对论证方式的质疑**：`WA` 认为把 SQL 和 React 对比削弱了论点——SQL 是一门语言，React 只是一款软件；而且 30 年前的 JS 在现代浏览器里照样能跑。他还指出大多数程序员的 SQL 知识落后了 20 年，很多 DB 新特性对自己来说都很"新"。`kibibu` 更是辛辣地说，这篇文章读起来像是"嘿 ChatGPT，给我写篇关于 SQL 的文章"。

**"唯一基于数学"的说法被群嘲**：`red_admiral` 反问"唯一？那 Haskell、原版 SICP 里的 Lisp/Scheme、以及 Lean 这类证明助手语言算什么"；`pimlottc` 调侃 Fortran 名字本身就是"Formula Translation（公式翻译）"。还有评论者推荐了基于关联数组代数的 D4M（teleforce）、Datalog（andersmurphy 称之为梦想，但也认可 SQL 配上 Clojure 的 honeysql 查询构造器加 SQLite + Litestream 已经相当好用）等替代或衍生方案。
