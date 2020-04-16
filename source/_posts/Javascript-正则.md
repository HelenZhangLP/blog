---
title: 'Javascript-正则'
date: 2020-01-13 15:35:23
category: 技术
tags:
- Javascript
- RegExp
---

## RegExp
```javascript
let reg = /[\u4e00-\u9fa5]/
console.dir(reg)
/**
 * /[\u4e00-\u9fa5]/
    dotAll: false
    flags: ""
    global: false
    ignoreCase: false
    lastIndex: 0
    multiline: false
    source: "[\u4e00-\u9fa5]"
    sticky: false
    unicode: false
 * /
```
<!--more-->
### RegExp.prototype.dotAll 在正则表达式中是否使用 `s` 修饰符，`s` 修饰符可以匹配任意单个字符。只读属性，属于单个正则表达式实例。
`***暂留，我承认我没有搞明白***`

### RegExp.prototype.flags 返回由当前正则表达式对象的标志组成的字符串，是一个只读属性
```javascript
let reg = /[\u4e00-\u9fa5]/sg
console.dir(reg)
/**
 * /[\u4e00-\u9fa5]/gs
    dotAll: (...)
    flags: "gs"
    global: (...)
    ignoreCase: (...)
    lastIndex: 0
    multiline: (...)
    source: (...)
    sticky: (...)
 */
```

### RegExp.prototype.global 正则表达式是否使用了 `g` 标志，是找到所有匹配，而不是第一个匹配后停止，是一个只读属性
```javascript
let reg = /[gimuy]*$/g
console.dir(reg)
/**
 * /[gimuy]*$/g
    dotAll: false
    flags: "g"
    global: true
    ignoreCase: (...)
    lastIndex: 0
    multiline: (...)
    source: (...)
    sticky: (...)
    unicode: (...)
 */
```

### RegExp.prototype.ignoreCase 正则表达式是否使用了 `i` 标志，是否忽略大小写，只读属性
```javascript
let reg = /[\u4e00-\u9faf]/gi
console.dir(reg)
/**
 * /[\u4e00-\u9faf]/gi
      dotAll: false
      flags: "gi"
      global: true
      ignoreCase: true
      lastIndex: 0
      multiline: false
      source: "[\u4e00-\u9faf]"
      sticky: false
      unicode: false
 */ 
```

### RegExp.prototype.multiline 正则表达式中使用 `m` 属性，多行匹配，只读属性
> "m" 标志意味着一个多行输入字符串被看作多行
> 使用 "m"，"^" 和 "$" 将会从只匹配正则字符串的开头或结尾，变为匹配字符串中任一行的开头或结尾。
```javascript
let reg = /[\u4e00-\u9faf]/gim
console.dir(reg)
/**
 * /[\u4e00-\u9faf]/gim
      dotAll: false
      flags: "gim"
      global: true
      ignoreCase: true
      lastIndex: 0
      multiline: true
      source: "[\u4e00-\u9faf]"
      sticky: false
      unicode: false
 */ 
```

### RegExp.prototype.exec() 在一个指定字符串中执行一个搜索匹配。返回匹配数组或 null。
> regexObj.exec(str)

*** notice ***
- [ ] 设置了 global 或 sticky 标志位后 RegExp 对象是`有状态`的；
- [ ] 会将上次成功匹配的位置记录在 lastIndex 属性中。
- [ ] 如此 exec() 可用来对单个字符串中的多次匹配结果进行逐条遍历。

```javascript
reg = /[\u4e00-\u9fa5]/
reg.exec('1233 我是张丽萍 abc')
// ["我", index: 5, input: "1233 我是张丽萍 abc", groups: undefined]
```
#### ["我", index: 5, input: "1233 我是张丽萍 abc", groups: undefined] 返回值解析
|索引|值|描述|
|--|--|--|
|0|我|匹配的全部字符串|
|1|index:5|匹配字符串开始索引|
|2|1233 我是张丽萍 abc|匹配原始字符串|
|3|groups:undefined|分组捕获的结果|

加全局匹配标识符，会记录上次匹配的位置，可以循环遍历。`注意不要在遍历条件中写入正则表达式，如果一直匹配，会造成死循环`。
```javascript
const reg = /(\w+)(\s)/g
const str = 'abc abc abc'
reg.exec(str)

/**
  [ 'abc ', 'abc', ' ', index: 0, input: 'abc abc abc', groups: undefined ]
> reg.exec(str)
[ 'abc ', 'abc', ' ', index: 4, input: 'abc abc abc', groups: undefined ]
> reg.exec(str)
null
 */
```
#### 返回值 []
* [0] 匹配的字符串（['abc ']）
* [1],...[n] 分组捕获（['abc', ' ']）
* index 匹配到的字符位于原始字符串的基于0的索引值 （index: 0）
* input 原始字符串
---


- [ ] `\w` 匹配任意来自基本拉丁字母表中的字母数字字符，还包括下划线。等价于 [A-Za-z0-9_]
- [ ] `\w+` 匹配数字、字母、下划线 x 1 或多次。等价于 {1,}
- [ ] `(\w+)` 匹配数字、字母、下划线至少一次，并且捕获匹配项
- [ ] `\s` 匹配空白符，包括空格、制表符、换页符、其它 Unicode 空格

#### 字符集合(character sets)
[xyz] [x-z] 匹配是 xyz 的字符集
```javascript
/[x-z]/.exec('zxyd')
// ["z", index: 0, input: "zxyd", groups: undefined]
/[^x-z]/g.exec('xyzabdxyz')
// ["a", index: 3, input: "xyzabdxyz", groups: undefined]
```
[^xyz] [^x-z] 匹配非 xyz 的字符集

#### 分组（grouping）
```javascript
/* 分组匹配 */
regGroups = /(\d+)([\u4e00-\u9fa5]+)(\w)/
regGroups.exec('1233我是张丽萍abc')
// ["1233我是张丽萍a", "1233", "我是张丽萍", "a", index: 0, input: "1233我是张丽萍abc", groups: undefined]
```
#### ["1233我是张丽萍a", "1233", "我是张丽萍", "a", index: 0, input: "1233我是张丽萍abc", groups: undefined]
|索引|值|描述|
|--|--|--|
|0|1233我是张丽萍a|匹配的全部字符串|
|1|1233|分组匹配的结果 RegExp.$1|
|2|我是张丽萍|分组匹配的结果 RegExp.$2|
|3|a|分组匹配的结果 RegExp.$3|
|4|index|匹配到的字符位于原始字符串的基于0的索引值||
|5|groups:undefined|分组捕获的结果|

#### 字符类别(Character Classes)
|字符|含义|
|--|--|
|`\d` `[0-9]`|匹配任意阿拉伯字符串|

#### 边界（Boundaries）
|字符|含义|
|--|--|
|^|匹配输入`开始`位置，如果设置 flag multiline，该标识会匹配每个断行（line-break）符后的`开始`位置|
|$||匹配输入`结束`位置，如果设置 flag multiline，该标识会匹配每个断行（line-break）符后的`结束`位置|