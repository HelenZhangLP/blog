---
title: async await promise
date: 2019-07-24T14:06:56.000Z
tags:
  - JavaScript
  - 语法糖
---

```javascript
async function async1() {
    console.log( 'async1 start' )
    await async2()
    console.log( 'async1 end' )
}

async function async2() {
    console.log( 'async2' )
}

console.log( 'script start' )

setTimeout( function () {
    console.log( 'setTimeout' )
}, 0 )

async1();

new Promise( function ( resolve ) {
    console.log( 'promise1' )
    resolve();
} ).then( function () {
    console.log( 'promise2' )
} )

console.log( 'script end' )
```

<!-- more -->

## Promise
解决采用 callback 机制产生的如回调地狱一样的潜在问题，允许采用近乎同步的逻辑写异步代码

## async/await 异步编程的终级解决方案

> JavaScript 的 async/await 实现，也离不开 Promise，async 用于申明一个方法是异步的，await 用于等待一个异步方法执行完成。await 只能出现在 async 函数中。

## async 如何处理它的返回值的

```javascript
async function fn() {
    return "hello async";
}

// 返回值
Promise {<resolved>: "hello async"}
__proto__: Promise
[[PromiseStatus]]: "resolved"
[[PromiseValue]]: "hello async"
```

所以 async 函数返回的是一个 Promise 对象，async 会把 return 变量通过 Promise.resolve() 封装成 Promise 对象。可以用 then() 链处理 Promise 对象。

```javascript
async function fn() {
    return "hello async";
}

fn().then(res=>{console.log(res)})

// 返回值
hello async
Promise {<resolved>: undefined}__proto__: Promise[[PromiseStatus]]: "resolved"[[PromiseValue]]: undefined
```

## await

> async 函数返回 Promise, await 可以用于等待 async 异步的完成，异步操作返回的都是 promise, await 按顺序执行。

[更多案例](https://github.com/HelenZhangLP/demo/tree/master/node/node-demo/demo-1)
