---
title: CSS position absolute
date: 2020-10-12 12:40:01
tags:
- css
---

> position: absolute 与 float 一样，使用后**脱离文档流**；具有**破坏性**和**包裹性**；使用会创建 BFC(Block_formatting_context)。
absolute 相对于最近的非 ‘static’ 元素定位；如果该元素不存在，则相对于 initial container block 定位

```html
<div style="width: 300px; height: 100px; background: #efefef;">
  <div style="width: 100px; height: 50px; background: #390;"></div>
</div>
```
![position-absolute-1](/images/css/position-absolute-1.jpg)
<!--more-->

### 1. **`具有破坏性`** 子元素加入 position: absolute; 至父元素高度塌陷
```html
<div style="width: 300px; height: 100px; background: #efefef;">
  <div style="width: 100px; height: 50px; background: #390; position: absolute"></div>
</div>
```
![position-absolute-2](/images/css/position-absolute-2.jpg)

### 2. **`具有包裹性`** 父元素使用 position: absolute，父元素包裹子元素
```html
<div style="width: 300px; height: 100px; background: #efefef; position: absolute">
  <div style="width: 100px; height: 50px; background: #390;"></div>
</div>
```
![position-absolute-3](/images/css/position-absolute-3.jpg)

### 3. **`创建 [BFC(Block_formatting_context)](https://helenzhanglp.github.io/2019/02/20/CSS-BFC/)`** 使用 position: absolute，会形成块级作用域，行内元素可使用 width/height 等属性。
[以下 demo 代码](https://github.com/HelenZhangLP/demo/blob/master/postionAbsoluteFixedBlock/index.html)

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

### 4. demo-图标与提示文本对齐，效果如下图
![图标与文本对齐](/images/css/position-absolute-4.jpg)
> 1. 利用 float

```html
<span class="regist-remark regist-warn">
	<i class="icon-warn" style="float: left"></i>
  <i style="font-style: normal;">邮箱格式不准确</i>
</span>
```

> 2. 利用 position

```html
<span class="regist-remark regist-warn">
	<i class="icon-warn" style="position: absolute; margin-left: -20px;"></i>邮箱格式不准确
</span>
```

### 5. demo-利用 position: absolute, 在提示文字溢出显出区域时，不折行或隐藏，正常整行显示。

### 6. demo-利用 position: absolute，实现遮罩层
> position: absolute 在文档流之外创建了一个层
  left: 0 + right: 0; 新创建的层拉伸至与定位层等宽，相当于 width: 100%;
  同理： left: 0 + right: 50% = width: 50%;（可用于一半遮罩）
  top: 0 + bottom: 0; 新创建的层拉伸至与定位层等高，相当于 height: 100%;

  **`注意：`** 这种方法适用于 `IE7+` 以上浏览器

```css
div {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  background-color: #000;
  opacity: .5;
  filter: alpha(opacity=50); // 兼容 IE 浏览器
}
```
