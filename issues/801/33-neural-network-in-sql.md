---
layout: article
title: "我用 SQL 实现了一个神经网络"
issue: 801
number: 33
category: data
original_url: "https://github.com/xqlsystems/xarray-sql/blob/claude/xarray-sql-mnist-demo/benchmarks/nn.py"
hn_url: "https://news.ycombinator.com/item?id=48897975"
date: 2026-07-17
---

## 文章摘要

作者 alxmrs 完全用 SQL 查询训练了一个多层感知机（MLP）来分类 Fashion-MNIST，其载体是他自己的数组数据库库 Xarray-SQL。项目的核心前提是：任何 N 维数组都可以映射为二维的表格模型——N 维数组中彼此正交的维度，正好对应表格表示中的主键。作者在给一位 GSoC 实习生做代码评审的间隙萌生了这个 demo：既然数组能被当作表来处理，那么把神经网络的张量运算也表达成关系查询是否可行？

具体做法上，网络是一个 4 层 MLP：784 个输入像素 → 196 → 32 → 10 个输出类别，隐藏层用 tanh 激活，输出层为线性。数据被表示为关系：图像存进带有 (sample, height, width) 维度的 `pixels` 表，权重按层组织成带 (inp, out) 维度的表，偏置单独按层存储。前向传播中的逐层矩阵乘法被实现为表连接加分组聚合，即 `SELECT SUM(activation * weight) GROUP BY output_unit` 的形式；激活函数（tanh、softmax 中的 exp）在聚合之后应用；反向传播则通过从输出误差开始的链式连接反向传播，用 SQL 的 `grad()` 函数计算局部导数。为提速，代码缓存了中间结果（激活值、delta、梯度）以避免在 60 个训练步中反复扫描源数据。一个值得一提的优化是 `SKIP_ZERO_PIXELS`：在第一层收缩时过滤掉零值像素，由于零贡献在数学上是中性的，这一步能"精确地缩小连接"，在真实数据上带来约 1.8 倍加速。

## HN 评论精华

讨论从最初的"翻白眼"迅速转向对底层理论的欣赏与补充。

- **0xnyn** 的评论获得广泛认同：起初听到"用 SQL 写神经网络"直翻白眼，但读完代码后深受触动——本质在于"把关系代数当作中间表示（IR），让数据库优化器去推理张量程序"。作者 alxmrs 回应说自己一开始也会有同样的直觉反应，并认为 SQL 作为一门面向数据的逻辑式编程语言，或许恰恰是写张量程序的理想工具。

- **sporkl** 从数学上点出关键联系：einsum（爱因斯坦求和）和数据库连接本质上是同一回事，只是作用在不同的半环上——einsum 用实数、数据库用布尔值。他推荐了 Datalog 相关论文以及 Dyna 项目做深入探讨。

- **AlotOfReading** 泼了理性的冷水，指出这个思路"并不新"：与爱因斯坦求和的等价性早有论文指出，Sandia 实验室几年前就基于 graphBLAS 写过名为 TenSQL 的 SQL 数据库，今年还有论文把"关系代数作为 AI 基础"命名为 Tensor Logic。他也提醒自然连接因组合爆炸存在根本性的扩展问题，并推荐了 Differential Dataflow。作者欣然接受这些参考文献，表示"很高兴听到这不算新东西——还带引用"。

- **sixdimensional** 补充了更早的工业界先例：MADlib 是把 ML 算法（含神经网络）集成进关系数据库和 SQL 的著名开源项目，他 2005 年前后就在 SQL Server 里用过 ML 算法。他还提出一个更深的问题：究竟是"SQL 作为可优化成任意 DAG 的声明式语言"的特性起了作用，还是关系数据库特有的关系代数性质起了作用，希望作者厘清。

- **simonw** 顺势分享了同一天看到的另一个"用 SQLite 递归 CTE 实现 DOOM 光线追踪"的项目（DoomQL），呼应了 **soupspaces** 把这个 demo 类比为"X 能跑 DOOM"式炫技的说法。**goosethe** 则贴出自己类似的 `pg_gpt2` 项目。
