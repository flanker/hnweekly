---
layout: article
title: "Blender 5.3：加入高斯泼溅支持，也保留明确限制"
issue: 818
number: 33
category: design
original_url: "https://www.blender.org/download/releases/5-3/"
hn_url: "https://news.ycombinator.com/item?id=50442426"
date: 2026-11-13
---

## 文章摘要

Blender 5.3 把三维高斯泼溅带进常规创作流程：这种表示方式用大量带位置、大小、方向和颜色信息的高斯元素描绘场景，并不等同于传统的三角网格。它适合表达由照片重建的复杂外观，但也带来编辑、光照和渲染一致性方面的新问题。官方说明确认该版本于 2026 年 11 月 10 日发布。

原发布页在本次抓取中返回浏览器挑战页，因此摘要依据[官方开发者发布说明](https://developer.blender.org/docs/release_notes/5.3/)及其[渲染章节](https://developer.blender.org/docs/release_notes/5.3/rendering/)，而非把挑战页当作正文。说明介绍了 PLY、SPZ 等数据的导入，以及在点云中保存尺度、旋转、辐射和球谐属性的方式；Workbench、EEVEE 和 Cycles 均加入相应支持。几何节点也能参与处理，使这种场景表示不再只是外部查看器里的最终结果。

支持并不意味着完整成熟。文档明确指出当前没有相应的高斯泼溅导出功能，性能仍有改进空间，训练数据的色彩空间与线性渲染之间可能出现偏差；部分变换操作对新增属性也存在限制。默认偏发光的表现与真正可重新打光的材质不是一回事。对于希望把实景扫描直接纳入制作管线的人，这是一项重要入口，但是否满足具体项目仍要以实际文件测试，本文未运行 Blender 或进行性能比较。

## HN 评论精华

- [Retro_Dev](https://news.ycombinator.com/item?id=50442662) 提到使用三角形来实现泼溅的研究方向，讨论如何借用显卡已有的三角形处理能力。

- [lambda](https://news.ycombinator.com/item?id=50443122) 强调高斯表示平滑、适合优化等优势，提醒不能只以最后如何光栅化来判断表示方法的价值。

- [debugnik](https://news.ycombinator.com/item?id=50443069) 在关于 Dreams 的支线中解释其 SDF、采样点云和绘制 flecks 的关系，纠正了把相似视觉效果都称为高斯泼溅的混淆。
