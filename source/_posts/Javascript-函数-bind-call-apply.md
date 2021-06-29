---
title: 函数 bind call apply
date: 2019-03-18 14:17:37
tags:
- JavaScript
- function
- 函数作用域
---

## Function.prototype.call()
`call()` 使用一个指定的 `this` 值和单独给出一个或多个参数来调用一个函数
<font color="#f33">对象实例 A 需要调 对象 B 的方法，采用 call 传入对象实例 A，执行调用</font>

```JavaScript
  // 调用一个具有给定 this 的函数
  function TempObj(){
    this.tempArray = [1,4,6,2]
  }
  
  let tempObj = new TempObj()

  // 指定作用域为 tempObj
  console.log(Math.max.call(tempObj, ...tempObj.tempArray))
```
----
> 每个数据类型都有自己的 toString 方法，但实现方式不同。现在需要 emptyArray，调用对象 toString ，拿到对象 toString 方法的返回值
```javascript
let emptyArray = []
window.toString() // "[object Window]"
emptyArray.toString() // ""
window.toString.call(emptyArray) // "[object Array]"
```

### bind、call、apply
[bind](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)
[call](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call)
[apply](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
#### 从定义上对比看
* bind - 创建一个新的函数
* call - call 与 apply 相似，仅传参不同
* apply - 调用一个具有给定 this 值的函数
> func.apply(thisArg, [argsArray])

** DEMO **
```JavaScript
// 数组合并
let a = [1,2], b = [3,4]
a.push(b) // 3，返回数组 length
a.concat(b) // [1, 2, 3, 4] 创建了一个新数组
a.push.apply(a, b) // 4
console.log(a) // [1, 2, 3, 4]
a.push.call(a, ...b) // 6
console.log(a) // [1, 2, 3, 4, 3, 4]
```
