---
title: OSI七层网络参考模型
tags:
- 计算机网络
---

> OSI(Open System Interconnection) 七层网络参考模型。<span class='custom-box custom-box-933'>是用于计算机或通信系统间互联的标准体系。</span> 通俗的讲就是为了解决主机之间的网络通信。
```mermaid
---
title: OSI七层网络参考模型
---
 flowchart TB
    id1[① 应用层]:::applicationLayer
    id2[② 表示层]:::applicationLayer
    id3[③ 会话层]:::applicationLayer
    id4[④ 传输层]:::transmissionLayer
    id5[⑤ 网络层]:::networkLayer
    id6[⑥ 数据链路层]:::dataLinkLayer
    id7[⑦ 物理层]:::physicalLayer

    id1 ~~~ id2
    id2 ~~~ id3
    id3 ~~~ id4    
    id4 ~~~ id5    
    id5 ~~~ id6    
    id6 ~~~ id7

    classDef applicationLayer stroke:#a33,fill:#a33,color:#fff,width: 150px;
    classDef transmissionLayer stroke: #66c,fill:#66c,color:#fff,width: 150px;
    classDef networkLayer stroke: #33f,fill:#33f,color:#fff,width: 150px;
    classDef dataLinkLayer stroke: #93f,fill:#93f,color:#fff,width: 150px, justfyContent: right;
    classDef physicalLayer stroke: #496,fill:#496,color:#fff,width: 150px;
```

> TCP/IP(Transmission Control Protocol/Internet Protocol),传输控制协议/因特网互联协议。网络通讯协议。当今互联网广泛采用。
```mermaid
---
title: TCP/IP
---
 flowchart TB
 id1[① 应用层]:::applicationLayer
 id2[② 传输层]:::transmissionLayer
 id3[③ 网络层]:::networkLayer
 id4[⑥ 数据链路层]:::dataLinkLayer

 id1 ~~~ id2
 id2 ~~~ id3
 id3 ~~~ id4

 classDef applicationLayer stroke:#a33,fill:#a33,color:#fff,width: 150px;
 classDef transmissionLayer stroke: #66c,fill:#66c,color:#fff,width: 150px;
 classDef networkLayer stroke: #33f,fill:#33f,color:#fff,width:150px;
 classDef dataLinkLayer stroke: #93f,fill:#93f,color:#fff,width: 150px;
 classDef dataLinkLayer stroke: #93f,fill:#93f,color:#fff,width: 150px;
 classDef physicalLayer stroke: #496,fill:#496,color:#fff,width: 150px;
```

## 应用层
应用之间的沟通方式，常见的应用层协议是 HTTP 。开发者根据 HTTP 协议编写应用程序，使得应用之间实现沟通。
最靠近用户的那一层，<span class='custom-box custom-box-933'>逻辑上把两个应用连通</span>。
```mermaid
 sequenceDiagram
 Client -->> Server: HTTP 请求
 Server ->> Client: HTTP 响应
```

## 物理层
<span class='custom-box custom-box-933'>实际物理上的连通是需要物理层</span>，物理层沟通的语言是 0/1，叫比特。
物理层将bit(比特，二进制数据)通过不同的媒介（如：电、光或其它形式的电磁波来表示和传输的信号）传输出去。

```mermaid
 flowchart LR
 data --> interface(('网络接口/wifi'))
 interface --> topology[网络拓扑]
```

`中继器和集线器`

### 网络拓扑
![网络拓扑图](/images/network/image.png)

## 数据链路层
需要高层的网络模型进行数据定向
```mermaid
 flowchart LR
 bit['物理层-比特0/1'] -->|bit+mac地址| frame['数据链路层-帧']
```

`mac 地址存在于网卡之上，是网卡的唯一标识，mac 地址也称为物理地址，全球唯一`。
<span class='custom-box custom-box-393'>二层交换机（存储发送端和接收端mac地址）</span>帮助应用程序，通过 mac 地址对不同设备进行数据传输。

```mermaid
 flowchart LR
 C0["fa:fa-computer 主机"]
 C1["fa:fa-computer 主机1"]
 C2["fa:fa-computer 主机2"]
 C3["fa:fa-computer 主机3"]
 exchangeBoard["fa:fa-rotate 二层交换机"]
 C0["fa:fa-computer 主机"] ----|跳| exchangeBoard
 exchangeBoard ----|跳| C1
 exchangeBoard ----|跳| C2
 exchangeBoard ----|跳| C3
```

物理层在传输 bit 数据时，会进行差错比对和一定的差错纠正。
设备间的传输能力与接收能力不同，可能传输端喷水式传输，另一端夹逢式接收，<span class='custom-box custom-box-933'>需要使用流控制避免这种不对称</span>

## 网络层
互联网中用唯一的 mac 地址寻址是不科学的。如果 mac 地址差别较小，但两台通信主机距离较远，仅通过 mac 地址很难快速定位。这时需要 IP 地址帮助寻址和路由选择。
<span class='custom-box custom-box-933'>IP 地址实现的是端对端的数据传输，</span>而不是数据链路层那样地址间跳到跳的传输。
除了寻址外，还有路由选择，<span class='custom-box custom-box-393'>路由器也是网络层的核心</span>
```mermaid
 flowchart LR
 bit['物理层-比特0/1'] -->|bit+mac地址| frame['数据链路层-帧']
 frame -->|帧+IP| package['网络层-包']
```

## 传输层
mac地址 + IP 地址可以到达对方主机，对主主机可能运行着多个软件进程，这时就需要端口号准确定位到具体的软件应用。
```mermaid
 flowchart LR
 client["fa:fa-laptap 客户端:8080"]
 client1["fa:fa-laptap 客户端:9000"]
 server["fa:fa-server 服务端:80"]

 client -->|:8080| server
 client1 -->|:9000| server

 server --> client
 server --> client1
```
<span class='custom-box custom-box-933'>传输层在端到端的基础上实现了服务进程到服务进行间的传输。</span>

```mermaid
 flowchart LR
 bit['物理层-比特0/1'] -->|bit+mac地址| frame['数据链路层-帧']
 frame -->|帧+IP| package['网络层-包']
 package -->|包+端口号| paragraph['传输层-段']
```