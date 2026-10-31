---
layout: article
title: "Crystal 1.22：原生事件循环、ARM Linux 与运行时改进"
issue: 816
number: 24
category: code
original_url: "https://crystal-lang.org/2026/10/12/1.22.0-released/"
hn_url: "https://news.ycombinator.com/item?id=50194938"
date: 2026-10-30
---

## 文章摘要

Crystal 1.22 的更新覆盖语言、标准库和运行时。常量可以显式声明类型，DWARF 信息改为按需解析并建立索引，回溯结果增加缓存。所有支持目标默认采用平台原生事件循环，libevent 转为可选后备；aarch64 Linux 的 glibc 和 musl 目标升为 Tier 1，Windows ARM64 也首次提供构建。

兼容性变化需要单独看：标准输出或错误输出遇到断开的管道时，默认立即退出，不再按旧方式抛出并打印 IO 错误；此前长期弃用的 Process.fork 被移除。HTTP 解析、头部验证和连接错误处理也有多项修正。原文还追加说明，1.22 曾默认启用的 Kernel TLS 在 1.22.1 中撤回，因此不能把它当作持续有效的新默认值。Crystal 借用了 Ruby 熟悉的语法，但它是独立的编译型语言，这次发布也不意味着既有 Ruby 或 Rails 应用可以直接切换运行。

## HN 评论精华

**语法亲切不等于生态兼容。** sumnole 用「有类型的 Ruby」帮助介绍，onlyrealcuzzo 则提醒区分并发与并行，不能简单把运行时等同于 Go。pelcg 也质疑缺少 Ruby 互操作时的迁移吸引力。

**实际用途有支持也有保留。** sho 分享长期运行小型网络代理的经验，但后来因库生态改用 Go；rubiquity 回忆过去遭遇运行时问题，同时仍认可命令行工具场景。这些是不同时间和环境中的个人经历。

**语言选择也受开发方式影响。** 部分评论认为编码代理降低了语法偏好的重要性，另一些人坚持学习和手写代码本身有价值。opem、alberth 则更直接关注 ARM Linux 与 Windows 支持的进展。
