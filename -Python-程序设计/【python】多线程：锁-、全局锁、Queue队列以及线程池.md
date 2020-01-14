---

####关于如何使用锁 

【介绍】关于如何加锁，获取钥匙，释放锁。
- **lock = threading.Lock()**：生成锁对象，全局唯一；
- **lock.acquire()**：获取锁。未获取到会阻塞程序，直到获取到锁才会往下执行；
- **lock.release()**：释放锁，归回后，其他人也可以调用；


【注意事项】：**lock.acquire() 和 lock.release()**必须成对出现，否则就有可能造成死锁。

为了规避这个问题，可以使用使用上下文管理器来加锁。如下所示：
```
import threading
lock = threading.Lock()
with lock:
    # 这里写想要实现的代码
    pass
```
【解释】with 语句会在这个代码块执行前自动获取锁，在执行结束后自动释放锁

---

#### 为何要“上”锁 ？

```
import threading
import time

g_num = 0

def test1(num):
    global g_num
    for i in range(num):
        mutex.acquire()  # 上锁
        g_num += 1
        mutex.release()  # 解锁

    print("---test1---g_num=%d"%g_num)

def test2(num):
    global g_num
    for i in range(num):
        mutex.acquire()  # 上锁
        g_num += 1
        mutex.release()  # 解锁

    print("---test2---g_num=%d"%g_num)

# 创建一个互斥锁
# 默认是未上锁的状态
mutex = threading.Lock()

# 创建2个线程，让他们各自对g_num加1000000次
p1 = threading.Thread(target=test1, args=(1000000,))
p1.start()

p2 = threading.Thread(target=test2, args=(1000000,))
p2.start()

# 等待计算完成
while len(threading.enumerate()) != 1:
    time.sleep(1)

print("2个线程对同一个全局变量操作之后的最终结果是:%s" % g_num)
```
输出：
```
---test1---g_num=1909909
---test2---g_num=2000000
2个线程对同一个全局变量操作之后的最终结果是:2000000
```
【总结】入互斥锁后，其结果与预期相符。

---

#### 关于死锁

【解释】在线程间共享多个资源的时候，如果两个线程分别占有一部分资源并且同时等待对方的资源，就会造成死锁。

来看个实例：
```
import threading
import time

class MyThread1(threading.Thread):
    def run(self):
        # 对mutexA上锁
        mutexA.acquire()

        # mutexA上锁后，延时1秒，等待另外那个线程 把mutexB上锁
        print(self.name+'----do1---up----')
        time.sleep(1)

        # 此时会堵塞，因为这个mutexB已经被另外的线程抢先上锁了
        mutexB.acquire()
        print(self.name+'----do1---down----')
        mutexB.release()

        # 对mutexA解锁
        mutexA.release()

class MyThread2(threading.Thread):
    def run(self):
        # 对mutexB上锁
        mutexB.acquire()

        # mutexB上锁后，延时1秒，等待另外那个线程 把mutexA上锁
        print(self.name+'----do2---up----')
        time.sleep(1)

        # 此时会堵塞，因为这个mutexA已经被另外的线程抢先上锁了
        mutexA.acquire()
        print(self.name+'----do2---down----')
        mutexA.release()

        # 对mutexB解锁
        mutexB.release()

mutexA = threading.Lock()
mutexB = threading.Lock()

if __name__ == '__main__':
    t1 = MyThread1()
    t2 = MyThread2()
    t1.start()
    t2.start()
```
【重点】**标准的锁对象(threading.Lock)并不关心当前是哪个线程占有了该锁；如果该锁已经被占有了，那么任何其它尝试获取该锁的线程都会被阻塞，包括已经占有该锁的线程也会被阻塞。**

【 获取锁和释放锁的语句也可以用Python的with来实现】

【知识提升】如有某个线程在两个函数调用之间修改了共享资源，那么我们最终会得到不一致的数据。【最直接的解决办法】是在这个函数中也使用lock。然而，这是不可行的。`里面的两个访问函数将会阻塞，因为外层语句已经占有了该锁。`

---
### 饱受争议的GIL（全局锁）

#####什么是GIL呢？
>【解释】任何Python线程执行前，必须先获得GIL锁，然后，每执行100条字节码，解释器就自动释放GIL锁，让别的线程有机会执行。这个GIL全局锁实际上把所有线程的执行代码都给上了锁，所以，多线程在Python中只能交替执行，`即使100个线程跑在100核CPU上，也只能用到1个核。`

#####GIL执行过程

- 1). 设置一个GIL；
- 2). 切换线程去准备执行任务(Runnale就绪状态)；
- 3). 运行；
- 4). 可能出现的状态:
   - 线程任务执行结束;
   - time.sleep()
   - 需要获取其他的信息才能继续执行(eg: 读取文件, 需要从网络下载html网页)

5). 将线程设置为睡眠状态;
6). 解GIL的锁；

【重点】python解释器中任意时刻都只有一个线程在执行;

 I/O密集型(input, output)：
 计算密集型(cpu一直占用)：

##### 那么如何避免受到GIL的影响？

- 使用多进程代替多线程。
- 更换Python解释器，不使用CPython

---
####Queue队列
谈及多线程，就不得不说Queue队列，这是从一个线程向另一个线程发送数据最安全的方式。创建一个被多个线程共享的 Queue 对象，这些线程通过使用put() 和 get() 操作来向队列中添加或者删除元素。
 
关于Queue队列的重要的函数
```
from queue import Queue
# maxsize默认为0，不受限
# 一旦>0，而消息数又达到限制，q.put()也将阻塞
q = Queue(maxsize=0)

# 阻塞程序，等待队列消息。
q.get()

# 获取消息，设置超时时间
q.get(timeout=5.0)

# 发送消息
q.put()

# 等待所有的消息都被消费完
q.join()

# 以下三个方法，知道就好，代码中不要使用

# 查询当前队列的消息个数
q.qsize()

# 队列消息是否都被消费完，True/False
q.empty()

# 检测队列里消息是否已满
q.full()
```
---
####生产者-消费者模型（继承实现）

#####什么是生产者-消费者模型?


某个模块专门负责生产+数据， 可以认为是生产者;
另外一个模块负责对生产的数据进行处理的， 可以认为是消费者.
在生产者和消费者之间加个缓冲区(队列queue实现), 可以认为是商店。
>【生产者】 ===》【缓冲区】 ===》【 消费者】

![生产者与消费者概念图](https://upload-images.jianshu.io/upload_images/17476267-05163a6f15460e8d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######生产者-消费者模型的优点

- 1). 生产者和消费者的依赖关系减少，逻辑联系少了，简化代码;
- 2). 生产者和消费者是两个独立的个体， 可并发执行;

---

#### 关于线程池

在Python3中，创建线程池是通过concurrent.futures函数库中的ThreadPoolExecutor类来实现的。

future对象:在未来的某一时刻完成操作的对象. submit方法可以返回一个future对象.

先看实例：简单线程池实现
```
#线程执行的函数
def add(n1,n2):
    v = n1 + n2
    print('add :', v , ', tid:',threading.currentThread().ident)
    time.sleep(n1)
    return v
#通过submit把需要执行的函数扔进线程池中.
#submit 直接返回一个future对象
ex = ThreadPoolExecutor(max_workers=3)      #制定最多运行N个线程
f1 = ex.submit(add,2,3)
f2 = ex.submit(add,2,2)
print('main thread running')
print(f1.done())                            #done 看看任务结束了没
print(f1.result())                          #获取结果 ,阻塞方法
```
简单线程池实现
```
import Queue
import threading
import time

'''
这个简单的例子的想法是通过：
1、利用Queue特性，在Queue里创建多个线程对象
2、那我执行代码的时候，去queue里去拿线程！
如果线程池里有可用的，直接拿。
如果线程池里没有可用，那就等。
3、线程执行完毕，归还给线程池
'''

class ThreadPool(object): #创建线程池类
    def __init__(self,max_thread=20):#构造方法，设置最大的线程数为20
        self.queue = Queue.Queue(max_thread) #创建一个队列
        for i in xrange(max_thread):#循环把线程对象加入到队列中
            self.queue.put(threading.Thread)
            #把线程的类名放进去，执行完这个Queue

    def get_thread(self):#定义方法从队列里获取线程
        return self.queue.get()

    def add_thread(self):#定义方法在队列里添加线程
        self.queue.put(threading.Thread)

pool = ThreadPool(10)

def func(arg,p):
    print arg
    time.sleep(2)
    p.add_thread() #当前线程执行完了，我在队列里加一个线程！

for i in xrange(300):
    thread = pool.get_thread() #线程池10个线程，每一次循环拿走一个！默认queue.get()，如果队列里没有数据就会等待。
    t = thread(target=func,args=(i,pool))
    t.start()


'''
self.queue.put(threading.Thread) 添加的是类不是对象，在内存中如果相同的类只占一份内存空间
并且如果这里存储的是对象的话每次都的新增都得在内存中开辟一段内存空间

还有如果是对象的话：下面的这个语句就不能这么调用了！
for i in xrange(300):
    thread = pool.get_thread()
    t = thread(target=func,args=(i,pool))
    t.start()
    通过查看源码可以知道，在thread的构造函数中：self.__args = args  self.__target = target  都是私有字段那么调用就应该这么写

for i in xrange(300):
    ret = pool.get_thread()
    ret._Thread__target = func
    ret._Thread__args = (i,pool)
    ret.start()
```
---

【map 方法】
返回值和提交的序列是一致的. 即`是有序的`。

```
#下面是map 方法的简单使用.  
#注意:map 返回是一个生成器 ,并且是有序的
URLS = ['http://www.baidu.com', 'http://www.qq.com', 'http://www.sina.com.cn']
def get_html(url):
    print('thread id:',threading.currentThread().ident,' 访问了:',url)
    #这里使用了requests 模块
    return requests.get(url)            
ex = ThreadPoolExecutor(max_workers=3)
#内部迭代中, 每个url 开启一个线程
res_iter = ex.map(get_html,URLS)        
for res in res_iter:
    #此时将阻塞 , 直到线程完成或异常                    
    print('url:%s ,len: %d'%(res.url,len(res.text)))
```
---
【 as_completed】
用于解决submit什么时候完成，避免一次次调用future.done 或者是使用 future.result 。

` concurrent.futures.as_completed(fs, timeout=None) `：返回一个生成器,在迭代过程中会阻塞。

【关联】map方法返回是有序的, as_completed 是那个线程先完成/失败 就返回。
【举个栗子】
```
#as_completed 返回一个生成器，用于迭代， 一旦一个线程完成(或失败) 就返回
URLS = ['http://www.baidu.com', 'http://www.qq.com', 'http://www.sina.com.cn']
def get_html(url):
    time.sleep(1)
    print('thread id:',threading.currentThread().ident,' 访问了:',url)
    return requests.get(url)            #这里使用了requests 模块
ex = ThreadPoolExecutor(max_workers=3)   #最多3个线程
future_tasks = [ex.submit(get_html,url) for url in URLS]    #创建3个future对象
for future in as_completed(future_tasks):       #迭代生成器
    try:
        resp = future.result()
    except Exception as e:
        print('%s'%e)
    else:
        print('%s has %d bytes!'%(resp.url, len(resp.text)))
```
输出：
```
"""
thread id: 5160  访问了: http://www.baidu.com
thread id: 7752  访问了: http://www.sina.com.cn
thread id: 5928  访问了: http://www.qq.com
http://www.qq.com/ has 240668 bytes!
http://www.baidu.com/ has 2381 bytes!
https://www.sina.com.cn/ has 577244 bytes!
"""
```
---
【强调】关于回调函数add_done_callback(fn) 

回调函数是在调用线程完成后再调用的,在同一个线程中.
```
import os,sys,time,requests,threading
from concurrent import futures


URLS = [
        'http://baidu.com',
        'http://www.qq.com',
        'http://www.sina.com.cn'
        ]

def load_url(url):
    print('tid:',threading.currentThread().ident,',url:',url)
    with requests.get(url) as resp:
        return resp.content
def call_back(obj):
    print('->>>>>>>>>call_back , tid:',threading.currentThread().ident, ',obj:',obj)

with futures.ThreadPoolExecutor(max_workers=3) as ex:
    # mp = {ex.submit(load_url,url) : url for url in URLS}
    mp = dict()
    for url in URLS:
        f = ex.submit(load_url,url)
        mp[f] = url
        f.add_done_callback(call_back)
    for f in futures.as_completed(mp):
        url = mp[f]
        try:
            data = f.result()
        except Exception as exc:
            print(exc, ',url:',url)
        else:
            print('url:', url, ',len:',len(data),',data[:20]:',data[:20])
"""
tid: 7128 ,url: http://baidu.com
tid: 7892 ,url: http://www.qq.com
tid: 3712 ,url: http://www.sina.com.cn
->>>>>>>>>call_back , tid: 7892 ,obj: <Future at 0x2dd64b0 state=finished returned bytes>
url: http://www.qq.com ,len: 251215 ,data[:20]: b'<!DOCTYPE html>\n<htm'
->>>>>>>>>call_back , tid: 3712 ,obj: <Future at 0x2de07b0 state=finished returned bytes>
url: http://www.sina.com.cn ,len: 577333 ,data[:20]: b'<!DOCTYPE html>\n<!--'
->>>>>>>>>call_back , tid: 7128 ,obj: <Future at 0x2d533d0 state=finished returned bytes>
url: http://baidu.com ,len: 81 ,data[:20]: b'<html>\n<meta http-eq'
"""
```
---
最后的最后，来两个栗子总结一下关于多线程的应用。
- 【多线程实现文件复制】
```
import concurrent.futures as fu
import os

ex_pools = fu.ThreadPoolExecutor(max_workers = 3)

def copy(org_file,dest_file):
    """
    复制文件
    """
    print("开始从%s复制文件到%s" % (org_file,dest_file))
    with open(org_file,'rb+') as f:
        content = f.read()

    with open(dest_file,'wb+') as f:
        f.write(content)
    print("从%s复制文件到%s，完成！" % (org_file, dest_file))


def copy_dir(base,dest):
    """
    复制目录
    """
    if not os.path.exists(dest):
        print("创建文件夹：%s" %dest)
        os.mkdir(dest)

    org_dir_files = os.listdir(base)
    for file_name in org_dir_files:
        file = os.path.join(base,file_name)
        dest_file = os.path.join(dest,file_name)

        if os.path.isfile(file):
            ex_pools.submit(copy,file,dest_file)

        if os.path.isdir(file):
            ex_pools.submit(copy_dir, file, dest_file)

# 要复制的目标文件路径
base = r"C:\Users\42072\Desktop\python"
# 复制到该文件路径
dest = r"C:\Users\42072\Desktop\python123"
copy_dir(base,dest)
```
- 【实现网络上图片的下载】
(初衷想下音乐的，不过好像都收费了，还是尊重版权吧)

```

import requests
import os
import random
import concurrent.futures as futures

def download_img(url):
    resp = requests.get(url)
    filename = os.path.split(url)[1] # 获取文件名
    with open(filename,'wb+') as f:
        f.write(resp.content)
    num = random.randint(2,5)
    print(filename + "generate:",num)
    time.sleep(num)
    return filename

urls = ["http://pic27.nipic.com/20130320/8952533_092547846000_2.jpg",
        "http://pic19.nipic.com/20120212/9337475_104548381000_2.jpg",]

ex = futures.ThreadPoolExecutor(max_workers = 3)
res_iter = ex.map(download_img,urls)
type(res_iter)
for res in res_iter:
    print(res)

def cf(rs):
    print(rs.result())

# [ex.submit(download_img,url).add_done_callback(cf) for url in urls]
# for future in futures.as_completed(fu_tasks):
for url in urls:
    f = ex.submit(download_img,url)
    f.add_done_callback(cf)
```
【输出】：
```
9337475_104548381000_2.jpggenerate: 3
8952533_092547846000_2.jpggenerate: 3
8952533_092547846000_2.jpg
9337475_104548381000_2.jpg
9337475_104548381000_2.jpggenerate: 3
8952533_092547846000_2.jpggenerate: 4
9337475_104548381000_2.jpg
8952533_092547846000_2.jpg
```
