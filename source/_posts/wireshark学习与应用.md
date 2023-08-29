---
title: wireshark学习与应用
date: 2022-05-26 19:10:35
tags:
  - 计算机网络
  - wireshark
  - 抓包
  - 软件学习
categories: 
  - 计算机网络
index_img: https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwireshark%2FWireshark-Free-Download-Logo.png
description: 计算机网络上机实验一：Wireshark抓包工具使用
---

# wireshark抓包工具使用

## wireshark简介

wireshark是一个网络封包分析软件，其功能是撷取网络封包，并尽可能显示出最为详细的网络封包资料。Wireshark使用WinPCAP作为借口，直接与网卡进行数据报文交换。

## wireshark主要应用

+ 网络管理员用来解决网络问题
+ 网络安全工程师用来检测安全隐患
+ 开发人员用来测试协议执行情况
+ 用来学习网络协议

## waireshark快速分析数据包技巧

+ 确定wireshark的物理位置。
+ 选择捕获接口。
+ 使用捕获过滤器
+ 使用显示过滤器
+ 使用着色规则
+ 构建图表
+ 重组数据

## wireshark抓包及快速定位数据包技巧

### 常见协议包

+ ARP协议
+ ICMP协议
+ TCP协议
+ UDP协议
+ DNS协议
+ HTTP协议

## wireshark过滤器使用

+ 其中一例：使用过滤器筛选udp的数据包

  使用过滤器输入“udp”以筛选出udp报文。但输入udp之后出现了多种协议。原因就是oicq以及dns都是基于udp的传输层之上的协议。

  扩展：客户端向DNS服务器查询域名，一般返回的内容都不超过512字节，用UDP传输即可。不用经过三次握手，这样DNS服务器负载更低，响应更快。理论上说，客户端也可以指定向DNS服务器查询时用TCP，但事实上，很多DNS服务器进行配置的时候，仅支持UDP查询包。

+ ip.src_host为源ip地址
+ ip.dst_host为目的ip地址
+ ip.addr不分源和目的ip地址
+ 查询条件为或是`or`
+ 查询条件为且是`and`

## wireshark对常用协议抓包并分析原理

**协议分析的时候关闭混杂模式，避免一些干扰的数据包存在**

### 常用协议分析-ARP协议

+ 地址解析协议（英语：Address Resolution Protocol，缩写ARP）是一个通过解析网络层地址来找寻数据链路层地址的网络传输协议，它在IPv4中及其重要。ARP是通过网络地址来定位MAC地址。

+ 开始抓包---过滤arp

​		![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwireshark%2F66%20-%20SzNv8iT.png)

​		使用`nmap (-sn)` （对目标进行ping检测，不进行端口扫描）来基于ARP协议进行扫描

+ 请求包（`Opcode:_request(1)`）中，目标mac地址为0，因为不清楚。
+ 应答包（`Opcode:_reply(2)`）中，应答包补全了自己的MAC地址，目的地址和源地址做了替换。

​		![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwireshark%2F67%20-%20KRNZG8P.png)

### 常用协议分析-ICMP协议

+ 把之前的数据包清空掉然后筛选ICMP协议的数据包
+ 打开一个终端 ping baidu.com
+ 基于IP协议

#### ICMP协议分析请求包

![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwireshark%2F69%20-%20EVF8tfn.png)

#### ICMP协议分析应答包

![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwireshark%2F68%20-%200Kgv5ze.png)

**工作过程：**

+ 本机发送一个ICMP Echo Request的包。
+ 接收方返回一个ICMP Echo Reply，包含了接受到数据拷贝和一些其他命令。

### 常用协议分析-TCP协议

+ 启动抓包。选择http://www.sina.com.cn作为目标地址
+ 打开命令夫，通过ipconfig查看本机IP地址
+ Filter对话框中填入过滤条件：tcp and ip.addr == 10.19.129.113,得到过滤结果。

+ 连接三次握手，断开连接四次挥手

#### 三次握手



##### 第一次握手

+ ##### 客户端给服务器发送一个SYN段，该段中也包含客户端的初始序列号(Sequence number)

  ![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwireshark%2F71%20-%205MX78iG.png)

##### 第二次握手

+ 服务器返回客户端SYN+ACK段，该段中包含服务器的初始序列号(Sequence number = J);同时使Acknowledgment number = J + 1来表示确认已收到客户端的SYN段。

  ![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwireshark%2F72%20-%20ydQ9DoP.png)

##### 第三次握手

+ 客户端给服务器相应一个ACK段，该段中使Acknowledgment number = K + 1来表示确认已收到服务器的SYN段(Sequence number = K)

  ![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwireshark%2F73%20-%20frRzNZS.png)

#### 四次挥手

![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwireshark%2F74%20-%20zxOkltZ.png)

wireshark卡退了，下面几张图和端口对不上，但不影响

##### 第一次挥手

+ 客户端打算断开连接，向服务器发送FIN报文，FIN报文中会指定一个序列号，之后客户端进入FIN_WAIT_1状态。也就是客户端发出连接释放报文段(FIN报文)，指定序列号seq = u，主动关闭TCP连接，等待服务器的确认。

  ![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwireshark%2F75%20-%206BqFDbA.png)

##### 第二次挥手

+ 服务器收到连接释放报文段(FIN报文)后，就向客户端发送ACK应答报文，以客户端的FIN报文的序列号 seq+1 作为ACK应答报文段的确认序列号ack = seq+1 = u + 1。接着服务器进入CLOSE_WAIT(等待关闭)状态，此时的TCP处于半关闭状态，客户端到服务器的连接释放。客户端收到来自服务器的ACK应答报文段后，进入FIN_WAIT_2状态。

  ![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwireshark%2F76%20-%20jRYYGkm.png)

##### 第三次挥手

+ 服务器也打算断开连接，向客户端发送**连接释放(FIN)报文段**，之后服务器进入**LASK_ACK(最后确认)状态**，等待客户端的确认。服务器的连接释放(FIN)报文段的FIN=1，ACK=1，序列号seq=m，确认序列号ack=u+1。

  ![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwireshark%2F77%20-%20FTERgJ4.png)

##### 第四次挥手

+ 客户端收到来自服务器的连接释放(FIN)报文段后，会向服务器发送一个ACK应答报文段，以连接释放(FIN)报文段的确认序号 ack 作为ACK应答报文段的序列号 seq，以连接释放(FIN)报文段的序列号 seq+1作为确认序号ack。之后客户端进入TIME_WAIT(时间等待)状态，服务器收到ACK应答报文段后，服务器就进入CLOSE(关闭)状态，到此服务器的连接已经完成关闭。客户端处于TIME_WAIT状态时，此时的TCP还未释放掉，需要等待2MSL后，客户端才进入CLOSE状态。

  ![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwireshark%2F78%20-%20gFP1ulp.png)

### 常用协议分析-HTTP协议

+ 还是筛选TCP协议因为HTTP是TCP的上层协议，所以过滤TCP的数据会包含HTTP协议的数据包

+ 使用curl -I baidu.com
+ curl是一个在命令行下工作的文件传输工具，这里用来发送http请求 -I标识仅返回头部信息 看tcp协议时也可用此方法

​		![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwireshark%2F79%20-%20GqXXPLH.png)

+ 第一步：发送了一个HTTP的HEAD请求

+ 第二步：服务器收到我们的请求返回了一个Seq/ACK进行确认

+ 第三步：服务器将HTTP的头部信息返回给我们客户端 **状态码为200标识页面正常**

+ 第四步：客户端收到服务器返回的头部信息向服务器发送Seq/ACK进行确认

+ 发送完成之后客户端就会发送FIN/ACK来进行关闭链接的请求

  ![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%2Fwireshark%2F80%20-%20xUuVOxl.png)

## 习题与思考题

**网络工程师能通过WIRESHARK做哪些工作**

+ 检擦网络协议的执行情况。
+ 排查网络故障，解决网络问题。
+ 网络攻防，检测网络安全隐患。
