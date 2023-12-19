---
title: JavaScript-ES6新特性
date: 2017-11-25 16:23:44
tags:
- JavaScript
- ES6
---

## let/const 声明变量
1. 解决 ES5 var 初始化变量时出现的变量提升问题
2. 在 ES5 全局作用域和函数作用域的基础上新增块级作用域
3. 解决因变量没有及时回收造成的内存泄漏问题

<u>解决使用闭包时出错的问题</u>

> 暂时性死区（Temporal Dead Zone, TDZ) 是只要进入当前作用域，所要使用的变量就已经存在了，但不可获取。只有等到变量的那一行代码出现，才可以获取和使用该变量。
```JavaScript
// temporal dead zone start
console.log(a) // a is not defined, a is not declare
let a; // Declare variable a
// temporal dead zone end，because the variable has emerged
console.log(a) // undefined
```

## 对象字面量增强
> 使用对象字面量建立对象时，变量指定到同名特性时，如下：
```javascript
let x, y;
let o = {
  x,
  y
}

/**
 * ES6 之前
 * x: x
 * y: y
 */
```
> 对象特性参考临时建立的匿名函数时，如下
```javascript
let o = {
  '0': 0,
  '1': 1,
  '2': 2,
  length: 3,
  forEach(callback) {
    for(let i=0; i<this.length; i++) {
      callback(this[i])
    }
  }
}

/**
 * ES6 之前
 * forEach: function(callback) {
 *   ....
 * }
 */
```

> ES6 中可以使用 [] 中指定表达式为对象字面量指定特性
```javascript
var prefix = 'dosome', n = 1
let o = {
  [prefix + n]() {
    // ...
  }
}

o.dosome1()
/**
 * ES6 之前
 * o[prefix + n] = function() {
 *   // ...
 * }
 */
```

> ES6 字面量在设值方法 setter/ 取值方法 getter 时：
> <font color="#f33">setter/getter 方法的作用之一，是进行存取的前置或后置处理。也可以实现一定程度的私有特性隐藏</font>
```javascript
function object() {
  let obj_privates = {};
  let obj = {
    set name(value) {
      obj_privates.name = value.trim()
    },
    get name() {
      return obj_privates.name.toUpperCase()
    }
  }
  
  return obj
}

/**
 * ES5 通过 Object.defineProperty() 或 Object.defineProperties() 函数设值、取值
 */

let obj = object()
obj.name = ' helen  '
console.log(obj.name) // helen
```

## 解构赋值（Destructuring）
> 按照一定模式从数组或对象（可枚举）中提取值，对变量进行赋值。**解构赋值的对象是数组或对象，作用是赋值**

```javascript
let scores = [{English: 120}, {Math: 100}, {chemistry: 111}]
let [x,y,z] = scores
x // {English: 120}

/**
 * ES6 之前
 * let x = scores[0]
 * let y = scores[1]
 * ...
 */
```

可枚举值，都可使用 Destructuring
```javascript
let [a,b,c] = 'abc'
b // "b"

let obj = {a:1,b:2}
let {a,b,c=100,d} = obj
a //1
b //2
z //100
d //undefined
```

<font color="#f33">在数组中解构赋值，是从数组中提取值，对应位置赋值给对应变量，解构失败赋值 undefined。等号右边为不可遍历结构会报错。</font>
```javascript
let [a,b,c] = 'ab'
c // undefined
```

<font color="#f33">解构过程变量也可以赋初值，解构元素少于变量时会取初值</font>
```javascript
let [a,b=2,c=3] = 'ab'
b // b
c // 3
```

<font color="#f33">可以只解构某几个元素，或解构尾部元素</font>
```javascript
let [,...tail] = 'tail'
tail

let [s,,i,,] = 'tail'
s //t
i //i
```

<font color="#f33">变量转换</font>
```javascript
let a = 1, b = 10;
[a,b] = [b,a]
a // 10
b // 1
```

## 余集（Rest）
```javascript
let [a,b] = '123456'
b // 2
```
<font color="#f99">变量个数少于迭代元素时，多余元素会被忽略</font>
> 使用余集（Rest）将多余元素收集为数组
```javascript
let [a,...b] = '123456'
b // ["2", "3", "4", "5", "6"]
```

## 打散（Spread）
```javascript
let arr = [1,2,3],
    arr2 = [...arr, 4, 5];
arr2 // (5)[1, 2, 3, 4, 5]
```
<font color="#f33">ES9 中，可以对对象进行打散</font>
```javascript
let [a,b,...c] = {a:1,b:2,c:3,d:4,e:5}
a
b
c
```

## 扩展运算符
1. 合并数组
```JavaScript
var a = [1,2]
var b = ['a', 'b']
var c = [...a,...b]
```
<font color="red">push 方法的参数不能为数组</font>
```JavaScript
var a = [1,2]
var b = ['a', 'b']
b.push(...a)
```
2. 数组复制
复制底层数据结构的指针，并不是复制全新数组
3. 与解构赋值结合
3. 函数调用
  * 箭头函数内置不可改变的 this
  * 箭头函数不能使用 new 关键字实例对象
  * 箭头函数没有 arguments 对象，无法通过 arguments 对象获取传入的参数


## 虚拟 Dom
是利用 JS 去构建真实 DOM 树，用于在浏览器中展示。每当有数据更新时，重新计算整个 DOM 树，新旧对比，进行最小程度的更新，避免大范围页面重排导致的性能问题。虚拟 DOM 树是内存中的数据，本身操作性能高很多。

## 单向数据流
从组件流向子组件，让组件间的关系变得简单可预测
props 和 state 是 React 中两个非常重要的概念。
props 是外来数据
state 是内部数据
父组件通过 props 传给子组件，props 改变，react 会递归地向下遍历整个组件树，使用到这个属性的组件中重要渲染
组件内部数据只能在组件内部修改
状态变化可以被记录和跟踪，可以溯源。

`hello ${name}` 模板字符串
