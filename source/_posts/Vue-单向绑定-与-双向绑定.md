---
title: Vue 单向绑定 与 双向绑定
date: 2019-03-03 21:32:19
categories:
- 技术
tags:
- Vue
- 小程序
- 数据绑定
---
在数据绑定这里纠结了有断日子了，一直想把这块好好梳理一下。则日不如撞日，就今天了！
[参考文档不能丢](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/001475449022563a6591e6373324d1abd93e0e3fa04397f000)
### 认识下 MVVM
很讨论的一个概念，在它之前我只认识 MVC，曾经有一经面试就在它这里卡壳。那个时候我只认识 MVC.
[哎呀，妈呀，不易啊，终于找到正主啦](https://docs.microsoft.com/zh-cn/windows/uwp/data-binding/data-binding-and-mvvm)文档中说的很清楚，mvvm 指的模型层（model）、视图层（View)及视图模型层（ViewModel)
正文还是有些枯燥的。其实廖老师的文档里其实已经很好的将jQuery 操作 Dom 与 MVVM 作了很好的对比，给出的实际案例，说明了`MVVM 是通过操作对象，从而自动更新 DOM 状态`
以下是两个案例对比：
```javascript
var name = 'Homer';
var age = 51;

// 回忆下原生 js
document.getElementById('#name').innerText('HTML')
document.getElementById('#age').innerText('51')

//jQuery
$('#name').text(name);
$('#age').text(age);
```
```javascript
// MVVM 中通过对象自动更新 DOM
var person = {
    name: 'Bart',
    age: 12
};
person.name = 'HTML'
person.age = 51
```
### 数据绑定
模模糊糊中认为小程序是属于单向数据绑定，别人的博客中有说是双向绑定，为些还特意百度找盟友，结果发现我并不孤独。接下来分析分析数据绑定。
