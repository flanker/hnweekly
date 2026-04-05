---
layout: article
title: "在浏览器中运行的开源 CAD"
issue: 788
number: 18
category: show_hn
original_url: "https://solvespace.com/webver.pl"
hn_url: "https://news.ycombinator.com/item?id=47586614"
date: 2026-04-03
---

## 文章摘要

SolveSpace 是一款免费（GPLv3 许可）的参数化 2D/3D CAD 工具，现在推出了实验性的浏览器版本。SolveSpace 最初是作为桌面软件开发的，但由于其非常紧凑，使用 Emscripten 编译后在浏览器中也能出色运行。虽然存在一些速度损失和不少 bug，但在处理较小的模型时，体验通常非常好用。

浏览器版本从最新的开发分支构建，加载完成后无需任何网络依赖即可完全离线使用。用户也可以自行构建并托管静态 Web 内容。

SolveSpace 的主要应用场景包括：**3D 零件建模**——支持拉伸、旋转、螺旋和布尔运算（并集/差集/交集）操作；**2D 零件建模**——将零件绘制为单个截面，导出 DXF、PDF、SVG 格式，并使用 3D 装配来验证配合度；**3D 打印零件**——导出大多数 3D 打印机所需的 STL 或其他三角网格格式；**CAM 数据准备**——导出 2D 矢量图用于水切割或激光切割机，或生成 STEP/STL 导入第三方 CAM 软件进行机加工；**机构设计**——使用约束求解器模拟平面或空间连杆机构，支持铰链、球铰和滑动关节；**平面与立体几何**——用动态标注图代替手工三角函数和电子表格计算。

SolveSpace 的一大特色是其极其小巧的代码量。原始创建者 jwesthues 在 HN 评论中提到，OCCT（OpenCASCADE）有超过 100 万行代码，而 SolveSpace 的 NURBS 内核不到 1 万行。正是这种整体的精简性使得浏览器版本成为可能。该项目支持所有主要平台，兼容过去 26 年的操作系统（MacOS 9 除外）。

## HN 评论精华

**功能局限性讨论**：MrDOS 指出 SolveSpace 是参数化 CAD 的一个"奇妙且与众不同的方式"，但开发速度放缓了，而且缺少一些基础功能如倒角。维护者 phkahler 回应说倒角和圆角已列为下一个主要任务，虽然不会很快实现，但已排到了优先列表的顶部。throwup238 解释了三角倒角复杂性的技术原因——公差和浮点数学问题导致交线"混乱到几乎不相交"，这会破坏几乎所有下游算法。

**与 FreeCAD 的比较**：IshKebab 表示 FreeCAD 没有 SolveSpace 的局限性，而且现在 UX 已经相当不错了。bhouston 称赞 FreeCAD 已经完全取代了他的 Fusion 360 用于木工项目。但 ur-whale 反驳说，FreeCAD 的 UI 仍然"非常复杂"，错误处理对复杂模型不友好，不过承认它是"免费软件，不受 Autodesk 这个软件界最糟糕的公司之一控制"。

**代码内核之争**：amelius 问为什么 SolveSpace 不采用 OCCT 作为几何引擎。phkahler 解释说原始创建者 Jonathan 当时走的是 DIY 路线，而布尔运算至今仍是 bug 的主要来源。原创者 jwesthues 亲自出现并回复，指出 OCCT 超过 100 万行代码而 SolveSpace 的 NURBS 内核不到 1 万行，正是这种精简性才让浏览器版本成为可能。这引发了关于是否应该切换到 OCCT 或将其作为可选项的深入技术讨论。

**Dune3D 与 SolveSpace 的关系**：julbaxter 提到 Dune3D 在幕后使用了 SolveSpace。phkahler 澄清说 Dune3D 只用了 SolveSpace 的约束求解器，实体模型使用的是 OCCT。

**用户情感与体验**：somat 说 SolveSpace "使用起来有一种愉悦感"，尽管它有局限性；mordae 表示因为没有时间学习其他工具，一直用 SolveSpace 做 3D 打印设计；masonhensley 认为 SolveSpace 是设计激光切割零件的出色轻量级工具。

**LLM 生成 CAD 的讨论**：ponyous 正在研究 LLM 到 CAD 的工具，询问 OpenSCAD 是否是 LLM 表示的最佳选择。ur-whale 强烈反对，指出 OpenSCAD 缺少光滑表面、任意面之间的圆角和原生约束求解。

**关于"vibe code"CAD 引擎的争论**：faangguyindia 表示希望有天才程序员用无限的 Claude 和 Codex 额度来构建出色的 CAD 引擎。ezst 回怼说，"论坛上的人们说服自己用 vibe code 就能写出有用的几何内核，这真是令人深感沮丧。"progbits 补充说专业内核是专有的，超出了 LLM 的训练数据，而 OCCT 也远落后于 Parasolid 等商业内核。

**字体渲染争议**：JoshTriplett 批评浏览器版本中像素化的字体渲染"非常糟糕"。维护者 ruevs 辩护说使用 GNU Unifont 是有意为之——这是一个 973KiB 的文件，几乎覆盖了所有 Unicode 字符。
