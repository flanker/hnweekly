---
layout: article
title: "Ruby 如何区分没传参数与显式传入 nil"
issue: 817
number: 48
category: watching
original_url: "https://www.aha.io/engineering/articles/moment-of-ruby-an-argument-for-nothing"
hn_url: "https://news.ycombinator.com/item?id=50343293"
date: 2026-11-06
---

## 文章摘要

Chris Zempel 介绍一个很小却容易忽略的 Ruby 语义：默认参数表达式只会在调用者省略参数时执行。若某个旧接口同时承担读取和设置功能，就不能简单用参数的真假值决定行为，因为 false 和 nil 本身也可能是调用者希望存入的合法值。只检查 nil 同样无法区分「没有提供参数」和「明确提供 nil」。

常见办法是创建一个唯一的哨兵对象，把它作为默认值，再用对象身份判断调用者是否传参。文章展示另一种写法：在默认值表达式里给一个局部标志赋 true。省略参数时，赋值发生，方法走读取分支；显式传入任意值时，默认表达式跳过，标志仍被 Ruby 识别为局部变量，但值为 nil，于是进入设置分支。这里利用的是局部变量与表达式求值规则，不是检查传入内容长什么样。

作者是在 Rails 的 index_with 实现中注意到这一模式，文章也补充了 byroot 引入该实现的出处。技巧适用于接口确实需要保留「缺省」与「空值」差别的场合，但并不自动优于独立 getter/setter。对维护者来说，节省一个哨兵对象是否值得增加阅读门槛，仍取决于既有 API 合约、代码习惯和清楚的测试。

## HN 评论精华

- [matharmin](https://news.ycombinator.com/item?id=50377995) 更推荐用 Ruby 的赋值方法定义 setter，认为示例应说明为什么必须把读写合并。
- [vidarh](https://news.ycombinator.com/item?id=50378934) 澄清：调用者传入一个赋值表达式时，方法收到的是表达式的结果，不会因此触发方法内部默认参数的标志。
- [lucasluitjes](https://news.ycombinator.com/item?id=50392711) 建议收集可变参数并检查数组是否为空；作者 zemptime 认可可读性，同时指出每次调用会创建数组。这是可读性与分配成本之间的取舍。
