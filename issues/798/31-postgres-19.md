---
layout: article
title: "展望 Postgres 19：是时候谈谈“时间”了"
issue: 798
number: 31
category: data
original_url: "https://www.pgedge.com/blog/looking-forward-to-postgres-19-its-about-time"
hn_url: "https://news.ycombinator.com/item?id=48506372"
date: 2026-06-19
---

## 文章摘要

标题中的"时间"一语双关，指向 Postgres 19 期待已久的原生**时态表（temporal tables）**支持——它能回答一个根本性问题："这条数据在上周二是什么样子？"这填补了自 SQL:2011 标准引入时态功能以来长达十年的空白。

主要新特性包括：

- **原生时态表与 WITHOUT OVERLAPS**：无需复杂变通，只需 `PRIMARY KEY (product_id, valid_at WITHOUT OVERLAPS)` 这样的简洁语法，即可自动杜绝时间区间重叠。
- **FOR PORTION OF 形式的 DML**：用一条语句即可更新或删除数据区间内的特定时间窗，如 `UPDATE products FOR PORTION OF valid_at FROM '2025-03-01' TO '2025-09-01'`，Postgres 会自动拆分行并防止产生空隙。
- **时态外键**：借助 PERIOD 关键字，外键约束可校验被引用数据是否覆盖了整个有效期，从而在时间维度上保证引用完整性。

相比旧做法（需要 btree_gist 扩展、复杂的排他约束以及应用层手动管理区间），Postgres 19 通过内建的时间感知能力让时态数据管理变得直观可靠。不过它仍有明显局限：暂不支持系统时间（事务时间），只支持应用时间的时态表，也就是说只实现了双时态（bitemporal）功能的一半。

## HN 评论精华

**pjungwir**：表示自己正是该特性的开发者之一，感谢大家的期待——他常听到"没人需要这个"的质疑，或许这正是各厂商迟迟不加的原因；系统时间尚缺，若无人接手他希望亲自来做，并附上了自己的时态特性路线图演讲。

**horticulturist**：感慨双时态数据极难向人解释，尽管它对系统中几乎每个关键数据域都至关重要——即便对理解该概念的资深开发者，也常让人"大脑融化"。

**IgorPartola**：用一个生动案例说明价值——追踪会手动录入、易出错的州销售税率：当你今天才发现 2026 年的税率应是 7.35% 而非已录入的 7.25%，时态+版本化能正确表达"它从 1 月起就该是 7.35%"，而非"今天才改变"。

**jacques_chester**：兴奋地认为这一特性对推进双时态设计的作用，将超过过去几十年的纸上谈兵；区间是实现它的绝佳基底，他曾在不支持的 SQL 数据库里手工实现，既笨拙又性能平平。

**ris**：略感不安——UPDATE 操作竟会向表中新增行，这打破了很多 DBA 的固有假设。

**bonesmoses**（文章作者）：评价 Postgres 19 是个扎实的版本，自 v10 以来还没见过单个版本里有这么多"新东西"。
