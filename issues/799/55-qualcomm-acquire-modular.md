---
layout: article
title: "高通将收购 AI 创业公司 Modular"
issue: 799
number: 55
category: startup_news
original_url: "https://www.reuters.com/business/qualcomm-buy-ai-startup-modular-2026-06-24/"
hn_url: "https://news.ycombinator.com/item?id=48659798"
date: 2026-06-26
---

## 文章摘要

2026 年 6 月 24 日，高通（Qualcomm）宣布将收购 AI 软件创业公司 Modular。Modular 由编译器领域的传奇人物 Chris Lattner（LLVM、Clang、Swift 语言的创造者，曾任职于苹果、Google、特斯拉）创立，公司的核心产品包括面向高性能计算的编程语言 Mojo，以及用于生成式 AI 建模与服务的 MAX 框架。根据 Modular 官方博客的表述，这笔交易旨在"强化高通在数据中心与边缘环境中面向生成式 AI 和智能体式（agentic）AI 的软件基础"。

这次收购的战略意图非常清晰：高通长期以来以移动 SoC 芯片（骁龙系列）和无线通信技术著称，但在数据中心和高端 AI 加速领域一直缺乏存在感。通过收购 Modular，高通获得的不仅是一支顶尖的编译器与 AI 基础设施团队，更是一套能够将 AI 模型高效部署到异构硬件（包括非 NVIDIA 硬件）上的软件栈。Mojo 语言的设计目标本身就是为了在保持 Python 易用性的同时提供接近 C/C++ 的性能，并能针对各种加速器后端进行编译优化——这与高通试图进入 AI 推理（inference）市场、摆脱对 CUDA 生态依赖的诉求高度契合。

关于交易的具体财务条款，路透社原文未能成功抓取（被反爬机制拦截），公开渠道也未披露明确的交易金额。但据 HN 评论中多位疑似知情者透露，这是一笔全股票（all-stock）交易，且对 Modular 的员工而言条款并不优厚。值得关注的一点是，有 Modular 员工在评论区确认，Mojo 编译器仍将按计划在 2026 年 8 月开源——这意味着收购至少在短期内不会改变其开源路线。整体来看，这标志着高通正在"组建一个面向 AI 与云的产品组合"，向手机芯片之外的 RISC-V、数据中心推理等方向进行实质性扩张。

## HN 评论精华

**roflcopter69（237 赞）** 表达了对 Mojo 方向的惋惜，认为 Lattner "浪费"了自己的才华去把 Mojo 做成类 Python 的语言，而不是从第一性原理出发设计一门全新的语言。他理解为了开发者采用率而向 Python 妥协的商业逻辑，但仍觉得这限制了语言的潜力。

**samuell** 质疑高通在收购后是否会继续优先发展 Mojo，对开源承诺和长期投入的稳定性表示担忧。对此，Modular 员工 **agreeablegoose** 出面确认编译器将于 8 月如期开源，部分打消了社区疑虑。

**maxloh、sobkas** 等人讨论了战略动机：高通并没有高端 GPU 产品线，为何要收购 Modular？sobkas 推测，高通买下它更多是为了"防止竞争对手拥有这项技术"，而非看中其即时价值。

**melodyogonna** 则给出更乐观的解读，认为高通正在"组建一个面向 AI 和云的产品组合"，意在突破手机芯片的边界，押注 RISC-V 与推理市场。

**mathisfun123 和 fvrghl** 提到，听说 Modular 的员工在这笔全股票交易中"在财务上吃了亏"。

此外，多位用户援引历史先例（SYCL、OpenCL、oneAPI 以及夭折的 Swift for TensorFlow），担忧 Mojo 这类跨平台计算方案尽管出发点良好，最终仍可能面临采用率不足的困境。
