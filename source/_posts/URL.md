---
title: URL
tags:
  - grocery
date: 2023-02-24 15:51:42
---


keyWord: 路由器，调制解调器（实现网络信息与电话设施可处理的信息之间的转换），ISP 将个人网络连接到互联网的服务提供商
web 核心概念：HyperText/Http/Url

## URL
统一资源定位符（Uniform Resource Locator），在 Internet 上可以找到资源的位置的文本字符串。
是浏览器用来检索 web 上公布的任何资源机制。
给定的某一资源在 web 上的地址
<div style="display: flex">
	<span class="custom-link custom-box-339">https://</span>
	<span class="custom-link custom-box-933">helenzhanglp.github.io</span>
	<span class="custom-link custom-box-939">:80</span>
	<span class="custom-link custom-box-993">/2023/02/18/URL/</span>
	<span class="custom-link custom-box-399">?key=value&key1=value</span>
	<span class="custom-link custom-box-963">#archor</span>
</div>
<p><span class="custom-flag-339">https(http/ftp/mailto)-scheme，表明浏览器要使用何种协议</span></p>
<div style="display: flex"><span>Authority&nbsp;&nbsp;</span> <span class="custom-flag-933">Domain 域名 表示正在请求哪个 web 服务器</span>&nbsp;<span class="custom-flag-939">port 端口——用于访问 Web 服务器上的资源的技术“门”。如果 Web 服务器使用 HTTP 协议的标准端口（HTTP 为 80，HTTPS 为 443）来授予其资源的访问权限，则通常会被忽略。否则是强制性的。</div>
<p class="custom-flag-993">Path to resource 资源路径，即网络服务器上资源的路径，早期阶段，像这样的路径表示 Web 服务器上的物理文件位置。</p>
<p class="custom-flag-399">?key=value&key1=value —— 提供给网络服务器额外的参数，返回资源之前 web 服务器可以用这些参数做额外处理。每个 web 服务器都有关于自己参数的规则。唯一可靠的方式来知道特定 Web 服务器是否处理参数是通过询问 Web 服务器所有者。</p>
<p class="custom-flag-963">#anchor —— 是资源本身的另一部分锚点。锚点表示资源中的一种“书签”，给浏览器显示位于该“加书签”位置的内容的方向</p>

### URL 的使用场景
* 使用 `<a>` 在其他文档中新建链接
* 使用 `<link>`,`<script>` 将文档与其它资源关联
* `<img>`,`<vadio>`,`<audio>` 等显示多媒体图片
* `<iframe>` 在文档中显示其它 html 文档

### 绝对 URL 和相对 URL
url 必须部分在大多数情况下取约于 url 的上下文
> 如果 URL 的路径部分以“/”字符开头，则浏览器将从服务器的顶部根目录获取该资源，而不引用当前文档给出的上下文。

#### 绝对URL
* 完整的网络 URL 地址
<div style="display: flex">
	<span class="custom-link custom-box-339">https://</span>
	<span class="custom-link custom-box-933">helenzhanglp.github.io</span>
	<span class="custom-link custom-box-939">:80</span>
	<span class="custom-link custom-box-993">/2023/02/18/URL/</span>
	<span class="custom-link custom-box-399">?key=value&key1=value</span>
	<span class="custom-link custom-box-963">#archor</span>
</div>

* 隐去协议 —— 在这种情况下，浏览器将使用与用于加载该 URL 的文档相同的协议来调用该 URL。
<div style="display: flex">
	<span class="custom-link custom-box-933">helenzhanglp.github.io</span>
	<span class="custom-link custom-box-939">:80</span>
	<span class="custom-link custom-box-993">/2023/02/18/URL/</span>
	<span class="custom-link custom-box-399">?key=value&key1=value</span>
	<span class="custom-link custom-box-963">#archor</span>
</div>

* 隐去域名 —— 这是 HTML 文档中绝对 URL 最常见的用例。浏览器将使用与用于加载托管该 URL 的文档相同的协议和相同的域名。<font color="#a33">注意：不可能省略该域名而不省略协议。</font>
<div style="display: flex">
	<span class="custom-link custom-box-993">/2023/02/18/URL/</span>
	<span class="custom-link custom-box-399">?key=value&key1=value</span>
	<span class="custom-link custom-box-963">#archor</span>
</div>

#### 相对域名
假设当前地址为 `http://localhost:4000/html/)`
* 子资源
<div style="display: flex">
	<span class="custom-link custom-box-993">2023/02/18/URL/</span>
</div>
url 不以 `/` 开头，浏览器会尝试在包含当前资源的子目录中查找。当前案例，将在 `html` 目录下继续查找

* 回到目录树中
<div style="display: flex">
	<span class="custom-link custom-box-993">../2023/02/18/URL/</span>
</div>
回到上层目录，在上一层目录中查找文件

## url 知识学习相关预备
### 怎么样设计个人网站
很多项目的失败是由于缺乏明确的目标。
项目启动前需要关心三个问题，即项目构思（project ideation）：
* 要完成什么？
+ 按重要程序进行排序
* 网站如何实现个人目标
* 做什么？以怎样的顺序做？才能实现目标？

### 互联网是如何工作的
网线将两台电脑联系在一起，实现了两台电脑的通信。若 10 台电脑，每个电脑 9 个插头，则需要45根网线
路由器（router）一个特殊的小电脑，网络上每台电脑都链接到路由器上。路由器确保每台电脑上发出的信息准确到达另一台电脑。路由器十个插口，十条网线，分别连接十台电脑。
{%plantuml%}
archimate #Technology "Computer 01" as Computer01 <<technology-device>>
archimate #Technology "Computer 02" as Computer02 <<technology-device>>
archimate #Technology "Computer 03" as Computer03 <<technology-device>>
archimate #Technology "Computer 04" as Computer04 <<technology-device>>
archimate #Technology "Computer 05" as Computer05 <<technology-device>>
archimate #Technology "Computer 06" as Computer06 <<technology-device>>
archimate #Technology "Computer 07" as Computer07 <<technology-device>>
archimate #Technology "Computer 08" as Computer08 <<technology-device>>
archimate #Technology "Computer 09" as Computer09 <<technology-device>>
archimate #Technology "Computer 10" as Computer10 <<technology-device>>

rectangle router

Computer01 <-(0)-> router
Computer02 <-(0)-> router
Computer03 <-(0)-> router
Computer03 <-(0)-> router
Computer04 <-(0)-> router
Computer05 <-(0)-> router
Computer06 <-(0)-> router
Computer07 <-(0)-> router
Computer08 <-(0)-> router
Computer09 <-(0)-> router
Computer10 <-(0)-> router

{%endplantuml%}
在连接成百上千台电脑时，可以考虑使用多台路由器，相互连接
{%plantuml%}
archimate #Technology "Computer 01" as Computer01 <<technology-device>>
archimate #Technology "Computer 02" as Computer02 <<technology-device>>
archimate #Technology "Computer 03" as Computer03 <<technology-device>>
archimate #Technology "Computer 04" as Computer04 <<technology-device>>
archimate #Technology "Computer 05" as Computer05 <<technology-device>>
archimate #Technology "Computer 06" as Computer06 <<technology-device>>
archimate #Technology "Computer 07" as Computer07 <<technology-device>>
archimate #Technology "Computer 08" as Computer08 <<technology-device>>
archimate #Technology "Computer 09" as Computer09 <<technology-device>>
archimate #Technology "Computer 10" as Computer10 <<technology-device>>

rectangle router1
rectangle router2
rectangle router3
rectangle router4

router4 -- router1
router4 -- router2
router4 -- router3

Computer01 <-(0)-> router1
Computer02 <-(0)-> router1
Computer03 <-(0)-> router1
Computer04 <-(0)-> router1
Computer05 <-(0)-> router3
Computer06 <-(0)-> router2
Computer07 <-(0)-> router2
Computer08 <-(0)-> router2
Computer09 <-(0)-> router3
Computer10 <-(0)-> router3

{%endplantuml%}
电线或电话将电缆连接到你家，电话基础设施已经可以把你家连接到世界的任何角落，所以它就是我们需要的线。为了连接电话这种网络我们需要一种基础设备叫做**调制解调器（modem）**，<u>调制解调器可以把网络信息变成电话设施可以处理的信息</u>，反之亦然。
我们要将信息发送到想要到的地方，需要把我们的网络连接到网联网服务商（ISP）
{%plantuml%}
archimate #Technology "Computer 01" as Computer01 <<technology-device>>
archimate #Technology "Computer 02" as Computer02 <<technology-device>>
archimate #Technology "Computer 03" as Computer03 <<technology-device>>
archimate #Technology "Computer 04" as Computer04 <<technology-device>>
archimate #Technology "Computer 05" as Computer05 <<technology-device>>
archimate #Technology "Computer 06" as Computer06 <<technology-device>>
archimate #Technology "Computer 07" as Computer07 <<technology-device>>
archimate #Technology "Computer 08" as Computer08 <<technology-device>>
archimate #Technology "Computer 09" as Computer09 <<technology-device>>
archimate #Technology "Computer 10" as Computer10 <<technology-device>>

rectangle router

Computer01 <-(0)-> router
Computer02 <-(0)-> router
Computer03 <-(0)-> router
Computer03 <-(0)-> router
Computer04 <-(0)-> router
Computer05 <-(0)-> router
Computer06 <-(0)-> router
Computer07 <-(0)-> router
Computer08 <-(0)-> router
Computer09 <-(0)-> router
Computer10 <-(0)-> router

{%endplantuml%}

### 网站，网页，网络服务员，搜索引擎
||||
|--|--|--|
|网页|webpage|能够在 web 浏览器上展示的文档|
|网站|website|由多个页面组合在一起，并以多种方式相互连接的网页的集合|
|网络服务器|web server|在互联网上托管网站的计算机|
|搜索引擎|web engine|帮助你寻找其他见面的网站，如百度|

* 网页（webpage）
由超文本标记语言（HTML）编写的文档，可插入样式、脚本、图片、视频等多媒体资源。网络上所有的页面都可以通过一个独一无二的地址访问到
* 网站（website）
共享唯一域名且相互链接的网站的集合
* 网络服务器（web server）
一个网络服务器是一台托管一个多个站点的计算机
* 搜索引擎（web search engine）
帮助用户在其它网站中寻找网页的特定网站
浏览器是接收并显示网页的网站

#### 网络服务器
可以代指硬盘或者软件，或者他们协同工作的整体。
* 硬件部分 —— 是一个计算机，用于存储网络服务软件 + 网站组成文件（HTML 文件，图片，CSS 样式文件表，JavaScript 文件）。该计算机接入互联网并与其他接入互联网的计算机进行物理数据交换
* 软件部分
网络服务器——包括了<u>控制网络用户如何访问托管文件</u>的几个部分
HTTP 服务器——理解 URL（网络地址） 和 HTTP（浏览器用来查看网络协议）软件。通过存储的域名访问服务器。
{%plantuml%}
!include <office/Servers/database_server>
sprite $aService jar:archimate/application-service

card #Application Browser as "浏览器 <&browser>"
card Server as "服务器" <<$database_server>> {
	file Files
	rectangle HTTPServer <<$aService>><<behavior>>

}

Browser -left-> Server: HTTP Request
Server -right-> Browser: HTTP Response

{%endplantuml%}
要发布一个网站，需要静态 与 动态 服务器
* 静态服务器（static web server）服务器将它托管的文件原封不动传给浏览器
+ 硬件（计算机） + 软件（HTTP 服务）
* 动态网络服务器（dynamic web server）应用服务器会在 HTTP 服务器把托管的文件发送给浏览器之前**对文件进行动态更新**
+ 硬件（计算机）+ 软件（HTTP 服务，应用服务器，数据库）
托管文件
一个网络服务器首先要存储这个网站的文件，包括 html、图片、CSS、js 脚本等。
可以在自己的电脑上存储以上文件，但专业的网络服务器存储有以下优势：
* 一直启动运行
* 一直与互联网连接
* 一直拥有一样的 ip 地址，互联网服务供应商（Internet Service Provider，简称 ISP）不会一直会一个家庭地下提供固定的 ip
* 由一个第三方提供者维护
通过HTTP交流
一个网络服务器提供了 HTTP(超文本传输协议)支持。协议是为两台电脑间交流制定的制定的。http 协议是文本化（textual），无状态化（stateless）的协议。
* 文本化（textual）所有的命令都是纯文本的（plain-text）和人类可读的（human-readable）。
* 无状态（stateless）应用服务器帮助记录客户或服务器之前的交流、业务进展或输入信息

只有用户可制定 http 请求，服务器只能响应 http 请求。
当通过 HTTP 请求一个文件时，客户必须提供这个文件的URL。
网络服务器必须应答每一个 HTTP 请求，至少也要回复一个错误信息。

在网络服务器上，服务器负责处理和应答传入的请求
* 当收到一个请求时，HTTP 服务器首先要检查所请求的 URL 是否与一个存在的文件相匹配。
* 如果是，网络服务器会传送文件内容回到浏览器。如果不是，一个应用服务器会建立必要的文件。
* 如果两种处理都不可能，网络服务器会返回一个错误信息到浏览器，最常见的是“404 未找到” ["404 Not Found"]。（这错误太常见以至于很多网页设计者花费许多时间去设计 404 错误页面 [404 error pages]。）

动态和静态内容

#### 超链接(HyperLink)
超链接（HyperLink）简称 link，是网络背后的核心概念
网络架构底层
1989 年，蒂姆·伯纳斯 - 李（Tim Berners-Lee）提出了网站三个支柱：
* URL 跟踪 web 文档的地址系统
* HTTP 一个传输协议，以便在给定 URL 时查找文档
* HTML 允许嵌入超链接的文档格式
链接可以将任何文本与URL相关联，用户只要激活链接（手指点击或鼠标点击，键盘 Enter 键击活）就可以到达目标文档
* 链接类型
+ 内链
网页之间的链接，是网站基础
+ 外链
从当前网站链接到其他网站，外链是 web 的基础。外部链接提供给您自己维护信息之外的信息
+ 传入链接
从其他网站，链接到自己网页的链接
**锚**
锚是将一个网页中的段落相连。点击锚点链接，跳转到相应部分，而不是重新加载页面
**链接和引擎**
链接对用户和搜索引擎都很重要。搜索引擎抓取一个网站时，会按照网页上提供的链接对网站进行搜索。
搜索引擎可以通过链接来发现网站的各种网页，使用链接的可见文本来确定哪些搜索查询适合到达目标网页。
***链接会影响搜索引擎链接到您的网站的方式***
* 链接的可见文本会影响哪些搜索查询会找到给定的网址。
* 一个网页所拥有的链接越多，它在搜索结果中排名越高。
* 外部链接影响源和目标网页的搜索排名，但有多少不明确。
SEO (search engine optimization) 是如何使网站在搜索结果中排名高的研究。改善网站的链接使用是一种有用的搜索引擎优化技术。
