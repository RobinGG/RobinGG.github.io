---
layout: page
title: MQTT 笔记
---

## MQTT 介绍

> MQTT[1]消息队列遥测传输(Message Queuing Telemetry Transport)是ISO 标准(ISO/IEC PRF 20922)[2]下基于发布/订阅范式的消息协议。它工作在 TCP/IP协议族上，是为硬件性能低下的远程设备以及网络状况糟糕的情况下而设计的发布/订阅型消息协议。

以上摘自 Wiki：[MQTT](https://zh.wikipedia.org/wiki/MQTT)

## 角色

MQTT 协议里面有 2 个角色：服务端(Broker)与客户端(Client)。

### 客户端

在 MQTT 协议里面，客户端主要是做这么几件事：
- 向服务端发起订阅(Subscribe)主题(topic)请求。
- 从服务端接受订阅主题的消息
- 向其他主题发布(Publish)消息

### 服务端

服务端主要的功能：
- 记录客户端订阅的主题
- 转发发布请求到相应的主题

需要注意的是，MQTT 协议没有提到服务端要对消息做持久化处理。所以市面上的很多 MQTT Broker 也没有提供这样的功能。

## 控制报文

控制报文分为 3 个部分：固定报头(Fixed header)，可变报头(Variable header) 和有效载荷(Payload)。

### 固定报头

固定报头格式如下：
<table style="text-align:center">
  <tr>
    <td align="center"><strong>Bit</strong></td>
    <td align="center"><strong>7</strong></td>
    <td align="center"><strong>6</strong></td>
    <td align="center"><strong>5</strong></td>
    <td align="center"><strong>4</strong></td>
    <td align="center"><strong>3</strong></td>
    <td align="center"><strong>2</strong></td>
    <td align="center"><strong>1</strong></td>
    <td align="center"><strong>0</strong></td>
  </tr>
  <tr>
    <td>byte 1</td>
    <td colspan="4" align="center">MQTT控制报文的类型</td>
    <td colspan="4" align="center">用于指定控制报文类型的标志位</td>
  </tr>
  <tr>
    <td>byte 2..5</td>
    <td colspan="8" align="center">剩余长度</td>
  </tr>
</table>

#### 报文类型

| **名字**    | **值** | **报文流动方向** | **描述**                            |
|-------------|--------|------------------|-------------------------------------|
| Reserved    | 0      | 禁止             | 保留                                
| CONNECT     | 1      | 客户端到服务端   | 客户端请求连接服务端                
| CONNACK     | 2      | 服务端到客户端   | 连接报文确认                        
| PUBLISH     | 3      | 两个方向都允许   | 发布消息                            
| PUBACK      | 4      | 两个方向都允许   | QoS 1消息发布收到确认               
| PUBREC      | 5      | 两个方向都允许   | 发布收到（保证交付第一步）          |
| PUBREL      | 6      | 两个方向都允许   | 发布释放（保证交付第二步）          |
| PUBCOMP     | 7      | 两个方向都允许   | QoS 2消息发布完成（保证交互第三步） |
| SUBSCRIBE   | 8      | 客户端到服务端   | 客户端订阅请求                      
| SUBACK      | 9      | 服务端到客户端   | 订阅请求报文确认                    
| UNSUBSCRIBE | 10     | 客户端到服务端   | 客户端取消订阅请求                  
| UNSUBACK    | 11     | 服务端到客户端   | 取消订阅报文确认                    
| PINGREQ     | 12     | 客户端到服务端   | 心跳请求                            
| PINGRESP    | 13     | 服务端到客户端   | 心跳响应                            
| DISCONNECT  | 14     | 客户端到服务端   | 客户端断开连接                      
| Reserved    | 15     | 禁止             | 保留                                

#### 报文标志位

除了前 4bit 用于声明报文类型外，后 4bit 还用来设了标志位。其中需要特别注意 PUBLISH 报文，他的标志位有具体的含义。

PUBLISH 标志位：
- bit1: DUP 标志，标明这条消息是否是重发的消息
- bit2,3: QoS 标志，服务质量，标明消息的服务质量等级
- bit4: RETAIN 标志，标明消息是否保留

#### 剩余长度

剩余长度标明了可变报头加有效载荷的长度。

剩余长度可以是 1-4 byte(s)。这意味着其实在 MQTT 协议里面一个消息最大能传输的大小在 256MB(268,435,455)。

### 可变报头

可变报头不是所有报文都有，只有一些与消息传输相关的报文（订阅和发布）才有。可变报头里面的内容随着报文类型的不同而不同，有一个比较通用的是报文标识符(Packet identifier)。这个与交互的行为也有关系。

### 有效载荷

有效载荷在连接，订阅和发布相关的消息里面有。其中发送发布消息的时候有效载荷其实就是真正的应用消息了

## 操作行为

### 发布流程

```plantuml
participant sender
note right : 可能是 broker
participant receiver
note right : 可能是客户端
== QoS 0 ==
sender -> receiver : PUBLISH QoS 0, DUP=0

== QoS 1 ==
sender -> sender : 保存消息
sender -> receiver : PUBLISH QoS 1, DUP=0, packetId
receiver -> receiver : 分发消息
note right: 异步操作
sender <-- receiver : PUBACK, packetId
== QoS 2 ==
sender -> sender : 保存消息
sender -> receiver : PUBLISH QoS 2, DUP=0, packetId
alt 先发消息
    receiver -> receiver : 存储 packetId
    receiver -> receiver : 分发消息
    note right: 异步操作
else 先存消息
    receiver -> receiver : 存储消息
end
sender <-- receiver : PUBREC, packetId
sender -> sender : 丢弃消息，存储 packetId
sender -> receiver : PUBREL, packetId
alt 先发消息
    receiver -> receiver : 丢弃 packetId
else 先存消息
    receiver -> receiver : 分发消息
    note right: 异步操作
    receiver -> receiver : 丢弃消息
end
sender <-- receiver : PUBCOMP, packetId
sender -> sender : 丢弃当前消息状态
```

### 消息重发

只有在客户端重连并且没有要求清理消息的场景下才会发起重发。并且可以发起重发的消息是 PUBLISH(QoS = 1,2)，PUBREL 报文。
