---
layout: article
title: "凭经验观察：程序员不喜欢 reduce"
issue: 810
number: 26
category: code
original_url: "https://evanhahn.com/posts/2026-09-13-programmers-dislike-reduce/"
hn_url: "https://news.ycombinator.com/item?id=49692844"
date: 2026-09-18
---

## 文章摘要

Evan Hahn 写了一篇极短的观察性博文，核心命题一句话就能说完：同样是函数式三件套，`map` 和 `filter` 在代码评审里从来没人挑刺，`reduce` 却屡屡被人留言「这段不好读」。

他的依据完全是个人经验，也坦承这一点。他常年在代码里用 `map` 和 `filter`，提交评审时同事会就其他设计给一堆意见，但基本不会对这两个函数本身有意见。而只要补丁里出现 `reduce`，就大概率会收到一条要求改写的评论。除此之外，他也观察到 `reduce` 在代码库里出现的频率远低于 `map`、`filter`、`some` 这些同族函数。

对于原因，他列了几条自己也没法证实的猜想：`reduce` 本身更难读；大家对它更不熟悉；在某些情况下它的性能确实更差；在他常用的 JavaScript、Python、Swift 里，`reduce` 的写法就是不如在 Clojure 里优雅——他特意提到，在写 Clojure 的那段时间里，从来没人给过他这类反馈；最后一条猜想是「我看错了，这个趋势根本不存在」。

他的实际处理方式相当务实：收到意见就改成别的写法，虽然自己更偏爱 `reduce`，但不觉得值得争。文末他还顺带提了一句略带感慨的观察——最近这类反馈变少了，可能是因为现在的代码评审没以前那么仔细了。全文以一个开放问题收尾：你也有同感吗？你喜欢 `reduce` 吗？

文章本身只有几百字，真正的价值在于它精准戳中了一个几乎所有人都有体感、却很少被明确写下来的行业共识，于是在 HN 上引爆了一场 270 多条评论的大讨论。

## HN 评论精华

这是全期讨论最热闹的一篇（174 分，275 条评论），而且难得地**真的在讨论技术**。观点大致分成几派：

**一、`reduce` 的问题在于它不给你任何类型上的承诺**

得票最高的 **Skeime** 给出了全场最被认可的解释：在命令式语言里，`map` 和 `filter` 的返回值你一眼就知道是什么——前者是等长的列表且每项只依赖对应的原项，后者是原列表元素的一个子集且元素本身不变。这些约束本身就是信息。而 `reduce` 的结果可以是任何东西，在命令式语言里还可能带副作用，「所以它不过是一个语法更糟的 for 循环」。

**Hackbraten** 顺着这条线说，凡是提供这套 API 的命令式语言，都应该在契约里写明传入的高阶函数必须无副作用，并举了 Java `Stream#reduce` 的文档为例（要求累加器是「associative, non-interfering, stateless」的）。**speedstyle** 补充说 Rust 的做法更细致：这些方法接收的是 `FnMut` 而非 `Fn`，`fold` 明确保证迭代顺序，还专门提供了 `inspect` 来容纳副作用。

**二、名字与参数顺序是真正的认知负担**

**rspeele** 的总结被广泛引用：`map` 和 `filter` 大多只有一个参数（有第二个也基本是索引），即便微软把它们叫成 `Select` 和 `Where`，形态也完全一致。而 `reduce` 有累加器和一个双参数回调，各语言之间毫无共识——参数是 `(acc, elem)` 还是 `(elem, acc)`，初值放前面还是后面，都不一样；有些语言的版本干脆不收初值，遇到空集合就是个等着炸的地雷。

**futune** 提出一个有意思的假设：同一个操作有 `reduce`、`fold`、`accumulate`、`inject` 半打名字，光是名字多就让人觉得「这里面有很多东西要学」。**3836293648** 补充说，在某些语境下 `fold` 和 `reduce` 还有微妙区别——后者要求运算满足结合律并有单位元（即 monoid）。**ketzu** 则指出 `reduce` 这个名字本身就误导：结果可以是任意形状，他给了个反例把 4 元列表 reduce 成 12 元列表，「这里的 reduce 是复杂度理论意义上的归约（变换），不是日常语义里的变小」。

围绕 `filter` 语义的一段离题讨论意外地成了全场最有趣的部分。**jiehong** 说自己永远记不清 `filter` 是保留匹配项还是丢弃匹配项：「就像滤网，过滤面条和水时你留面条；但换成茶你就想留水了。」下面接了一长串各语言的对策：Smalltalk 和 Ruby 的 `select` / `reject`、Kotlin 的 `filter` / `filterNot`、Elixir 的 `Enum.filter` / `Enum.reject`、Common Lisp 的 `remove-if` / `remove-if-not`。**SamBam** 贡献了全场最惨的真人真事：熬了三小时的高汤，肌肉记忆作祟直接倒进水槽里的滤锅，眼睁睁看着汤流走。

**三、反方：`fold` 很好用，是你们不懂**

**japgolly** 明确站到对立面，认为文章说的其实是 `fold`（`[A] -> B -> ((B,A) -> B) -> B`），而 `fold` 是把集合压成单值最方便的手段。**wannabe44** 回敬了一句尖刻的「你迟早会发现有个东西叫 for 循环，它很好用」，随后引出一长串对线。**mrkeen** 反驳说 for 循环意味着多次遍历，还得先把答案设成一个错的值，而且会把你踢出惰性/流式的范式；**KPGv2** 并排贴出 `fold sum 0 collection` 与三行 Python、五行 C 的对照，主张一行就能读懂的东西没必要换成更多出错机会的循环。**kaoD** 则一针见血地调侃：「你能用 Hindley-Milner 记法写这条评论，本身就说明你是个异类。」

**四、性能上的真实翻车**

**theamk** 认为在 Python 里 `reduce` 几乎永远不是最优解：能用 `sum`、`min`、`max`、`any`、`all`、`next` 就该用，配合生成器表达式又快又好读，重数值计算该上 numpy。**Pinus** 追问 `sum` 拍平嵌套列表的性能问题修好没有，**theamk** 当场跑了个 benchmark 并直呼 TIL：拍平一个 1000×1000 的嵌套列表，`itertools.chain.from_iterable` 和手写 `extend` 约 0.07 秒，列表推导 0.12 秒，而 `sum(x, [])` 和 `functools.reduce(operator.add, ...)` 都要 13 秒——**慢 200 倍**。**mahboi** 的回复精准点题：「对，这正是大家讨厌 reduce 的那类原因。」

**五、少数派的实用辩护**

**el_oni** 说自己工作里只用过半打次 `reduce`，每次都觉得别扭，但拼接一堆 Spark DataFrame 时 `reduce(DataFrame.union, list_of_dfs)` 确实比手写循环干净得多，「只是现在还得从 functools 里 import，大家就更觉得怪了」。**tehnub** 回复说自己团队全员抗拒函数式，唯独这一个写法人人都乐意用。**ducaale** 给出了一个清晰的适用判据：当两个入参和返回值同类型（`ADD`、`MAX`、`CONCAT`）且存在单位元时，`reduce` 就是对的工具——本质上就是 monoid。

**kccqzy** 认为 Haskell 在这件事上做对了：同时提供 `foldr` 和 `foldl'`，两者回调参数顺序相反，助记法是「折叠函数替换掉列表里的逗号」；正因为大多数语言只提供其中一个，才没人去认真考虑这个维度。

最后还有一条很短但传播很广的吐槽，来自 **olivewong**：很多时候 `reduce` 难读不怪它本身，而是累加器的命名太随意——单字母还算好的，他有个同事把 accumulator 缩写成了 `cum`。
