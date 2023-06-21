---
title: JavaScript-正则-常用正则case
date: 2020-03-10 13:47:53
tags:
- JavaScript
- RegExp
---

### 1、 Demo(1) —— 身份证验证
```javascript
// 15 位数字/18 位数字/17 位数字 + （X|x）
const reg = /(^\d{15}$)|(^\d{18}$)|(^\d{17}(\d|X|x)$)/
```
<!-- more -->

### 2、 Demo(2)
```javascript
let value = '<html>   $%^##sbd</html>'
// 替换 html 代码
name = value.replace(/[<>\w+/]/g, '');
// "   $%^##"

// 替换是否存在表情
name = name.replace(/\uD83C[\uDF00-\uDFFF]|\uD83D[\uDC00-\uDE4F]/g, '');

// 多个空格替换为一个空格
name = name.replace(/\s+/g, ' ');

// 特殊符号
const regEn = /[`~@#$%^&*()_+<>?:"{},./;'[]]/g;
const regCn = /[·#￥（——）：；“”‘、|《》？、【】[]]/g;

name = name.replace(regEn, '');
name = name.replace(regCn, '');
```

### 3、  Demo(3) —— 手机号中间四位替换为 ****
```javascript
var str = '13381895220'
str.replace(/^(.{3})(.{4})(.{4})$/, "$1****$3") // "133****5220"
```

### 4、  Demo(4) —— 首字母大写
```javascript
var value = 'front-end development engineer'
value.replace(/^./,value[0].toUpperCase()) // "Front-end development engineer"

value.split(' ').map(item => item.replace(/^./, item[0].toUpperCase())).join(" ")
// "Front-end Development Engineer"
```
