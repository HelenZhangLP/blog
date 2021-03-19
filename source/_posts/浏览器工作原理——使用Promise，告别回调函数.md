---
title: 浏览器工作原理——使用Promise，告别回调函数
date: 2021-01-13 16:18:53
tags:
- browser
- 浏览器工作原理
---

## JavaScript 引入 Promise 的动机
**`Promise 解决了异步编码风格的问题`**

### 异步编程问题：代码逻辑不连续
页面事件循环系统，页面中的任务都是在主线程上执行的，在执行如下载网络文件任务、获取摄像头等耗时任务，要放在页面主线程之外的进程或线程中去执行，避免耗时任务霸占页面主线程。
{%plantuml%}
skinparam componentStyle rectangle

left to right direction
rectangle 渲染进程 {
  left to right direction
  component 主线程 as mainThread #55f {
    component 主线程上耗时发起任务 #fff
    component 主线程上处理任务 #fff
    component 处理结果 #a00
  }
  queue 消息队列 #a11 {
    left to right direction
    card 处理结果 as result
    card 其它任务 as task1
    card 其它任务 as task2
    card 其它任务 as task3
  }
  component IO #55F
  mainThread -[hidden]->mainThread
}

rectangle 另外一个进程或线程 as anotherProcess #fff
主线程上耗时发起任务 -[#blue,bold]-> anotherProcess
note on link
  另外一个进程或处理中的任务
end note
anotherProcess -[#green,bold]-> result
note on link
  通过 IO 把处理结果添加到消息队列中，并排队等循环系统处理
endnote
消息队列 --> 主线程上处理任务
note on link
  排队结束，循环系统取出任务进行处理
endnote
主线程上处理任务 --> 处理结果
{%endplantuml%}
<!--more-->
上图为一个异步编程编程模型，页面主线程发起一个耗时的任务，并将任务交给另一个进程处理，页面主线程继续执行消息队列中的任务。anotherProcess 处理完这个任务后，会将任务添加渲染进程的消息队列中，并排队等待循环系统处理。排队结束后，循环系统会取出消息队列中的任务进行处理，并触发相关回调操作。

如上是页面编程的一大特点 **异步回调**
web 页面的单线程架构决定了异步回调 **异步回调影响到了我们的编码方式**

`使用 XMLHttpRequest 实现一个下载的需求`
```JavaScript
// 失败回调
function onReject(error) {
  console.log(error)
}
function onResolve(response) {
  console.log(response)
}

let xhr = new XMLHttpRequest()
xhr.ontimeout = function(e) {onReject(e)}
xhr.onerror = function(e) {onReject(e)}
xhr.onreadystatechange = function() {onResolve(xhr.response)}

let URL = 'http://test.com'
xhr.open('Get', URL, true); // 设置请未类型

// 设置参数
xhr.timeout = 3000
xhr.responseType = 'test' // 设置响应返回的数据格式
xhr.setRequestHeader('X_TEST','test')

xhr.send()
```
上面代码问题在于回调多，导致逻辑不连贯、不线性。接下来封装代码，降低处理异步回调次数

### 封装异步代码，让处理流程变得线性
{%plantuml%}
rectangle "let request = {\n\r method:'Get',\n\r url:'http:\/\/xx.com', body:'', \n\r headers: '', \n\r credentials: true, \n\r sync: false, \n\r requestType: 'text', \n\r referrer: ''\n\r}" as request #afafaf
rectangle 封装起来的 as box #efefef {
  rectangle 发起任务 #fd0
  rectangle 处理任务 #fd0
  rectangle 任务完成 #fd0

  发起任务 --> 处理任务
  处理任务 --> 任务完成 : 回调
  发起任务 ... 任务完成 : 正在处理
}

rectangle 结果输出 as out #fff {
  rectangle 出错处理 #f33
  rectangle 继续下一步业务 #f33
}

request --> box : 输入
box --> out: 输出
任务完成 --> 出错处理 : onReject **出错**
任务完成 --> 继续下一步业务 : onResolve **成功**

{%endplantuml%}
关注 request（输入内容）和 response (输出内容)，封装异步请求过程
1.  把输入 http 请求的信息封装到一个 request 对象中
```javascript
// makeRequest 用于构造 request 对象
function makeRequest(request_url) {
  let request = {
    method: 'Get',
    url: request_url,
    headers: '',
    body: '',
    credentials: false, //安全设置
    sync: false,
    responseType: 'text',
    referrer: '',
    timeout: 3000
  }
  return request
}
```
2.  封装请求过程，请求过程细节封装到 XFetch 函数
```javascript
/**
 *
 * request 输入 http 的信息，如请求头、延时、返回类型等
 * resolve 成功回调函数
 * reject 失败回调函数
 */
function XFetch(request, resolve, reject) {
  let xhr = new XMLHttpRequest()
  xhr.ontimeout = function(e) {reject(e)}
  xhr.onerror = function(e) {reject(e)}
  xhr.onreadystatechange = function() {
    if (xhr.state == 200) {
      resolve(xhr.response)
    }
  }

  // 创建一个请求
  xhr.open(request.method, request.url, request.sync)
  xhr.timeout = request.timeout
  xhr.requestType = request.requestType

  xhr.send()
}
```
3.  业务代码
```javascript
XFetch(
  makeRequest('http://xxx.com'),
  function resolve(data) {console.log(data)},
  function reject(error){console.log(error)})
```
### 新的问题：回调地域
多个接口请求相互依赖，就是存在请求之间嵌套，嵌套太多的回调函数，会使自己陷入 **回调地狱**，如下代码
```javaScript
XFetch(
  makeRequest('http://request1.com'),
  function resolve(response) {
    XFetch(
      makeRequest('http://request2.com'),
      function resolve(response) {
        XFetch(
          makeRequest('http://request3.com'),
          function resolve(response) {
            ...
          },
          function reject(error) {
            console.log(error)
          }
        )
      },
      function reject(error) {
        console.log(error)
      }
    )
  },
  function reject(error) {console.log(error)}
)
```
上面代码中， **嵌套** 调用，内层的任务执行依赖外层，并在上个任务的回调函数内部执行新的业务逻辑，多层嵌套，代码可读性变差；另外执行每个任务都有成功或失败两种结果，体现在代码中就需要对每个任务结果做两次判断，这样每个任务都要进行一次错误处理，使得代码混乱。

## Promise 解决的几个核心关键点 —— 消灭嵌套调用和多次错误处理
使用 Promise 重新构造 XFetch
```javascript
function XFetch(request){
  function executor(resolve, reject) {
    let xhr = new XMLHttpRequest()
    xhr.open('Get', request.url, true)
    xhr.ontimeout = function(e) {reject(e)}
    xhr.onerror = function(e) {reject(e)}
    xhr.onreadystatechange = function() {
      if (this.readyState === 4) // 'DONE'
      {
        if (this.state === 200) {
          resolve(this.responseText, this)
        } else {
          let error = {
            code: this.status,
            response: this.response
          }
          reject(error, this)
        }
      }
    }

    xhr.send()
  }
  return new Promise(executor)
}
```
如上：
* 使用 Promise，调用 XFetch 时，返回一个 Promise 对象
* 构建 Promise 对象时，传入 executor 函数，XFetch 主要业务在 executor 中执行
* 如果运行在 executor 函数中的业务执行成功了，会调用 resolve 函数；如果执行失败，调用 reject 函数
* 在 executor 函数中调用 resolve 函数时，会触发 promise.then 设置的回调函数；而调用 reject 函数时，会触发 promise.catch 设置的回调函数

**使用 Promise 封装后的 XFetch，来解决嵌套调用和多次异常处理的问题**
产生嵌套函数的主要原因是在发起任务请求时会带上回调函数，任务结束后，下个任务是在回调函数中处理的。
```javaScript
var x1 = XFetch(makeRequest('http://request1.com'))
var x2 = x1.then(value => {
  console.log(value)
  return XFetch(makeRequest('http://request2.com'))
})
var x3 = x2.then(value => {
  console.log(value)
  return XFetch(makeRequest('http://request3.com'))
})
x3.catch(error => {
  console.log(error)
})
```

**Promise 解决嵌套回调的方式**
1.  Promise 实现了回调函数的延时绑定。
2.  需要将回调函数 onResolve 的返回值穿透到最外层
```javaScript
// executor 执行业务逻辑
function executor(resolve, reject) {
  resolve()
}
// 创建 Promise 对象
var promiseObj1 = new Promise(executor)

// 回调函数
function onResolve(value) {
  // 回调中可以执行下个异步请求，返回 promise 对象
  return new Promise((resolve, reject) => {
    resolve(value + 1)
  })
}

//promiseObj1.then(onResolve) 延迟执行回调函数，不存在嵌套
var promiseObj2 = promiseObj1.then(onResolve)
// 取出上个回调返回的 promise
promiseObj2.then(res => {
  console.log(res)
})
```

Promise 通过 **回调函数延迟绑定** 和 **回调函数返回值穿透的技术** 解决`循环嵌套`的问题

**promise 是怎么处理异常的**
```javaScript
// 业务处理代码
function executor(resolve, reject) {
  let rand = Math.random();
  console.log(1)
  console.log(rand)
  if (rand < 0.5) {
    resolve()
  } else {
    reject()
  }
}
var p0 = new Promise(executor)
var p1 = p0.then(value => {
  console.log('success-1')
  return new Promise(executor)
})
var p2 = p1.then(value => {
  console.log('success-2')
  return new Promise(executor)
})
var p3 = p2.then(value => {
  console.log('success-3')
  return new Promise(executor)
})
p3.catch(error => {
  console.log('catch error');
})
console.log(2)
```
Promise 对象的错误具有“冒泡”性质，会一直向后传递，直到被 onReject 函数处理或 catch 语句捕获为止

## Promise 与 微任务
```javaScript
function executor(resolve, reject) {
  resolve(100)
}
let demo = new Promise(executor)
function onResolve(value) {
  console.log(value)
}
demo.then(onResolve)
```
代码执行顺序：
1.  首先执行 new Promise 时，Promise 的构造函数会被执行
2.  Promise 构造函数会调用参数 executor 函数。executor 中执行 resolve, resolve 内部调用 demo.then 设置的函数 onResolve

**browmise**
```javaScript
function Bromise() {
  var onResolve_ = null
  var onReject_ = null

  this.then = function(onResolve, onReject) {
    onResolve_ = onResolve
  }

  function resolve(value) {
    // 定时器推迟 onResolve_ 执行
    setTimeout(()=>{
      onResolve_(value)
    },0)
  }

  executor(resolve, null)
}

function executor(resolve, reject) { resolve(100)}  //将Promise改成我们自己的Bromsielet
demo = new Bromise(executor)
function onResolve(value){
  console.log(value)
}
demo.then(onResolve)
```
> onResolve_ is not a function 加入定时器让 onResolve 延迟执行
上面采用了定时器来推迟 onResolve 的执行，不过使用定时器的效率并不是太高，好在我们有微任务，所以 Promise 又把这个定时器改造成了微任务了，这样既可以让 onResolve_ 延时被调用，又提升了代码的执行效率
