---
title: BFC
date: 2019-02-20 15:27:42
catergories:
- 技术
tags:
- 前端
- CSS
---
[个人的官方是 MDN 文档，信得过，值得依赖](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)
BFC - Block Formatting Context Web 页面可视化css渲染的一部分，是布局过程中生成生成块级盒子的区域。
同时它还列出了创建块级格式化上下文的方式（Block Formatting Context）
[同时借鉴 前端精选文摘：BFC 神奇背后的原理 ]（https://www.cnblogs.com/lhb25/p/inside-block-formatting-ontext.html）发现真正要理解 BFC 不仅仅要清楚创建块级格式化上下文的方式还要清楚一些它布局的规则。
下面，结合 demo 对 BFC 作一个简单的解析
#### 1. overflow 值不为 visible（default visible）
#### 2. 浮动元素（float 不为 none）
### 使用案例，两栏弹性布局
> 盒子 .bfc1 与 盒子 .bfc2，左右布局，左侧盒子宽 100，左侧盒子根据父元素高自适应
```html
<body>
  <div class='content'>
  	<div class='bfc1'>bfc</div>
		<div class='bfc2'>bfc</div>
  </div>
</body>
```
```css
.content {
	width: 400px;
	border: solid 3px #dd9;
	background: #ccc;
	margin-left: 10px;
}

.bfc1 {
	width: 100px;
	height: 150px;
	background: #f99;
}

.bfc2 {
	height: 300px;
	background: #fcc;
}
```
{% qnimg bfc-float-overflow-1.png extend: ?imageView2/2/w/500 %}
> 由图可以看出，块状元素布局是由上自下，每个元素与父元素左边框接触
左浮动实现两列表布局

```css
.content {
	width: 400px;
	border: solid 3px #dd9;
	background: #ccc;
	margin-left: 10px;
}

.bfc1 {
  float: left;  // 左浮动实现两列表布局
	width: 100px;
	height: 150px;
	background: #f99;
}

.bfc2 {
	height: 300px;
	background: #fcc;
}
```
{% qnimg bfc-float-overflow-2.png extend: ?imageView2/2/w/500 %}
> 由图可以看出，两个子元素依然紧贴父元素左边框
  如果两个fqgx
