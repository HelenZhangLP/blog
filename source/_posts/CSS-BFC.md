---
title: CSS BFC
date: 2019-02-20 15:27:42
categories:
- 技术
tags:
- 前端
- css
---
[个人的官方是 MDN 文档，信得过，值得依赖](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)
[同时借鉴 前端精选文摘：BFC 神奇背后的原理]（https://www.cnblogs.com/lhb25/p/inside-block-formatting-ontext.html）发现真正要理解 BFC 不仅仅要清楚创建块级格式化上下文的方式，还要清楚一些它布局的规则。

**BFC - Block Formatting Context Web** 页面可视化css渲染的一部分，是布局过程中生成块级盒子的区域，也是浮动元素与其它元素交互的区域。

### 创建块级格式化上下文的方式（Block Formatting Context）
- `绝对定位元素`**position: absolute/position: fixed** 会创建块级上下文
> 图标居定位右上角 [以下 demo 代码](https://github.com/HelenZhangLP/demo/blob/master/postionAbsoluteFixedBlock/index.html)

```html
<ul>
  <li><a href="#">navigator1</a></li>
  <li><a href="#">navigator2</a></li>
  <li><a href="#">navigator3<i>vip icon</i></a></li>
</ul>
```
```css
i {
  position: absolute;
  margin: -2px 0 0 1px;
  color: #f90;
  text-indent: -9999px;
  width: 28px;
  height: 11px;
  background: url(http://img.mukewang.com/545304730001307300280011.gif) no-repeat;
}
```
![position-absolute-block-1](/images/demo/positionAbsoluteFixedBlock/position-absolute-block-1.jpg)
上图是一个注释掉`position: absolute`后的错误示例，如 `<i>` 非块级元素，`text-indent: -9999px` 不生效，`margin: -2px 0 0 1px`在垂直方向同样不生效。注释文本撑开，显示vip图片，若无`vip icon`，背影图同样不显示，因为 `width/height` 对非块级元素不生效。`放开position: absolute 注释后，BFC 化后，实现效果如下图`

![position-absolute-block-2](/images/demo/positionAbsoluteFixedBlock/position-absolute-block-2.jpg)

---
- overflow 值不为 visible（default/visible）
- 浮动元素（float 不为 none）
> 浮动实现两列表布局
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
