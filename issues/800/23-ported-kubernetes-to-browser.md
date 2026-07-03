---
layout: article
title: "我把 Kubernetes 移植到了浏览器里"
issue: 800
number: 23
category: code
original_url: "https://ngrok.com/blog/i-ported-kubernetes-to-the-browser"
hn_url: "https://news.ycombinator.com/item?id=48738985"
date: 2026-07-03
---

## 文章摘要

作者 Sam Rose 打造了一个名为 **Webernetes** 的项目——把 Kubernetes 移植进了浏览器。他没有选择将 Kubernetes 编译成 WebAssembly（那样体积会达到好几兆字节），而是用 TypeScript 重写了一个精简版实现，包括：一个能执行和探测 Pod 的部分 kubelet 实现、多个 Kubernetes 控制器（调度器 scheduler、命名空间、kube-proxy、deployment）、一个让 Pod 之间能互相通信的浏览器版容器网络接口（CNI）、一个面向浏览器环境的自定义容器运行时接口（CRI），以及一套与官方 kubernetes-client/javascript 库对齐的 API。最终成品约 140KiB（gzip 后），还自带一个浏览器版镜像仓库——用 TypeScript API 代替真实的 Docker 镜像。

这个项目的规模相当惊人：约 10 万行代码、629 个文件，在 2 个月内完成，配有 204 个集成测试和 1855 个单元测试。绝大部分代码由 LLM 生成，但 Rose 强调他审查了每一行代码，并编写了可以同时对 webernetes 和真实 k3s 集群运行、以确保行为一致的测试。他也坦言 LLM 需要精心管控——它们会走捷径、发明无用的抽象、遗漏测试用例，因此人工审查不可或缺。

Webernetes 的目标定位是制作关于 Kubernetes 的交互式教学内容，而非用于生产部署。未来计划按需扩展 ConfigMaps、Secrets 和持久卷等特性。

## HN 评论精华

这条帖子被网友预言会成为"爆款"（**duncangh** 称其为"instant classic"），讨论既有对项目本身的赞赏，也有对 AI 辅助工程方法论的思考。

- **doctoboggan** 认同作者的观点：区分"vibe slop（凭感觉糊出来的垃圾）"和真正工程的关键就在于是否真的去读那些代码行。但他觉得我们正处在一个临界点上——也许再过几代模型，只需读测试而无需逐行读代码就够了。作者 **samwho** 回应说，对很多项目而言只读测试确实够用，但这个项目是对既有代码的移植，需要验证移植的精确性，所以还得逐行审。**ambicapter** 补充这属于"手里有现成 spec 可对照"的特例，并非所有编码任务都有这种条件。
- **raychis** 认为这恰恰示范了 LLM 辅助工程该有的框架：AI 能生成惊人数量的代码，但真正的价值在于审查纪律和围绕它的测试；尤其是"拿真实 k8s 的行为来对照验证"而非仅凭"看起来对"，这可能是未来几年大家都会走的方向。
- **jaggederest** 借 Fred Brooks 的"本质复杂性 vs 偶然复杂性"论点辩护 Kubernetes：对于它真正要解决的那类任务，这种复杂度是必要的；只有当你拿它去做本可以更简单完成的事情时，它才沦为偶然复杂性。
- **dinkleberg** 从教学内容作者的经验出发表示看好，回忆起当年用 Katacoda 之类平台的体验；**samwho** 补充说 Katacoda 后来收费了、类似平台也因缺乏资金相继消失，他希望 Webernetes 能成为一个替代品。
- 也有不少玩梗：**lstodd** 请求"把 Kubernetes 移植到家蝇身上，让它们被多余的开销累死"；**malisper** 则指出一个元趋势——大量项目正在用 AI 把既有系统重写成新语言（尤其是 Rust），并列举了 Bun、React 编译器等一串例子。
