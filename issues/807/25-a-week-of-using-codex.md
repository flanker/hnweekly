---
layout: article
title: "用 Codex 多过 Claude 的一周"
issue: 807
number: 25
category: code
original_url: "https://allaboutcoding.ghinda.com/a-week-of-using-codex-more-than-claude/"
hn_url: "https://news.ycombinator.com/item?id=49393051"
date: 2026-08-28
---

## 文章摘要

Ruby / Rails 开发者 Lucian Ghinda 记录了自己用一周时间把主力从 Claude Code TUI 切换到 Codex TUI 之后的十条即时印象。他强调这是「非常个人化」的快速观察，而非严谨评测。文章发布后因为 HN 上的质疑而做了更新，补上了关键信息：Codex 用的是 gpt-5.6-sol xhigh，Claude Code 用的是 opus-5 xhigh。

十条观察大致如下：

1. **工具链不对等**。他今年一直想让两边保持同等配置（同样的插件/skills），但 Claude 那边的 skills 更多，因为很多是从会话里现场提炼出来的、没来得及移植。解法很简单：让 Codex 直接读 Claude 的 skills 目录并转换格式。
2. **紧急时刻还是会下意识开 Claude**。调试着火的问题时，他仍旧觉得 Claude「更像家」——不是说更好，而是熟悉，而调试时用熟悉的工具很重要。
3. **Codex 生成的 Ruby/Rails 代码注释少得多**，他非常喜欢这一点。
4. **两者的「气质」不同**。Claude 的输出像是同事在 Tuple 结对会话里跟你聊天，Codex 则像《星际迷航》里的 Data 少校。
5. **会话组织方式变了**。他倾向于开很多个专注的小 Codex 会话，而不是像以前那样开一个大 Claude 会话。
6. **Codex 改代码更快，但整体没省时间**。主要改动做得快，可做完之后为了收尾 PR（反复跑测试、review 等）花了很久。他欣赏这种彻底，但最终耗时没有优势。
7. **Codex 的架构更简单**。Claude 倾向于造一大堆东西：抽象、概念、Sorbet 签名、类型别名……Codex 更克制。不过他也承认，在用同样文档实现同样需求时，Claude 的代码虽然更复杂，但处理了更多边界情况。
8. **Codex 也会犯错**。Claude 能理解他从其他分支拉出新分支并保持同步的意图；Codex 干了些糟糕事——分支 A 指向分支 B、B 指向 main，让它 rebase 时它直接对着 main rebase，搞出一个 4000+ 行新增的 PR。必须明确要求「只对目标分支 rebase」。
9. **Jira/Atlassian 集成体验差**。在他用 CLI 而非 MCP 的环境里，Codex 在浏览器登录和 CLI 之间来回横跳；这一项 Claude 更愿意揣摩他的意图并按他习惯的方式办事。
10. **MCP 认证流程 Codex 更好**。Codex 会明确要求你执行 `codex mcp login` 并打开正确的授权流；Claude 有时会试图在一个回合里自动跑掉，然后卡死。

他的总结是一句相当传神的对比：**Claude 会试图超出你的要求、猜测你可能还想要什么并直接动手；Codex 更像一个只做你交代的事、绝不多做的搭档，一看到「大概完成了」的迹象就会停下**。文末他还注明「本文由我撰写，使用 Grammarly 校对」。

## HN 评论精华

247 分、198 条评论。讨论的重头戏不在原文本身，而在于**几乎每一条观察都有人给出完全相反的亲身经验**——这本身成了帖子最有价值的产出。

**方法论批评：不说模型名等于没说。** ukuina 第一条就问「用的哪个模型？不指明模型去比较 harness 是没有意义的」，NyxWulf 追问「还有 effort level 是多少」。agentdev001 的长评把这件事说透了：作者比较的不是「Codex」和「Claude」，而是 Codex TUI 配 gpt-5.6-sol 对 Claude Code TUI 配 Opus-5；「Claude」是一个包含模型和 harness 的产品家族，全文 Ctrl+F 搜 5.6 / sol / sonnet / opus / fable 一个结果都没有，「每听一次『Claude 写代码很棒』我就少活一小时」。asa123 承认自己本想写句刻薄回复，读完原文后发现「什么都没指明」，并感慨「讨论似乎只从标题长出来，正文只是二三阶效应」。作者事后确实更新了文章补上模型信息。

**注释多是不是坏事。** beering 直接问「注释少为什么是好事」，引出一串精准吐槽。muglug 说 Claude 写的注释「本该是 PR 评论而不是代码注释」——你问它一件事，它就把答案注释进代码里；更糟的是**如果代码本身是错的，注释反而会为错误背书**。grim_io 说 Claude 的注释常常包含整条迭代决策链，「对 LLM 理解 why 有用，但人类不会这么写」。rebeccajae 描述得最形象：它像是在给 diff 写评论而不是给代码写说明，被你反驳后改成你建议的方案，还会留下一句「用 git CLI 而不是自己重新实现 git」的自言自语。CollinEMac 一句话总结全场：「注释少通常是坏事，AI 生成的注释少通常是好事。」

**「谁更爱过度设计」出现了完全相反的两派。** 有意思的是，作者说 Codex 更克制，但相当多人经验相反。aleksiy123 让 Codex 做个爬虫加数据处理，它却大搞 provenance、要求「至少三个来源达成共识才能提升为事实」，还定义一堆 enum 和闸门，「我只想爬点站点数据塞进 SQLite，冷静点 Codex」。spudlyo 说 Sol（xhigh）架构目标不错但会陷进认证与校验的泥潭，不信任 packer/Ansible/gcloud 这类原生工具会正确失败，非要自己实现校验；而且**没有清晰的威胁模型**，会为想象中的攻击者建防御体系——「哥们，这些 SVG 只有我们自己的系统会生成，永远不会是用户提供的」。Kovah 和 smusamashah 干脆说自己的体验与原文完全相反：Codex 爱把事情搞复杂，甚至无视指令和 skills，最后要交给 Claude 去简化。enraged_camel 提供了最极端的案例：给 GPT 5.6 一个中小型 ticket（本该几百行加测试），结果产出 **25000+ 行的 diff**，另开一个全新上下文的 Sol 看完说 98% 该扔掉，Claude 的判断是几小时里几十次 compaction 导致模型漂移了。他的结论是「Sol 太不知停手，Opus 则太容易放弃」。rossant 和 aksss 的解法一致：明确告诉它「这一阶段先简化，以后再说」，它就会冷静下来。

**2026 年年中的模型全景。** 用户 217 那条「本周 SOTA 速览」是全帖信息密度最高的一条：Codex 好用且各档套餐额度都足；Sol 适合写长而周密的 prompt 然后放它跑一小时；omp 是极强的 harness，Claude Code 和 Codex 新加的功能它几个月前就有；Claude「还行但算不上出色，所有模型的限制都在收紧」；Gemini 3.7 快得被低估；Kimi K3 前端强，「是少数愿意为你犯罪并且真有能力得手的模型之一」；本地模型「终于开始变好」。这条评论下面的回复也基本各说各话：trjordan 认为漏掉 Grok 4.6 是犯罪；tecoholic 反驳 Gemini「不是被低估，是真不好用」，一个未固定版本依赖导致的构建失败它兜了五七分钟还在跑偏，Opus 4.8 一分钟解决；solarkraft 更刻薄：「这是说它现在能更快地弄坏你的代码了吗？那些 LLM 灾难性删库的故事，主角永远是 Gemini。」

**成本与额度成了新的痛点。** lifty 报告 Codex 套餐额度约一周前悄悄缩水，「以前从来用不完周额度，上周一天就干光了」，并询问是否普遍。MuffinFlavored 说 200 美元/月的 Codex 套餐两天就撞上周限。jmaker 则表示 Claude 的 Max 100 美元档「五小时窗口两三小时就用完」，并且已经开始大量改用 Grok、GLM、Kimi、DeepSeek 做 subagent，「我搞出了一个工厂，想减少对闭源前沿模型的依赖——它们各自单独已经不再是开发的 SOTA 了」。azuanrb 和 cageface 都推荐「Sol 做规划、Luna 干活」这套省钱组合，后者还担心「Luna 现在的价格是不是可持续」。

**最被认同的元观察。** slopinthebag 说「读到大家关于各种模型的完全矛盾的个人经验，本身就挺有意思的」。seamossfet 给出了解释：人们严重低估了自己的技能和背景在这些轶事里的权重，「说到底，司机和车一样重要」。rossant 补充：希望大家至少说明自己在做什么类型的任务、什么语言、什么行业。guywithahat 的说法则最实用——不同 agent 有不同「性格」，他被 Codex 训练出来的写指令习惯拿去用 Claude 时，「Claude 好像什么都做错」，其实只是指令风格不匹配。ReptileMan 用一句话完成了人格化：「Claude 是个自负刻薄的势利眼，Codex 是那个把活干完的蓝领。」piazz 则给出自己的「2026 夏季 meta」：Sol 干常规活、Opus 做前端和设计、Fable 处理复杂或含混的架构问题，且 Fable 驱动 Sol 当 subagent 效果极佳。

最后 AnodicElegy 注意到文末那句「本文由我撰写，用 Grammarly 校对」的声明：「多么勇敢的新世界，这种声明居然成了必需品。不过还是要感谢——我有种感觉，真正让 LLM 代笔的人反而不会加这种声明。」
