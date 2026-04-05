---
layout: default
title: 首页
---

# HN Weekly 中文

每周翻译 [Hacker Newsletter](https://hackernewsletter.com/)，用中文深度解读科技圈最值得关注的文章和讨论。

---

{% assign issue_dirs = site.pages | where: "layout", "issue" | sort: "issue" | reverse %}
{% for issue in issue_dirs limit: 10 %}
<div class="issue-card">
  <h3><a href="{{ issue.url | relative_url }}">{{ issue.title }}</a></h3>
  <time>{{ issue.date | date: "%Y-%m-%d" }}</time>
  {% if issue.quote %}
  <div class="preview">"{{ issue.quote }}" — {{ issue.quote_author }}</div>
  {% endif %}
</div>
{% endfor %}

[查看所有期数 &rarr;]({{ '/archive' | relative_url }})
