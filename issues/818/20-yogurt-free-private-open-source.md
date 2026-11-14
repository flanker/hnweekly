---
layout: article
title: "Yogurt：本地转录、总结与检索会议的开源应用"
issue: 818
number: 20
category: show_hn
original_url: "https://github.com/FrigadeHQ/yogurt"
hn_url: "https://news.ycombinator.com/item?id=50454625"
date: 2026-11-13
---

## 文章摘要

Yogurt 是采用 MIT 许可证的桌面会议助手，目标是不让机器人加入会议，也不把录音默认交给云端服务。它在电脑上捕获音频，用 whisper.cpp 等本地方案转录，再通过 llama.cpp 运行摘要模型，提供会议问答、文件夹、搜索与 Markdown 导出。首次使用仍需下载模型，下载体积与硬件负担并不小；所谓离线工作，指模型准备完成后的核心处理流程，而非安装、更新期间完全不联网。

官方 README 列出 macOS、Windows 和 Linux 支持，但部分体验并不对等：读取 Zoom、Teams、Meet 发言者名称及日历集成属于 Mac 功能，依赖相应权限。多语言转录与界面语言也分别配置，不能把界面支持的语言数直接当成识别能力。录音无需会议机器人，不代表免除告知义务，项目明确要求用户通知与会者正在录制。

值得单独阅读的是 MCP 安全边界。该接口默认关闭，启用后允许助手只读访问会议资料，使用本机用户级通信通道，却没有逐个应用认证的令牌；同一系统账号下的其他进程也可能读取内容。若接入的外部助手把会议文本发往自己的云端，本地保存并不能阻止这种后续传输。摘要依据公开代码仓库说明及讨论，未安装应用，也未审计录音权限、网络行为或模型在不同机器上的表现。

## HN 评论精华

- [yevmiyeci](https://news.ycombinator.com/item?id=50457460)：追问 MCP 是否有认证机制，将讨论从「本机接口」推进到哪些程序能够读取会议记录。

- [MSP_Logic](https://news.ycombinator.com/item?id=50466388)：称检查实现后没有发现令牌或握手认证；这是评论者的代码检查结果，和 README 所述同用户访问边界相呼应。

- [lavendermon](https://news.ycombinator.com/item?id=50457825)：提醒会议中途新加入的人也需要被告知正在录音，单次开场提示未必覆盖整个会议。
