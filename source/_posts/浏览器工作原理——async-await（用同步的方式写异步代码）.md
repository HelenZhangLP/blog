---
title: 浏览器工作原理——async/await（用同步的方式写异步代码）
date: 2021-01-18 14:25:46
tags:
- browser
- 浏览器工作原理
---

fetch 定义在 window 对象中，用来发起远程资源请求，返回一个 Promise 对象，浏览器原生支持。

*接口嵌套请求需求：request1 请求成功后再做 request2请求，运用 fetch 实现如下：*
```javaScript
fetch('http://request1.com')
  .then(response => {
    return fetch('http://request2.com')
  }).then(response => {
    console.log(response)
  }).catch(error => {
    console.log(error)
  })
```
<!--more-->
async/await 这是 JavaScript 异步编程，提供了在不阻塞主线程的情况下使用同步代码实现异步访问资源的能力，并且使得代码逻辑更加清晰
```javascript
async function foo() {
  try{
    let response = await fetch('http://request.com')
    console.log(response, 'response')
    let response1 = await fetch('http://request1.com')
    console.log(response1, 'response1')
  } catch(err) {
    console.error(err)
  }
}
```
## 生成器（Generator）是如何工作的
> 生成器函数是一个 **带星号函数**，是可以 **暂停执行和恢复执行** 的。Generator 函数除了可以使用 return 关键字返回外，还可以使用 yield 关键字中断。调用 Generator 函数并非执行函数体内的内容，而是返回一个迭代器对象，通过这个迭代器对象的 next 指针来分步执行 Generator 函数中的内容任务。

```javascript
function* genDmo() {
  console.log('开始执行第一行')
  yield 'Generator 2'

  console.log('开始执行第二行')
  yield 'Generator 2'

  console.log('开始执行第三行')
  yield 'Generator 2'

  console.log('执行结果')
  yield 'Generator 2'
}

console.log('main')
let gen = genDmo();
console.log(gen.next().value)
console.log('main 1')
console.log(gen.next().value)
console.log('main 2')
console.log(gen.next().value)
console.log('main 3')
console.log(gen.next().value)
console.log('main 4')

//main
// demo-1.html:19 开始执行第一行
// demo-1.html:34 Generator 2
// demo-1.html:35 main 1
// demo-1.html:22 开始执行第二行
// demo-1.html:36 Generator 2
// demo-1.html:37 main 2
// demo-1.html:25 开始执行第三行
// demo-1.html:38 Generator 2
// demo-1.html:39 main 3
// demo-1.html:28 执行结果
// demo-1.html:40 Generator 2
// demo-1.html:41 main 4
```
如上执行结果：
在生成器函数内部执行一段代码，如果遇到 yield 关键字，JavaScript 引擎则返回关键字后面的内容给外部、并暂停该函数执行；
外围函数可以通过 next 方法恢复函数的执行

## Generator 的底层实现机制——协程（Coroutine）
协程是一种比线程更加轻量级的存在
可以看作是跑在线程上的任务，一个线程上可以存在多个协程，但是在线程上同时只能执行一个协程【当前执行的 A 协程，要启动 B 协程，那么 A 协程就要把主线程上的控制权给 B 协程，体现为 A 协程暂停执行，B 协程恢复执行；同样，也可以从 B 协程中启动一个 A 协程】**通常，如果从 A 协程启动 B 协程，我们把 A 协程称为 B 协程的父协程。**

一个线程可以拥有多个协程，`协程不是被操作系统内核管理，而是由程序控制，也就是在用户态执行。如此带来的好处是性能得到了很大的提升，不会像线程切换那样消耗资源`。

1.  gen 协程和父协程在主线程上交互执行，并不是并发执行的，它们之前的切换是通过 yield 和 gen.next 来配合完成
2.  当在 gen 协程中调用了 yield 方法，JavaScript 引擎会保存 gen 协程当前的调用栈信息，并恢复父协程的调用栈信息。同样，当在父协和中执行 gen.next 时，JavaScript 引擎会保存父协程的调用栈信息，并恢复 gen 协程的调用栈信息。

```javascript
function* foo() {
  let response1 = yield fetch('http://response1.com');
  console.log('response1', response1)
  let response2 = yield fetch('http://response2.com');
  console.log('response2', response2)
}

let gen = foo()
function getGenPromise(gen) {
  return gen.next().value
}

getGenPromise(gen).then(response => {
  console.log('response-1', response)
  return getGenPromise(gen)
}).then(response => {
  console.log('response-2', response)
})

```
1. 首先执行的是 `let gen = foo()`，创建 gen 协程
2. 然后父协程中通过执行 gen.next 把主线程的控制权交给 gen 协程
3. gen 协程获取到主线程的控制权后，就调用 fetch 函数创建了一个 Promise 对象 response1，然后通过 yield 暂停 gen 协程的执行，并将 response1 返回给父协程
4. 父协程恢复执行后，调用 response1.then 方法等待请求结果
5. 等通过 fetch 发起的请求完成后，会调用 then 中的回调函数，then 中的回调函数拿到结果之后，通过调用 gen.next 放弃主纯种的控制权，将控制权交给 gen 协程继续执行下个请求

> 通常我们把执行生成器的代码封装成一个函数，并把这个执行生成器代码的函数称为执行器
```javascript
function* foo() {
  let response1 = yield fetch('http://request.com')
  console.log('response1', response1)
  let response2 = yield fetch('http://request1.com')
  console.log('response2', response2)
}
co(foo())
```
生成器配合执行器，就能实现使用同步方式写出异步代码。

## async/await 使用了 Generator 和 Promise 两种技术
async/await 技术就是 Promise 和生成器的应用，低层说就是微任务和协程应用
1.  async 是一个通过 **异步执行** 并 **隐式返回 Promise** 任务结果的函数
```javascript
async function foo() {
  return 2
}
console.log(foo())

// async 声明的 foo 函数返回一个 Promise 对象，状态是 resolve
Promise {<resolved>: 2}
```

2. await
```javascript
async function foo() {
  console.log(1)
  let a = await 100
  console.log(a)
  console.log(2)
}
console.log(0)
foo()
console.log(3)
```
(1).  执行 console.log(0)，打印出 0
(2).  执行 foo 函数，foo 函数由 async 标记，当进入函数时，JavaScript 引擎会保存当前的调用栈信息，然后执行 foo 函数中的 console.log(1)，并打印 1
(3).  执行到 await 100 时，JavaScript 引擎会做以下事情：
  * 默认创建一个 Promise 对象，JavaScript 引擎会将该任务提交给微任务队列
  ```javascript
  let promise_ = new Promise((resolve, reject) => {
    resolve(100)
  })
  ```
  * JavaScript 引擎暂停当前协程的执行，将主线程的控制权交给父协程执行，同时会将 promise_ 对象返回给父协程
  * 父协程调用 promise_.then 来监控 promise 状态改变
