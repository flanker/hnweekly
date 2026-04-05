---
layout: default
title: 归档
permalink: /archive/
---

# 归档

{% assign issues = site.pages | where: "layout", "issue" | sort: "issue" | reverse %}

<ul class="archive-list">
{% for issue in issues %}
  <li>
    <a href="{{ issue.url | relative_url }}">{{ issue.title }}</a>
    <time>{{ issue.date | date: "%Y-%m-%d" }}</time>
  </li>
{% endfor %}
</ul>
