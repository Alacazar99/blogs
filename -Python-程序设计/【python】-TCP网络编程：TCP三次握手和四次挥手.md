
### TCP介绍<sup> Transmission Control Protocol</sup>
【概述】：TCP协议是一种**面向连接的、可靠的、基于字节流**的传输层通信协议，由IETF的RFC 793定义。

【三步走】TCP通信需要经过创建连接、数据传送、终止连接三个步骤。

![TCP客户端服务器通信流程图](https://upload-images.jianshu.io/upload_images/17476267-9645cdd77a4e9bb0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---
## 关于 TCP “三次握手”以及“四次挥手”
##### 三次握手，建立连接
- ① 【连接建立】：请求连接方（客户端）发送SYN请求连接； 
- ② 【数据传送】：服务器返回SYN/ACK确认收到发送端请求； 
- ③ 【连接释放】：客户端回馈给服务器ACK，表示确认收到服务器发送的确认。

#### 补充：注意事项
- ① 如果服务器没有收到客户端发送的ACK，则启动**超时重传**机制，这确保了TCP连接的准确性。
- ② TCP的握手**至少有三次**。

【拓展】：连接的建立需要经过多次交互，这就是我们日常中所说的建立连接是**高成本**的操作。因此，在实际生产中，通过建立连接池，来减少连接建立的频度，**传输数据时直接从连接池中获取连接，而不是新建连接。**

#### 四次挥手【断开连接】
- ①请求方（客户端）数据已经发送完毕，向服务器发送FIN请求断开连接； 
- ②服务器向客户端发送ACK，对客户端发送的FIN进行确认，并不需要即使断开； 
- ③服务器将接收到的数据处理完毕后发送FIN，断开连接； 
- ④客户端发送确认消息ACK。

【性质】：TCP是双向通信，因此关闭连接时需要双向关闭连接。首先是关闭操作的发起方关闭本端的连接，然后是关闭接收方在收到发起方的关闭请求后，除了回复关闭应答外，还要确保数据传输完成后发起一个关闭连接的请求，保证双向同时关闭。

#### 【知识拓展】：
标志位（URG、ACK、PSH、RST、SYN和FIN）的含义介绍。
- ACK: 确认序号有效。
- RST：重置连接
- SYN：发起一个新连接
- FIN：释放一个连接

---
###  TCP编程 
####  服务器端： 
1、创建 [套接字<sup> **socket**</sup> ](https://baike.baidu.com/item/%E5%A5%97%E6%8E%A5%E5%AD%97/9637606?fr=aladdin)<br>
2、bind（命名IP地址和端口号） <br>
3、listen（创建监听队列） <br>
4、accept（拿到已经完成连接的） <br>
5、recv/send（收发数据） <br>
6、close（关闭）<br>

【知识积累】
- TCP 服务器一般情况下都需要绑定端口号，否则客户端找不到这个服务器；
- TCP 服务器中通过监听可以将 套接字创建出来的主动套接字变为被动的，这是做 TCP 服务器时必须要做的；
##### 客户端： 
1、[socket ](https://baike.baidu.com/item/%E5%A5%97%E6%8E%A5%E5%AD%97/9637606?fr=aladdin)<br>
2、connect <br>
3、recv/send <br>
4、close<br>

- TCP 客户端一般不绑定端口号，使用随机生成的端口号即可；

4、当 TCP 客户端和服务端建立好连接才可以收发数据，UDP 是不需要建立连接，直接就可以发送数据；当一个 TCP 客户端和服务端连接成功后，服务器端会有1个新的[套接字](https://baike.baidu.com/item/%E5%A5%97%E6%8E%A5%E5%AD%97/9637606?fr=aladdin)，这个套接字用来标记这个客户端，单独为这个客户端服务；<br>

5、关闭 accept 返回的套接字意味着这个客户端已经服务完毕.<br>
6、【关于套接字】:是一个抽象层，应用程序可以通过它发送或接收数据，可对其进行像对文件一样的打开、读写和关闭等操作。套接字允许应用程序将I/O插入到网络中，并与网络中的其他应用程序进行通信。网络套接字是IP地址与端口的组合。<br>


---
【引言】：在网络应用程序设计时，由于TCP/IP的核心内容被封装在操作系统中，如果应用程序要使用TCP/IP，可以通过系统提供的TCP/IP的编程接口来实现。在Windows环境下，网络应用程序编程接口称作Windows Socket。为了支持用户开发面向应用的通信程序，大部分系统都提供了一组基于TCP或者UDP的应用程序编程接口（API），该接口通常以一组函数的形式出现，也称为套接字（Socket）。
