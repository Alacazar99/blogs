##### 关于python 进程间通信
Process之间有时需要通信，操作系统提供了很多机制来实现进程间的通信。

进程间通讯有多种方式，包括信号，管道，消息队列，信号量，共享内存，socket等

#####Queue的使用
可以使用multiprocessing模块的Queue实现多进程之间的数据传递，Queue本身是一个消息列队程序
【小实例】
```
from multiprocessing import Queue
q=Queue(3) #初始化一个Queue对象，最多可接收三条put消息
q.put("消息1") 
q.put("消息2")
print(q.full())  #False
q.put("消息3")
print(q.full()) #True

#因为消息列队已满下面的try都会抛出异常，第一个try会等待2秒后再抛出异常，第二个Try会立刻抛出异常
try:
    q.put("消息4",True,2)
except:
    print("消息列队已满，现有消息数量:%s"%q.qsize())

try:
    q.put_nowait("消息4")
except:
    print("消息列队已满，现有消息数量:%s"%q.qsize())

#推荐的方式，先判断消息列队是否已满，再写入
if not q.full():
    q.put_nowait("消息4")

#读取消息时，先判断消息列队是否为空，再读取
if not q.empty():
    for i in range(q.qsize()):
        print(q.get_nowait())
```
输出：
```
False
True
消息列队已满，现有消息数量:3
消息列队已满，现有消息数量:3
消息1
消息2
消息3
```
【解释】：初始化Queue()对象时（例如：q=Queue()），若括号中没有指定最大可接收的消息数量，或数量为负值，那么就代表可接受的消息数量没有上限（直到内存的尽头）；

- Queue.qsize()：返回当前队列包含的消息数量；

- Queue.empty()：如果队列为空，返回True，反之False ；

- Queue.full()：如果队列满了，返回True,反之False；

- Queue.get([block[, timeout]])：获取队列中的一条消息，然后将其从列队中移除，block默认值为True；
>【解释】 如果block使用默认值，且没有设置`imeout（单位秒）`消息列队如果为空，此时程序将被`阻塞（停在读取状态）`，直到从消息列队读到消息为止;
如果设置了timeout，则会等待timeout秒，若还没读取到任何消息，则抛出`"Queue.Empty"`异常；

> 【补充】如果block值为False，消息列队如果为空，则会立刻抛出`"Queue.Empty"`异常；

- **Queue.get_nowait()**：相当**Queue.get(False)**；

- **Queue.put(item,[block[, timeout]])**：将item消息写入队列，block默认值为True；
> 【补充】如果block使用默认值，且没有设置timeout（单位秒），消息列队如果已经没有空间可写入，此时程序将被阻塞（停在写入状态），直到从消息列队腾出空间为止;
如果设置了timeout，则会等待**timeout**秒，若还没空间，则抛出`"Queue.Full"`异常;；

> 如果block值为False，消息列队如果没有空间可写入，则会立刻抛出"Queue.Full"异常；

- **Queue.put_nowait(item)**：相当**Queue.put(item, False)**；
【看个小实例】
```
from multiprocessing import Process, Queue
import os, time, random

# 写数据进程执行的代码:
def write(q):
    for value in ['A', 'B', 'C']:
        print('Put %s to queue...' % value)
        q.put(value)
        time.sleep(random.random())

# 读数据进程执行的代码:
def read(q):
    while True:
        if not q.empty():
            value = q.get(True)
            print('Get %s from queue.' % value)
            time.sleep(random.random())
        else:
            break

if __name__=='__main__':
    # 父进程创建Queue，并传给各个子进程：
    q = Queue()
    pw = Process(target=write, args=(q,))
    pr = Process(target=read, args=(q,))
    # 启动子进程pw，写入:
    pw.start()    
    # 等待pw结束:
    pw.join()
    # 启动子进程pr，读取:
    pr.start()
    pr.join()

    print('')
    print('所有数据都写入并且读完')
```














