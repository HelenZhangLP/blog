---
title: 'JavaScript 数组'
date: 2020-11-06 11:22:46
category:
- 技术
tags:
- JavaScript
---

## 数组重组方法
### 1.  数组转换成字符串
* Array.prototype.toString() 将数组转换成 **以逗号分隔的字符串**
```javascript
var arr = ["cookie", "local storage", "session storage", "web storage"];
arr.toString();
// "cookie,local storage,session storage,web storage"
```
* Array.prototype.join([separator]) 将数组转换成 **指定分隔符的字符串**
```javascript
arr.join('/')
// "cookie/local storage/session storage/web storage"
```
  - `注意：` 如果数组只有一个元素，则该连接字符不显示
  ```javascript
  var arr1 = ['category']
  arr1.join('/')
  // "category"
  ```
### 2.  创建新数组
* Array.prototype.concat() 合并两个以上数组，最终拿到生成后的新数组，生成的新数组是需要合并的数组的浅拷贝。
  - `注意：`如果将引用类型复制到数组中,引用类型改变，新的数组数组发生改变，反之一样。因为他们指向同一个引用对象。
  ```javascript
  var arr_reference_type = [['concat'],'array prototype function'];
  var arr_concat = arr.concat(arr_reference_type);
  console.log(arr_concat);
  // ["cookie", "local storage", "session storage", "web storage", ["concat"], "array prototype function"]
  // 修改 arr_reference_type
  arr_reference_type[0].push('slice');
  console.log(arr_concat);
  // ["cookie", "local storage", "session storage", "web storage", ["concat", "slice"], "array prototype function"]
```
