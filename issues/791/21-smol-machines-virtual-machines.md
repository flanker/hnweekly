---
layout: article
title: "Smol Machines：亚秒级冷启动、可移植的轻量级虚拟机"
issue: 791
number: 21
category: code
original_url: "https://github.com/smol-machines/smolvm"
hn_url: "https://news.ycombinator.com/item?id=47808268"
date: 2026-04-27
---

## 文章摘要

smolvm 是一款命令行工具，目标是用"虚拟机的隔离性 + 容器的人体工学 + 亚秒级启动时间"来重新定义本地软件分发与运行方式。作者来自 AWS 的容器和 Firecracker 团队，他们认为传统容器是"多余的一层"，让启动慢、隔离弱；而 Firecracker 又太偏 AWS 内部架构，不适合本地开发。smolvm 是这两条路线的混合体——保留容器的便捷使用方式，同时获得真正的硬件级隔离。

主要功能包括：

- **亚秒级冷启动**：在 macOS 与 Linux 上都能做到 sub-second cold start，依赖 macOS 的 Hypervisor.framework 或 Linux 的 KVM。
- **弹性内存使用**：VM 的内存按需扩张/收缩，避免传统 VM 的预分配浪费。
- **`.smolmachine` 单文件打包**：可以把一个有状态的 VM 打包成单个文件，分发到任意兼容机器上原样运行——这一点被许多评论者视为"WASM 一直承诺却没真正兑现的可移植执行单元"。
- **临时 VM**：在一次性 VM 里跑命令，跑完自动清理。
- **持久 VM**：用于开发工作流的长期机器。
- **OCI 镜像兼容**：支持直接拉取标准 OCI 容器镜像，无需 Docker daemon。
- **SSH agent 安全转发**：转发 SSH agent 而不暴露私钥。
- **Smolfile 配置**：用声明式配置文件复现环境。
- **默认零网络**：沙箱默认禁网，可显式 allowlist 主机访问。

技术核心是基于 **libkrun**（用作 VMM）配合自定义内核 libkrunfw。支持平台是 macOS（Apple Silicon / Intel）和 Linux x86_64/aarch64（需 KVM）。它的定位很清楚：不是替代 Kubernetes 集群编排，而是给本地开发者和 AI agent 沙箱、可信代码执行场景提供一个"比 Docker 更轻、比传统 VM 更快、比 Firecracker 在 macOS 上能用"的中间方案。

## HN 评论精华

- **binsquare**（作者本人）开门见山：他想做"一个用 VM 取代 Docker 容器、但保留容器人体工学"的工具，亚秒级启动是核心。背景是来自 AWS 容器组与 Firecracker 团队，深感容器是不必要的中间层。
- **PufPufPuf** 表达了实际需求：他在做 AI 沙箱（Lima + Incus），但很多 microVM 不能跑 Docker / Kubernetes。能否在 smolvm 里跑 k3s？作者 **fqiao** 立刻建了一个 issue 跟踪，并表示后续会加回 Docker 支持。
- **topspin** 提出了一个常被新型 microVM 忽略的关键能力：**live migration（在线迁移）**。Cloud-native 应用不在乎，但很多遗留数据库、缓存型 web 应用、长时间训练任务都需要这个能力。Azure / GCP / OCI / AWS 在底层都用了 LM 来给用户"主机长时间不重启"的假象。**genxy** 回应：CRIU 在某些场景下能做，但作者要不要先把 use case 收清楚。**benswerd** 补充说："Live migration 是我做过最难的事情，我们团队花了 4 个月只为做内存层的实现。"他预测随着 hypervisor 自身完善，未来 12 个月内这类 OSS 沙箱方案大多会获得 LM 能力。
- **gysakai** 给出了一个非常有意思的定位评价："`.smolmachine` 单文件打包让我感觉这是 WASM 一直承诺却没兑现的'可移植执行单元'，但还保留了完整的 Linux 二进制生态。"作者 **binsquare** 直接确认："这正是我瞄准的用例。"
- **harshdoesdev** 分享自己也做了类似的项目 shuru.run，初衷一样：Firecracker 不能在 macOS 上用，普通 microVM 又太重。**sahil-shubham** 介绍了基于 Firecracker 的 bhatti 项目（"任何人有一台空闲 Linux 机器都该能切成可编程隔离机器"）。这一群"做小型 VM 的开发者"在评论区互相打招呼，俨然形成一个新生的小型 VM 生态。
- **lacoolj** 直接问"这代码 LLM 写了多少？"，作者 **binsquare** 坦言："大约 50%。在它没见过的领域不大有用，但现在核心已经搭好，AI 帮助很大。"
