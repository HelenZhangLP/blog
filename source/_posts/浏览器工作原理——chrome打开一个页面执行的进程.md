---
title: 浏览器工作原理（二）——浏览器进化史
date: 2020-11-13 12:44:23
tags:
- browser
- 浏览器工作原理
---

> ## 知识点目录：
>> ### <a href="#进程和线程">进程和线程</a>
       进程和线程相关以及合并处理
>> ### <a href='#单进程浏览器时代'>单进程浏览器时代</a>
       单进程浏览器时代的问题在于插件、渲染进程、主线程都处于同一进程，会引发安全、稳定性和流畅性的问题
>> ### <a href="多进程浏览器时代">多进程浏览器时代</a>

浏览器网络进程
浏览器页面渲染过程
JavaScript 执行流程
Web 安全理论

Chrome 浏览器多进程架构
国内大部分主流浏览器都是基于 Chromium 二次开发
Chrome 是目前世界上使用率最高的浏览器

浏览器多进程架构
基于 Chrome 分析浏览器多进程架构
**Chrome 启动一个页面需要启动多少进程——`启动 Chrome 任务管理器查看`**
> chrome 选项 -> more tools -> task manager
![img](/images/browser/task-manager.png)
<!--more-->

为什么打开一个页面要启动那么多的进程呢?

## <a name="进程和线程">1. 进程和线程</a>
多线程并行处理任务
线程由进程启动管理，不能单独存在

### 1-1. 并行处理
>同一时间处理多个任务

```javascript
a = 1+2
b = 2*1
c = 3%2
```
以上代码分四个任务
前三个任务是分别计算 a,b,c，
第四个任务是计算结果。
如果采用单线程，则按顺序执行四个任务。
<u>多线程分两步，第一步是三个线程同时执行前三个任务；第二步执行第四个任务。</u>
<font color="red">多线程 **并行处理提升性能**</font>

> 线程是依附于进程的，而进程中使用多线程并行处理提升运算效率

### 1-2. 进程
一个进程是一个程序的运行实例
> 进程 —— 启动一个程序的时候，操作系统会为该程序创建一块内存，用于 **存放代码** 及 **运行中的数据** 和 **一个执行任务的主线程**。

{%plantuml%}
card processA as "进程A：单线程" #aliceblue;line:blue;line.dotted;text:black {
  card card #fff;line:blue;line.dotted; [
    <color:blue>[代码]</color><color:red>[数据]</color>[文件]
  ]
  card 主线程 #aliceblue;line:blue;line.dotted;text:blue [
    任务1
    ---
    任务2
    ---
    任务3
    ---
    任务4
  ]
  card -[#transparent]-> 主线程
}
{%endplantuml%}

{%plantuml%}
card processA as "进程B：多线程" #aliceblue;line:blue;line.dotted;text:black {
  card card #fff;line:blue;line.dotted; [
    <color:blue>[代码]</color><color:red>[数据]</color>[文件]
  ]
  rectangle card1 as " " #line.dotted {
    rectangle 线程1 #aliceblue;line:blue;line.dotted;text:blue [
      任务1
      ---
      任务2
      ---
      任务3
    ]
    rectangle 线程2 #aliceblue;line:blue;line.dotted;text:blue [
      任务4
    ]
  }
  card -[#transparent]-> card1
}
{%endplantuml%}
<font color="red">**线程依附于进程，进程中使用多线程并行处理能提升运算效率**</font>

### 1-3. 进程和线程之间的关系
1.  进程中做任意一线程执行出错，会导致整个线程崩溃
2.  线程之间共享进程中的数据
3.  当一个进程关闭后，操作系统会回收进程所占用的内存
    当一个进程关闭之后，操作系统会回收进程所申请的所有资源；即使其中任意线程因为操作不当导致内存泄漏，当进程退出时，这些内存也会被正确回收。
4.  进程之间的内容相互隔离
    进程隔离是为了保护系统中进程互不干扰的技术。每个进程只能访问自己占有的数据，避免出现进程A写入数据到进程B。进程之间的数据严格隔离，如果一个进程崩溃或挂起，不会影响到其他进程。如果进程之间需要进行数据通信，需要使用用于进程间通信（IPC）机制


## <a name="单进程浏览器时代">2. 单进程浏览器时代</a>
> 浏览器的所有功能模块<font color="red">（像网络、插件、javascript 运行环境、渲染引擎和页面）</font>运行在同一个进程里。多个功能模块运行在一个进程中，导致浏览器不稳定、不安全、不流畅。

单进程浏览器包括了其他线程、网络线程和<font color="red">**页面线程**，页面渲染、展现、JavaScript 环境插件都运行在页面线程中</font>
如此多的模块运行在一个线程中，会导致以下问题：

### 2-1. 不稳定
2007 年以前的浏览器大多是单进程，一些如 web 视频、web 游戏等功能需要借助插件实现。插件易出问题并且运行在浏览器进程中，一个插件崩溃，会导致整个浏览器崩溃。渲染引擎同样不稳定，一些复杂的 JavaScript 代码会使得渲染引擎模块崩溃。进而使得浏览器崩溃。
### 2-2. 不安全
不安全同样是**因为需要实现高级功能的插件引起**的，插件用 C/C++ 编写，可获取操作系统的任意资源，当页面运行一个恶意插件时，它便可以释放病毒、窃取帐号密码从而引发安全问题
**页面脚本**同样可以通过浏览器的漏洞获取系统权限，这些脚本获取系统权限后，同样可以做些恶意的事情
### 2-3. 不流畅
单进程浏览器中，页面的渲染模块、JavaScript 执行环境以及插件都运行在同一个线程中，也就意味着同一时刻只能有一个模块可以执行。
一个无限循环的脚本 会占整个线程，会导致其它运行在该线程中的模块没有机会执行，整个浏览器失去响应，变卡顿。
页面内存泄漏，当运行一个复杂点的页面再关闭，会存在内存不能完全回收，时间越长，内存占用越高，浏览器会越慢
<font color="red">脚本、插件、页面的内存泄漏</font>都是单进程浏览器变卡顿的原因

## <a name="多进程浏览器时代">3. 多进程浏览器时代</a>
### 3-1. 早期的多进程浏览器时代
{% plantuml %}
  collections "插件进程\n(Plugin Process)" as pluginProcess #fff;line.dashed;line:blue

  collections "渲染进程\n(Render Process)" as renderProcess #fff;line:blue
  note right of renderProcess #aliceblue;line.dotted;line:blue;: 渲染进程运行沙箱中，\n不能读写硬盘上的数据，\n不能获取操作系统权限
  note top of renderProcess #aliceblue;line.dotted;line:blue;: 渲染进程负责: \n<color:red>**解析**</color>\n<color:red>**渲染**</color>\nJavaScript 执行合成网页图片

  pluginProcess ..[#blue]. renderProcess: IPC

  usecase BrowserProcess #fff;line:blue; [
    <color:blue>**浏览器主进程**</color>
    (Browser Process)
      <b><color:red>浏览器主进程负责下载资源</color></b>
      <b><color:red>管理 IPC</color></b>
      <b><color:red>显示渲染进程生成的图片</color></b>
  ]

  pluginProcess ..[#blue]. BrowserProcess:IPC
  renderProcess ..[#blue]. BrowserProcess:IPC

{% endplantuml %}
**<center>【2008 年 Chrome 进程框架图】</center>**

Chrome
页面运行在渲染进程中，
页面中的插件运行在插件进程中，
进程之间通过 IPC 机制进行通信

#### 3-1-1. 解决不稳定
单进程浏览器不稳定的原因有两个：
一是插件崩溃导致的，在多进程浏览器中，插件由插件进程中。进程之间相互隔离，不会影响到其它进程。
二是渲染引擎引起的不稳定同理。
#### 3-1-2. 解决不安全
渲染页面是在沙箱中进行的，不能在硬盘上写入任何数据，也不能在敏感位置读取数据，恶意程序不能通过沙箱破坏系统，解决了 **不安全** 的问题。
#### 3-1-3. 解决不流畅
Javascript 渲染进程中，阻塞渲染进程，并不会影响到浏览器其它页面，脚本运行在自己的渲染进程中，死循环影响的仅仅是当前页面的渲染进程，解决了 **不流畅** 的问题。
内存泄漏，只需要关闭当前页面，资源就能被系统回收。

### 3-2. 目前的多进程框架
{% plantuml %}
rectangle " " #fff;line.dashed {
  together {
    collections "**插件进程**\n(Plugin Process)" as pluginProcess #fff;line:blue;line.dashed
    note right of pluginProcess #aliceblue;line.dotted;line:blue;: 负责插件运行，\n保证不会因插件崩溃\n导致浏览器和页面受影响
    collections "**渲染进程**\n(renderProcess)" as renderProcess #fff;line:blue;
    note left of renderProcess #aliceblue;line:blue;line.dotted;: 渲染进程的核心任务\n<b>将 html/css/javascript\n转换为用户可以交互的页面</b>\n排版引擎 Blink 和 Javascript 引擎 \n V8 都是运行在该进程中
  }

  together {
    card networkProcess #aliceblue;line:blue; [
      **网络进程**
      负责网络资源加载
      之前作为一个模块运行在浏览器进程中
      最近独立出来，作为一个新的进程
    ]
    card BrowserProcess #pink [
      **浏览器主进程**
      主要负责界面显示
      用户交互
      子进程管理
      同时提供存储等功能
    ]

    card GPUProcess #aliceblue;line:blue; [
      **GPU进程**
      GPU 的初衷是实现 3D CSS 效果
      chrome 的 UI 界面采用 GPU 来绘制
    ]
  }

  renderProcess -[#transparent]->networkProcess
}

{% endplantuml %}

* **浏览器进程**：主要负责界面显示、用户交互、子进程管理，提供存储等功能
* **渲染进程**：它的核心任务是将 html/css/javascript 转换为用户可以与之交互的网页，排版引擎 Blink 和 JavaScript 引擎 V8 都运行在该进程中。默认情况下，Chrome 会为每个 Tab 标签创建一个渲染进程。出于安全考虑，渲染进程运行在沙箱模式下。
* **GPU进程**：GPU 的初衷是为了实现 3D CSS 的效果，只是随后网页、Chrome UI界面都选择采用 GPU 来绘制。
* **网络进程**：主要负责网络资源加载。
* **插件进程**：负责插件运行，保证插件崩溃不影响浏览器页面

因为每个进程都会包含基本框架副本，多进程浏览器会 **占用更高资源**，这就意味着浏览器会消耗更多内存资源。更复杂的体系架构，模块之间会有耦合高，扩展性差。现在的框架难适应新的需求。

## 面向服务的架构(Services Oriented Architecture,SOA)
也就是说 Chrome 会朝向现代操作系统所采用的“面向服务的架构”方向发展，原来的各种模块会被重构成独立的服务（Service），每个服务都在独立的进程中运行。访问服务必须采用接口，通过 ipc 来通信，从而构建一个更内聚，松耦合、易于维护和扩展的系统，更好的实现 Chrome 简单、稳定、高速、安全的目标。
{% plantuml %}
rectangle " " #fff;line.dashed {
  together {
    collections "**插件进程**\n(Plugin Process)" as pluginProcess #fff;line:blue;line.dashed
    note right of pluginProcess #aliceblue;line.dotted;line:blue;: 负责插件运行，\n保证不会因插件崩溃\n导致浏览器和页面受影响
    collections "**渲染进程**\n(renderProcess)" as renderProcess #fff;line:blue;
    note left of renderProcess #aliceblue;line:blue;line.dotted;: 渲染进程的核心任务\n<b>将 html/css/javascript\n转换为用户可以交互的页面</b>\n排版引擎 Blink 和 Javascript 引擎 \n V8 都是运行在该进程中
  }

  together {
    card networkProcess #aliceblue;line:blue; [
      **网络进程**
      负责网络资源加载
      之前作为一个模块运行在浏览器进程中
      最近独立出来，作为一个新的进程
    ]
    card "浏览器主进程" as BrowserProcess #fff {
      rectangle UI模块 #aliceblue;line:blue;
      rectangle 网络模块 #aliceblue;line:blue;
      rectangle 文件模块 #aliceblue;line:blue;
      rectangle 设备模块 #aliceblue;line:blue;
      rectangle Profile模块 #aliceblue;line:blue;
      rectangle "..." #aliceblue;line:blue;
    }

    card GPUProcess #aliceblue;line:blue; [
      **GPU进程**
      GPU 的初衷是实现 3D CSS 效果
      chrome 的 UI 界面采用 GPU 来绘制
    ]
  }

  renderProcess -[#transparent]->networkProcess
}

{% endplantuml %}
