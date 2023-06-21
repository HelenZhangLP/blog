---
title: CSS 长度单位
date: 2020-10-22 16:31:44
tags:
- CSS
- CSS 单位
---

## CSS 中长度单位
### CSS 中绝对长度单位【Absolute Length Units】
|单位	|名称	|等价交换			|
|--		|--		|--					|
|cm		|厘米	|1cm=96px/2.54		|
|in		|英寸	|1in = 96px = 2.54cm|
|px		|像素	|1/96th of 1in		|

以上单位多用于打印，屏幕主要使用 `px`
### CSS 中相对长度单位【Relative Length Units】
> 相对于一些其他元素，如父元素字体大小、视图端口大小。相对单位经过仔细规化，可以使元素大小与页面上其它内容对应

|单位	|描述									|
|--		|--										|
|em		|相对于父元素字体大小(font-size)进行计算|
|rem	|相对于根元素字体大小(font-size)进行计算|
|vw		|1vw = 1% 的视口宽度					|
|vh		|1vh = 1% 的视口高度					|

### CSS3 中新增的单位
* ch 字符 0（零的宽度）；
* rem 根元素（html元素）的 font-size；
> font size of the root element
Equal to the computed value of 'font-size' on the root element. when specified on the font-size property of the root element, the rem units refer to the property's initial value.
This means that 1rem equals the the font-size html element (which for most browsers has a default value of 16px)
1. 字体或宽、高等单位，值根据 html 元素 font-size 计算得出
2. 动态修改 html 的 font-size，实现适配
3. IOS6 和 Android 2.1 以上基本适配
4. 1rem = html's font size(大多数浏览器是 16px)

* em 是一个相对单位，非固定值。相对于其父 font-size 计算
