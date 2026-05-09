---
layout: article
title: "Ask HN：谁在招人？（2026 年 5 月）"
issue: 793
number: 39
category: working
original_url: "https://news.ycombinator.com/item?id=47975571"
hn_url: "https://news.ycombinator.com/item?id=47975571"
date: 2026-05-08
---

## 文章摘要

HN 每月一次的"Who is hiring"招聘帖在 2026 年 5 月延续了过去几年的格局，但 AI 与基础设施的浓度更高。整体职位池可以分成几个清晰的板块：

**AI / 基础模型领域**占比最大。Pathos AI（NYC，肿瘤学 AI 操作系统，背后有 200+ PB 多模态患者数据）、Hyperspell、Form AI、Campus 等公司持续扩张；同时基础设施层的 Loophole Labs（开发能让 Kubernetes Pod 在原地"冬眠"50ms 内 TCP 不断连即可唤醒的 Architect 运行时，需要 Go / Zig / Rust / C / eBPF / CRIU 经验）、Railway、Spacelift 都有 senior 岗位。AI-native 工具链相关的 junior 岗也开始出现——比如 INDATA 直接在 JD 上写"主要工具是 Claude Code 或 Cursor，而不是你自己的打字"，年薪 95–115k 美元。

**金融 + AI 跨界**：Berlin 的资管 BIT Capital 招 Principal Engineer 搭建数据平台，明确写了"自成立以来年化 >30%"；Shepherd（SF onsite）做"完全自动化的商业承保"，主打 AI-native 商业保险。

**气候 / 机器人 / 生物**：Charge Robotics、Gridmatic（电网 AI）、Project Debug（用基因工程蚊子做疾病防控）等。

**地理与远程**模式：Remote (US) 和 Remote (Europe) 仍是主流，Onsite 集中在 SF / NYC / 伦敦 / 柏林 / 阿姆斯特丹，加拿大岗位（如 Steel.dev、Hive）也在变多。Senior 岗位主流薪资带在 USD 150k–250k + equity，小型创业公司股权高但底薪低。

值得注意的几个信号：传统 SaaS / Fintech 占比明显下降；NetBird、PostHog 这种开源公司继续扩招，强调"创始人深度参与招聘"；Swiss IT 服务商 entex 这类老牌公司也开始重新出现在帖子里。

## HN 评论精华

1. **Pathos AI（chrisposhka）**：纽约 hybrid（每周 3–4 天 onsite），\$180–200k + equity + visa sponsorship，做"药物开发的 AI 操作系统"，PB 级患者数据 + 在研最大肿瘤学基础模型。这种"AI + 严肃行业领域知识"的岗位在本期最具代表性。

2. **Loophole Labs（gerhardlazu）**：完全远程（美/欧），\$150–195k + equity。技术栈非常硬核：Go/Zig/Rust/C/eBPF/CRIU/Kubernetes。"早上在追一条 x86 指令、下午在追控制平面的 race condition"——典型的 systems engineer 招贴。团队只有 5 人，你会是第 6 个。

3. **INDATA（jnelsonindata）**：远程或圣地亚哥，初级 dev 岗，年薪 95–115k 美元。明确要求"AI-native"——Claude Code/Cursor 是主要工具，不是辅助。这是本期"junior 岗位的形态正在变"的标志：底薪并不低，但默认你是 AI 工具使用者而不是手敲代码者。

4. **NetBird（verobytes）**：柏林 onsite + 部分 remote，开源安全远程访问（25k+ stars，Series A）。代表了"GitHub 上有声量的开源公司继续扩招"这条副线。

5. **BIT Capital（bbrosch）**：柏林资管基金招 Principal Engineer 做数据/平台。一家自成立以来年化 >30% 的对冲基金，硬性 onsite 2 天/周。"金融 × AI"在本期帖子里非常显眼。

6. **Shepherd（shepherd-eng）**：SF onsite，"完全自动化的商业承保"——给数据中心、半导体厂、可再生能源资产做 AI-native 保险。AI 应用从"内容/通用 SaaS"向"重资产行业"扩散的代表样本。

7. **整体观察**：相比 2024–2025 年的招聘帖，2026 年 5 月这一期：(a) AI 公司渗透到几乎每个细分行业；(b) 系统/底层（eBPF、CRIU、Kubernetes 内核）岗位明显增多；(c) Junior 岗位开始把 AI 编码工具写进必备项；(d) 远程仍是默认设置，但 hybrid 比例在回升，特别是基金、保险、生物这类"敏感数据"行业。
