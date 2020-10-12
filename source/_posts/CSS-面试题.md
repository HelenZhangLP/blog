---
title: CSS 面试题
date: 2019-03-25 15:35:18
tags:
- css
---

- [x] absolute 相对于最近的非 ‘static’ 元素定位；如果该元素不存在，则相对于 initial container block 定位。该定位方式脱离文档流，同样脱离文档流的还是 float

- [x] margin/padding 设置百比时，根据父元素宽运算

- [ ] 使用css实现一个自适应浏览器大小并且宽和高的比是2:1，有几种实现方法？
> * width 与 height 百分比是根据父元素计算的，`<body>` 是根据宽一般是浏览器的宽，高一般为 0，不同浏览器有所差异，这里不细说。
> * padding 百分比是相对于包含块的宽度,指定一个值时 该值指定四个边的内边距
- [x] 用 padding 撑 50% 的高

<!-- more -->

```CSS
.father {
  width: 100%; // 根据父元素计算
  padding: 25% 0; // 高度根据父元素的宽计算
  background-color: #f90;
  box-sizing: border-box;
}
```

- [ ] @supports、cacl()、@media 的用法和含义
---
> * @supports 在这里卡了许久，想要找出当红 ui 框架中使用的案例，但最终一无所获。但是吧，既然有这样一道面试题，那就看看文档，整理整理
    ` 兼容性：移动端浏览器兼容较好，pc 端 IE 不兼容 `
    声明语法，如下 demo，包含 not, and, or 三种操作符
    ```CSS
    @@supports (transform-origin: 5% 5%) {}
    ```
    **总结一下**，@supports 是用来检验某个 css 属性是否支持。是一个判断条件。
---
> * @media 以下 demo 源自 bootstrap
    ```CSS
    h1, .h1 {
      font-size: calc(1.375rem + 1.5vw);
    }

    @media (min-width: 1200px) {
      h1, .h1 {
        font-size: 2.5rem;
      }
    }

    h2, .h2 {
      font-size: calc(1.325rem + 0.9vw);
    }

    @media (min-width: 1200px) {
      h2, .h2 {
        font-size: 2rem;
      }
    }
    ```
    **总结一下**：根据不同的媒体类型，定义不同的样式，作响应式处理

---
> * cacl() 函数允许对 css 属性值进行一些 + - * / 的运算，如上例

- [ ] [水平垂直居中](https://helenzhanglp.github.io/2019/05/05/%E5%B1%85%E4%B8%AD/)

- [x] 使用 vw 为单位
> * vw: viewport width, 1vw = 视窗宽度的 1%
> * vh: viewport height, 1vh = 视窗高度的 1%
> * vmin: vh 与 vw 哪个小取哪个
> * vmax: vh 与 vw 哪个大取哪个

```CSS
.warp {
  width: 100vw;
  height: 50vw;
  background-color: #f90;
}
```
