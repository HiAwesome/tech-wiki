# CSS

## [CSS 基础](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web/CSS_basics)

### 多元素选择

也可以选择多种类型的元素并为它们添加一组相同的样式。将不同的选择器用逗号分开。例如：

```text
p, li, h1 {
  color: red;
}
```

### 注释

* 注：CSS 文档中所有位于 /* 和 */ 之间的内容都是 CSS 注释，它会被浏览器在渲染代码时忽略。你可以在这里写下对你现在要做的事情有帮助的笔记。
* 译注：/\*\*/ 不可嵌套，/* 这样的注释是 /* 不行 */ 的 */。CSS 不接受 // 注释。

### 盒子模型

CSS 布局主要就是基于盒模型的。每个占据页面空间的块都有这样的属性：

padding：即内边距，围绕着内容（比如段落）的空间。
border：即边框，紧接着内边距的线。
margin：即外边距，围绕元素外部的空间。

![box model](https://mdn.mozillademos.org/files/9443/box-model.png)

### [CSS 第一步](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/First_steps/What_is_CSS)

* 在 HTML概述 模块我们学习了HTML是什么，以及如何使用它来组成页面。 浏览器能够解析这些页面。标题部分看起来会比正常文本更大，段落则会另起一行，并且相互之间会有一定间隔。链接通过下划线和不同的颜色与其他文本区分开来。这些都是浏览器的默认样式——当开发者没有指定样式时，浏览器通过这些最简单的样式使页面具有基本可读性。
* CSS是一门基于规则的语言 —— 你能定义用于你的网页中特定元素样式的一组规则. 比如“我希望页面中的主标题是红色的大字”
* 
