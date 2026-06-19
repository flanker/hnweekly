---
layout: article
title: "免费 SQL→ER 图工具：在浏览器本地运行，数据不上传"
issue: 798
number: 28
category: data
original_url: "https://sqltoerdiagram.com/"
hn_url: "https://news.ycombinator.com/item?id=48523992"
date: 2026-06-19
---

## 文章摘要

SQL to ER Diagram 是一款免费、开源的 Web 应用，可将 SQL 表结构代码转换为交互式实体关系图（ERD），且全程在浏览器内运行。用户只需把 `CREATE TABLE` 语句粘贴进编辑器，工具便会即时生成可视化图表。

它的关键卖点是隐私：所有计算都在本地浏览器完成，"你的 SQL 表结构永远不会被上传或存储到任何服务器"，无需安装、无需注册。在交互上，用户可拖动表格自由排布、滚轮缩放、双击重命名，并支持自动布局（横/竖方向及间距调节）。

功能方面，它兼容 PostgreSQL、MySQL、SQLite、SQL Server 等多种 DDL 语法，能识别主键、外键、唯一约束和非空约束；支持导出为 PNG、SVG、Mermaid、DBML、PlantUML 多种格式，还能保存项目、生成分享链接，并兼容 Prisma、SQLAlchemy、Sequelize、Django 等相关结构格式。作者在实现上也分享了一些有趣细节：基于 `<canvas>`（而非 DOM/SVG）渲染，把表格栅格化为缓存位图并做视口裁剪，因而即便屏幕上有数百张表也依然流畅；SQL 解析器会追踪每个 token 的源码位置，使画布上的编辑能反向同步回 SQL。

## HN 评论精华

**robhati**（作者）：解释了创作动机——常需可视化数据库结构，但现有工具不是付费墙、强制注册，就是要把 SQL 发到别人的服务器；于是自己做了一个无后端、无账号、数据不出本机的工具。

**written-beyond**：给移动端可用性打了"100/10"，称平移、缩放、选择和移动都流畅得让他以为自己产生了幻觉。

**michaelmior**：从概念上"较真"地指出，严格来说无法仅凭 SQL 得到 ER 图，因为实体与表本质不同，单凭 SQL 信息不足以构建真正的 ER 图；但也承认这不妨碍工具有用。

**eventinbox**：回应上述观点，认为"实体 vs 表"的区分确实存在，但对大多数只想快速可视化现有结构的开发者而言已足够，"完美是有用的敌人"。

**CodesInChaos**：提出实用需求——希望能隐藏到特定表的连线或按名称过滤外键，例如多租户应用中 90% 的表都连向租户表，这些连线价值不大。

**多位评论者**：列举了相似工具用作对比，如 Azimutt、dbdiagram、MySQL Workbench、wwwsqldesigner，以及可视化查询计划的 explain.dalibo.com。
