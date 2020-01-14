##### TCP客户端服务器通信
TCP客户端服务器通信主要有以下五类，下面将对此进行详细的介绍。
- 单进程TCP Server
- 多进程TCP Server
- 多线程TCP Server
- 协程版TCP Server
- IO多路复用
- selector
---
###### 单进程 TCP Server
【特点】：一次只能为一个客户服务。

【注意】：当服务器为这个客户服务的时候，只要服务器的listen队列还有空闲，那么当其它新的客户端发起连接后，服务器就会为新客户端建立连接，并且新客户端也可以发送数据，但服务器还不会处理。

【服务器端】
```
# -*- coding: utf-8 -*-
# TCP Echo Server，单进程，阻塞 blocking I/O
import socket

# 创建监听socket
server_sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# socket默认不支持地址复用，OSError: [Errno 98] Address already in use
server_sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

# 绑定IP地址和固定端口
server_address = ('', 9090)
print('TCP Server starting up on port {}'.format(server_address[1]))
server_sock.bind(server_address)

# socket默认是主动连接，调用listen()函数将socket变为被动连接，这样就可以接收客户端连接了
server_sock.listen(5)

try:
    while True:
        print('Main Process, waiting for client connection...')

        # client_sock是专为这个客户端服务的socket，client_addr是包含客户端IP和端口的元组
        client_sock, client_addr = server_sock.accept()
        print('Client {} is connected'.format(client_addr))

        try:
            while True:
                # 接收客户端发来的数据，阻塞，直到有数据到来
                # 事实上，除非当前客户端关闭后，才会跳转到外层的while循环，即一次只能服务一个客户
                # 如果客户端关闭了连接，data是空字符串
                data = client_sock.recv(4096)
                if data:
                    print('Received {}({} bytes) from {}'.format(data, len(data), client_addr))
                    # 返回响应数据，将客户端发送来的数据原样返回
                    client_sock.send(data)
                    print('Sent {} to {}'.format(data, client_addr))
                else:
                    print('Client {} is closed'.format(client_addr))
                    break
        finally:
            # 关闭为这个客户端服务的socket
            client_sock.close()
finally:
    # 关闭监听socket，不再响应其它客户端连接
    server_sock.close()
```
---
【解释一下】：只有当第1个客户关闭连接后，服务器才会一次性将第2个客户发送的所有数据接收完，并继续只为第2个客户服务，（第三位及以后的客户需要继续等待）。

【客户端】
```
import time
from datetime import datetime
import socket


server_ip = input('Please enter the TCP server ip: ')
server_port = int(input('Enter the TCP server port: '))
client_num = int(input('Enter the TCP clients count: '))

# 保存所有已成功连接的客户端TCP socket
client_socks = []

for i in range(client_num):
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.connect((server_ip, server_port))
    client_socks.append(sock)
    print('Client {}[ID: {}] has connected to {}'.format(sock, i, (server_ip, server_port)))

while True:
    for s in client_socks:
        data = str(datetime.now()).encode('utf-8')
        s.send(data)
        print('Client {} has sent {} to {}'.format(s, data, (server_ip, server_port)))
    # 睡眠3秒后，继续让每个客户端连接向TCP Server发送数据
    time.sleep(3)
```
【单进程TCP Server出现的问题】：client_sock.recv(4096)会一直阻塞；
【导致】不能跳转到外层while循环中去为其它新的客户端创建socket，只能一次为一个客户服务。（满足不了实际应用需要）

---

###### 多进程 TCP Server
【优势】：为了实现并发处理多个客户端请求，可以使用多进程，应用程序的主进程只负责为每一个新的客户端连接创建socket，然后为每个客户创建一个子进程，用来分别处理每个客户的数据。
```
import os
import socket
from multiprocessing import Process


def client_handler(client_sock, client_addr):
    '''接收各个客户端发来的数据，并原样返回'''
    try:
        while True:
            # 接收客户端发来的数据，阻塞，直到有数据到来
            # 如果客户端关闭了连接，data是空字符串
            data = client_sock.recv(4096)
            if data:
                print('Child Process [PID: {}], received {}({} bytes) from {}'.format(os.getpid(), data, len(data), client_addr))
                # 返回响应数据，将客户端发送来的数据原样返回
                client_sock.send(data)
                print('Child Process [PID: {}], sent {} to {}'.format(os.getpid(), data, client_addr))
            else:
                print('Child Process [PID: {}], client {} is closed'.format(os.getpid(), client_addr))
                break
    except:
        # 如果客户端强制关闭连接，会报异常: ConnectionResetError: [Errno 104] Connection reset by peer
        pass
    finally:
        # 关闭为这个客户端服务的socket
        client_sock.close()


# 创建监听socket
server_sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# socket默认不支持地址复用，OSError: [Errno 98] Address already in use
server_sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

# 绑定IP地址和固定端口
server_address = ('', 9090)
print('TCP Server starting up on port {}'.format(server_address[1]))
server_sock.bind(server_address)

# socket默认是主动连接，调用listen()函数将socket变为被动连接，这样就可以接收客户端连接了
server_sock.listen(5)

try:
    while True:
        print('Main Process [PID: {}], waiting for client connection...'.format(os.getpid()))

        # 主进程只用来负责监听新的客户连接
        # client_sock是专为这个客户端服务的socket，client_addr是包含客户端IP和端口的元组
        client_sock, client_addr = server_sock.accept()
        print('Main Process [PID: {}], client {} is connected'.format(os.getpid(), client_addr))

        # 为每个新的客户连接创建一个子进程，用来处理客户数据
        client = Process(target=client_handler, args=(client_sock, client_addr))
        client.start()
        # 子进程已经复制了一份client_sock，所以主进程中可以关闭此client_sock
        client_sock.close()
finally:
    # 关闭监听socket，不再响应其它客户端连接
    server_sock.close()
```
【多进程 TCP Server的问题】：
- 每个客户端连接，都要创建一个进程。当用户量较大时，**系统开销会非常大，内存会被耗尽，导致系统崩溃或者性能将急剧下降。**
- 进程太多的话，CPU进行进程间切换的代价太大。

【改进办法】：使用进程池**concurrent.futures.ProcessPoolExecutor**创建固定数量的进程。

---

###### 多线程TCP Server
【优势】：多线程版本比多进程版本的系统开销小几个数量级，操作系统可以同时开启更多的线程，而线程间的调度切换比多进程也小很多

```
import socket
import threading


def client_handler(client_sock, client_addr):
    '''接收各个客户端发来的数据，并原样返回'''
    try:
        while True:
            # 接收客户端发来的数据，阻塞，直到有数据到来
            # 如果客户端关闭了连接，data是空字符串
            data = client_sock.recv(4096)
            if data:
                print('Child Thread [{}], received {}({} bytes) from {}'.format(threading.current_thread().name, data, len(data), client_addr))
                # 返回响应数据，将客户端发送来的数据原样返回
                client_sock.send(data)
                print('Child Thread [{}], sent {} to {}'.format(threading.current_thread().name, data, client_addr))
            else:
                print('Child Thread [{}], client {} is closed'.format(threading.current_thread().name, client_addr))
                break
    except:
        # 如果客户端强制关闭连接，会报异常: ConnectionResetError: [Errno 104] Connection reset by peer
        pass
    finally:
        # 关闭为这个客户端服务的socket
        client_sock.close()


# 创建监听socket
server_sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# socket默认不支持地址复用，OSError: [Errno 98] Address already in use
server_sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

# 绑定IP地址和固定端口
server_address = ('', 9090)
print('TCP Server starting up on port {}'.format(server_address[1]))
server_sock.bind(server_address)

# socket默认是主动连接，调用listen()函数将socket变为被动连接，这样就可以接收客户端连接了
server_sock.listen(5)

try:
    while True:
        print('Main Thread [{}], waiting for client connection...'.format(threading.current_thread().name))

        # 主进程只用来负责监听新的客户连接
        # client_sock是专为这个客户端服务的socket，client_addr是包含客户端IP和端口的元组
        client_sock, client_addr = server_sock.accept()
        print('Main Thread [{}], client {} is connected'.format(threading.current_thread().name, client_addr))

        # 为每个新的客户连接创建一个线程，用来处理客户数据
        client = threading.Thread(target=client_handler, args=(client_sock, client_addr))
        client.start()

        # 因为主线程与子线程共享client_sock，所以在主线程中不能关闭client_sock
        # client_sock.close()
finally:
    # 关闭监听socket，不再响应其它客户端连接
    server_sock.close()
```
【优化】：可以使用 **线程池**concurrent.futures.ThreadPoolExecutor实现。

---

###### 协程版TCP Server
```
import gevent
from gevent import socket,monkey
monkey.patch_all()
import threading


def client_handler(client_sock, client_addr):
    '''接收各个客户端发来的数据，并原样返回'''
    try:
        while True:
            # 接收客户端发来的数据，阻塞，直到有数据到来
            # 如果客户端关闭了连接，data是空字符串
            data = client_sock.recv(4096)
            if data:
                print('Child Thread [{}], received {}({} bytes) from {}'.format(threading.current_thread().name, data, len(data), client_addr))
                # 返回响应数据，将客户端发送来的数据原样返回
                client_sock.send(data)
                print('Child Thread [{}], sent {} to {}'.format(threading.current_thread().name, data, client_addr))
            else:
                print('Child Thread [{}], client {} is closed'.format(threading.current_thread().name, client_addr))
                break
    except:
        # 如果客户端强制关闭连接，会报异常: ConnectionResetError: [Errno 104] Connection reset by peer
        pass
    finally:
        # 关闭为这个客户端服务的socket
        client_sock.close()


# 创建监听socket
server_sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# socket默认不支持地址复用，OSError: [Errno 98] Address already in use
server_sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

# 绑定IP地址和固定端口
server_address = ('', 9090)
print('TCP Server starting up on port {}'.format(server_address[1]))
server_sock.bind(server_address)

# socket默认是主动连接，调用listen()函数将socket变为被动连接，这样就可以接收客户端连接了
server_sock.listen(500)

try:
    while True:
        print('Main Thread [{}], waiting for client connection...'.format(threading.current_thread().name))

        # 主进程只用来负责监听新的客户连接
        # client_sock是专为这个客户端服务的socket，client_addr是包含客户端IP和端口的元组
        client_sock, client_addr = server_sock.accept()
        print('Main Thread [{}], client {} is connected'.format(threading.current_thread().name, client_addr))

        gevent.spawn(client_handler, client_sock,client_addr)
        # 为每个新的客户连接创建一个线程，用来处理客户数据
        # client = threading.Thread(target=client_handler, args=(client_sock, client_addr))
        # client.start()

        # 因为主线程与子线程共享client_sock，所以在主线程中不能关闭client_sock
        # client_sock.close()
finally:
    # 关闭监听socket，不再响应其它客户端连接
    server_sock.close()
```

######【内容补充】：gevent猴子补丁

【优点】：方便的导入非阻塞的模块，不需要特意的去引入。
【特点】：猴子补丁充分利用了动态语言的灵活性，可以对现有的语言Api进行追加，替换，修改Bug，甚至性能优化等等。

- gevent的猴子补丁就可以对**ssl、socket、os、time、select、thread、subprocess、sys等模块**的功能进行了增强和替换；
-**monkey.patch_socket()**，将**同步的socket读写操作，替换成了异步版本**。
-  monkey.patch_all() 包含了 monkey.patch_socket() server.py

---

###### IO多路复用

【特点】IO操作是不占用CPU的，IO多路复用，是为了管理起所有的IO操作。

![IO多路复用模型](https://upload-images.jianshu.io/upload_images/17476267-ec14206240bc94a6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

IO多路复用模型：是建立在内核提供的多路分离函数select基础之上的，使用select函数可以避免同步非阻塞IO模型中轮询等待的问题。

【补充拓展】：服务器端编程经常需要构造高性能的IO模型，常见的IO模型有四种：
- （1）同步阻塞IO<sup>（Blocking IO）</sup>：
即传统的IO模型。

- （2）同步非阻塞IO<sup>（Non-blocking IO）</sup>：
默认创建的socket都是阻塞的，非阻塞IO要求socket被设置为NONBLOCK。注意这里所说的NIO并非Java的NIO（New IO）库。

- （3）IO多路复用<sup>（IO Multiplexing）</sup>：
即经典的Reactor设计模式，有时也称为异步阻塞IO，Java中的Selector和Linux中的epoll都是这种模型。

- （4）异步IO<sup>（Asynchronous IO）</sup>：
即经典的Proactor设计模式，也称为异步非阻塞IO。

##### 同步 & 异步
同步与异步是针对多个事件(线程/进程)来说的。

- 如果事件A需要等待事件B的完成才能完成，这种串行执行机制可以说是同步的，这是一种可靠的任务序列，要么都成功，要么都失败。
- 如果事件B的执行不需要依赖事件A的完成结果，这种并行的执行机制可以说是异步的。事件B不确定事件A是否真正完成，所以是不可靠的任务序列。


同步、异步可以理解为多个事件的执行方式和执行时机如何，是串行等待还是并行执行。同步中依赖事件等待被依赖事件的完成，然后触发自身开始执行，异步中依赖事件不需要等待被依赖事件，可以和被依赖事件并行执行，被依赖事件执行完成后，可以通过回调、通知等方式告知依赖事件。

######阻塞 & 非阻塞
阻塞与非阻塞针对单一事件(线程/进程)。
- 对于阻塞，如果一个事件在发起一个调用之后，在调用结果返回之前，该事件会被一直挂起，处于等待状态。
- 对于非阻塞，如果一个事件在发起调用以后，无论该调用当前是否得到结果，都会立刻返回，不会阻塞当前事件。

**【各位可以略过上面的概念文字，下面将是一些干货】**

【拉回主题】：IO多路复用，实际上实现了类似并发效果的伪并发。内部实际使用了循环来高效的处理阻塞请求。
在Python中，有一个select模块，提供了：**select、poll、epoll**三个方法来调用系统的 select，poll，epoll 从而实现IO多路复用。
（Windows 中，Python只提供： select）

######【select方法】
请参阅：[https://www.cnblogs.com/cthon/p/9046544.html](https://www.cnblogs.com/cthon/p/9046544.html)

【实例】：通过select，实现读写分离（收发分离）
```
import socket
import select

# 创建socket对象,绑定IP端口,监听
sk = socket.socket()
sk.bind(('127.0.0.1', 1559))
sk.listen(5)

inputs = [sk]
while True:
    rList, w, e = select.select(inputs, [], [], 1)
    print("select当前监听socket对象的数量>", len(inputs), " | 发生变化的socket数量>", len(rList))

    for s in rList:
        # 判断socket对象如果是服务端的socket对象的话
        if s == sk:
            conn, address = s.accept()
            # conn也是一个socket对象
            # 当服务端socket接收到客户的请求后,会分配一个新的socket对象专门用来和这个客户端进行连接通信

            # 当服务端分配新的socket对象给新连接进来的客户端的时候
            # 我们也需要监听这个客户端的socket对象是否会发生变化
            # 一旦发生变化,意味着客户端向服务器端发来了消息
            inputs.append(conn)
            conn.sendall(bytes('hello', encoding='utf8'))
        # 其他的就都是客户端的socket对象了
        else:
            try:
                # 意味着客户端给服务端发送消息了
                msg = s.recv(1024)

                # Linux平台下的处理
                if not msg:
                    raise Exception('客户端已断开连接')
                print(msg)

                # 向客户端回复消息
                # 这种写法是完全可以的,但是缺点是读写都混在了一起
                s.sendall(msg)
            except Exception as ex:
                # Windows平台下的处理
                inputs.remove(s)
```
---

【IO多路复用适用场合】 

　　1.当客户处理多个描述字时（一般是交互式输入和网络套接口），必须使用I/O复用。 
　　2.当一个客户同时处理多个套接口时，而这种情况是可能的，但很少出现。 
　　3.如果一个TCP服务器既要处理监听套接口，又要处理已连接套接口，一般也要用到I/O复用。 
　　4.如果一个服务器即要处理TCP，又要处理UDP，一般要使用I/O复用。 
　　5.如果一个服务器要处理多个服务或多个协议，一般要使用I/O复用。
#####  selector

```
import selectors
import socket

class BaseServer:
    def __init__(self, host, port):
        self.selector = selectors.DefaultSelector()
        self.sock = socket.socket()
        self.address = (host, port)
        self.request_queue_size = 100
        self.msgs = []
        print("listening on", (host, port))
        self.open_socket()

    def accept(self, sock, mask):
        sel = self.selector

        conn, addr = sock.accept()  # Should be ready
        conn.setblocking(False)
        sel.register(conn, selectors.EVENT_READ | selectors.EVENT_WRITE, self.process)

    def process(self, conn, mask):
        if mask & selectors.EVENT_READ:
            self.read(conn)
        else:
            self.write(conn)

    def write(self, conn):
        if len(self.msgs):
            data = self.msgs.pop(0)
            conn.send(data.encode())

    def read(self, conn):
        sel = self.selector
        try:
            data = conn.recv(1000)  # Should be ready
            if data:
                conn.setblocking(False)
                print('echoing', repr(data), conn)

                msg = data.decode('utf-8')
                self.msgs.append(msg)
            else:
                print('closing', conn)
                sel.unregister(conn)
                conn.close()
        except ConnectionResetError:
            print('closing', conn)
            sel.unregister(conn)
            conn.close()

    def server_close(self):

        self.sock.close()
        self.selector.close()

    def server_bind(self):
        """
        绑定
        """
        sock = self.sock
        sock.bind(self.address)

    def server_listen(self):
        """
        监听
        """
        self.sock.listen(self.request_queue_size)

    def open_socket(self):
        sock = self.sock

        self.server_bind()
        self.server_listen()
        sock.setblocking(False)

    def serve_forever(self):
        sock = self.sock
        sel = self.selector

        sel.register(sock, selectors.EVENT_READ, self.accept)
        try:
            while True:
                events = sel.select()
                for key, mask in events:
                    callback = key.data
                    callback(key.fileobj, mask)
        finally:
            print('close')
            self.server_close()


server = BaseServer('127.0.0.1', 5002)
server.serve_forever()
```
代码重点剖析请看下文的sel.register方法和accept回调方法。

---

【sel.register方法】


sel.register方法有三个参数，三个参数的功能如下：
```
sel.register(sock, selectors.EVENT_READ, self.accept)
```
- 第一个参数sock：是要绑定事件的socket；
- 第二个参数selectors.EVENT_READ：是事件类型；
- 第三个参数self.accept：是回调方法 （key.data）；

客户端连接到服务器上时，EVENT_READ事件触发，self.accept方法被调用。
【示例】：accept回调方法的应用
```
  def accept(self, sock, mask):
        sel = self.selector

        conn, addr = sock.accept()  # Should be ready
        conn.setblocking(False)
        sel.register(conn, selectors.EVENT_READ | selectors.EVENT_WRITE, self.process)
```


【解释】accept方法在有新客户端连上来时被触发，所做的处理是获得和对端通信的socket对象conn，给conn绑定**selectors.EVENT_READ | selectors.EVENT_WRITE**事件，其中EVENT_READ事件在**底层读缓冲区收到对端发过来数据时触发**，而EVENT_WRITE事件在底层写缓冲区不满时，不断被触发；可以在相应的事件处理函数中，向对端发送数据。
