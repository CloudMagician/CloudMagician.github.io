---
layout:	post
title:	"the Generality of Network"
image:	''
date:	2018-12-24 17:43:44
tags:	
- Network
- Exam
description: 'My life for pass!'
categories:
- Network
---

<script type="text/javascript" src="../MathJax/MathJax.js?config=default"></script>

## 计算机网络的组成

* 网络由若干**结点(node)**和连接这些结点的**链路 (link)**组成。 

* 连接在网络上的计算机称为**主机(host)**。

* 按结构功能划分成两部分: 

  **资源子网：**负责信息处理，向网络用户提供各种 网络资源与服务。如:计算机系统、外设、软件资源等。

  **通信子网：**负责信息传递，完成数据传输及转发。 如:节点转发设备、通信设备等。

## 计算机网络的拓扑结构

### 星状拓扑(Star Topology)

| 优点 | 结构简单 | 较好的健壮性 |   便于管理、安全控制   |
| :--: | :------: | :----------: | :--------------------: |
| 缺点 | 单点失效 |              | 中央控制器是网络的瓶颈 |

### 环状拓扑(Ring Topology)

| 优点 |   易安装   | 维护管理方便 |
| :--: | :--------: | :----------: |
| 缺点 | 传输延迟大 |              |

### 总线型拓扑(Bus Topology)

| 优点 |    易安装    |        不存在路由        |
| :--: | :----------: | :----------------------: |
| 缺点 | 故障隔离问题 | 总线长度有限，设备数受限 |

### 网状拓扑(Mesh Topology)

| 优点 | 好的安全性 | 较好的健壮性 | 便于管理 | 避免拥塞问题 |
| :--: | :--------: | :----------: | :------: | :----------: |
| 缺点 |   费用高   |              |          |              |

### 树状拓扑(Tree Topology)

- 扩展了网络距离；
- 允许网络隔离不同计算机的通信。

### 混合型拓扑(Hybrid Topology)

## 计算机网络体系结构

### 网络协议和分层

* **网络协议三要素：**
  - 语法—数据与控制信息的结构或格式，如：数据格式、信号电平；
  - 语义—需要发出何种控制信息、完成何种动作、如何应答；
  - 时序(同步)—事件实现顺序的相信说明，包括信息同步，速度匹配和顺序。 

### OSI参考模型

|         名称          |   传输单位    |                             功能                             |
| :-------------------: | :-----------: | :----------------------------------------------------------: |
|   物理层(Physical)    |   比特(Bit)   |                   将计算机连接到通信媒体上                   |
| 数据链路层(Data-Link) |   帧(Frame)   |                    处理相邻节点的数据传输                    |
|    网络层(Network)    |  包(Packet)   |                 路由选择、阻塞控制、网际互连                 |
|   传输层(Transport)   | 报文(Message) |                  端到端的流量控制、差错控制                  |
|    会话层(Session)    |               | 两个计算机上的用户进程建立连接，<br/>双方互相确认身份，协商会话连接的细节。 |
| 表示层(Presentation)  |               | 解决用户信息的语法问题，<br/>对用户数据进行翻译、编码和交换。 |
|  应用层(Application)  |               |    处理用户的数据和信息，<br/>完成用户所希望的实际任务。     |

* 在计算机网络中，每层的功能由该层的实体来实现；下层实体为上层实体提供服务；上层通过下层的服务完成自己的功能。 

## TCP/IP网络体系结构

## 网络互连

## 计算机网络性能

### 速率(bitrate/data rate)

速率即数据率(data rate)或比特率(bit rate)是计算机网络中最重要的一个性能指标。速率的单位是 b/s 。

### 带宽(bandwidth)

数字信道所能传送的“最高数据率”的同义语。

### 信道利用率(utilization)

除去全部控制信息后的数据率与信道吞吐量之比；

或发送数据的时间和信道被占用时间之比。 

### 延迟时间(delay/letancy)

1. 排队时延：在发送队列中的等待时间；

2. 处理时延：主机或中间节点接收处理数据的时间；

3. 发送时延：从发送数据帧的第一个比特算起，到该帧的最后一个比特发送完毕所需的时间；
   $$
   发送时延=\frac{数据块长度(比特)}{信道带宽(比特/秒)}
   $$

4. 传输时延：电磁波在信道中需要传播一定的距离而花费的时间。
   $$
   传播时延=\frac{信道长度(米)}{信号在信道上的传播速率(米/秒)}
   $$





### 往返时间(RTT) 

发送数据开始—收到对方的确认

### 时延带宽积

$$
时延带宽积=传播时延\cdot带宽
$$


