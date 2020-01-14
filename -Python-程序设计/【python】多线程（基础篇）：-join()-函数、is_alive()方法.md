#### 线程

##### 什么是线程

（1）线程是操作系统能够进行运算调度的最小单位。
（2）它被包含在进程之中，是进程中的实际运作单位。
（3）一条线程指的是进程中一个单一顺序的控制流，一个进程中可以并发多个线程，每条线程并行执行不同的任务。
（4）一个线程是一个execution context（执行上下文），即一个cpu执行时所需要的一串指令。

【重点】一个程序运行后至少有一个进程，一个进程中可以包含多个线程；

---

##### 线程的工作方式

CPU会给你一个在同一时间能够做多个运算的幻觉，实际上它在每个运算上只花了极少的时间，本质上CPU同一时刻只干了一件事。它能这样做就是因为它有每个运算的execution context。就像你能够和你朋友共享同一本书一样，多任务也能共享同一块CPU。

---

##### 启动多个线程（函数实现）

Python提供了一个内置模块 threading.Thread，可以很方便地让我们创建多线程。 threading.Thread() 一般接收两个参数：

- 线程函数名：要放置线程让其后台执行的函数，由我们自已定义，注意不要加()；

- 线程函数的参数：线程函数名所需的参数，以元组的形式传入。若不需要参数，可以不指定。

【举个例子】：
```
import threading
import time
def run(n):
    print("task ",n )
    time.sleep(2)

# run("t1")
# run("t2")
t1 = threading.Thread(target=run,args=("t1",))#生成一个线程实例
t2 = threading.Thread(target=run,args=("t2",))
t1.start()
t2.start()

【输出】：
task  t1
task  t2

```
【解释】由于每个方法都有单独的进程，会同时开始执行，上面代码会同时输出


---
###### 启动多个线程（类实现）
相比较函数而言，使用类创建线程，会比较麻烦一点。 首先，我们要自定义一个类，对于这个类有两点要求：

- 必须继承 threading.Thread 这个父类；

- 必须覆写 run 方法。
【看个实例】
```
# 类方法
class MYThread(threading.Thread):

    def __init__(self,n):
        super().__init__()
        self.n = n

    def run(self):
        print("task",self.n)
        time.sleep(2)
        print("{} finished\n".format(self.n))

t1 = MYThread('t1')
t2 = MYThread('t2')


t = time.time()
t1.start()
t2.start()

# 调用者会等待该线程结束后，再执行；

t2.join()

t1.join()

t1 = time.time()

print('main finished...')
print("线程所耗时间为：",t1 - t)
```
【输出】
```
task t1
task t2
t1 finished
t2 finished


main finished...
线程所耗时间为： 2.0015668869018555
```
##### 获取线程的执行结果
- f1.result() 
- map()
-  as_completed 
- wait
- add_done_callback
---
【关于 join() 函数】

```
import threading
import time
def run(n):
    print("task ",n )
    time.sleep(2)
    print("task done",n)

start_time = time.time()
for i in range(12):
    t = threading.Thread(target=run,args=("t-%s" %i ,))
    t.start()

print("----------all threads has finished...")
print("cost:",time.time() - start_time)
```
输出：
```
task  t-0
task  t-1
task  t-2
task  t-3
task  t-4
task  t-5
task  t-6
task  t-7
task  t-8
task  t-9
task  t-10
task  t-11----------all threads has finished...
cost: 
0.001996278762817383
task donetask done t-2
 t-0
task done t-4task done task done t-3t-1

task done t-7
task done t-9task done 
task done task done t-8
t-5t-6

task donetask done  t-11
t-10
```

---

我们知道线程有 ` 就绪、阻塞、运行 `三种基本状态。 
1. 就绪状态是指线程具备运行的所有条件，逻辑上可以运行，在等待处理机； 

2. 运行状态是指线程占有处理机正在运行； 
3. 阻塞状态是指线程在等待一个事件（如某个信号量），逻辑上不可执行。

![线程状态转换](https://upload-images.jianshu.io/upload_images/17476267-20bb6782a45f6f70.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---

关于【多线程】的小程序
```
from threading import Thread
import os, time

#计算密集型任务
def work():
    res = 0
    for i in range(100000000):
        res *= i

if __name__ == "__main__":
    l = []
    print("本计算机是",os.cpu_count(),"核 CPU")
    start = time.time()
    for i in range(4):
        p = Thread(target=work)  # 多进程
        l.append(p)
        p.start()
    for p in l:
        p.join()
    stop = time.time()
    print("本计算机计算密集型任务，多线程耗时 %s" % (stop - start))
```
输出：
```
本计算机是 8 核 CPU
本计算机计算密集型任务，多线程耗时 25.76108407974243
```
---

【关于is_alive()方法】：
可以用来判断一个线程是否结束。

```
import threading
import time


def run(n):
    print("task ", n)
    time.sleep(2)
    print("task done", n)


start_time = time.time()
t_objs = []  # 存线程实例
for i in range(5):
    t = threading.Thread(target=run, args=("t-%s" % i,))
    t.start()
    t_objs.append(t)  # 为了不阻塞后面线程的启动，不在这里join，先放到一个列表里

for t in t_objs:
    print(t.is_alive())

for t in t_objs:  # 循环线程实例列表，等待所有线程执行完毕
    t.join()

for t in t_objs:
    print(t.is_alive())

print("----------all threads has finished...")
print("cost:", time.time() - start_time)
```
输出：
```
task  t-0
task  t-1
task  t-2
task  t-3
task True
True
True
True
True
 t-4
task done task done t-1
t-0
task done t-4task done 
task done t-3t-2

False
False
False
False
False
----------all threads has finished...
cost: 2.0031514167785645
```


【续】：[【python】多线程（高级篇）：锁 、GIL（全局锁）、Queue队列以及线程池]([https://www.jianshu.com/p/04af68ab2c9b](https://www.jianshu.com/p/04af68ab2c9b)
)：https://www.jianshu.com/p/04af68ab2c9b
