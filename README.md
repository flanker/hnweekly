# HN Weekly 中文

[Hacker Newsletter](https://hackernewsletter.com/) 每周中文翻译。

每期翻译全部文章，包含中文深度摘要和 Hacker News 评论区精华讨论。

## 访问

[hnweekly.cn](https://hnweekly.cn)

## 已翻译期数

- [#788](issues/788/) (2026-04-03)

## 本地运行

```bash
bundle install
bundle exec jekyll serve
```

访问 http://localhost:4000

## 翻译流程

使用 Claude Code 的 `/translate-hn` skill 自动翻译：

```
/translate-hn 789
```

流程：抓取 Newsletter → 解析文章列表 → 并行翻译每篇文章 → 生成 Markdown → 人工审核 → 发布

## 致谢

- [Hacker Newsletter](https://hackernewsletter.com/) by Kale Davis
- [Hacker News](https://news.ycombinator.com/)
- 翻译由 [Claude](https://claude.ai/) 辅助生成

## License

内容翻译版权归原作者所有。站点代码 MIT License。
