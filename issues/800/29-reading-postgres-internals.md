---
layout: article
title: "深入 Postgres 内部：数据库集群、数据库与表的存储机制"
issue: 800
number: 29
category: data
original_url: "https://www.buraksen.dev/articles/internals-of-postgresql-db-cluster-and-tables"
hn_url: "https://news.ycombinator.com/item?id=48718716"
date: 2026-07-03
---

## 文章摘要

这篇文章带读者深入 Postgres 的底层存储，讲清楚「数据库集群（database cluster）、数据库、表」在磁盘上究竟是怎么组织的。首先厘清概念：Postgres 里的「数据库集群」指的是由单个服务器实例管理的多个数据库，并非分布式的多台服务器。每个数据库是表、索引、视图等对象的集合，对象由 **OID（object identifier，无符号整数）** 唯一标识；系统目录 `pg_database` 记录集群内所有数据库，`pg_class` 存放表和索引、`pg_index` 存放索引元数据——这些系统目录本身就是普通表。

在物理布局上，一切都存放于 `$PGDATA` 目录下：`base/` 为每个数据库建一个以 OID 命名的子目录，`global/` 存放集群级目录，`pg_wal/` 存放预写日志（WAL），`pg_tblspc/` 存放表空间符号链接。每个数据库目录内的文件以 `relfilenode`（物理文件标识符）命名。文章特别辨析了 OID 与 relfilenode 的区别：二者初始相同，但 OID 是永久标识，relfilenode 在 `VACUUM FULL` 重写数据到新文件时会改变。表空间（tablespace）则是命名的存储位置，允许管理员把数据分散到多块磁盘以优化性能。

文章还讲解了堆表（heap table）的页结构：表由 8KB 的页组成，每页含页头（记录空闲空间与校验和）、行指针（line pointer，从页头向后增长）、以及元组（tuple，从页尾向前存储），中间留出「空闲空间空洞」。对于超过约 2KB 的超大字段，Postgres 通过 **TOAST** 机制自动建立独立的 TOAST 表，把压缩后的数据分块存储、主表只留指针，从而避免元组跨页溢出，且对查询透明。读取时借助 B-tree 索引高效定位，必要时执行顺序扫描。

## HN 评论精华

该帖讨论较少，但整体评价正面，读者普遍欣赏这类深入底层的科普。

- **TheColorYellow**：感慨自己一直好奇数据库在 SQL 之下的更深层发生了什么。文中的内存管理技巧（页、溢出、指针等）提醒了他这些平日「有幸无需操心」的底层抽象，并感叹「真正的编程似乎存在于操作系统层面」。

- **lanycrost**：认为 Postgres 的设计初看很复杂，但设计得极为出色且有章可循，尤其欣赏其对表空间（table-space）的支持，直呼「好文，读得舒服」。
