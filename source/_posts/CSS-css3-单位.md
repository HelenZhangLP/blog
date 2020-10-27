---
title: CSS3 单位
date: 2020-10-22 16:31:44
tags:
- css
---

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
