##### 关于UDP网络编程
UDP（user datagram protocol）的中文叫`用户数据报协议`，属于传输层。UDP是面向非连接的协议，它不与对方建立连接，而是直接把要发的数据发给对方。

【UDP网络编程的特点】

1，每个数据中都给出了完整的地址信息，因此无需要建立发送方和接收方的连接。

2，UDP传输数据时是有大小限制的，每个被传输的数据报必须限定在64kB之内。

3，UDP是一个`不可靠的协议`，发送方所发送的数据报并不一定以相同的次序到达接收方。

4，总之，一句话，UDP网络编程不安全！

![UDP网络编程-流程图](https://upload-images.jianshu.io/upload_images/17476267-d612e621315bd03f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---

#### 代码实现：【服务端与客户端的聊天（AI机器人模式）】
先看实现的结果：

【客户端】
![客户端](https://upload-images.jianshu.io/upload_images/17476267-db83569c7a492399.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
【客户端】输入：你好吗？
[b'Sun Jul  7 17:17:58 2019']【机器人】小Zurich 好!
【客户端】输入：你觉得我是最帅最帅的吗？
[b'Sun Jul  7 17:18:34 2019']【机器人】小Zurich 觉得自己是最帅最帅的!
【客户端】输入：你觉得自己丑吗？
[b'Sun Jul  7 17:18:48 2019']【机器人】小Zurich 觉得自己丑!
【客户端】输入：你自己把自己丑哭了吗？
[b'Sun Jul  7 17:19:33 2019']【机器人】小Zurich 自己把自己丑哭了!
【客户端】输入：
```
【服务器端】
```
waiting for message...
【机器人】小Zurich 好!
【服务器端】回复: 【机器人】小Zurich 好!
waiting for message...
【机器人】小Zurich 觉得我是最帅最帅的!
【服务器端】回复: 【机器人】小Zurich 觉得自己是最帅最帅的!
waiting for message...
【机器人】小Zurich 觉得自己丑!
【服务器端】回复: 【机器人】小Zurich 觉得自己丑!
waiting for message...
【机器人】小Zurich 自己把自己丑哭了!
【服务器端】回复: 【机器人】小Zurich 自己把自己丑哭了!
waiting for message...
```
---
下面就直接上代码

【服务器端】 server.py
```
# 服务器端
import re
from socket import *
from time import ctime

HOST = ''
PORT = 8888
BUFSIZ = 1024
ADDR = (HOST, PORT)


udpservSock = socket(AF_INET, SOCK_DGRAM)
udpservSock.bind(ADDR)

while True:
    print("waiting for message...")
    data,addr = udpservSock.recvfrom(BUFSIZ)

    # print("接收到的数据：")
    # data = data.decode("utf-8")
    # content = '[%s]%s'% (bytes(ctime(), 'utf-8'),data)


    # 将客户端传回来的数据进行处理
    res = re.match(r'你(\D{1,})吗\？', data.decode('utf-8'))
    if res == None:

        data = "啊啊啊，【机器人】小Zurich 刚刚脑子出差了，请您再说一次~"
    else:
        data = "【机器人】小Zurich " + res.group(1) + '!'

    content = '[{}]{}'.format(bytes(ctime(), 'utf-8'), data.replace("我",'自己'))
    # data = data.replace('我',new= "自己")
    print(data)
    udpservSock.sendto(content.encode("utf-8"),addr)
    print("【服务器端】回复:", data.replace("我",'自己'))
```
【客户端】 client.py
```
# 客户端
from socket import *

HOST = "127.0.0.1"
PORT = 8888
BUFSIZ = 1024
ADDR = (HOST, PORT)

updCliSock = socket(AF_INET, SOCK_DGRAM)

while True:
    data = input("【客户端】输入：")
    if not data:
        break

    updCliSock.sendto(data.encode("utf-8"),ADDR)
    data,ADDR = updCliSock.recvfrom(BUFSIZ)
    if not data:
        break

    print(data.decode("utf-8"))

updCliSock.close()
```






