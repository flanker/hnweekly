---
layout: article
title: "活在你 SQL 里的统计学"
issue: 799
number: 31
category: data
original_url: "https://kolistat.com/blog/the-stats-duck-v0-6-0/"
hn_url: "https://news.ycombinator.com/item?id=48629810"
date: 2026-06-26
---

## 文章摘要

The Stats Duck 是一个开源的 DuckDB 扩展，目标是把统计计算能力直接搬进 SQL 查询里，让用户无需离开数据库环境就能完成复杂的统计分析。它是 Bedevere 和 KoliLang 所依赖的统计引擎，采用 MIT 许可证，可以在任何运行 DuckDB 的地方使用，甚至包括浏览器（通过 DuckDB-WASM）。所谓“活在你 SQL 里的统计学”，指的就是把分布计算、假设检验、回归建模、数据可视化等操作都作为原生 SQL 操作来执行，而不再需要把数据导出到 R 或 Python 等外部工具中折腾一圈。

v0.6.0 版本带来了多项重要更新。首先是数据画像功能：通过 `meta()` 表函数可以“一次性给整张表做画像”，返回每一列的统计信息，包括缺失值、唯一值数量、均值、中位数、标准差和众数等。其次是回归建模，用户可以用 R 风格的公式语法（如 `'body_mass_g ~ flipper_length_mm + bill_length_mm'`）直接在 SQL 中做最小二乘回归，底层使用 Cholesky 分解实现，并已与 R 交叉验证到小数点后四位。此外还新增了 `bootstrap()` 函数，支持非参数重抽样来计算置信区间，无需假设数据服从正态分布。

可视化方面，新增了小提琴图、二维分面（`FACET BY row, col`）、逐层 LOESS 平滑（`STAT smooth`）、线性回归线（`DRAW regression`），以及基于 Vega-Lite 的 ggsql 绘图体系。概率分布方面新增了十种分布（负二项、超几何、Weibull、对数正态、泊松等），每种都配齐了 d/p/q/r 四套函数（密度、累积概率、分位数、随机抽样）。性能上最亮眼的改进是修复了读取 SAS/SPSS/Stata（XPT 格式）文件时的 O(N²) 解析 bug，使导入速度提升了 52 倍——一个 20 万行的文件从 67 秒降到 1.3 秒。该扩展的核心价值在于消除工作流割裂：分析师不再需要导出数据、切换到 R/Python、分析完再导回，所有复杂统计都能在数据库层直接、可复现、可组合地完成。

## HN 评论精华

**pacbard**（最有价值的技术反馈）：他给出了实质性的批评，指出该工具目前能胜任“探索性描述统计、配对检验和基础线性回归”，但缺乏生产级特性，比如稳健标准误、多层模型和广义线性模型（GLM）。他还提出了重要的市场定位问题：相对于 SAS、R、Stata 这些成熟工具，The Stats Duck 该如何立足？他类比了 Octave/MATLAB 以及 SageMath/Mathematica 之间的关系，建议项目收窄范围，聚焦统计核心，而不是去重复其他 DuckDB 扩展已有的功能。

**caerbannogwhite**（作者本人的回应）：开发者认可这是“相当多的高质量反馈”，并给出了明确的开发优先级：先通过新的基于 Eigen 的线性代数内核实现稳健标准误（HC0–HC3 及聚类标准误），然后是 margins 和 GLM。他重新阐述了定位：“不是想把谁从 Stata/R 那里撬走……这个利基是‘统计就在数据已经待着的地方做’”，针对的是那些只想快速分析、不愿往返 Python/R 的 SQL 用户，尤其是通过浏览器内的 DuckDB-WASM 集成场景。

**thomasp85**（ggsql 的开发者，合作视角）：作为官方 ggsql 的开发者，他对同时维护多个项目、保持功能一致性表达了担忧。他对合作持开放态度，但也提出疑问：为什么选择独立实现，而不是直接给 ggsql 扩展贡献代码？整段交流体现出专业与建设性，而非对可视化功能重叠的防御性反应。
