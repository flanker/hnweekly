---
layout: article
title: "MicroLLM Lab：在浏览器里试玩 7 个微型语言模型"
issue: 812
number: 26
category: data
original_url: "https://stateofutopia.com/experiments/microllmlab/"
hn_url: "https://news.ycombinator.com/item?id=49882781"
date: 2026-10-02
---

## 文章摘要

MicroLLM Lab 是 stateofutopia.com 上的一个交互式实验页面，作者在 HN 上的账号是 logicallee，代码开源在 GitHub（robss2020/microllm-lab）。它的卖点一句话就能说完：不用 API key、不用 Python、不用装 CUDA，在已经打开的浏览器标签页里，通过 WebGPU 直接调用本机 GPU，跑一批参数量在 2600 万到 3.6 亿之间的"小语言模型"（SLM）。模型权重以 4 比特（Q4，每 32 个一组）量化，一个 1 亿多参数的模型只有 50 到 80 MB 左右，下载后缓存在浏览器的 IndexedDB 里，提示词和数据从不离开本机。

**为什么要折腾小模型。** 项目 README 开门见山："云端模型更好，但这不是重点。"作者列出的理由是：隐私是默认的，适合不该贴进聊天机器人的草稿或病历；首个 token 在本地生成，没有排队、冷启动或"容量已满"；下载之后成本为零，在飞机上也能用；而且这些模型"诚实地承认自己小"，1.35 亿参数的网络会算错算术、会编造事实，这个实验室的目的就是**测量**这一点，而不是用一层聊天外壳把它藏起来。页面上的说明还把小模型定位为一个"边缘层"：在毫秒级内完成分类、过滤垃圾信息、提取意图，再决定要不要把请求交给昂贵的云端大模型。对于要在设备端做功能的开发者，它提供了一种在用户同款浏览器里直观感受质量与速度取舍的方式。

**模型清单。** 标题说的是 7 个模型，当前 README 列出了 8 个：

- **PetitGPT research-v1**（1.246 亿参数，74 MB）：GitHub 用户 yangqi0 在一张 RTX 4090 上用约 130 亿 token 训练的 Llama 风格解码器（RMSNorm、9/3 分组查询注意力、RoPE、SwiGLU），也是这个实验室的起点和基准，通常是最快的一个。
- **SmolLM2 135M Instruct**（80 MB）：Hugging Face Smol Models 团队 2025 年的第二代小模型，预训练约 2 万亿 token 后做指令微调，形状与 PetitGPT 相同，适合做速度与准确率的对照。
- **SmolLM 135M Instruct**：2024 年的第一代，约 6000 亿 token，可以看出一年的数据和训练配方进步带来了什么。
- **L20-Edu 135M**：AliceYin 在一张 NVIDIA L20 上用约 130 亿 token 训练，定位是"单卡能做到什么"的可审计教学检查点。
- **SmolLM2 360M Instruct**（216 MB）：更大、更慢，通常也更准。
- **MiniMind2 104M** 与 **MiniMind2 Small 26M**：jingyaogong 面向中文学习者的从零训练教学项目，中文比英文强，26M 版本只有 15 MB。
- **GPT-2 124M**：OpenAI 2019 年的模型，nanoGPT 的架构原型，只是补全模型，没有做对话微调，贪心解码经常陷入循环，README 特别说明"那是模型本身的问题，不是解码器的 bug"。

**功能。** 页面分为聊天、基准测试、对比与证书、自定义评测四个标签页。基准测试是客观检查（正则或精确 token 匹配），例如 `1+1=2`、`2+2=4`、德国首都、从上下文复制内容等，页面明确写着"1.35 亿参数的模型允许失败，失败本身就是测量结果"。对比页会绘制各模型的准确率、平均 tok/s 和测试总耗时；还能生成一张带有本机硬件信息和速度成绩的"性能证书"分享到社交网络；自定义评测允许用户用 JavaScript 写自己的检查。运行时面板会显示后端类型、tok/s、首 token 延迟、JS 堆和 GPU 缓冲区占用等。引擎优先使用 WebGPU（为 Apple Metal 和 NVIDIA 分别写了融合内核），不可用时回退到 WASM 再到纯 JS。

**实测数据。** 作者在 Apple M4（Safari、Metal Q4、贪心解码）上的结果是：回答"Say hello"，PetitGPT 87 毫秒、115 tok/s，SmolLM2-135M-Instruct 227 毫秒、66 tok/s；在扩展的 20 项客观测试上，SmolLM2 通过 14 项，PetitGPT 在早期 14 项短测试中通过 10 项。指令微调模型更慢（词表更大，还有 ChatML 前缀），但在这些"笨问题"上明显更准，作者说这正是这个实验室想展示的对比。整套测试在 M4 上 PetitGPT 约 1.5 秒、SmolLM2 约 6 秒。README 还提供了把其他 Llama 风格或 GPT-2 风格的 Hugging Face 检查点转换成 Q4 格式的脚本。限制方面：上下文上限 2048 token（为了让 KV 缓存放进笔记本 GPU），只支持贪心解码，模型会幻觉，不要用于重要问题。

## HN 评论精华

帖子 284 分、约 100 条评论。评论区一半在晒小模型的离谱回答，一半在吐槽页面 UI 和兼容性问题，作者 logicallee 全程在线逐条回复、修 bug。

**晒离谱回答成了主旋律。** tolugenius 问 PetitGPT"2+2 等于几"，得到"2 + 2 = 4，所以 2 + 2 = 4 + 2"，他只回了一个词"精彩"。inventor7777 得到的是"2+2 是 2"，dvh 接话"对于非常小的 2 来说确实如此"。tcgv 贴出一整段和 PetitGPT 的拉锯：被指出错误后，模型先是答"是的，我很乐意帮你"，又说"答案不应被视为完整的陈述"，最后才说出"二加二等于二 + 2 等于四"，他说这让人想起早年的提示工程时代。touchme 问"猫是动物吗"，PetitGPT 答"猫不是动物。猫是有脊椎和心脏的温血动物，它们不是哺乳动物"。philipallstar 和 boonzeet 问南非首都，模型坚持是"维多利亚"，被纠正"南非有三个首都"后答"中国、印度、俄罗斯"。not2b 问加州人口，SmolLM2 360M 在同一段话里给出 1.53 亿和 3.25 亿两个数字。mgaunard 问它和 Anthropic 的 Claude Fable 5.1 相比能力如何，模型答道"两者都是关于一个狡猾、善于操纵的小丑动物的故事"，Reviving1514 说"至少小模型更有自知之明"。tecleandor 发现 GPT-2 尤其糟，krackers 和 logicallee 解释它是 2019 年的基础模型，没有对话微调；wyrdcurt 补充说应该把它当文本补全器用，按对的结构给提示就能得到相对连贯的续写，"提示工程"这个词正是从那个年代来的，如今这个想法已经基本过时，不到十年间模型的进步"绝对疯狂"。

**这些模型到底有什么用？** botanrice 的疑问代表了不少人：他让每个模型给出一份五分钟内能做好的早餐食谱，没有一个能用，有的还要"8 杯奶酪"，连页面自带的基准都过不全，那它们能干什么？smokel 回答：小模型几乎没有知识，也缺乏推理能力，用途在于情感分析、文本分类、实体抽取之类，不该拿来和 Opus、Fable 比。ironqcold 也说这是 1 亿参数的模型，不是助手。不过 Mbarley、hbroom 等人对推理速度印象深刻，觉得本地浏览器推理比预想的快得多。derliebej 和 Rexxar 则调侃起命名："微型大语言模型"到底算什么，是不是该叫 TLM 或 μLM。Surac 发现模型只支持英语，德语问题会被原样重复。

**UI 被批"AI 味"。** demibabs 建议作者好好看看 UI：字太小、信息太密，产品这么简单，却要先滚过一整页"大多没用、AI 生成"的说明才能看到界面，页脚还莫名其妙地链回站点本身。post-it 说这是"Claude 特供"风格，fl0id 说这些仪表盘都长得差不多。随后 phist_mcgee 说"这就是软件的未来，粗糙的 UI"，azan_ 反讽"好像 LLM 出现之前没有粗糙的 UI 似的"，aleksiy123 认为这次更糟，因为它冗长且乍看像模像样；post-it 的观察很有意思：以前加一个文本框是要花功夫的，所以老式粗糙 UI 是大段文字加错位的字段，而 AI 写代码毫不费力，所以 AI 生成的粗糙 UI 到处都是文本框。

logicallee 后来给了一个很详细的回复：帖子在 HN 首页第 3、4 位挂了 12 小时，带来了 3.6 万个独立 IP、超过 30 万次请求（平时两天大约 1600 个独立 IP），用户总共下载了 600 GB 模型，第一个模型的下载发起了 8500 次，其中 3000 次完成。他认为这说明呈现方式引起了共鸣，所以保留了说明内容，但在顶部加上醒目的 tl;dr，把大段介绍标为"选读"，放大了字号，删掉了页脚中不合理的部分。janalsncm 不认同这个推论：先说明这是个很酷的 demo，但帖子是"尽管有那些烦人的部分"才火的；那些说明里有明显错误却没人发现，恰恰说明大家把它当噪音忽略了，demo 应该"展示而不是讲述"，延迟低不需要写出来，用户自己能看到。

**兼容性问题与作者的修复。** anonimous-emacs、ad_fontes、cwnyth、langurmonkey 都在 Linux 上的 Firefox 遇到 `GPUShaderStage is not defined` 错误，utopiah 和 Doohickey-d 指出 Linux 版 Firefox 默认没有开启 WebGPU，butz 批评"优先使用 WebGPU"开关根本不起作用，建议至少做个能力检测。logicallee 逐一回复，表示已修复，在 Linux Firefox 上会退回到较慢的 WASM 实现。rvz 说"这个网站上没有一样东西能用"，作者回应自己在 Windows、Mac、iPhone 和四种浏览器上都测过，只是服务器在 1Gbps 带宽上同时给许多人送约 600 MB 权重，加载会慢一些，fragmede 建议改用 CDN。gslepak 说页面让 macOS 崩溃了。

**同类项目。** 不少人顺势分享了自己的作品：bhouston 用 Three.js 的着色语言实现了浏览器内 LLM 推理，可以跑 Qwen 0.8B，作者试用后得到 12 tok/s 且输出连贯；sourweasel 推荐加入 MiniCPM5-1B，说它在 Pixel 9 上 CPU 推理能到 33 tok/s；kenzic 提出了一个名为 Web Models API 的浏览器标准提案，让网页可以调用设备端的开放权重模型，作者和他讨论了模型从哪里下载、是否可以用 P2P 分发等问题；moooff 分享了单文件 HTML 的 WebGPU 聊天应用 HermitUI，强调可以在离线隔离环境中向非技术人员展示家用电脑无需安装就能做到什么；atobe 分享了一个基于 TinyStories 训练的小模型 demo，还能体验三值模型。
