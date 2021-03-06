---
title: 'CSS display: none vs visibility: hidden'
date: 2019-05-13 14:58:59
tags:
- CSS
- 面试题
---

### display:none 与 visibility:hidden 区别
> display 指定元素显示框架的类型，none 隐藏不占文档流，不会影响布局
> visibility 显示或隐藏元素而不更改文档的布局，hidden 相当于此元素变成透明并且 text-indent: -9999px，会影响布局

<!--more-->

#### 可以用以下实例进行验证

```html
<ul class='item-wrap'>
  <!-- <li class="style1">
    <img src="" alt="">
    <div></div>
  </li> -->
  <li class="style2">
    <img src="" alt="">
    <div></div>
  </li>
  <li class="style3">
    <img src="" alt="">
    <div></div>
  </li>
  <li>last dom</li>
</ul>
```
<!-- more -->

```CSS
.style1 {
  width: 30px;
  height: 100px;
  background-image: url('http://img0.fengqu-inc.com/cmsres/20190430/2f836f6d-91ed-4c5e-ae2a-81c32357786e.png');
  background-repeat: no-repeat;
}
.style2 {
  width: 30px;
  height: 100px;
  background-image: url('http://img0.fengqu-inc.com/cmsres/20190430/a8c73c89-6175-404a-b2b5-533c05d0f601.png');
  background-repeat: no-repeat;
  display: none;
}
.style3 {
  width: 30px;
  height: 100px;
  background-image: url('http://img0.fengqu-inc.com/cmsres/20190430/0e16b8ee-4251-456a-bf35-093bace3d252.png');
  background-repeat: no-repeat;
  visibility: hidden;
}
```

### 以上三张背景图是否下载 ？(`没有在官方文档中找到详细说明，以下仅代表个人实验结果`)
- [ ] 定义样式不引用，图片资源不会下载，其它形式不管是 display: none 还是 visibility: hidden 都会下载图片资源，如下图

---

{% codepen ewpVBp result 265 100% helenzhanglp|anonymous|anon 0 %}
![图片下载资源](https://s2.ax1x.com/2019/06/14/V4D334.png)
