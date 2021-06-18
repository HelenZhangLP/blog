---
title: 'JavaScript 数组'
date: 2020-11-06 11:22:46
category:
- 技术
tags:
- JavaScript
---

对于需要收集的 **有序、具有索引的数据** 的需求，可以使用数组

## 数组的静态方法
是以 Array 为命名空间的方法，包括以下几个：
### Array.isArray()
> Array.isArray() 的判断依据是<u>数组内部实现特性[[Class]]的值'Array'</u>

```JavaScript
class SubArray extends Array {}
let subArray = new SubArray()
Array.isArray(subArray) // true
```
用类语法继承，子类实例会被 Array.isArray() 判断为 true
<font color="#f33">类语法的继承能继承标准 API，而且内部实现特性以及特殊行为也会继承</font>

```JavaScript
let obj = {}
obj.prototype = Array.prototype
obj instanceof Array // false
Object.setPrototypeOf(obj, Array.prototype)
obj instanceof Array // true
Array.isArray(obj) // false
```
对象原型被修改的类数组，可以骗过 instanceof 但 Array.isArray() 判断为 false

### Array.of()
```JavaScript
var arr = new Array(10) 
console.log(arr) // (10) [empty × 10]
var arr2 = new Array(1,2,3)
console.log(arr2) //(3)[1, 2, 3]
```
以上 demo 通过 Array 构造函数创建了数据，参数两层函数，一是数组的长度。二是数组的元素。较混乱<br/>
常用字面量形式定义数组，方便，容易理解
> 可使用 Array.of() 建立指定元素数组
```JavaScript
Array.of(10) // 建立一个元素为 10 的数组 [10]
Array.of(1,2,undefined,4) // [1, 2, undefined, 4]
[1, 2, , 4]
Array.of(1,2,,4) // Uncaught SyntaxError: Unexpected token ','
```
<font color="#f33">Uncaught SyntaxError: Unexpected token ','</font> 异常说明 与字面定义数组语法不同[1, 2, , 4]，Array.of() 不支持空项目
```javascript
class SubArray extends Array{}
var subArray = SubArray.of(1,2,3,4,5)
subArray instanceof SubArray
subArray // SubArray(5) [1, 2, 3, 4, 5]
```

### Array.from()
Array.from() 可以接受类数组或可迭代对象，传回的新数组包含类数组或可迭代对象元素
```javascript
Array.from('helen') // (5) ["h", "e", "l", "e", "n"]
Array.from('helen', item => item.toUpperCase()) // ["H", "E", "L", "E", "N"]
Array.from('helen', (item, idx) => !idx ? item.toUpperCase() : item) // ["H", "e", "l", "e", "n"]
```

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
