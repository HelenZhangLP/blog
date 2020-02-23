---
title: Javascript 字符串
date: 2020-01-09 10:14:03
categories: 技术
tags:
- Javascript
- String
---

> `需求删除最后一个为 & 的字符`
### String.prototype.replace() 返回一个部分或全部匹配由替代模式所取代的新的字符串。
> str.replace(regexp|substr, newSubStr|function)

```javascript
let str = 'zhangliping&'
// regexp = /&$/
str.replace(/&$/, "") // zhangliping
/*
 * --- 引申 ---
 * 删除字符串全部的 &
 */
str.replace(/&$/g, "") // 全局替换
str.replace(/^&/,"") // 替换开头
str.replace(/[?&]$/, "") //替换结尾的 ? 或 &
```
<!-- more -->
### String.prototype.slice() 方法取字符串的一部分，返回新字符串，原字符串不动
> str.slice(beginIndex[, endIndex])
* beginIndex 从该索引（以 0 这基数）处开始提取原字符串。如果为负数，会被当做 strlength + beginLength
* endIndex 可选，以 0 基数，endIndex 处结束提取字符串。省略则视为默认 strlength。或为负数，endIndex = strlength - endIndex

```javascript
let str = 'zhangliping&'
// 取默认值，str.slice() 不传参
str.slice() // zhangliping& beginIndex 默认值 0， endIndex 默认值 strlength
// beginIndex >= strlength, endIndex 取默认值
str.slice(13) // ''
// beginIndex < 0，endIndex 取默认值
str.slice(-1) // '&' beginIndex = strlength - 1，截取到字符串长度
// start < 0 && abs(start) > strlength, length 取默认值
str.slice(-100) // 'zhangliping&'
// beginIndex 取默认值 0，endIndex >= strlength
str.slice(0, 12) // zhangliping&
// beginIndex 取默认值 0，endIndex < 0
str.slice(0, -1) // 'zhangliping' endIndex = strlength - 1 截取最后一位
// beginIndex 取默认值 0，endIndex == 0
str.slice(0, 0) // ''
```

### String.prototype.substring() 返回从开始位置到结束位置的一个子集。
> str.substring(indexStart[, indexEnd])
* indexStart 需要截取的第一个字符的索引，该索引位置的字符作为返回字符串的首字母
* indexEnd 0 到字符串长度之间的整数，以该数字为索引的字符不包含截取位置内

```javascript
let str = 'zhangliping&';
// str.substring() indexStart 默认值 0； indexEnd 默认值 strlength
str.substring() // 'zhangliping&'
// indexStart < 0 && abs(indexStart) < strlength
str.substring(-1) // indexStart = 0
// indexStart < 0 && abs(indexStart) > strlength
str.substring(-15) // indexStart = 0
// indexStart = 0 indexEnd > strlength
str.substring(0, 15) // 'zhangliping&'
str.substring(0, str.length-1);// zhangliping
```

### String.proptotype.substr() 返回一个字符串从指定位置开始
> 被认作是遗留的函数，非 Javascript 核心语言的一部分。目前没有严格被废弃。但将来可能被移除
str.substr(start[, length])
* 不会改变原字符串
* start 与 slice 的第一个参数相同从该索引（以 0 这基数）处开始提取原字符串，若为负数，看作 strlength（字符串长度） + start。
** Microsoft' JScript 不支持负 start 索引 **
* length 提取的字符数

```javascript
let str = 'zhangliping';
// str.substr() 不传参
str.substr() // 'zhangliping' start 默认值 0， length 默认值字符串长度
// start >= strlength length 取默认值
str.substr(13) // ''
// start < 0 length 取默认值
str.substr(-4) // start = strlength - 4 从 index = 8 开始截取
// start < 0 && abs(start) > strlength, length 取默认值
str.substr(-14) // 'zhangliping' 取出全部字符串
// start 取默认值 0，length >= strlength
str.substr(0, 14) // 'zhangliping'
// start 取默认值 0， length <= strlength
str.substr(0, -1) // ''
// 截取字符串最后一位
str.substr(0, strlength - 1) // 'zhangliping'
```

### slice()/substr/substring 对比
| 方法名 | 参数一 | 参数一解释 | 参数一为负数（如 -5） | 参数一大于 strlength | 参数二解释 | 参数二为负数（如 -5）| 参数二大于 strlength
|-------| ----- | -------- | ---------- | ---------- | -------- |
| slice | beginIndex | 开始位置索引，以 0 为基数 | abs(-5) > strlength ? beginIndex = 0 : beginIndex = strlength - 5 | 返回空串 | 提取字符串结束位置(不包括当前数字索引) | abs(-5) > strlength ? endIndex = strlength : endIndex = strlength - 5 | 返回整个字符串 | 返回整个字符串 |
| substr | start | 同上 | 同上 | 同上 | 截取字符长度 | 同上 | 同上 |
| substring | indexStart | 同上 | indexStart = 0 | 同上 | 同上 | indexEnd = 0 | 同上 |

### match() 检索返回一个匹配正则表达式的结果
> str.match(regexp)
> 参数：一个正则表达式对象。
> 若参数为非正则表达式对象，会隐式地使用 `new RegExp(obj)` 转换为 RegExp
> 若没给参数，参数默认为包含空字符串数组，如 `[""]`
```javascript
const str = 'abc abc'
let reg = /(\w+)/g
str.match(reg)
/**
  [ 'abc', 'abc' ]
 */
reg = /(\w+)/
str.match(reg)
/**
 [ 'abc', 'abc', index: 0, input: 'abc abc', groups: undefined ]
*/
```
> 返回值：
> 使用 g 标志，返回表达式匹配的所有结果，不返回匹配捕获组(`[ 'abc', 'abc' ]`)
> 不使用 g 标志，返回第一个完整匹配及其相关的捕获组（input 搜索字符串；groups 捕获数组或 `undefined`; index 匹配结果开始位置）。
