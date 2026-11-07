---
layout: article
title: "《PostgreSQL 内部机制》：从存储到复制的在线教材"
issue: 817
number: 35
category: books
original_url: "https://www.interdb.jp/pg/index.html"
hn_url: "https://news.ycombinator.com/item?id=50358702"
date: 2026-11-06
---

## 文章摘要

Hironobu Suzuki 的《The Internals of PostgreSQL》是一份持续维护的在线技术书，侧重解释数据库内部如何工作，而非只教 SQL 语法。目录从数据库集簇、进程与内存架构、查询处理展开，逐步进入并发控制、VACUUM、缓冲管理、预写日志及复制。对于已经使用 PostgreSQL、却难以把性能现象与内部机制联系起来的读者，这种从结构到运行过程的组织尤其有用。

维护记录也说明它不是一次发布后就冻结的文档。站点标注 2026 年补充了异步 I/O 内容和处于 beta 状态的逻辑复制章节，并持续修订章节结构与说明。阅读时应同时关注所用 PostgreSQL 版本：长期文档常包含历史设计与新版本行为，不能因为目录覆盖某主题，就假定每一处都对应当前生产环境。

本文依据在线目录、更新记录和说明页介绍全书定位，没有逐章重述内容，也没有复制其图示。作者明确保留版权，对教学、非商业使用和其他用途作出区分，并解释选择 HTML 是为了适合其讲解方式。因此，将它当作学习入口很合适，但重新打包、转换成其他格式或使用图像时，不应把「可以在线读」误解成作者已经给予任意再发布许可。

## HN 评论精华

**建议从 HOT 理解存储代价。** [gopalv](https://news.ycombinator.com/item?id=50359266) 推荐先看 Heap-Only Tuple 更新，再联系其他存储方案与 TOAST，建立数据库能做什么、不能做什么的心智模型。

**扩展项目不能冒充上游现状。** [jon128](https://news.ycombinator.com/item?id=50361046) 提到 OrioleDB；[amw-zero](https://news.ycombinator.com/item?id=50361423) 回应它并未被 PostgreSQL 上游采用。讨论有助于区分核心实现和替代存储项目。

**真实使用经验与宣传要分开。** [hubertzhang](https://news.ycombinator.com/item?id=50362154) 回忆团队曾结合 CMU 数据库课程研读本书，随后介绍自己的缓存产品；其产品性能主张不是这本书的结论，也没有在本摘要中验证。
