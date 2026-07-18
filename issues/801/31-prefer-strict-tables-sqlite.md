---
layout: article
title: "在 SQLite 中优先使用 STRICT 表"
issue: 801
number: 31
category: data
original_url: "https://evanhahn.com/prefer-strict-tables-in-sqlite/"
hn_url: "https://news.ycombinator.com/item?id=48873940"
date: 2026-07-17
---

## 文章摘要

SQLite 从 3.37.0 版本（2021 年 11 月）起引入了 STRICT 表——只要在建表语句末尾加上 `STRICT` 关键字（如 `CREATE TABLE people (name TEXT) STRICT;`），SQLite 就会对列类型做严格检查。作者强烈建议默认使用这一特性，因为它能在早期拦截一整类数据完整性问题。

作者指出普通（非 STRICT）表存在几个"反直觉"的行为：其一，向 INTEGER 列插入文本不会报错；其二，你可以给列声明根本不存在的类型（如 DATETIME、UUID、JSON，甚至写错成 BLOBB），SQLite 照单全收，从而把拼写错误或误解悄悄埋进 schema；其三，你甚至可以完全不写类型。更糟的是，SQLite 的类型转换是"尽力而为"：字符串 `'10'` 会被转成整数 10，但无法转换的值（如 `'1O'`）则原样存入整数列，导致同一列里混着不同类型的值。

STRICT 表的优点是：无损转换依然有效（`'123'` 仍会存为整数 123）；提供了 `ANY` 类型以便在确实需要灵活性时使用；作者实测没有观察到明显的性能损耗。其局限在于：无法通过 ALTER 把已有的非 STRICT 表原地转换为 STRICT，必须重建表（若已有非法数据会有丢失风险）；不兼容 3.37.0 之前的版本；而且 SQLite 官方明确"不同意"这一偏好，认为灵活类型在键值存储、导入脏数据等场景中有其合理用途。

## HN 评论精华

评论区的主流观点是希望 STRICT 成为默认，同时也有人细致地解释了 SQLite 为何不这么做。

- **rogerbinns** 贡献了最有价值的历史背景：SQLite 最初是作为本地开发用的数据库库，底层用 dbm 存储，所有值本质上都是字符串，加法运算符会自动把字符串转成数字再运算——这种"无类型"哲学从 TCL 时代一路继承下来。直到 2004 年 SQLite 3 才有了自己的存储后端和 5 种存储类型，但为了 API 兼容性，动态类型被保留了下来。他强调："SQLite 是 fopen 的竞争对手，不是 Oracle/Postgres。你不想在字符串列里放数字？那就别这么做。"

- **simonw** 解释了官方不改默认值的根本原因：SQLite 对向后兼容有极强承诺，不希望针对 3.53 写的软件升级到 3.54 后，因为 `CREATE TABLE` 突然变成创建 STRICT 表而崩溃。**boredatoms** 建议引入"默认值集合"机制，让开发者用一句 `use 2026.1 defaults` 选择新默认；**inigyou** 补充说 WITHOUT ROWID 也面临同样困境。

- **mort96** 吐槽了 SQLite 的另一个痛点：缺少时间戳类型，只能用文本列存 `yyyy-mm-dd HH:MM:SS` 格式并默认它是 UTC，连 ISO 的 `T`/`Z` 都没有。他曾要收拾一个烂摊子——有人往部署到数千台设备的布尔列里存了字符串 `'1'` 和 `'0'`。他总结：这是个伟大的项目，但有些设计决策令人费解。

- **masklinn** 反复澄清了一个技术要点：`NUMERIC` 不是类型而是"亲和性（affinity）"，底层类型只有 real 和 integer，这正是 NUMERIC 在 STRICT 表中不合法的原因。围绕 **petilon** 抱怨"STRICT 模式下没有 Date 类型"，多人指出日期本就不是 SQLite 的类型，只能用数值或文本存储，取舍在于是否愿意失去"这一列语义上是日期"的元数据。

- **Cyberdog** 给出了实用替代方案：若被迫用旧版本或不想重建表，可以用 CHECK 约束来强制列的取值规则（如用 `CHECK (LENGTH(user_id) = 36)` 约束长度），列类型只是给人看的注释、真正生效的是约束。他也感叹："要让全球最流行的 RDBMS 认真对待数据正确性，居然需要这么多额外的样板代码。"

- **lenkite** 列了一份 SQLite"脚枪"清单：默认动态类型、默认关闭外键、AUTOINCREMENT 的 ID 复用、WAL 模式需显式开启、双引号/单引号歧义、位置占位符与命名参数的混用问题，以及"2026 年了还没有 TIMESTAMP 类型"。
