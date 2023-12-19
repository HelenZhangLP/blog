---
title: JavaScript-数据类型检测
date: 2012-12-15 10:38:24
tags:
 - JavaScript
---

## typeof[value]
> `typeof value` 返回 `undefined`，返回值是一个字符串类型，字符串代表当前值的数据类型。
```JavaScript
    // 基本数据类型
    typeof value // 'undefined'
    typeof undefined // 'undefined'
    typeof 'value' // 'string'
    typeof '' // 'string'
    typeof 0 // 'number'
    typeof false // 'boolean'
    typeof Symbol() //'symbol'
    typeof 9007199254740992n // bigint
    typeof BigInt(10) // bigint
    typeof null // object

    // 引用数据类型
    typeof {} // Object
    typeof function(params) {} // function
    typeof (()=>{}) // function

    typeof [] // object
    typeof /^$/ // object
    typeof new Date() // object
```
<span class='custom-box custom-box-933'>总结：除基本数据类型或者说是原始数据类型外，引用数据类型与 null 都不能正确给出区分。</span>

><center>检测原理</center>
1. 所有数据类型在计算机中都按照二进制的值进行存储；
2. **`对象数据类型的二进制值都是以 000 开头，不论是普通对象还是数组或正则，null 在计算机中的存储也是以二进制 000 存储`**
3. <span class='custom-box custom-box-393'>typeof 检测数据类型是根据检测值的二进制检测的，只要检测是以 000 开头，都认为是对象。</span>

### 隐式转换
```JavaScript
    // 浏览器发现比较两边类型不一样，会默认进行隐式转换，转换为相同类型再进行比较
    if (a == 'undefined') {}
```
> 浏览器发现比较两边类型不一样，会默认进行隐式转换，转换为相同类型再进行比较

#### `==` 比较规则
* NaN == NaN 不相等
* null == undefined 相等
* 字符串 == 对象 先将对象转换为字符串（a. 对象[Symbol.primitive]; b. 对象.valueOf() 获取对象的原始值；c. 若无，对象.toString()），再进行比较
* 其它数据类型比较，都是按照“转换为数字类型（（a. 对象[Symbol.primitive]; b. 对象.valueOf() 获取对象的原始值；c. 若无，对象.toString()）；d. Number(c的结果字符串)）然后再比较”的规则处理的。

## [value] instanceof [constructor]
## [value].constructor
## Object.prototype.toString.call([value])
