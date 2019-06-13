---
title: 函数 bind call apply
date: 2019-03-18 14:17:37
tags:
- javaScript
- function
- 函数作用域
---
### bind、call、apply
[bind](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)
[call](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call)
[apply](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
#### 从定义上对比看
* bind - 创建一个新的函数
* call - call 与 apply 相似，仅传参不同
* apply - 调用一个具有给定 this 值的函数
> func.apply(thisArg, [argsArray])

```javascript
  // 调用一个具有给定 this 的函数
  function TempObj(){
    this.tempArray = [1,4,6,2]
  }
  let tempObj = new TempObj()

  // 指定作用域为 tempObj
  console.log(Math.max.apply(tempObj, tempObj.tempArray))
  console.log(Math.max.call(tempObj, ...tempObj.tempArray))
```

** DEMO **
```javascript
// 数组合并
let a = [1,2], b = [3,4]
a.push(b) // 3，返回数组 length
a.concat(b) // [1, 2, 3, 4] 创建了一个新数组
a.push.apply(a, b) // 4
console.log(a) // [1, 2, 3, 4]
a.push.call(a, ...b) // 6
console.log(a) // [1, 2, 3, 4, 3, 4]
```
