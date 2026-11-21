---
layout: article
title: "Jolt：通过 Scheme 运行 Clojure，并连接原生 C 库"
issue: 819
number: 22
category: code
original_url: "https://jolt-lang.net/"
hn_url: "https://news.ycombinator.com/item?id=50519345"
date: 2026-11-19
---

## 文章摘要

Jolt 是以 Scheme 为运行基础的 Clojure 实现，试图在保留语言习惯的同时摆脱 JVM 依赖。官网介绍，编译器读取 Clojure、生成与宿主无关的中间表示，再输出 Scheme；原生环境使用 Chez，浏览器方向通过 Gambit 编译到 JavaScript。编译器本身用 Clojure 编写并可自举，另提供提前编译为独立可执行文件的路线。

原生 C 接口是项目的主要卖点之一，数据库、网络等库可通过它接入现有生态。语言层面支持持久化集合、延迟序列、转换器、多方法、协议、运行时求值及并发设施等，但「兼容 Clojure」仍有明确边界。官方列出没有一般 Java 互操作、反射及部分类生成机制，BigDecimal 不可用，正则表达式实现也与 Java 不同。部分 Java 接口的兼容层不意味着任意依赖 JVM 的库都能直接运行。

HN 讨论一部分转向项目的 AI 开发方式与代码可理解性。作者强调规范和一致性测试，批评者则担心文档及实现是否便于人维护。这两类问题应分别检验：语言功能可以通过测试确认，长期维护能力则需要持续观察。本文依据官网与评论整理，未安装编译器、运行兼容性测试或验证独立二进制部署。对于准备采用的项目，应优先检查实际依赖和原生库连接，而不把功能清单理解成对整个 Clojure 生态的无条件替代。

## HN 评论精华

- [yogthos](https://news.ycombinator.com/item?id=50521602)：作者强调以规范、模块和一致性测试控制复杂度，同时承认大型项目仍会出现缺陷。

- [Jeaye](https://news.ycombinator.com/item?id=50522232)：以 jank 作者身份澄清不同原生 Clojure 方言的定位，尤其区分 C/C++ 互操作与模拟常见 Java 接口。

- [flavio81](https://news.ycombinator.com/item?id=50535101)：欢迎以 Scheme 为基础的实现，同时追问依赖 Java 库的实际项目如何工作，抓住了生态兼容的关键。
