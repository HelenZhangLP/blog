---
title: 浏览器工作原理——浏览器中的 setTimeout和XMLHttpRequest 是怎么实现的
date: 2021-01-08 09:27:04
tags:
- browser
- 浏览器工作原理
---

## 关于 setTimeout
`setTimeout` 不是由 ECMAScript 维护，而是由 host environment 提供，具体遵循的规范由 whatwg 维护
### 关于 setTimeout 的几点描述
*   If timeout is less than 0, than timeout to 0
*   If nesting(嵌套) level is greater than 5, and timeout is less than 4, than set timeout to 4
    **嵌套层级5级 + timeout 小于 4ms，设置 timeout 4ms**
    ```javascript
        setTimeout(()=>{
          // level 1
          setTimeout(()=>{
            // level 2
            setTimeout(()=>{
              // level 3
              setTimeout(()=>{
                // level 4
                setTimeout(()=>{
                  // level 5
                },0) 
              },0) 
            },0)  
          },0)
        },0)
    ```
*   Increment nesting level by one
*   let task's timer nesting level be nesting level

## setTimeout 的使用

## 浏览器中的 setTimeout 是怎么实现的
### chromium 中 setTimeout 的实现
```c++
// 用户转发的最大间隔时间
static const int maxIntervalForUserGestureForwarding = 1000;
static const int maxTimerNestingLevel = 5;
static const double oneMillisecond = 0.001; //s
static const double minimumInterval = 0.004; //s
double intervalMilliseconds = std::max(oneMillisecond, interval * oneMillisecond)
if (intervalMilliseconds < minimumInterval && m_nestingLevel >= maxTimerNestingLevel) intervalMilliseconds = 
minimumInverval
/**
  * chromium uses a minimum timers interval of 4ms. we'd like to go lower. however, there are poorly coded websites 
  out there which do create CPU-spnning loops. using 4ms prevents the CPU from spinning too busily and provides a 
  balance between CPU spinning and the smallest possible interval timer
  */
```
#### 分析 chromium 中 setTimeout 的实现
三个常量
*   `maxTimerNestingLevel = 5` 嵌套层级最多是 5
*   `minimumInterval=0.004` 最小延迟
*   `std::max(oneMillisecond, interval * oneMillisecond)` 在 1ms 和 延尽时间之间取一个最大值。<u>也就是说，在不满足嵌套层级的情况下，最小延迟时间是 1ms</u>
    ```javascript
        // Version 91.0.4472.114 (Official Build) (x86_64)
        setTimeout(console.log, 3, 3)
        setTimeout(console.log, 2, 2)
        setTimeout(console.log, 1, 1)
        setTimeout(console.log, 0, 0)
        
        /**
          1
          0
          2
          3
        **/
    ```

浏览器页面是由消息队列和事件循环系统来驱动的。
渲染进程中所有运行在主线程上的任务都需要先添加到消息队列中，事件循环系统再按照顺序执行消息队列中的任务。如下：
* 接收到 html 文档数据时，渲染引擎会将 `解析 DOM 事件` 添加到消息队列；
* 用户改变窗口大小时，渲染引擎会将 `重新布局` 事件添加到消息队列中；
* 触发 JavaScript 垃圾回收机制时，渲染引擎会将 `垃圾回收任务` 添加到消息队列；
* 执行一段异步 JavaScript 代码时，也会将`执行任务`添加到消息队列

setTimeout 定时器，用来指定回调函数参数多少毫秒后执行。返回一个整数，作为定时器编号。同时可以通过这个编号取消定时器。
`** setTimeout 需要在指定时间执行回调函数，而消息队列中任务是按顺序先进先出。Chrome 的解决办法是维护了另外一个需要延迟的执行任务的消息队列。这个消息队列中包括了定时器和 Chromium 内部一些需要延迟的任务 **`

```
  // chrominum 中延迟代码的定义
  DelayedIncomingQueue delayed_incoming_queue;
```
> 模拟代码 —— 创建回调任务，并添加至延迟队列中
创建回调任务 delayTask
回调函数、发起时间、延迟时间

```
struct DelayTask {
  int64 id;
  CallBackFunction cbf;
  int start_time; // 发起时间
  int delay_time; // 迟延时间
}

DelayTask timerTask;
timerTask.cbf = showName;
timerTask.start_time = getCurrentTime(); // 获取当前时间
timerTask.delay_time = 200;  // 设置延迟时间

// 将任务添加到延迟执行队列中
delay_incoming_queue.push(timerTask)
```
如上通过定时器发起的任务就添加至了延迟队列，那么事件循环系统如何触发延迟队列？
```
void ProcessDelayTask() {
  // 从 delay_incoming_queue 中取出到期的任务
  // 今次执行任务
}

// 主线程从任务队列中读取任务
TaskQueue task_queue;
void ProcessTask();
bool keep_running = true;
void MainThread() {
  for(;;){
    Task task = task_queue.takeTask(); // 队列中取出任务
    processTask(task); //执行任务

    ProcessDelayTask(); // 执行延迟队列中的任务

    if (!keep_running) break; // 如果设置了退出标志，直接退出线程循环
  }
}
```
ProcessDelayTask 函数根据发起时间和延迟时间计算出到期任务，然后依次执行这些任务。
```JavaScript
// 清除定时器
clearTimeout(timer_id)
```
取消定时器浏览器实现方式是从 delayed_incoming_queue 延迟队列中，通过 ID 查找对应任务，然后删除。

### 使用 setTimeout 的注意事项
1. 如果当前任务执行时间过久，会影响定时器任务的执行
很多因素会导致回调函数执行比设定的预期值要久
当前任务执行时间过久从而导致定时器设置的任务被延后执行
```JavaScript
function bar() {console.log('bar')}

function foo() {
  setTimeout(bar, 0);
  for(let i=0; i<5000; i++) console.log(i)
}

foo();
```
执行 foo 函数所消耗的时长是 500 毫秒，这也就意味着通过 setTimeout 设置的任务会被推迟到 500 毫秒以后再去执行，而设置 setTimeout 的回调延迟时间是 0。

2. 如果 setTimeout 存在嵌套调用，那么系统会设置最短时间间隔为 4 毫秒
也就是说在定时器函数里面嵌套调用定时器，也会延长定时器的执行时间
```JavaScript
function cb() {setTimeout(cb, 0);}
setTimeout(cb, 0)
```

```
static const int kMaxTimerNestingLevel = 5;
static constexpr base::TimeDelta kMinimumInterval = base::TimeDelta::FromMilliseconds(4);


base::TimeDelta interval_milliseconds =
      std::max(base::TimeDelta::FromMilliseconds(1), interval);

  if (interval_milliseconds < kMinimumInterval &&
      nesting_level_ >= kMaxTimerNestingLevel)
    interval_milliseconds = kMinimumInterval;

  if (single_shot)
    StartOneShot(interval_milliseconds, FROM_HERE);
  else
    StartRepeating(interval_milliseconds, FROM_HERE);
```
定时器被嵌套调用 5 次以上，系统会判断该函数方法被阻塞了，如果定时器的调用时间间隔小于 4 毫秒，那么浏览器会将每次调用的时间间隔设置为 4 毫秒.所以，一些实时性较高的需求就不太适合使用 setTimeout 了

3. 未激活的页面，setTimeout 执行最小间隔是 1000 毫秒
未被激活的页面中定时器最小值大于 1000 毫秒，也就是说，如果标签不是当前的激活标签，那么定时器最小的时间间隔是 1000 毫秒，目的是为了优化后台页面的加载损耗以及降低耗电量

4. 延时执行时间有最大值
Chrome、Safari、Firefox 都是以 32 个 bit 来存储延时值的，32bit 最大只能存放的数字是 2147483647 毫秒，这就意味着，如果 setTimeout 设置的延迟值大于 2                                                                         147483647 毫秒（大约 24.8 天）时就会溢出，那么相当于延时值被设置为 0 了，这导致定时器会被立即执行。如果将延时值修改为小于 2147483647 毫秒的某个值，那么执行时就没有问题了。

5. 使用 setTimeout 设置的回调函数中的 this 不符合直觉
```JavaScript
var name= 1;
var MyObj = {
  name: 2,
  showName: function(){ console.log(this.name); }
}
setTimeout(MyObj.showName,1000)
```
这段代码在编译的时候，执行上下文中的 this 会被设置为全局 window，如果是严格模式，会被设置为 undefined。

## 浏览器中的 XMLHttpRequest 是怎么实现的
XMLHttpRequest 提供了从 Web 服务器获取数据的能力，如果你想要更新某条数据，只需要通过 XMLHttpRequest 请求服务器提供的接口，便可以获取到服务器的数据，然后操作 DOM 更新页面内容，整个过程只需要更新见面的一部分就可以了，不用刷新整个页面，这样既有效率又不会打扰用户。

### 同步回调、异步调用
```JavaScript
let callback = function() {
  console.log('this is a callback function')
}

function doWork(cb) {
  console.log('start do work')
  cb()
  console.log('end do work')
}

doWork(callback)
```
如上，将函数 callback 作为参数传递给函数 doWork，那么作为参数的函数 callback 就是 **回调函数**。
如上，回调函数 callback 是在主函数 doWork 返回之前执行的，这个回调过程称为 **同步回调**。

### 异步回调
```JavaScript
let callback = function() {
  console.log('fingers crossed')
}

function doWork(cb) {
  console.log('to do homework')
  setTimeout(cb, 1000)
  console.log('end do homework')
}

doWork(callback)
```
如上，doWork 函数中使用了 setTimeout 函数让 cb 在主函数 doWork 执行完后延迟 1 秒执行。callback 没有在函数内部调用。回调函数在主函数外部执行的过程称为 **异步回调**

### 系统调用栈
消息队列与主线程循环机制保证了页面有条不紊地运行
当循环系统在执行一个任务的时候，都要为这个任务维护一个 **系统调用栈**
系统调用栈的信息可以通过 `chrome://tracing/` 抓取
也可以通过 Performance 来抓取它的核心调用信息。
[img](https://static001.geekbang.org/resource/image/d3/77/d3d66afb1a103103e5c3f86c823efb77.png)

## XMLHttpRequest 运作机制
{%plantuml%}

component 渲染进程 as renderProcess #FFF {
  card 渲染主线程 as renderMainProcess {
    control "for(;;)" as cf
    MainThread --> cf
  }
  queue 消息队列 as messageQueue {
    [任务n]
    note right of 任务n
      添加任务
    endnote
    component "任务..."
    component 任务1{
    }
  }
  card IO线程

  任务1 --> renderMainProcess: 取出任务
}

[网络进程] as networkProcess #d99
note top of networkProcess #dad
  let xhr = new XMLHttpRequest()
  xhr.onreadystatechange = function() {...}
  ..
  xhr.send()
end note
[web服务器] as webService #99d

renderMainProcess --> networkProcess:IPC
networkProcess --> webService: 请求资源
webService --> networkProcess: 响应资源
networkProcess --> 任务n: IPC(网络进程将返回的状态封装成任务，添加在消息队列尾部)
{%endplantuml%}

** XMLHttpRequest 的用法 ** 使用 XMLHttpRequest 来请求数据
```JavaScript
function getWebData(url) {
  // 新建 XMLHttpRequest 请求对象
  let xhr = new XMLHttpRequest()

  // 注册相关事件回调函数
  // onreadystatechange 监控请求过程中的状态
  xhr.onreadystatechange = function() {
    switch (xhr.readyState) {
      case 0:
        console.log('请求初始化');
        break;
      case 1:
        console.log('OPENED');
        break;
      case 2:
        console.log('HEADERS_RECEIVED');
        break;
      case 3:
        console.log('LOADING');
        break;
      case 4:
        if(this.state == 200 || this.state == 304) console.log(this.responseText);
        console.log('DONE');
        break;
    }
  }
  // 注册回调函数 ontimeout 用来监控后台请求是否超时
  xhr.ontimeout = function(e) {console.log('ontimeout')}
  xhr.onerror = function(e) {console.log('onerror')}

  // 打开请求
  xhr.open('Get', URL, true) // 创建一个 Get 请求，异步
  xhr.timeout = 3000 // 3000 毫秒后还没有响应则判断为请求失败
  xhr.responseText = "text" // 配置服务器返回的格式，将服务器数据转换为自己想要的格式，这里为 utf-16 的字符串
  xhr.setRequestHeader("X_TEST",'helen_tests')

  // 发送请求
  xhr.send();
}
```
1.  创建 XMLHttpRequest 对象，来执行实际的网络请求操作 `let xhr = new XMLHttpRequest()`
2.  为 xhr 对象注册回调函数
    后台执行任务可以通过回调函数来告诉执行结果
    XMLHttpRequest 的回调函数主要有以下几种：
    * ontimeout 监控超时请求，如果后台请求超时了，调用该函数
    * onerror 监控出错信息，如果后台请求出错，调用该函数
    * onreadystatechange 监控后台请求过程中的状态。如：监控 http 头加载完成的信息、http 响应体消息、数据加载完成消息等。
3.  配置基本的请求信息
    `xhr.open` 配置基础的请求信息，包括：请求地址、请求方法、请求方式
    配置其它可选信息 xhr.timeout = 3000 配置超时信息
    其它可选配置信息 xhr.responseText = "text" 下表为返回数据类型
    |类型|描述|
    |---|----|
    |""|将 responseText 设置为空字符串，默认类型 UTF-16 字符串|
    |"text"|返回 UTF-16字符串文本|
    |"json"|response 是一个 JavaScript 对象|
    |"document"|response 是一个 DOM 对象|
    |"blob"|response 是一个包含二进制数据的 Blob 对象|
    |"arraybuffer"|response 是一个包含二进制数据的 JavaScript ArrayBuffer|
    如果 xhr.responseText = json 那么系统会自动将服务器返回的数据转换为 JavaScript 对象格式
    其它可选配置 xhr.setRequestHeader 来添加自己专用的请求头属性
4.  发起请求
    经过 2、3 步，一切准备就绪后 xhr.send() 发起请求
    渲染进程会将请求发送给网络进程，然后网络进程负责资源下载，等网络进程接收到数据之后，就会利用 IPC 来通知渲染进程；渲染进程接收到消息之后，会将 xhr 的回调函数封装成任务并添加到消息队列中，等主线程循环系统执行到该任务的时候，会根据相关状态调用回调函数。
    * 网络请求超时：xhr.ontimeout
    * 请求出错：xhr.onerror
    * 正常接收： xhr.onreadystatechange 返馈相应状态

### XMLHttpRequest 使用过程中的“坑”
1.  跨域问题
```javaScript
var xhr = new XMLHttpRequest(); //  1.  新建 XMLHttpRequest 网络请求对象
var url = 'http://img-ads.csdn.net/2018/201811150919211586.jpg '

// 处理请求过程中的状态(onreadystatechange 监控后台请求过程中的状态)
function handler() {
  switch (xhr.readyState) {
    case 0:
      console.log('请求初始化');
      break;
    case 1:
      console.log('OPENED');
      break;
    case 2:
      console.log('HEADERS_RECEIVED');
      break;
    case 3:
      console.log('LOADING');
      break;
    case 4:
      if(this.state === 200 || this.state === 304) console.log(this.responseText);
      console.log('DONE');
      break;
  }
}

function callOtherDomain() {
  if (xhr) {
    /**
     * 2. 注册相关回调
     */
    xhr.onreadystatechange = handler
    xhr.ontimeout = function(e) {console.log('request timeout')}
    xhr.onerror = function(e) {console.log('error')}
    // 3. 配置基础请求信息
    xhr.open('GET', url, true)
    xhr.timeout = 3000
    xhr.responseText = 'json'

    // 4. 发送请求
    xhr.send()
  }
}
callOtherDomain()
```
> Access to XMLHttpRequest at 'https://time.geekbang.org/' from origin 'http://localhost:4000' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
由于跨域导致访问失败

2.  HTTPS 混合内容的问题
HTTPS 混合内容是 HTTPS 页面中包含了不符合 HTTPS 安全要求的内容，比如包含了 HTTP 资源，通过 HTTP 加载的图像、视频、样式表、脚本等，都属于混合内容。
> Mixed Content: The page at 'https://www.ximalaya.com/waiyu/18797993/243864198' was loaded over HTTPS, but requested an insecure XMLHttpRequest endpoint 'http://img-ads.csdn.net/2018/201811150919211586.jpg'. This request has been blocked; the content must be served over HTTPS.
通过 HTML 文件加载的混合资源，虽然给出警告，但大部分类型还是能加载的。而使用 XMLHttpRequest 请求时，浏览器认为这种请求可能是攻击者发起的，会阻止此类危险的请求。

XMLHttpRequest 发起请求，是由浏览器的其他进程或者线程去执行，然后再将执行结果利用 IPC 的方式通知渲染进程，之后渲染进程再将对应的消息添加到消息队列中
