---
layout: article
title: "AI 吃掉世界（2026 春）：Benedict Evans 的第四版幻灯片"
issue: 794
number: 10
category: favorites
original_url: "https://static1.squarespace.com/static/50363cf324ac8e905e7df861/t/6a0af5d0484fbf5fe9a7743e/1779103184855/2026-Spring-AI.pdf"
hn_url: "https://news.ycombinator.com/item?id=48179021"
date: 2026-05-22
---

## 文章摘要

Benedict Evans（前 a16z 合伙人，业内最知名的科技战略分析师之一）从 2024 年起每半年更新一版《AI eats the world》主题幻灯片，是科技圈最常被引用的策略材料之一。这次是**第四版**，2026 年春。整套幻灯片可在 ben-evans.com/presentations 找到。

HN 上 **btucker** 把四版的演化线总结得最清楚：

- **2024 年 11 月**：别小看这一波，它可能就是下一个平台级转折，但**所有关键问题都还没答案**——能否继续 scaling、有没有真用处、怎么部署、商业模式是什么。
- **2025 年 5 月**：模型层已经显现商品化（commoditization）苗头，重点应该从模型本身**移向"怎么用"**——产品、场景、UX、错误处理、企业级采购。
- **2025 年 11 月**：故事变成了**资本周期**——所有人都在大笔投，因为"错过平台转折"比"过度投入"更要命，但产品形态、护城河、价值捕获仍然没有共识，泡沫态势浮现。
- **2026 年 5 月**（本期）：暂定论是——**模型会变成基础设施（infrastructure）**，价值会向上游移动到应用、工作流、产品、专有数据 / context、go-to-market、以及"廉价自动化让我们能问的全新问题"。Evans 自己也明确写了"provisional"——还是暂定。

本期最尖锐的几张幻灯片：把 OpenAI / Anthropic 类比为 2010s 的移动运营商（AT&T / Verizon），暗示模型层会像 4G/5G 标准一样无差异化；slide 48 显示 YC 当批 startup 中 AI 占比创新高；他还引用 Sam Altman 那句被引用到滥俗的话——**"我们看到智能是像电、像水一样的公共事业（utility），人们在表上读数付费"**；另一句砸场的是"**Chat 是个糟糕的 UX，通用场景需要 'apps'**"。

## HN 评论精华

- **btucker**（最高赞）：上面提到的四版演化总结是 HN 评论区给出的"读 Evans 用什么姿势最有效"的标准答案——把他**当成连续观察者而不是预言家**。

- **aurareturn**（slide 22 异议）：他认为把模型实验室类比成电信运营商是**错的类比**——电信靠强制的 3G/4G 标准变得无差异，**而 OpenAI 和 Anthropic 更像 iOS 和 Android**：闭源、生态、品牌、UX 全部锁死。AWS / Azure / GCP / NeoCloud 才更像今天的"运营商"。他相信前沿模型是**典型的双寡头**：模型更聪明 → 使用更多 → 收入更多 → 算力更大 → 下一代更聪明，类似台积电最先进制程的 95% 份额飞轮。

- **kannanvijayan**：技术层面的隐忧——目前这种"硬塞参数 + 全球关联学习"的 mega-model 模式有**大型机时代的味道**，可能在掩盖大量低效。如果未来出现一种紧凑得多的表示方法（结合规则系统生成例子作为基础层），现在堆出来的超大数据中心就会像 PC 革命前夕的 mainframe。

- **arexxbifs**（对 Evans 那句"1997 年怎么会有人猜得到互联网会改变什么"反讽）：列出 1997 年人们就已猜到的东西——购物、广告、视频会议、协作、媒体消费、银行、金融、通信。"瓶颈是带宽和 PC 性能，不是想象力。"他暗示 AI 的真问题或许不在 PMF，而是**我们对'AI'的预期从 1950 年代就过满了**。

- **throwaw12**：套用经典 framing——硬件时代催生 IBM/Intel/MSFT，互联网时代催生 Amazon/Google/Meta，移动时代催生 Uber/TikTok，云时代催生 AWS/Snowflake/Databricks——**AI 时代的大赢家几乎注定会出现**，关键是不是当前这几家。

- **turtlesdown11**：阴阳怪气——"幻灯片里引了一堆 Zuckerberg 的话，但**关于他花在 metaverse 上的 800 亿美元一句都没引**。"

- **brainless**：押了相反方向——编程是 LLM agent 当下最大场景，但**最强模型未必能吃下最大蛋糕**。更好的 harness（工具/沙箱/数据库管理）会把更多场景挤到小模型上，"AI 吃掉软件，但 LLM 不一定吃掉 AI"。

- **dist-epoch**：引用 Ilya Sutskever 那句让人坐立不安的话——"我觉得地表大概率会被太阳能板和数据中心铺满。"AI 真的可能"字面意义上"吃掉世界。

- **2817635**：祭出怀疑论——Evans 曾经为比特币背书，**现在的"颠覆性技术"图里却没有比特币**了；他把整套演讲贬为"营销式 Gish Gallop"。

- **aworks**：自己的职业线穿越了"打孔卡 → 计算器 → 命令行 → GUI → 触屏 → 语音 → 聊天"七代界面，反对 Evans 那句"chat 是糟糕 UX"——**chat 是表现力和实用性最好的混合，外加一点魔法**。
