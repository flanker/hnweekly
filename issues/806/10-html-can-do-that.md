---
layout: article
title: "原来 HTML 已经能做这些事了"
issue: 806
number: 10
category: favorites
original_url: "https://chrisburnell.com/html-can-do-that/"
hn_url: "https://news.ycombinator.com/item?id=49362689"
date: 2026-08-21
---

## 文章摘要

Chris Burnell 在 HTML Day 2026 活动上用一个小时搭出了这个页面，主题是「HTML 正在一口一口吃掉过去属于 JavaScript 的地盘」，逐条列出如今只靠 HTML 就能实现的动态功能，每条都配可交互 demo、代码片段和 Can I Use 的浏览器支持链接。8 月 20 日他做了一次重要更新，专门加重了对浏览器实现质量和无障碍缺陷的警告，并强调「尽管试，但请尽可能做到无障碍」。

清单包括：

**popover 属性**：配合 popovertarget 和 popovertargetaction 属性，就能得到点击外部自动关闭、Esc 键关闭、自动进入 top layer 而无需操心 z-index 的弹层，全程零 JavaScript。

**dialog 元素**：专用的模态对话框元素，同样可以叠加 popover 属性来开关；作者还额外附了一段用 showModal() 和 close() 的 JS 写法作对照，尽管这「有点违背本页的精神」。

**分组 details**：给一组 details 元素设置相同的 name 属性，它们就变成互斥的手风琴——打开一个，其他自动关闭。

**command 与 commandfor**：新落地的 invoker commands，让页面上多个独立按钮控制同一个弹层，不写脚本。作者注明目前只有 show-modal、close、request-close、toggle-popover、show-popover、hide-popover 六个命令进入稳定版；未来还会支持增减数值、控制媒体元素、复制文本等。

**loading="lazy"**：图片进入视口附近才加载，完全不需要 IntersectionObserver。

**hidden="until-found"**：被隐藏的区块在浏览器页内查找命中或通过锚点链接跳转时会自动展开，浏览器会自己移除这个属性值。作者提醒这个特性还很新，只跟浏览器自带搜索配合得好，屏幕阅读器的搜索实现就不太行了。

**更多原生表单元素**：颜色选择器（input type="color"）、日期选择器、range 滑块、meter 和 progress。这一节挂了最重的警告：「其中一些元素感觉相当未完成」，浏览器默认样式差异极大、无障碍体验很差，「要非常小心地使用，坑很多」，他希望未来几年表单元素能得到更多关照。

**datalist**：原生自动补全建议，不需要下拉库。同样带警告：跨输入类型的支持仍相当零碎，浏览器实现存在若干问题，他建议读者参考 Adrian Roselli 的《Under-Engineered Comboboxen》，甚至「现阶段干脆先别用，观察未来几年会不会改善」。

整个页面的基调因此是双重的：一方面炫耀标准的进步，一方面反复提醒规范落地和无障碍之间的巨大落差。

## HN 评论精华

这条帖子拿到 1004 分、181 条评论，是本期讨论最热的一条。主线并不是「HTML 真棒」的一片叫好，而是分成三股：datalist 到底能不能用（最高票的批评帖甚至一度被标记为 dead 又被 vouch 救回）、无 JavaScript / 反 SPA 的立场表达，以及一个意外的话题——LLM 完全不会写这些新标准。

- **yurishimo** 的批评帖排在最前：如果你需要一份强约束，datalist 不是好方案——用户仍然可以在字段里输入任意内容，而且没有模糊过滤、没有拼写纠错；一旦有这些需求，用一个功能完整的 combobox 库就合理得多。**dematz** 专门发帖说这条评论不该被判 dead，**kotaKat** 补充科普：开了 showdead 的人可以点评论时间戳用 vouch 反向投票救回来。yurishimo 后来道谢并说明了他为什么这么较真：「只想看浏览器技术上能做什么，我们都会读 Chrome 开发者博客；来 HN 是为了更宽的视角，有人讲讲某个小特性怎么反咬他一口，这本身就有价值。」
- **vlucas** 现身作证：他自己走过一遍「HTML 能做这个！」的路，结论是 HTML 确实替代不了一个好的带搜索的 combobox，最后他违背自己的原则，在一个纯 HTML 页面里为这一个输入框插了个 React 岛。**Groxx** 查了 caniuse 说 Firefox 至今给不了他任何反馈、支持面相当零碎且明显还有 bug，「直接跳过」。**theandrewbailey** 说如果能过滤 datalist 并强制它显示也许还行，可惜没有办法强制显示，他最后改用 ul/ol 了。**honr** 则提出不同角度的反驳：用户反正能用开发者工具改掉所谓的强约束，所以让用户输入、前端只做即时反馈（比如变红），真正的校验放服务端；被 **yurishimo** 和 **jonahx** 指出这是正交问题——前者讨论的是 UX，不是后端校验。
- **jdlshore** 问 hidden until-found 到底有什么用，引出一串真实用例：**dofm** 说这是单向的「显示隐藏内容」机制，浏览器内置搜索命中时会自动弹开，适合商品页上「查看定价条款」「显示除外责任」这类只在需要时才读的内容；**akersten** 说他要的就是在大页面上按 Ctrl+F 能搜到折叠区块里的东西而不用先手动全部展开；**wvbdmp** 说他一直用 details 做这件事，例子是可折叠的组织架构树，Ctrl+F 仍能找到被折叠的团队和人；**megaman821** 用它做标签页内容，让页内搜索能命中并展开对应标签。**dspillett** 则想到一个非预期用途：这可能是个放投毒内容「款待」爬虫的方便地方，不过人类也会误撞进去，所以用带警告 summary 的 details 更友好；他还提醒要先确认老旧 UA 和无障碍工具遇到 hidden 属性上不认识的值时会当作隐藏还是显示。**pseudosavant** 猜是给「跳到主内容」这类跳过链接用的，**extra88** 纠正说那是靠聚焦时显示解决的，真正的动机大概是有人想让页内查找自动展开 details，然后意识到给布尔属性 hidden 加一个可选值就能顺带覆盖自定义折叠控件。
- **jamescun** 自陈是 2026 年还在用 NoScript、逐站放行 JavaScript 的那一小撮统计噪声，说这在现代 web 上越来越难，希望这些新特性能普及、也希望大家意识到多数场景其实不需要 SPA，最差配一点 HTMX 就够。这条引出了一大串支线：**evenhash** 说跑 NoScript 后最深的体会是 Google 无处不在，即使站点不接 Google 广告，也有相当概率从 ajax.googleapis.com 拉脚本，而 Google 靠脚本请求的 Referer 就知道你访问了这个站；**account42** 补刀说浏览器厂商（包括 Google 自己）还决定了缓存不跨源共享，所以哪怕你刚在别的站加载过同一个资源，这个请求每站都得重发一次。**gunalx** 抱怨 SPA 是他知道的最烦人的 web 模式之一，希望每件事都有独立 URL 好收藏；**tyre** 和 **ThunderSizzle** 反驳说 SPA 有路由、按经验会同步 URL 和历史，做得好完全能刷新恢复状态。**lo_fye** 感慨「我还记得页面加载超过 250 毫秒被认为荒谬的年代，现在 25 MB、30 多秒加载的页面没人皱眉」。**nozzlegear** 问 HTMX 在 NoScript 下能用吗，**ipsod** 答：不能，它需要 JS。**edparcell** 说他被那些本该只是文档却堆满 JS 库和第三方请求的站点烦透了，于是给「纯文档型站点」定了个标准并写了检查器（certifiedweb10.org），并类比说当年真正改变 web 的是浏览器开始标记非 SSL 站点，这里可能也需要类似机制。
- **dajonker** 是最有分量的实战反馈：他们整个生产应用大量使用 popover、dialog 和 invoker commands，效果非常好；dialog 和 popover 渲染在 top layer、嵌套 popover 自动堆叠并支持级联关闭，都说明这些标准设计得相当好。唯一麻烦的还是把 popover 定位到触发它的元素旁边（比如上下文菜单要在按钮上方或下方），CSS 锚点定位（anchor positioning）虽然有了但支持有限、心智模型也难。他还补了一句意外引爆讨论的话：**LLM 对这些新标准非常糟糕**，即使知道也常以为它们还没进 baseline，而且相比之前那座「奇形怪状的 JS 和 CSS 大山」，它们的训练数据几乎为零。
- 围绕这句话形成了整场最热的支线：**Wowfunhappy** 担忧「AI 编程会不会把我们永久锁死在 2024 年前后的语言和库上」；**bjackman** 提议应该发布一个「HTML 能做什么」的 Markdown 库供 AGENTS.md 调用，随后发现已经有人做了；**alwillis** 指出 Google 在 I/O 上发布了一套叫 Modern Web Guidance 的技能包，专门引导编码 agent 使用现代 CSS 和最佳实践，还有 paulirish 的 modern-css 技能，「新一些的模型比如 Opus 5 对现代 CSS 应该好得多」；**jmpeax** 直接调侃这是「skill issue」。
- 定位问题上，**nozzlegear** 和 **maya335** 都说锚点定位一旦想通就很简单，**maya335** 给出经验：别把它当绝对偏移，而是「声明偏好的边，让浏览器自己翻转」，嵌套上下文菜单仍别扭，但比在 resize observer 里测 getBoundingClientRect 健壮得多。**doginasuit** 给了个具体技巧：在 popover 类上写 top: anchor(bottom)，未设置 position-anchor 时它会自动解析到调用它的按钮。
- **peesem** 提了个逆向的 UX 问题：为什么大家这么在意让 details 只能开一个？「我想看什么就让我看什么！」**abanana** 支持他，说包括 Nielsen Norman Group 在内的 UX 专家多年来都建议不要在打开新项时自动关闭已打开项，理由就是用户挫败感——「我要关我会再点一次，别藏我可能还在读的内容」。**asdfsa32** 则替互斥手风琴辩护：面对很长的 FAQ 列表时能快速滑过是好事。
- 其他零碎但有用的点：**werdnapk** 问 details 还不能做动画吗，**adzm** 和 **esprehn** 答现在可以了，给 ::details-content 伪元素加 CSS 动画即可，只是没有默认动画（esprehn 说「当初真该让它默认平滑开合」）。**cush** 说纯 HTML 的 dialog 还是不处理 inertness，**extra88** 纠正说用 button command="show-modal" 打开模态 dialog 就有。**bingemaker** 补充 img 还支持 srcset，**theandrewbailey** 和 **callc** 认为 picture + source + img 更灵活，因为 source 的 media 是真正的媒体查询、能响应式换图，而 Chromium 系的 srcset 从不「降级」到更小的图。**hyperhello** 质疑为什么要发明这些只服务于 dialog 的新属性和动作机制，「这不正是 JavaScript 该干的事吗」。**devinhades333** 留下最讽刺的一条：这个页面在他那儿慢得根本没跑起来，「看来 HTML 还不够」。**220hertz** 则做了全场最好的收尾：「2026 年了，我们还在这儿像疯子一样讨论 HTML。」
