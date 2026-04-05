---
layout: article
title: "Build123d：Python 参数化 CAD 编程库"
issue: 788
number: 25
category: code
original_url: "https://github.com/gumyr/build123d"
hn_url: "https://news.ycombinator.com/item?id=47567242"
date: 2026-04-03
---

## 文章摘要

Build123d 是一个基于 Python 的参数化边界表示（B-Rep）CAD 建模框架，构建在 Open Cascade 几何内核之上，为 3D 打印、CNC 加工、激光切割等制造流程提供精确的 2D 和 3D 模型设计能力。它代表了"CAD 即代码"（CAD-as-Code）理念的现代实现。

该库的设计哲学强调几个核心原则：**无状态架构**（根据使用模式，内部状态最小化甚至完全没有）、**明确的类层次**（1D 边/线、2D 面/草图、3D 实体/零件各有清晰的几何对象和操作）、**可扩展性**（通过子类化和函数组合实现，不使用猴子补丁）、**Python 规范**（符合 PEP 8、mypy、pylint，包含完整的类型提示）。

Build123d 提供两种使用模式。**代数模式（Algebra Mode）** 是无状态范式，对象通过代数运算符显式追踪和修改，如 `obj += sub_obj` 和 `Plane.XZ * Pos(X=5) * Rectangle(1, 1)`。**构建器模式（Builder Mode）** 则是有状态设计，通过上下文管理器（`BuildPart`、`BuildSketch`、`BuildLine`）隐式管理待处理的形状和变换。

在 API 层面，库支持完整的几何操作链：1D 形状包括 `Line`、`JernArc`、`PolarLine` 等，支持位置（`@`）和切线（`%`）运算符；2D 形状提供 `Circle`、`Rectangle`、`RectangleRounded` 等基本形状及 `make_hull()` 等高级操作；3D 形状支持 `extrude()`、`fillet()`、`chamfer()` 等经典 CAD 操作。选择器系统返回 `ShapeList` 对象，支持过滤、分组和排序。

数据交换方面，Build123d 可导入 SVG 和 STEP 文件，导出为 STL、STEP 等格式，与外部 CAM 和切片工具无缝集成。安装只需 `pip install build123d`，采用 Apache License 2.0 开源。

## HN 评论精华

**"代码 CAD" vs "图形 CAD" 的核心辩论：**

IshKebab 直言"大多数 CAD 本质上是视觉的"，将其比作图形设计，认为代码优先的系统只适合"高度参数化和规则的物体"。hrmtst93837 反驳说"对于生活在代码仓库中、与其他零件共享尺寸的零件，代码胜出，因为你可以 diff 变更"。injidup 则认为 OnShape 证明了视觉 CAD 也能实现版本控制和差异比较。

**混合 GUI/代码 CAD 的愿景：**

CarVac 表达了对"围绕 Build123d 构建的混合鼠标/代码 CAD"的渴望。Cargo4286 指出 OCP CAD Viewer 已提供点击复制功能选择特征，但"目前完全没有拓扑命名（toponaming）问题的缓解措施"，使其脆弱不堪。bschwindHN 提出应该"为选中的项目生成健壮的几何查询"，但 throwup238 引用研究指出，当引入对称性时，生成健壮的几何查询是"不可能的"。

**相比 OpenSCAD 的优势：**

htgb 详细列出了 Build123d 解决的 OpenSCAD 痛点：圆角/倒角操作（trivial vs 不便）、路径拉伸、带圆角的折线、更好的代码组织、通过 VS Code 扩展的编辑器集成、局部坐标系、基于关节的装配。beering 解释了 OpenSCAD 的 CSG 方法与 B-rep 建模的根本区别，指出 OpenCASCADE 是开源 B-rep 内核的代表。

**"自反转金刚石螺纹"挑战赛：**

contingencies 向 Build123d 发起了生成"干净的自反转金刚石螺纹"的挑战，称"这比看起来难得多，很多工具都会失败"。Build123d 的创建者和 Cargo4286 都提供了可工作的代码解决方案，contingencies 确认成功并赞叹"你在其他工具失败的地方成功了"。

**生态工具链：**

z3ugma 提到配合 Gemini 3.1 Fast 和 OCP CAD Viewer 进行迭代。torayeff 推广了 texocad.ai 封装器。bvrmn 分享了 build123d_draft 扩展"让代码 CAD 尽可能简洁"。Cargo4286 引用了 solve123d 项目，基于 JAX 数值求解器添加约束求解。
