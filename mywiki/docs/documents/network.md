### OSI七层模型
物理层-数据链路层-网络层-传输层-会话层-表示层-应用层

也说**TCP/IP五层模型**
物理层-数据链路层-网络层-传输层-应用层

### IP/ICMP协议-网络层

IP/ICMP协议-网络层

IP协议-不可靠，不保证数据送达

数据包被加上IP头部，表示从哪里来到哪里去，IP协议负责将数据传给指定IP地址

ICMP协议-可靠

ping基于ICMP协议，当发生IP数据包错误比如主机或路由不可达时，会把错误信息封包传回给主机

### TCP/UDP协议-传输层

#### 三次握手和四次挥手

TCP-面向连接的、可靠性、基于字节流的传输层协议

TCP工作流程-三次握手和四次挥手简述

step1:通过TCP三次握手建立连接

客户端-服务器 发送SYN包，表示请求建立连接

服务器-客户端 回复SYN+ACK，表示确认收到连接请求，并同意建立连接

客户端-服务器 发送确认ACK包，表示确认收到了服务器同意连接请求

step2:传输数据

接收端对每个数据包进行确认，向发射端发送确认消息；如果没收到确认消息认为数据丢失，启动重发机制

step3:通过TCP四次挥手断开连接

客户端-服务器 发送FIN包请求关闭连接，客户端进入FIN_WAIT_1

服务器-客户端 回复ACK确认收到，服务器进入CLOSE_WAIT

服务器-客户端 发送FIN请求关闭连接，服务器进入LAST_ACK

客户端-服务器 回复ACK确认，客户端进入TIME_WAIT状态，等待一段时间后关闭

#### 详解：

**首先是三次握手**

1、  客户端发起，像服务器发送的报文SYN=1，ACK=0，然后选择了一个初始序号：seq=x。

SYN是干什么用的？

在链接的时候创建一个同步序号，当SYN=1同时ACK=0的时候，表明这是一个连接请求的报文段。如果对方有意链接，返回的报文里面SYN=1，ACK=1,。从这个意义上来说，SYN=1的时候，就表明这是一个‘请求’或者‘接受请求’的报文。

SYN=1的报文段不能携带数据。但是要消耗掉一个序号，

**ACK是干什么用的？**

仅当ACK=1的时候，确认消息（期望收到对方下一个报文段的第一个数据字节的编号）才有效。因此，TCP规定，当链接建立之后，所有往来的报文里面的ACK都应该是1

现在的状态：客户端进入SYN-SEND状态；

2、  服务器接收到了SYN=1，ACK=0的请求报文之后，返回一个SYN=1，ACK=1的确认报文。

同时，确认号ack=x+1，同时也为自己选择一个初始序号seq=y

现在的状态：服务器进入SYN-REVD状态；

3、  客户端接收到了服务器的返回信息之后，还要给服务器返回最后一条确认，ACK=1，确认号ack=y+1；

现在的状态：客户端进入ESTABLISHED状态。

**为什么两次握手不行，非得三次**

首先说明一种正常的情况，就是客户端发送了一条请求链接的报文，但是由于网络原因丢失了，所以，不可能接收到服务器端的确认。这个时候，客户端就就只有再一次发送原来的请求报文，这次服务器收到之后返回确认，客户端再确认一次，链接确立。

然后考虑一种不正常的情况，客户端发了两次请求链接的报文，第二条被服务器捕捉到，返回数据，完成了两次握手。数据传送完成之后，链接关闭。但是这时候，第一条拥塞的请求报文现在到达了服务器端，服务器还以为客户端要又一次建立连接，于是发送确认，然后把自己敞开，等着客户端发送过来数据。于是，很多的网络资源就是这样浪费掉了。

要是实行三次握手，服务器收到了一条过期的请求报文，返回确认信息，客户端接收到了服务器的信息之后感到莫名其妙，于是不置一词。服务器过一段时间还没有收到第三次握手的数据，知道客户端并没有要求建立链接的请求，含泪离开。

**四次辉手**

现在双方的状态都是ESTABLISHED状态。

1、  客户端发起请求，请求断开链接。FIN=1，seq=u。u是之前传送过来的最后一个字节的序号+1。

**FIN是干什么用的？**

FIN：用来释放一个链接，当FIN=1的时候，表明此报文的发送方已经完成了数据的发送，没有新的数据要传送，并要求释放链接。

客户端进入FIN-WAIT-1状态，等着服务器返回确认；

2、  服务器收到客户端的请求断开链接的报文之后，返回确认信息。ACK=1，seq=v，ack=u+1。

服务器进入CLOSE-WAIT状态。

这个时候，客户端不能给服务器发送信息报文，只能接收。但是服务器要是还有信息要传给服务器，仍然能传送。

3、  当服务器也没有了可以传的信息之后，给客户端发送请求结束的报文。FIN=1，ACK=1，

ack=u+1，seq=w。

这个时候的状态：服务器进入LAST-ACK状态。

4、  客户端接收到FIN=1的报文之后，返回确认报文，ACK=1，seq=u+1，ack=w+1。

发送完毕之后，客户端进入等待状态，等待两个时间周期。关闭。

**为什么最后还要等待两个时间周期呢？**

1、  客户端的最后一个ACK报文在传输的时候丢失，服务器并没有接收到这个报文。这个候。

服务器就会超时重传这个FIN消息，然后客户端就会重新返回最后一个ACK报文，等待两个时间周期，完成关闭。如果不等待这两个时间周期，服务器重传的那条消息就不会收到。服务器就因为接收不到客户端的信息而无法正常关闭。

2、  预防上一次在三次握手中提到的失效的报文干扰。两个时间周期过去之后，所有的报文都会在网络中消失，保证下一次重新连接的时候有乱七八糟的报文影响。



UDP-无连接、不可靠、但速度快、资源消耗小

应用：在线视频、互动游戏



### HTTP/HTTPS协议-应用层

**HTTP-超文本传输协议**

基于请求与响应的、无状态、无连接的应用层协议

**基于请求与响应**

客户端请求

请求行 请求方法+url+http版本信息

请求头 

请求体

服务器相应

响应行

响应头

响应体

**无连接**

默认情况下客户端和服务器端发送完请求和响应后连接会立即关闭

解决方法：

HTTP提供了keep-alive机制，使连接可以在一段时间内保持打开状态

keep-alive机制开启：

客户端在请求头部设置connection: keep-alive，服务器在响应头部设置connection: keep-alive，服务器还可以再响应头部设置keep-alive:timeout = 5,max = 1000,表示超过这个时间或连接次数后服务器主动关闭连接

设置

关闭连接 connection:close

**无状态**

HTTP无状态协议是指协议对于事务处理没有记忆。每次的请求都是独立的，服务器不会保留之前的请求信息。

解决方案通常涉及在客户端和服务器端共享状态的方法。这可以通过以下几种方式实现：

1. 使用Cookie来在客户端存储状态。服务器生成并在响应中发送一个Cookie，当客户端再次发送请求时，会包含这个Cookie，服务器就可以识别和管理会话状态。

2. 使用Session来在服务器端存储状态。当客户端发起请求时，服务器会创建一个Session对象，并将其与客户端的请求关联。

   **cookie和session的区别**

   - **存储位置**：Cookie存储在客户端浏览器，Session存储在服务器端。
   - **数据容量**：Cookie的容量较小，一般不超过4KB；而Session可以存储更多的数据。
   - **安全性**：由于Cookie存储在客户端，其中的数据可被用户和其他网站访问，因此安全性较低；而Session数据存储在服务器端，对客户端不可见，因此相对较安全。
   - **传输方式**：Cookie通过HTTP协议自动发送给服务器，每次请求都会携带Cookie数据；而Session可以通过Cookie或URL重写的方式传递Session ID到客户端。
   - **生命周期**：Cookie可以通过设置过期时间来指定存储的时间，可以是短期的或长期的；而Session默认情况下会持续到用户关闭浏览器或会话超时。
   - **应用场景**：Cookie适合存储少量的数据，常用于用户身份认证、记住登录状态等场景；Session适合存储较大的数据，常用于购物车功能、跨页面数据传递等场景。

3. 使用URL重写，在URL中明确传递会话状态。

4. 使用隐藏的表单字段存储状态。

5. 使用Token机制，如JSON Web Token (JWT)。



**HTTPS-超文本传输安全协议-披着SSL外壳的HTTP协议**

HTTP+加密数据+身份认证+数据完整性保护=HTTPS



### 页面输入网址经历的过程

step1-构建请求

step2-查找缓存

有缓存直接得到IP，无缓存则通过DNS域名解析得到IP并缓存

step3-准备IP地址和端口，http端口80，https端口443

step4-等待TCP队列

一个域名可以同时建立6个TCP连接，超出6个则需要等待

step5-TCP三次握手建立TCP连接

step6-发送HTTP请求

可能会涉及到重定向：浏览器请求的网址可能变更了，服务器会返回301状态码，告知浏览器需要用新的地址发送请求

step7-接受http响应

step8-TCP四次挥手断开连接



### HTTP状态码

1xx 表示接受的请求正在被处理

2xx 请求正常处理完毕

200 请求正常处理

204 请求被处理但无资源可以返回

3xx 重定向

301 永久重定向

302 临时重定向

4xx 客户端错误

400 请求报文语法有误，服务器无法识别

401 请求需要认证

403 请求的资源禁止访问

404 请求的资源不存在

5xx 服务器端错误

500 服务器内部错误

503 服务器正忙

### 其他
**CPU和GPU**

CPU和GPU是计算机的核心部分，首先要了解这两个部分的作用。

CPU是计算机中的一种芯片，由多个核组成，核越多处理能力越快

GPU是针对图像处理的组件，核没有CPU强但胜在核较多，并行处理能力强



**进程和线程**

在CPU或GPU执行一个应用程序时，首先在内存上分配一块该应用程序所需的私有空间，用于存储该程序所需的数据和代码等资源
同一个应用程序中会有多个不同逻辑线，它们共用同一块私有空间；可以建立时间精度更小的任务执行线去执行不同逻辑的任务

进程：也即应用程序，是资源分配的最小单位；程序启动时会分配一块内存，用来存储代码、运行数据和执行任务的主线程

线程：应用程序内部的一个个不同逻辑线，是CPU调度的最小单位

进程和线程的特点：

进程中有多个线程；线程之间可以在进程中共享数据；进程之间相互隔离；任一线程执行出错会导致进程崩溃；进程关闭后收回分配的内存









