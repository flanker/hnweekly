---
layout: article
title: "db-branch：用 ZFS 给代理和 CI 创建可丢弃的数据库分支"
issue: 819
number: 27
category: data
original_url: "https://github.com/db-branch/db-branch"
hn_url: "https://news.ycombinator.com/item?id=50506940"
date: 2026-11-19
---

## 文章摘要

db-branch 把数据库测试环境做成可快速创建、重置和销毁的分支。它在 ZFS 文件系统层使用写时复制克隆，每个分支运行独立的数据库进程，不必为每次迁移测试或拉取请求重新复制整库。仓库描述仍以 MySQL 为主，但本次读取的 README 明确支持 MySQL 与 PostgreSQL，不能只凭页面元数据遗漏后者。

项目声称，在自己的三节点测试集群中，PostgreSQL 分支约半秒、MySQL 分支约三秒可用；这些是作者环境的结果，本期没有安装或复现。快速创建的前提包括已经准备好基础数据集，基础快照采用干净关闭的实例以避免启动恢复时间。运行中分支的检查点则使用崩溃一致性快照，对数据库文件与日志所在卷有相应要求。

这里的“分支”也有明确边界：它是用于实验的块级副本，不能像 Git 那样把不同分支的写入合并回主库。写隔离更不等于隐私隔离，代理仍能读取副本中的敏感记录，因此需要在创建基础快照前脱敏。项目目前处于 Beta，核心采用 Apache-2.0 许可，认证、审计和历史等部分模块另为闭源付费组件。适用场景是自托管的临时数据库沙盒，而非不加治理地把生产数据开放给工具。

## HN 评论精华

- [nu_nu11](https://news.ycombinator.com/item?id=50511839) 作者确认分支目前用于丢弃式实验，没有合并回源库的能力；冲突解决仍是难点。

- [yjftsjthsd-h](https://news.ycombinator.com/item?id=50515478) 询问既然 PostgreSQL 具备崩溃一致性，是否必须停库拍快照，促使讨论区分数据一致性与启动恢复耗时。

- [nu_nu11](https://news.ycombinator.com/item?id=50515635) 指出文件系统层方案可以设想扩展到其他有状态服务，但同时承认多实例仍受内存和 I/O 限制；扩展方向不是已实现功能。
