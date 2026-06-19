---
layout: article
title: "你的 ePub 没问题，是 Kobo 不认——该怪 Adobe"
issue: 798
number: 37
category: books
original_url: "https://andreklein.net/your-epub-is-fine-kobo-disagrees-blame-adobe/"
hn_url: "https://news.ycombinator.com/item?id=48533848"
date: 2026-06-19
---

## 文章摘要

作者 André Klein 记录了自己制作电子书时遇到的一桩典型困境：一份完全符合规范、校验通过的 ePub 文件，却被 Kobo 阅读器拒绝或显示异常。问题的根源并不在文件本身，而在于阅读器底层使用的 Adobe 渲染引擎（如 Adobe Digital Editions 所代表的那套老旧实现）对现代 CSS 支持不足。

核心症结在于：尽管 ePub 本质上只是「一个压缩进 zip 里的普通 HTML 网页」，但不同阅读器支持的 CSS 版本参差不齐。一些较新的、合法的 CSS 特性（例如涉及 `max-width` 的函数写法）会被 Adobe/Kobo 的渲染器当作错误而拒绝，迫使作者退回去迁就过时的规范。这与本应「忽略非法值」而非直接报致命错误的 CSS 规范精神背道而驰。

文章借此揭示了电子书生态的深层困境：EPUB 3.2 标准引用了不断膨胀的浏览器规范，使得旧内容随时可能变得「不合规」；而阅读器实现各行其是，跨设备兼容毫无保证。作者认为，这种局面让电子书排版重演了 2000 年代初期网页开发的乱象——人们不得不去迁就那些实现得最差的渲染器，而非放心使用符合标准的特性。

## HN 评论精华

- **anenefan**：指出 ePub 本质就是「压缩进 zip 的普通 HTML 网页」，并质疑标准的向后兼容性，坦言自己更偏爱老旧的 PDF 阅读器而非新版 EPUB。
- **gsnedders**：纠正了对 CSS 解析的误解，强调按规范「非法值」应被忽略，而不应触发致命错误。
- **L-four**：一句俏皮话点题——「永远是 CSS 的锅」。
- **charcircuit**：认为渲染器因为微小的 CSS 问题就崩溃，本身就代表实现有缺陷。
- **tech234a**：补充信息，称 Adobe Digital Editions 业务已被卖给 Wipro Engineering。
- **tannhaeuser**：讨论了 W3C 接手维护标准后，EPUB 3.2 如何导致原本可用的 EPUB 文件变得不合规。
