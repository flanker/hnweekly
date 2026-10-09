---
layout: article
title: "Postgres 的 AT TIME ZONE 'UTC' 并不像你以为的那样工作"
issue: 812
number: 28
category: data
original_url: "https://bookofrevenue.com/blog/6ab81e9a97a13f0001f7e4e1/postgres-at-time-zone-u-does-not-do-what-you-think-it-does"
hn_url: "https://news.ycombinator.com/item?id=49865312"
date: 2026-10-02
---

## 文章摘要

这是 Book of Revenue 博客上的一篇短文，作者 Tanin（HN 账号 tanin）。副标题是"以及为什么你可能需要把 `AT TIME ZONE 'UTC'` 写两遍"。HN 上的标题后来被改成了更平实的"Postgres `at time zone 'UTC'` 的几个坑"。

**背景：两种时间戳类型。** Postgres 有 `timestamp`（即 `timestamp without time zone`）和 `timestamptz`（即 `timestamp with time zone`）两种类型。作者的理解是：`timestamp` 不包含时区信息，严格说并不代表现实中的某个时刻，就像我们说"下午 5 点"，其实是"我们所在时区的下午 5 点"。Postgres 官方 Wiki 的《Don't Do This》页面也明确建议不要用不带时区的 timestamp。作者说这一点他早就知道，他没意识到的是：自己可能根本躲不开 `timestamp`。

**问题场景。** 很多时间序列产品要画"逐月变化量"的图表，SQL 大致是把数据表和自身做连接，条件是 `a.month_start = b.month_start + INTERVAL '1 months'`。第一个问题是：加一个月这件事本身依赖时区。假设产品按 UTC 运营，而机器默认时区是太平洋时间，那么 `2026-03-01 00:00:00+00` 在 Postgres 看来等于 `2026-02-28 16:00:00-08`，给它加一个月得到的是 `2026-03-28 16:00:00-08`，而不是期望的 4 月 1 日零点（UTC）。

既然按 UTC 运营，那就先转到 UTC 再加一个月，于是改成 `(b.month_start AT TIME ZONE 'UTC') + INTERVAL '1 months'`。结果还是不对，原因在于：对一个 `timestamptz` 使用 `AT TIME ZONE 'UTC'`，会把它的类型从 `timestamptz` 变成 `timestamp`，结果是 `2026-03-01 00:00:00`，末尾没有 `+00`。于是连接条件变成了拿一个 `timestamp` 去和 `timestamptz` 比较。作者原文写的是"这种比较永远为 false"，后来在摘要里修正为"永远为 false……除非你的 Postgres 默认时区是 UTC"（这一点正是 HN 评论指出的）。更让人困惑的是，对两者分别做 `EXTRACT(EPOCH FROM ...)` 得到的值却是一样的。

**解决办法：再转一次。** 要让相等比较如预期工作，得把 `timestamp` 再用一次 `AT TIME ZONE 'UTC'` 转回 `timestamptz`，最终写法是：

```sql
ON a.month_start = ((b.month_start AT TIME ZONE 'UTC') + INTERVAL '1 months') AT TIME ZONE 'UTC'
```

作者的总结是：`AT TIME ZONE 'UTC'` 会把类型从 `timestamptz` 转成 `timestamp`，你就在不知不觉中用上了官方强烈不推荐的类型，而 `timestamp` 有一大堆坑；按 UTC 加月份时，必须先转到 UTC 再加，然后再转回来，所以要写两遍 `AT TIME ZONE 'UTC'`。

## HN 评论精华

帖子 169 分、约 110 条评论。评论区一方面指出了文章本身的几处不准确，给出了更好的写法，另一方面延伸成一场关于"时间到底该怎么存"的长篇讨论。epgui 的评价代表了不少人的感受："我很喜欢这篇文章里居然还有错误。时间戳和时区就是这么微妙，人们总以为它们比实际简单得多，聪明的工程师也一样。"

**纠错与更好的写法。** heurekamala 的评论最有干货：文中"timestamp 与 timestamptz 比较永远为 false"是错的，Postgres 会悄悄用会话时区把 `timestamp` 转成 `timestamptz` 再比较，所以结果取决于 TimeZone 设置。他实测：会话时区设为 `America/Los_Angeles` 时比较结果为 false，设为 UTC 时为 true。"这比永远为 false 更糟：生产环境用 UTC，一切正常；到了加州开发者的笔记本上就不灵了。"他还指出，Postgres 16 起有 `date_add(b.month_start, interval '1 month', 'UTC')`，第三个参数指定按哪个时区做加法，结果仍是 `timestamptz`，根本用不着写两遍 `AT TIME ZONE`。作者 tanin 现身致谢，说自己确实是在西雅图的笔记本上测的，也不知道还有 `date_add`。anitil 回了一句："这种时候我都怀疑世上怎么还有东西能正常运行。"

**语法本身就容易误导。** Felk 认为大部分困惑来自 `AT TIME ZONE` 这一个语法同时负责两个方向的转换，如果分别叫"AS ZONED"和"AS LOCAL"会直观得多。ScanMyTerms 解释得很清楚：`AT TIME ZONE` 是一次类型转换，不是注释。作用在 `timestamp` 上，意思是"把这些挂钟数字当作该时区的时间，产出 `timestamptz`"；作用在 `timestamptz` 上，意思是"把这个瞬间渲染成该时区的挂钟数字，产出 `timestamp`"。语法相同、方向相反，会话时区则是隐藏的第三个参数。他的做法是：事件统一存 `timestamptz`，所有连接（应用、迁移、副本、psql）都强制 TimeZone=UTC，只在边界上转成具体时区；`timestamp` 适合存那些本来就不是"瞬间"的东西，比如商店营业时间、生日。

ulrikrasmussen、globular-toast、saltcured 都指出命名才是罪魁祸首：`timestamp` 根本不是时间戳，只是一个需要结合时区解释的日期加时间，应该叫 datetime；`timestamp with time zone` 也并不存储时区，它存的是自 2000 年起的微秒数，"with time zone"只表示输入时按时区解释，时区信息解析完就丢了。globular-toast 甚至认为文章那句"timestamp 不代表现实中的时间"同样有误导性：如果非要说，`timestamp` 代表的才是"现实世界"的时间，`timestamptz` 代表的是"宇宙"里的时间。jokull 则指出作者把 Wiki 的建议过度泛化了，Wiki 原话是"不要用 timestamp 存 UTC 时间"；netzvieh 贴出 Wiki 原文，说明两种说法其实都在。

**"统一存 UTC"够不够？** beybol 说他从不在数据库层做时间计算，库里只存 UTC，在应用层结合用户时区计算，好处是可控、可单测。ahoka 讲了一个反例：某产品用 UTC 存营业时间，每年得用脚本把日期改写两次，因为营业时间本来就是本地时间，夏令时一变 UTC 值就跟着变。Macha 认为 `timestamptz` 基本是个没用的类型：存过去的事件，直接存 UTC 就行；存未来的"人类时间"，它又会急着转成 UTC，等时区数据库更新时你已经不知道写入时用户的原意是什么。他建议用普通 `timestamp` 加一个时区字符串列。WorldMaker 说他职业生涯到这个阶段，绝不会选纯 UTC 存储而不存 UTC 偏移，SQL Server 的 `datetimeoffset` 只多两个字节就能保留偏移，Postgres 也该有类似类型。

由此展开的支线相当长：pseidemann 提出一个启发式规则，需要"粘"在日历上的时间（如日程、每天 15 点的提醒）存不带时区的 datetime，需要标记物理时刻的（日志、计时）存 UTC；skrtskrt 和他来回争论这两者是否只是展示层的差别。jasode 把它们称为"科学时间"和"文化/政治时间"，并感叹业界没有统一的术语，导致很多开发者根本没意识到两者的区别；masklinn 补充了未来时间的不确定性，时区偏移在事件发生前随时可能被立法改变，萨摩亚就曾在不到一年的预告期内直接从日历上删掉一天。toast0 和 masklinn 提到可以同时存时区 ID 和偏移，在加载新版 tzdb 时校验两者是否一致，JavaScript 的 Temporal API 正是这样序列化的。

**"加一个月"本身就没有良好定义。** layer8 指出即使不涉及时区，对 28 号以后的日期加月份也没有明确定义，它甚至不满足结合律：加四个月再减四个月可能回不到原点。cryptonector 引用了 SQLite 文档中关于 ceiling 和 floor 修饰符的说明。linasdev404 分享了一个实例：HyperFormula 3.4 对 `EDATE(2028-01-31, 1)` 返回 2 月 28 日，而 Excel、Google Sheets 和 Numbers 都返回 2 月 29 日（2028 是闰年）；HyperFormula 的维护者 greenflux 随即确认问题并提交了修复。tibbar 还提到，他今年夏天在 AI 的帮助下发现 Postgres 在夏令时"拨快"空档内比较 `timestamp` 与 `timestamptz` 时存在不一致，会出现 B > A、B < C 但 C = A 的情况，可能导致 btree 索引返回错误结果，邮件列表认定这是 bug，但目前没有好的修法。
