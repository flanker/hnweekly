---
layout: article
title: "matklad 谈如何学软件架构"
issue: 794
number: 25
category: code
original_url: "https://matklad.github.io/2026/05/12/software-architecture.html"
hn_url: "https://news.ycombinator.com/item?id=48106024"
date: 2026-05-22
---

## 文章摘要

matklad（rust-analyzer 的主要作者，目前在 TigerBeetle）写了一篇关于"**软件架构是如何被学到的**"的反思。他的核心立场是反智的——**软件设计不是从课程里学到的，是从做项目里学到的**——尤其是 maintain 项目，而不是只造项目。学院软件和工业软件的差距，不是技术差距，是**激励结构的差距**。

他把康威定律（Conway's Law）抬到了最高的位置。"**代码不如架构重要，而架构不如社会问题重要。**"——这句话他不只是引用，是真信。matklad 用 rust-analyzer 举例——他在设计这个项目时，不是先想"什么数据结构最优"，而是先想：**贡献者会是谁，他们能投入多少精力**——他要让深度贡献者和周末玩玩的人**都能上船**。具体策略包括——尽可能简化构建复杂度、把 feature 隔离到运行时错误边界后面（一个 feature 崩了别影响别人）、把核心基础设施和外围 feature 分开。

他还引出了 TIGER_STYLE——TigerBeetle 内部那套以"defensive programming + assertion-heavy + 不用 alloc"出名的编码规范。但 matklad 的观点是——**TIGER_STYLE 之所以有效，不是因为规则本身，而是因为支撑它的组织文化**——同样一套规则贴到没有那套文化的团队里，会立刻失效。

他推荐了一份**反复出现在他思考里**的学习材料清单：
- **Gary Bernhardt 的 "Boundaries"** 演讲——把 functional core / imperative shell 这个 mental model 讲到位
- 他自己的 essay "**How to Test**"
- **Pieter Hintjens** 关于 Conway's Law 的文章
- **Ted Kaminski 的博客**——他认为目前最接近"有体系的软件开发理论"的作者
- 书——**《Software Engineering at Google》**和 **Ousterhout 的《A Philosophy of Software Design》**

## HN 评论精华

- **CSMastermind**（最被点赞的"备忘单"）：直接抛出一份**12 条架构 cheat sheet**——
  - 好的设计是**一个理念贯穿到底**
  - 更普遍——**最小化惊讶**
  - **系统允许的事就一定会有人做**
  - 凡是以"只要大家都……"开头的方案就不叫方案
  - **隔离数据转换与数据使用**——数据模型比代码活得久
  - **耦合是大多数邪恶的根源**
  - 版本化是必然的
  - **让状态显式**
  - 每条信息都该有**唯一可信来源**
  - 多花时间想**命名**
  - **如果测试很难写，说明设计错了**
  - **每一个不写文档的决策，你都会后悔**
  - 沟通是税——付之前先合理化
  - 工程师在任何级别的工作都是"用 rule of thumb 在信息不完整下解决问题"

- **deepsun**（直击 matklad 那句"maintain 而不是 create"）：进一步发挥——"**学架构最好的方式是 maintain 足够大的项目，而且至少做过两三个，才能比较**。"小项目什么架构都成立。但是真实问题是——**架构师晋升的标准往往是创造，不是维护**。Google 内部尤其明显——只有 ship 新东西能升职，所以一帮人造完就跳。**真正最适合做架构师的人，反而是那些被请来 maintain 别人留下烂摊子的外部承包商**——他们见过多个项目，能比较，但缺点是按小时计费容易过度设计。

- **mpweiher**（学院派）：matklad 推荐的书很好，但"是关于软件开发，不是真正的软件架构"。他推荐**经典学术文献**——Shaw/Garlan 的 *Software Architecture: Perspectives on an Emerging Discipline*、Mary Shaw 几篇反思软件架构作为学科为何没成的文章。更实操的——**研究 Unix pipes、REST、Hexagonal Architecture 成功在哪、失败在哪**。

- **miki123211**（书推荐）：推荐 **《Architecture of Open Source Applications》**——每章由一个真实开源项目的 maintainer 来讲架构、约束、历史变迁。"**你学的不只是架构是什么，更是塑造它的约束。**"虽然有些章节已经过时，但模型仍然成立。

- **noelwelsh**（替"心智模型"派辩护）：他认为 matklad 低估了**心智模型**的价值。举例——**编译器是 AST 上的一系列变换**——这个 mental model 一旦掌握，你会发现**很多业务应用其实也是 JSON 上的一系列变换——它们本质就是编译器**。他进一步把这个模型上升到代数数据 + 折叠 / 反折叠的高度——你由此能在 reactive programming 里看到同样的影子。"**这些东西文献里都有，只是没人把它压缩成易读的形式**。"——他在写一本书做这件事。

- **luodaint**（让"AI agent 暴露架构缺陷"成为一项任务）：他发现了一个有效练习——**把自己系统的来龙去脉，从零向一个完全不懂背景的 agent 解释**。"如果 agent 没法推断出某个约束，就说明你必须把它写下来——迁移规范、认证协议的 invariants、多租户行为。"——这个把 implicit reasoning 强制 explicit 的过程，**比他独自构建几个月得到的清晰度都高**——他立刻发现了三个架构错误。"**Learn by doing 是对的，但还要 learn by explaining**——agent 是个出奇挑剔的听众。"

- **ah1508**（对"clean code"的批判）：抛弃"clean / beautiful"这种模糊词汇，要给架构定义**清晰的目标列表**——可维护、性能、可扩展、高效、有弹性、可观测、可测、安全、对新人可读。多个目标互相制衡，列得越细越好，**也更方便和客户达成共识——他能知道自己付的钱买的是什么**。

- **finnnk**：提另一个角度——**残值理论（Residuality Theory）**——一个把"架构"和"设计"分开的理论框架，配套有书。

- **0xbadcafebee**（精辟类比）：从系统工程角度——"**软件架构就像房子的水管设计**——重要，但你不住在水管里。如果你的水管设计没考虑到房子其他部分，修起来可能很贵。"

- **runningmike**："**学软件架构，意味着理解'没有一个标准答案'**——它是艺术也是科学。"
