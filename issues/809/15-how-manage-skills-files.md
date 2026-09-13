---
layout: article
title: "Ask HN：你们是怎么管理 skills 文件的？"
issue: 809
number: 15
category: ask_hn
original_url: "https://news.ycombinator.com/item?id=49589914"
hn_url: "https://news.ycombinator.com/item?id=49589914"
date: 2026-09-11
---

## 文章摘要

这是一条拿到 319 分、297 条评论的 Ask HN。提问者 **imadtaieber** 的原帖非常简短，但精准戳中了一个正在快速扩散的痛点：

> 你们是怎么找到 skills 的？怎么把它们组织起来？怎么确保它们真的起作用？你们会随时间不断改进它们吗？
>
> 我相信 skills 最终会被模型能力吞掉，但在那之前，我只是想找到一种更好的管理方式。

**背景**：所谓「skills」是编码 Agent（Claude Code、Codex、Cursor 等）生态里近一年冒出来的一种约定——本质上就是放在 `.claude/skills/` 或 `.agents/skills/` 之类目录下的 Markdown 文件，用 frontmatter 描述触发条件，正文写清楚「做某件事的标准流程」。Agent 会根据描述按需加载（渐进式披露），从而避免把所有上下文一次性塞进 prompt。随着各家 harness 都支持了类似机制、社区又冒出各种「skills 市场」，「我该装多少 skills、装哪些、装在哪、怎么跨机器跨工具同步、怎么知道它有没有生效」就成了一个真实的工程问题。

这个帖子的价值在于：它意外地成了一次社区的**观点普查**。评论区里有人交出了成熟的工程方案，有人晒出了自己写的包管理器，也有相当大比例的人态度是「我根本不用 skills，这个概念本身就是个营销词」。下面按派别整理。

## HN 评论精华

### 一、「skills 不会被模型能力吞掉」——对提问前提的反驳

**serf** 的回复是全场最高票，直接否定了提问者的假设：

> 模型能力永远填不上一个自定义 skill（或你的范式里的等价物）能填的、不可知的空白。模型也许聪明到会去 `whoami`、翻 `.ssh` 目录找密钥、找过去连接过的痕迹，来搞清楚怎么连上 bob；但一个 skills 文件可以直接说「我们用密钥 Z、用户 X 连 bob」，于是这件事就办成了，不需要那一整套没必要的推理。在信息仍然有效的时间范围内，一条简洁、信息密度高的 skill 在「完成某个需要内部知识的任务所烧掉的 token」这个指标上永远占优——它直接消灭了整个调查阶段。

**ssivark** 从另一个角度切入：「只要 skills 是上下文性的指导（针对这个作者、这个项目），而不只是（原始的）能力，它们就不太可能被模型吃掉。」**alexhans** 的表述最简洁：「如果 skills 代表的是我团队或我个人特有的工作流，那它就不可能被模型能力吞掉。」**Kwpolska** 把这个道理反过来说：「那些通用到能在网上找到、且你觉得会被模型进步取代的 skills，是没用的，甚至可能有害——考虑到模型对上下文有多黏。有用的 skills 描述的是你项目特有的工作流，它们应该住在项目仓库里供所有人使用和改进。」

**someguynamedq** 用五个字概括：「skills 是工作流缓存。」

### 二、工程化方案：dotfiles、symlink、包管理器

这是回答最集中的一类。核心模式高度一致：**把 skills 放进 git，用某种机制同步/软链到各个 harness 的目录**。

- **jameshiew**：用 chezmoi 当 dotfiles 管，一个 `.agents/skills/` 目录，再从 `.claude/skills/` 软链过去。
- **winternewt** 用 Home Manager 仓库，通过 home manager 配置安装到 `.claude` / `.codex` 等目录。
- **ssivark** 用 guix home 同步到各个 harness（codex、pi、antigravity、Claude Code、Deepseek harness），而且刻意做成**双向链接**而非默认的只读，这样在任何 harness 里都能直接编辑、扩充 skills 库。他还提到一个元层面的建议：「这本身就是一个有用 skill 的例子——写一个管理 skills 的 skill，让 agent 自己把这些线接对。」
- **gregwebs** 开源了自己的 SDLC skills 仓库，配一个安装脚本做软链接，更新只需要推进 git 仓库。
- **jve** 上周刚解决跨项目复用的问题，最后采用的方案是**打包成 plugin，把自己的 git 仓库当作 marketplace**：`claude plugin marketplace add` 加上仓库地址，再 `claude plugin install`，Codex 侧同理。「安装毫不费力，我也不用再跟软链接较劲——我在不同平台上用同一个代码库，软链接会很麻烦。」
- **pdantix** 的做法类似但更省事：只用随 plugin 附带的那些 skills，这样自动更新，只需要在项目的 `.claude/settings.json` 里声明 marketplace 和 plugin。

不少人干脆自己造了轮子：

- **osr00** 写了 `ski`（osrim/ski），带更新命令和安全扫描，并推荐 vercel-labs/skills 和 withastro/rosie 作为现成选择。
- **politician** 写了一个命令行工具把 skill packs 装进各 agent 的项目目录，用法像 `brew`；关键设计是**把 skills 编译进二进制**，这样不用操心文件位置，换机器只要拷贝这一个工具。
- **mstr32** 的 `capshelf` 针对的是团队场景：`capshelf add security-review` 从仓库装一个 skill，`capshelf promote security-review` 把本地新写的 skill 提升到共享仓库供所有人安装。它**用内容哈希对 skill 做 pin**，避免有人偷偷改动破坏流程，同时也支持 MCP 配置和 agent 配置。
- **theletterf** 的方案最「企业级」：skills 放在一个仓库里，有一个 agent 工作流**每两周自动检查 skills 内容是否与文档发生漂移**，漂移了就自动开 PR。「最大的问题是让 skills 在所有用户之间保持最新，所以我又写了个小 Go 二进制来跨 harness 处理这件事。」
- **starefossen** 透露挪威政府的某个部门有一个**公开的 skill 注册中心**和一个按「profile」本地同步的工具（源码在 navikt/copilot）。
- **jdxcode**（mise 作者）刚制定了一个针对 CLI 工具的 skills 标准 packslip：「我以前对 skills 有点看空，觉得 LLM 直接看 `--help` 就行了，但我改主意了。skills 很适合用来描述那些跨多个命令的高层工作流。」由于 mise 将支持在装工具的同时装配套 skills，他预期采用率会不错。
- **SillyUsername** 的方案是全场最精巧的：用**本地 git（不要远端）**分五层。① 一个「skill finder」skill 常驻 prompt，用 git 自带的搜索来找 skill，从而避免 harness 把所有 skill 摘要都塞进 prompt；② 每个 agent 一个私有仓库，主干是 production，草稿走 `draft-<skill名>` 分支；③ 一个组内共享仓库；④ 找不到相关 skill 时回退到 harness 原生机制；⑤ 一个**月度 skill 审计 cron**，识别从没改过、从没在近期会话历史中出现过的垃圾 skill 供他决定去留。「这样既兼容现有 skill 目录，去掉 git 和 finder skill 也不会造成破坏，关键是把没用到的 skill 从 prompt 里清出去，需要时才懒加载。」

### 三、怎么保证 skills 真的有效？

这是提问者第三个问题，也是回答最少、但最有含金量的一块。

**alexhans** 给出了最严肃的答案：「我用 AI evals 来确保它们有效。把它们当成集成测试，用来证明行为。它们对优化流程很有用。我尽量让 skills 主要扮演自然语言和一批好用的小而快的工具之间的翻译层。」并强调「有新问题出现时才改 skills，而不是为了改而改」。

**0xbadcafebee** 给出了最完整的可操作循环：

> 别去找 skills。让 AI 做一件事，它做对了之后，让它把这件事变成一个 skill。清空会话，试着用这个 skill，修复发现的问题，必要时改你的仓库和 harness。重复直到这个 skill 能零样本工作。用同样的流程改进它。
>
> 这套流程很大程度上绑定在特定模型、特定 harness、特定 prompt、特定上下文上。……如果你确实找到了别人的 skills 想用，也让它过一遍上面这个循环。但记住它们是在它们自己的环境里被创造出来的，未必在你的环境里工作。
>
> 另外要把 rules 和 skills 分开：rules 告诉 AI **什么时候**做事，skills 告诉 AI **怎么**做事。

**winternewt** 的自愈方案很有意思：他专门写了一个「元 skill」——当 agent 没有很好地遵循某条指令时，让它基于自己对注意力机制和 LLM 一般行为的理解，去评估这条指令和它犯的错误，诊断为什么没按预期执行，并据此提出对该 skill 的改进。**hypercube33** 和 **skeledrew** 也有类似做法。后者补了一个扎心的观察：「从大多数 AI 帖子的评论看，人们几乎从不读 agent 的执行记录（而自我改进指令也不是每次都会被触发），所以他们的 skills 永远不会改进。」

**ramon156** 的手法值得一提：他在 Zed 里保留会话记录，做完一个大功能后让一个前沿模型通读这些会话并提出改进建议，「通常我用 Gemini，因为它非常擅长删减文本。Claude 和 GPT 不知为何总想往上加字。」最后得到的是更小但更「动作化」的 skills。

**bhkdotdev** 在做一个确定性的行为测试层（dynobox）：「完整的 eval 套件感觉有点重了，我真正关心的只是某些文件有没有被动、我的 skill 到底有没有被读到。」

**chandureddyvari** 分享了一条经验法则：「我发现最有效的 skill 命名范式是『how to do X』——比如『how to add logs』『how to review code』。如果我没法这么表述一件事，那它对我来说就不是一个好的 skill 用例。」他还强调：「另一个发现是 less is more。别装一堆 skills，保持在个位数——我到现在只有 9 个（很多人从市场和插件里装了几百个）。」

### 四、「skills 是营销词」——怀疑派

这一派人数不少，语气也最不客气。

**matsemann**：「skills 只是科技圈给『一个写着指令的简单 markdown 文件』起的新词。没必要搞复杂。把你觉得会重复用到的东西写下来就行，比如『新增一个 API 端点时需要做 x y z』『提 GitHub PR 时要 tag Æ 和 Å』。我主要是在它没自己推断出来的时候才加——非常被动，不主动。大部分公开 skills 都没用且过度复杂。很多人花在打磨 harness 上的时间比真正做东西的时间还多。」不过他也补了一句：「但可以从公开的 skills 里找灵感。」

**jiaosdjf** 写了最长的一段反驳：

> 「skill」到底是什么鬼？我真觉得大家想得太远了。你写了几条要点好让 agent 别老在错误的环境里构建？你有一套很特殊的调试配置？你的 agent 搞不清什么时候该 rebase？
>
> README 才是你写项目相关内容的地方，如果你担心上下文大小，那说明你的 README 太长了——它应该只包含足够让任何一个合格的开发者或 agent 抓住「我们这儿是怎么干活的、更深的答案去哪找」的信息。
>
> 把这个叫做「skills」是不诚实的，这个词是营销人员选的，暗示某种更深层的学习。我不是说调 prompt 没有价值，但你的「skills」只应该以两种方式管理：① 它是你项目特有的，那它是 README；② 它是你工具链特有的，那它是配置、系统 prompt 之类的东西。

**avaer**：「skills 在大多数人的用法下基本是骗人的（那种『从名人那里下载功夫』的幻想）。也许去年还有意义，但今天只要仓库和 prompt 写得好，agent 自己就能找到它需要的一切。『skills』作为开发者宏是有用的，但那顶多是在仓库里跟团队共享的东西，不是你从网上下载的。如果你的 skills 多到你觉得需要管理它们，那本身就是一种代码异味。」

**sornaensis**：「我不喜欢 skills。这是一种特别烦人的技术债，尤其是当人们往公司仓库里塞一堆他们自认为很酷的随机 skills 的时候。往仓库里污染自定义指令也一样。我只希望模型拥有完成我交办工作所需的工具。」

**shermantanktop**：「我看不出刻意策划一套 skills、还要按名字显式调用它们有什么意义——然后眼睁睁看着 AI 全部跳过，靠直接读代码和内外部网站反而做得更好。」

**WatchDog** 代表了最温和的怀疑：「我不用任何 skills。大家觉得最有用的 skills 是哪种？通用任务模型自己就能搞定；项目或环境特有的东西，我直接写在 README 或 agents.md 里。」**ekns** 的做法类似：靠代码库脚手架加 `AGENTS.md`，另外「我会不断引用我自己公开发表的文章——把思考外化出来供 LLM 上下文使用非常有用」。

**iamflimflam1** 提出了对这个帖子本身的怀疑：「互联网把我毁了。现在看到这种问题，我会自动预期它是某种营销——评论区某处一定会出现某个产品/服务/博客文章。」（这个判断不算错：评论区确实出现了 SkillCatalog、Skillshare、capshelf、dynobox、mininote、recall、SkillEd 等一串自家产品的介绍，大多数作者都做了披露。）

### 五、安全性：被低估的一面

**edf13** 提醒了一个大多数人没谈的维度：「在你考虑如何管理已安装的 skills 时，安全是必须考虑的一点。你还需要管理每个 skill 的权限。签名 skills 是正确方向上的一步，但它只证明来源，不证明行为。」

**alexhans** 的一句话适合作为整场讨论的注脚：「我不去找 skills，我自己创造它们。」
