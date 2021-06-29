---
title: 坑-JavaScript
date: 2021-06-28 20:05:15
tags:
- JavaScript
- 坑
---

## Uncaught SyntaxError: Illegal break statement（非法的 break 语句）
```javascript
[1,2,3,4,5,6].map(i => {if(i==3) throw Error(); console.log(i)})
```
> break 语句中止**当前循环**，**switch语句**或**label 语句**，并把程序控制流转到紧接着被中止语句后面的语句。
```javascript
let arr = [1,2,3,4,5,6]
for(var i=0; i<arr.length;  i++) {
    if(arr[i] === 3) break;
    console.log(arr[i])
} // 1,2
```
<font color="#f33">map,forEach 中使用<u>try...catch + throw Error 模拟 break</u></font>
```javascript
try {
    [1,2,3,4,5,6].map(i => {if(i==3) throw Error(); console.log(i)})
} catch {} // 1,2
```