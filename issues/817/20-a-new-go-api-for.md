---
layout: article
title: "Go 推出新的 JSON API：分离语法处理与类型映射"
issue: 817
number: 20
category: code
original_url: "https://go.dev/blog/jsonv2"
hn_url: "https://news.ycombinator.com/item?id=50369106"
date: 2026-11-06
---

## 文章摘要

Go 官方博客介绍，在经过实验和反馈后，encoding/json/v2 与 encoding/json/jsontext 已在 Go 1.27 成为稳定可用的标准库包。新设计将 JSON 语法层与 Go 值的语义映射分开：jsontext 处理 token、完整 JSON 值及流式读写，v2 则负责把它们转换为 Go 类型。这样自定义序列化方法可以直接操作编码器或解码器，减少旧接口要求完整缓冲、重复解析和重新校验带来的开销。

默认行为也更严格：拒绝非法 UTF-8 和重复对象成员名，字段匹配区分大小写，nil 切片与 map 默认编码为空数组或空对象；omitempty 的判定改为关注编码后的空 JSON 值。许多差异可通过选项控制，因此迁移不应只改导入路径，还要检查与现有客户端约定和边界输入的关系。

兼容性通过保留 v1 API 及相应选项来维护。官方称 v2 的 Marshal 总体与 v1 接近，而 Unmarshal 在某些基准下可快至十倍；这不是所有应用都能得到的加速保证。真正流式的自定义方法尤其能改善递归解析的重复工作。文章同时强调开发经历约六年、由多方贡献者完成，展示了在长期兼容承诺下修正基础 API 的方式。

## HN 评论精华

- **运行时错误仍有必要。** lolpanda 质疑 Marshal 为何返回 error；sgsjchs 提到非有限浮点值，duskwuff 提到自定义方法和运行时类型，都可能造成序列化失败。
- **类型检查与值检查是不同层次。** thayne 希望更强的类型系统提前拒绝不可序列化类型，但这不能排除所有依赖运行时值的失败。
- **缩进并未消失。** CamouflagedKiwi 怀念 MarshalIndent；arccy 给出通过 jsontext.WithIndent 传入选项的等效写法，说明迁移需要熟悉新的选项入口。
