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

先用 WebFetch 抓取 `https://buttondown.com/hacker-newsletter/archive/<期数>/` 拿到**日期、引言、引言作者、分类结构和文章标题**。

⚠️ **但 WebFetch 只会返回域名（如 `stratechery.com`），拿不到完整原文 URL**。必须另外 curl 页面自己提取 href：

```bash
curl -sL "https://buttondown.com/hacker-newsletter/archive/<期数>/" -o issue.html
```

然后用脚本按顺序抽出所有 `<h1-4>` 标题和 `<a href>`（标题 + URL），去掉 `utm_*` 参数。`comments→` 那些链接即 HN 讨论页，紧跟在对应文章之后。这样才能拿到准确的 original_url。

WebFetch 解析可用以下 prompt：

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

**抓 HN 评论：直接用 Algolia API，不要用 WebFetch**

`news.ycombinator.com` 对 WebFetch 几乎必然返回 **HTTP 429**（并发多个 Agent 时更是立刻触发）。直接用 `curl -s "https://hn.algolia.com/api/v1/items/<HN_ID>"`。

返回完整嵌套评论树的 JSON（`children` 递归，字段含 `author`/`text`/`points`）。写个小脚本递归扁平化并去掉 HTML 标签即可。相比 WebFetch 的摘要，这样拿到的是**全量评论原文 + 准确用户名 + 层级和票数**，质量更高。`hacker-news.firebaseio.com` 亦可，但 Algolia 一次请求即得整棵树，更省事。

顺带：Show HN / Ask HN 的**帖子正文**也在这个 JSON 里，往往就是作者的完整自述，是原文抓不到时最好的替代材料。

**原文抓取失败的处理顺序**（不要一失败就放弃，依次尝试）：

1. WebFetch 403/402 → `curl -sL -A 'Mozilla/5.0 ...'` 带浏览器 UA 重试
2. GitHub blob 链接 → 换成 `raw.githubusercontent.com` 域名
3. SSL 证书过期（老站点常见）→ `curl -k`
4. 客户端渲染 SPA（HTML 只有骨架）→ 找替代来源：项目的 GitHub 仓库 README、作者的 HN 自述、站点的其他静态页；必要时从 JS bundle 里 grep 路由附近的字符串字面量提取正文
5. 商业新闻站（AP News 等）彻底拒绝 → 用 HN 评论中引用的原始文书/一手来源
6. 以上都不行 → 主要基于 HN 评论撰写，并在报告中如实列出

**评论区为空时不要编造**。有些条目只有几分、零到一条评论。此时在「HN 评论精华」里如实说明讨论稀少，转而用原文要点补足，绝不虚构用户名和发言。

**注意讨论被合并或重复提交**：热门事件常有多个帖子，dang 会把讨论合并到另一个 id。如果 `items/<id>` 返回的评论异常少而话题本该很热，去 Algolia 搜同标题找到真正的主帖，从那里取评论并在摘要中说明。

**其他**：
- title 必须是中文翻译的标题，用双引号包裹，内部双引号改用中文引号「」（不要用反斜杠转义，YAML 里易出错）
- number 必须是纯整数，不加引号
- **正文里出现 `{%` 或 `{{`（代码示例中的模板语法、shell 参数展开 `${...}`）必须用 `{% raw %}` ... `{% endraw %}` 包住**，否则 Jekyll 构建直接失败。#804 的 Bonsai 那篇因正文含 OCaml 的 `{%html|...|}` 而中断过构建
- SEC (`sec.gov`) 返回 403 不是反爬 UA 的问题，它要求 User-Agent 里带联系方式：`curl -A "hnweekly research contact@example.com"` 即可（用占位地址，不要带真实邮箱）
- 文件保存到 <仓库路径>/issues/<期数>/<NN>-<slug>.md
- slug 从英文标题生成（小写、连字符分隔、去特殊字符、最长5个词）
- 完成后返回：每个文件的路径 + 原文是否抓取成功（失败的注明原因和替代来源）
```

**仓库路径**: `本仓库根目录`

### Step 4: 校验

所有 Agent 完成后，**先自动校验再交给人工**。写个脚本检查 `issues/<期数>/*.md`：

- 序号 1..N 无缺失，且文件名前缀与 front matter 的 `number` 一致
- front matter 八个字段齐全：`layout: article`、`title`、`issue`、`number`、`category`、`original_url`、`hn_url`、`date`
- `number` 为纯整数、`issue` 和 `date` 与本期一致
- `title` 被双引号包裹且内部无 ASCII 双引号
- 正文中的 `{%` / `{{` 均已被 `{% raw %}` 包裹（否则下一步构建会挂）
- 两个小节 `## 文章摘要` 和 `## HN 评论精华` 都存在，正文长度不过短
- 各分类篇数与 Newsletter 原页一致（防止漏抓整个分类）

再跑 `bundle exec jekyll build`，确认 `_site/issues/<期数>/` 生成了 N+1 个页面、期刊页链接数正确、站点首页出现该期。

（注：`issues/794/` 有三个历史遗留的 YAML 报错，与本期无关，忽略。）

### Step 5: 交付与部署

1. 报告翻译篇数、各分类分布、校验结果
2. **列出所有原文抓取失败的文章及其链接**，标明依据了什么替代来源、哪几篇依据最薄，供人工重点复核
3. 提示 `bundle exec jekyll serve` 本地预览
4. **等用户确认后**再提交推送。只 `git add issues/<期数>`，不要带上 `.claude/settings.local.json`
5. 提交信息沿用惯例：`Add Hacker Newsletter #<期数> 中文翻译 (<日期>)`；本仓库历史均为直接提交 master（GitHub Pages 从 master 发布），不开分支
6. 推送后验证部署：

```bash
gh api repos/flanker/hnweekly/pages/builds/latest --jq '{status,error:.error.message,commit}'
```

等 `status` 从 `building` 变为 `built`（约 80 秒），再 curl 线上页面确认 `https://hnweekly.cn/issues/<期数>/` 返回 200、文章链接数正确、抽查几篇文章页可访问。

## 注意事项

- 每个 Agent 应使用 `run_in_background: true` 以并行执行
- 每组 5-6 篇，Agent 数量不超过 15 个（约 60 篇文章分 10 组，全部跑完约 13 分钟）
- 跳过 #Classifieds 和 Sponsored 条目（赞助条目没有 comments 链接，易识别）
- Ask HN 帖子没有单独的原文链接，original_url 使用 HN 链接
- YouTube 视频抓不到正文，但能拿到官方简介和章节列表，配合 HN 评论足够成文
- 交互式 demo 站点（3D 演示、CSS 库、游戏）正文极少，务必结合 HN 讨论尤其作者自述写足篇幅
- **如实反映讨论的真实走向**：HN 讨论时常与标题无关（例如变成「这是不是 AI 生成的垃圾」之争）。照实写，并在开头加一句说明，别硬凑成技术总结
