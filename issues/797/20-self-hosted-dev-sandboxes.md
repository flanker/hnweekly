---
layout: article
title: "自托管的开发沙箱，带预览 URL（Docker、Go、无需 K8s）"
issue: 797
number: 20
category: code
original_url: "https://github.com/tastyeffectco/sandboxes"
hn_url: "https://news.ycombinator.com/item?id=48388909"
date: 2026-06-12
---

## 文章摘要

Sandboxd 是一个开源后端平台，面向 AI 应用构建（app-builder）类产品。它能创建带实时预览 URL 的隔离云开发环境，且完全运行在单台服务器上、通过 Docker 实现——无需 Kubernetes。

核心工作流是一组简单的 HTTP 接口：

- **POST /sandbox**：创建一个隔离的 Linux 容器，拥有独立文件系统与内存限制。
- **POST .../tasks**：在沙箱内运行 AI 编码 agent（OpenCode 或 Claude Code CLI）。
- **实时预览 URL**：把开发服务器即时暴露为可分享的链接。

技术架构刻意追求简单：本质是"一个 Go 程序，指挥 Docker 该做什么"，由 Traefik 负责 URL 路由，SQLite 管理状态。控制平面通过挂载宿主机 socket、以容器形式运行，并调用 Docker CLI 把沙箱作为兄弟容器启动在共享网络上。

之所以不用 Kubernetes，是因为项目专注于单机 Docker 部署，以规避运维复杂度，让没有集群经验的团队也能上手，同时仍提供多租户隔离、持久化存储与按需唤醒能力。

主要特性与用例：

- **空闲即停、请求即唤醒**的架构，让普通硬件也能跑下数十个沙箱。
- 预装编码 agent，省去搭建开销；支持按沙箱注入环境变量（如 API key）。
- 工作区持久化，借助 SQLite 状态对账在重启后存活；容器经过加固（丢弃 capabilities、只读 rootfs）。
- 适合 AI app-builder、agent 平台、编码 playground、按用户的预览环境，以及面向多用户的 SaaS 工厂。

## HN 评论精华

**rsyring**：希望有类似但基于 Firecracker microVM 的方案，本质上是一个"自托管版的 exe.dev"，以获得更强的隔离边界。

**babhishek21**：追问采用 microVM 的动机，提到有朋友用 Docker 配 gVisor 加固实现了类似效果，质疑内核隔离或快照能力是否值得这份额外复杂度。

**p2hari**：同样在找自托管的 exe.dev 替代品，反映现有方案的资源紧张——用 ionos 云的 4GB VPS 连 3 个基础 web 服务器都跑不动。

**benldrmn**：推荐自己基于 gVisor 的项目 Isola，支持快照与网络管控，并兼容 Kubernetes，作为不走 Firecracker 路线的替代选择。

**Bnjoroge**：批评该项目"重度 vibecoded"，并指出"对于 agent 而言，Docker 并不是一个严肃的隔离边界"。

**cedws**：警示安全风险——把仅靠 Docker 隔离的 vibe coding 项目暴露到公网"相当危险"。
