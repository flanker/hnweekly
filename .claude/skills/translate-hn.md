---
name: translate-hn
description: 翻译 Hacker Newsletter 指定期数，生成中文 Markdown 到 issues/ 目录
user_invocable: true
---

# /translate-hn <期数>

翻译 Hacker Newsletter 指定期数的所有文章，生成中文深度总结，输出到本仓库 `issues/<期数>/` 目录。

## 使用方式

```
/translate-hn 789
```

## 执行流程

### Step 1: 抓取并解析 Newsletter

使用 WebFetch 抓取 `https://buttondown.com/hacker-newsletter/archive/<期数>/`。

用以下 prompt 解析：

```
解析这个 Hacker Newsletter 页面，提取以下信息并以 JSON 格式返回：

1. date: 发布日期（格式 YYYY-MM-DD）
2. quote: 开头引言
3. quote_author: 引言作者
4. articles: 文章数组，每篇包含：
   - number: 序号（从 1 开始）
   - title: 英文标题
   - original_url: 原文链接（去掉 utm 参数）
   - hn_url: HN 讨论链接（去掉 utm 参数）
   - category: 分类，使用以下英文 slug：
     - #Favorites → favorites
     - #Ask HN → ask_hn
     - #Show HN → show_hn
     - #Code → code
     - #Data → data
     - #Design → design
     - #Books → books
     - #Working → working
     - #Learn → learn
     - #Watching → watching
     - #Startup News → startup_news
     - #Fun → fun

注意：
- 跳过 #Classifieds 分类的广告条目
- 跳过纯招聘帖 (Who is hiring / Who wants to be hired)
- original_url 和 hn_url 去掉所有 utm_* 查询参数
- 返回纯 JSON，不要 markdown 代码块包裹
```

### Step 2: 创建期刊目录页

在 `issues/<期数>/index.md` 写入：

```yaml
---
layout: issue
title: "Hacker Newsletter #<期数>"
issue: <期数>
date: <从 Step 1 获取的日期>
quote: "<引言>"
quote_author: "<引言作者>"
permalink: /issues/<期数>/
---
```

### Step 3: 并行翻译文章

将文章列表按每 5-6 篇分组，启动并行 Agent。每个 Agent 的 prompt 模板：

```
你需要抓取以下文章的正文和 HN 评论，然后用中文写出详细总结，保存为 markdown 文件。

每个文件格式：

---
layout: article
title: "中文标题"
issue: <期数>
number: <序号>
category: <分类slug>
original_url: "<原文URL>"
hn_url: "<HN URL>"
date: <日期>
---

## 文章摘要
（详细的中文总结，至少 300 字）

## HN 评论精华
（挑选最有价值的评论，翻译总结）

---

文章列表：
<逐篇列出：序号、标题、原文URL、HN URL、保存路径>

规则：
- 用 WebFetch 抓取原文和 HN 评论页
- 如果原文抓取失败（403等），主要基于 HN 评论内容撰写总结
- title 必须是中文翻译的标题，用双引号包裹，转义内部双引号
- 文件保存到 <仓库路径>/issues/<期数>/<NN>-<slug>.md
- slug 从英文标题生成（小写、连字符分隔、去特殊字符、最长5个词）
```

**仓库路径**: `本仓库根目录`

### Step 4: 完成

所有 Agent 完成后：

1. 报告总共翻译了多少篇文章
2. 列出任何抓取失败的文章
3. 提示用户：
   - 运行 `cd 本仓库根目录 && bundle exec jekyll serve` 本地预览
   - 确认无误后提交并推送

## 注意事项

- 每个 Agent 应使用 `run_in_background: true` 以并行执行
- 分组时注意 Agent 数量不要超过 15 个
- Ask HN 帖子没有单独的原文链接，original_url 使用 HN 链接
- YouTube 视频链接通常无法抓取，依靠 HN 评论内容
