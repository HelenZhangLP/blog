---
title: CSS 盒模型
date: 2019-02-20 10:21:14
category:
- 技术
tags:
- 前端
- CSS
---

该文档整理基于[怪异模式（Quirks Mode）对 HTML 页面的影响](https://www.ibm.com/developerworks/cn/web/1310_shatao_quirks/#N101C3)

切入正题，说说盒模型。通过上面那篇文章，我意识到了盒模型的差异源于浏览器的渲染解析差异。最突出的代表要数IE浏览器，在 IE 中，有四种标准模式（Standards Mode）- IE7/8/9/10 Standards Mode；两种怪异模式（Quirks Mode）IE5 Quirks 和 IE10 Quirks。这两类文档模式在盒模型的渲染上有所差异。

### 话不多说，先上图
![W3C标准盒模型](https://bkimg.cdn.bcebos.com/pic/a9d3fd1f4134970a37cf81a69fcad1c8a6865dfe?x-bce-process=image/watermark,image_d2F0ZXIvYmFpa2U4MA==,g_7,xp_5,yp_5);
**<center>【W3C标准盒模型】</center>**
![IE盒模型](https://www.ibm.com/developerworks/cn/web/1310_shatao_quirks/image005.jpg)
**<center>【IE盒模型】</center>**

由上图可以看出：
> IE Quirks 怪异模式 盒模型  width = border + padding + content
  W3C 盒模型 width = content width

<!-- more -->

### box-sizing（兼容 `IE8+`浏览器）
定义用户代理（user agent）计算元素总宽高计算方式的 css3 属性，文档定义如下：
```css3
box-sizing ： content-box || border-box || inherit
```
content-box: 默认值，W3C 盒模型，元素所占区域宽 = border+padding+（content=width）。
border-box: 元素渲染模式为 IE Quirks 渲染方式，即元素所占区域宽高 = width = border + padding + content
