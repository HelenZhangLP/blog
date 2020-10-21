---
title: CSS 居中
date: 2019-05-05 17:24:39
category: 技术
tags:
- CSS
- 面试系列
---

居中分为两种：一是水平居中，二是垂直居中。可借用不同的布局方式做多种实现。

## 水平布局 + Normal flow
不进行任何布局，采用 html 默认布局方式
1.  行内元素，父容器设置 text-align: center;
2.  块级元素：margin: 0 auto;

<!--more-->

## 水平布局 + Flexbox
1.  父容器设置：display: flex; justify-content: center; align-items: start; // 垂直方向不要拉伸
2.  父容器设置：display: flex; margin: 0 auto;

## 水平布局 + grid (IE 浏览器不兼容)
父容器：display: grid; 子容器 justify-self: center; align-self: center;

## 水平布局 + 定位 position: absolute
这里有三种实现(假设 元素 width: 100px)：
1. position: absolute; margin-left: 50%; transform: translateX(-50%);
2. position: absolute; margin-left: 50%; transform: translateX(-50px);
3. position: absolute; left: 50%; margin-left: 50px;
4. position: absolute; left: 0; right: 0; margin: auto;

>`注意：`
1.  absolute 相对于最近的非 ‘static’ 元素定位；如果该元素不存在，则相对于 initial container block 定位。[详见](https://helenzhanglp.github.io/2020/10/12/CSS-position-absolute/)
2.  transform 属性兼容 IE10 以上浏览器
3.  [有关第4种实现方式的理解](https://helenzhanglp.github.io/2020/10/19/CSS-Margin-auto/)

## 水平布局 + 定位 position: relative
需要居中的 block 元素上设置样式： position: relative; margin: auto;

---

## 垂直居中 + Normal Flow
1. 行内元素，父容器：line-height: height

## 垂直居中 + flexbox
1. 父容器：display: flex; 要实现居中的子容器：margin: auto 0(垂直水平居中 margin: auto)

> # display: block

## 块级元素垂直居中 - `display: block; position: relative; margin: -25px auto 0;`
> 居中元素相对于父元素向下偏移父元素的 left: 50%；再向上偏移该元素的 margin-top: -50%
  *** 必须 - 居中元素的高 ***

<img src="https://s2.ax1x.com/2019/05/10/ERVQnx.png" alt='块级元素垂直居中' width="300" hegiht="200" align=center />
```css
.db-vertical-center {
  position: relative;
  top: 50%;
  margin: -25px auto 0;
}
```

## 块级元素垂直居中 - `display: block; position: absolute; margin: auto;`
> position: absolute; 脱离文档流
  margin-left + left + width + margin-right + right = view width
  margin: auto 浏览器计算 margin
  脱离文档流后 margin 根据原位置进行位移

<img src="https://s2.ax1x.com/2019/05/10/ERVlB6.png" alt='块级元素水平居中' width="300" hegiht="200" align=center />
```css
.wrap {
  width: 300px;
  height: 200px;
  background-color: #efefef;
  overflow: hidden;
  position: relative;
}
.db-center {
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  margin: auto;
}
```

## 块级元素垂直居中 - `position: relative; left & top calc()`
> 根据父元素定位，calc 计算 left , top 位置。 用父元素的 50% - 子元素的 50%
  *** 必须 - 居中元素的宽高 ***

```css
.db-center {
  position: relative;
  left: calc(50% - 100px);
  top: calc(50% - 25px);
}
```

## 块级元素垂直居中 - `position: relative; top: 50%; transform: translateY(-50%)`
> position: 相对于父元素的宽高进行计算
  translate 元素自身的 50%
```CSS
.db-center {
  position: relative;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%)
}
```

> # display: inline-block

## 行内元素水平居中 - `text-align: center`
> 父容器为块状元素，子元素是为行内元素 display: inline-block;
  设置父元素的行内元素的水平居中

```CSS
.dib-horizontal-center {
  text-align: center;
}
```

## 行内元素水平居中 - `line-height`
> 父容器为块状元素，子元素是为行内元素 display: inline-block;
  设置父元素的行内元素的垂直居中
  line-height: 值为父容器的高
  *** 子容器为单行元素适用，如果是多行需要居中，就采用块级元素居中方案 ***

```CSS
.dib-horizontal-center {
  line-height: 300px;
}
```

> # display: flex

```css
.df-center {
  display: flex;
  justify-content: center;
  align-items: center;
}
```
{% codepen byGwXx result 265 100% helenzhanglp|anonymous|anon 0 %}

> # display: table-cell
```CSS
.dtable-center {
  display: table-cell;
  text-align: center;
  vertical-align: middle;
}
```
> # display: grid
```CSS
.fatherWrap {
    display: grid;
}
.son {
    align-self: center;
    justify-self: center;
}
```
