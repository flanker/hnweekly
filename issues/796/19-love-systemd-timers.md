---
layout: article
title: "你还不够爱 systemd timers"
issue: 796
number: 19
category: code
original_url: "https://blog.tjll.net/you-dont-love-systemd-timers-enough/"
hn_url: "https://news.ycombinator.com/item?id=48367904"
date: 2026-06-05
---

## 文章摘要

这篇文章为 systemd timers 摇旗呐喊，主张在 2026 年应当用它来取代传统的 cron 作为定时任务调度方案。作者承认 cron 是计算机领域无处不在的有用原语，但认为它已经过时，存在四个根本性缺陷：一是环境问题，模糊的 `$PATH` 设置让脚本执行变得不可预测；二是输出处理，标准输出和标准错误经常莫名其妙地消失，或者被发到一封没人想看的邮件里；三是可审计性差，执行历史难以追踪和排查；四是可读性差，类似 `01,31 04,05 1-15 1,6 *` 这样的调度语法毫无直观性可言。

systemd timer 是一种 unit 类型，用来按照预设的时间表调度其他 unit（通常是 service）。实现需要两个文件：一个 `.service` 文件包含真正要执行的动作，一个 `.timer` 文件定义时间表并关联到对应的 service。文章用 `roulette.service` 搭配 `roulette.timer`（使用 `OnCalendar=10:00` 实现每天上午 10 点执行）做了演示。

文章详细介绍了两类调度表达式：基于日历的（wallclock 时间，比如 "daily" 对应 `*-*-* 00:00:00`）和基于时间跨度的（相对调度，比如先 `OnBootSec=1h` 再 `OnUnitActiveSec=1h`）。基于时间跨度的方式在实践中很有用，例如一个清理任务可以相对于开机时间来运行，而非固定的钟点。

文章还列举了一系列亮眼特性：`systectl list-timers` 可以一览所有定时任务；`WakeSystem=` 能唤醒休眠的系统去执行重要任务；`RandomizedDelaySec=` 可以避免"惊群效应"式的同时执行；`Persistent=` 能在系统宕机后补跑错过的任务；`ExecCondition=` 提供原生的条件执行能力。作者强调应当善用 systemd 的原生选项（如 `OnFailure=`、`Restart=`），而不是用脚本去硬凑这些功能。关于环境差异，service 默认不继承 shell 环境，用 `ExecStart=/usr/bin/env bash` 可以确保拿到标准的 `$PATH`。作者用一个真实案例收尾：把 Advent of Code 的 API 轮询分散到不同时间间隔，而不是像 cron 那样同步触发，从而削平了协同请求带来的服务器负载尖峰。

## HN 评论精华

**thomashabets2** 对文章关于 cron 复杂性的论断提出质疑：他认为 cron 的 PATH 设置是可控的，并不比 systemd 更难预测；crontab 里本来就预印了语法说明和有帮助的注释；他也质疑对于复杂场景，systemd 的调度语法是否真的更简单。

**jerf** 反驳道：非平凡的 cron 表达式需要理解逗号、斜杠、星号以及它们的各种组合，远没有前者说的那么直白。

**PunchyHamster** 强调了 systemd 在实践中的优势：内置的随机延迟能在多台机器间保持稳定分布；persistent 模式可以补跑宕机期间错过的任务；原生防止任务重叠执行。

**gchamonlive** 提供了一个真实用例：systemd timer 能在系统上线时运行 service，而不必要求系统在某个固定时刻处于运行状态，这一点对备份工作流至关重要。

主要的争论线索集中在三个方面：调度语法的复杂度（基于 ISO8601 的 systemd 格式 vs cron 的字段式格式，两派都没说服对方）；环境可预测性（cron 的 PATH 模糊到底是设计缺陷还是只需显式配置的小问题）；以及功能缺口——大家基本认同 cron 缺少稳定随机化、宕机后强制补跑、防止任务重叠等能力，而 systemd 把这些都处理得很优雅。
