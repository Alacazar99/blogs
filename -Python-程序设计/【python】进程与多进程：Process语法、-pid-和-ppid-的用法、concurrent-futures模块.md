
#####关于python 进程与多进程
概念：
进程即正在执行的一个过程，进程是对正在云的程序的一个抽象。
进程的概念起源与操作系统，是操作系统最核心的概念，也是操作系统提供的最古老也是最重要的抽象概念之一，操作系统的其他所有内容都是围绕进程的概念展开的；

#####【进程】与【线程】

- 【进程】进程指正在运行的程序。确切的来说，**当一个程序进入内存运行，即变成一个进程**
进程是处于运行过程中的程序，并且具有一定独立功能。

- 【线程】线程是进程中的一个执行单元，负责当前进程中程序的执行，一个进程中至少有一个线程。
一个进程中是可以有多个线程的，这个应用程序也可以称之为多线程程序。

---
【理论知识拓展】：
- 操作系统的作用：

> 1：隐藏丑陋复杂的硬件接口，提供良好的抽象接口;
 2：管理、调度进程，并且将多个进程对硬件的竞争变得有序;

- 多道技术:
> 产生背景：针对单核，实现并发
【强调】 现在的主机一般是多核，那么每个核都会利用多道技术；

【重点】有4个cpu，运行于cpu1的某个程序遇到io阻塞，会等到io结束再重新调度，会被调度到4个cpu中的任意一个，具体由操作系统调度算法决定。

【空间上的复用：】如内存中同时有多道程序；

【时间上的复用：】复用一个cpu的时间片；

>【 强调：遇到 io 切，占用 cpu 时间过长也切，核心在于切之前将进程的状态保存下来，这样才能保证下次切换回来时，能基于上次切走的位置继续运行】
---
【进程与程序的区别】
程序仅仅只是一堆代码而已，进程指的是程序的运行过程。

---
【并发与并行】
>无论是并行还是并发，在用户看来都是‘同时’运行的，不管是进程还是线程，都只是一个任务而已；真是干活的是cpu，cpu来做这些任务，**而一个cpu同一时刻只能执行一个任务。**

- 并发：是伪并行，即看起来是同时运行，单个cpu+多道技术就可以实现并发，（并行也属于并发）

- 并行：同时运行，只有具备**多个cpu**才能实现并行；


![看图看图，关于进程与线程](https://upload-images.jianshu.io/upload_images/17476267-d585bd92727a1e57.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---
【同步与异步】

- 同步执行：一个进程在执行某个任务时，另外一个进程必须等待其执行完毕，才能继续执行。（打电话；视频聊天）

- 异步执行：一个进程在执行某个任务时，另外一个进程无需等待其执行完毕，就可以继续执行，当有消息返回时，系统会通知进行处理，这样可以提高执行效率。（发短息；）

---
【进入主题】

#####进程的创建

但凡是硬件，都需要有操作系统去管理，只要有操作系统，就有进程的概念，就需要有创建进程的方式，一些操作系统只为一个应用程序设计。而对于通用系统（跑很多应用程序），需要有系统运行过程中创建或撤销进程的能力，主要分为4中形式创建新的进程

- 系统初始化（查看进程linux中用ps命令，windows中用任务管理器，前台进程负责与用户交互，后台运行的进程与用户无关，运行在后台并且只在需要时才唤醒的进程，称为守护进程，如电子邮件、web页面、新闻、打印）

- 一个进程在运行过程中开启了子进程

- 用户的交互式请求，而创建一个新进程

- 一个批处理作业的初始化（只在大型机的批处理系统中应用）

【新进程的创建都是由一个已经存在的进程执行了一个用于创建进程的系统调用而创建的】
```
import multiprocessing
import time
def run(name):
    time.sleep(2)
    print(name, " 进程启动")

if __name__ == '__main__':
    mp = multiprocessing.Process(target=run, args=("LJ",))
    mp.start()
    mp.join() # 等待进程执行完毕
```
【解释】创建子进程时，只需要传入一个执行函数和函数的参数，创建一个Process实例，用start()方法启动。join()方法可以等待子进程结束后再继续往下运行，通常用于进程间的同步。

---

【关于python 进程与多进程】的知识写的超级详细：https://www.cnblogs.com/fanglingen/p/7447959.html

---
#### 关于Process语法

Process语法结构如下：

**Process([group [, target [, name [, args [, kwargs]]]]])**  由该类实例化得到的对象，表示一个子进程中的任务（尚未启动）

（1） 需要使用关键字的方式来指定参数；

（2） args指定的为传给target函数的位置参数，是一个元组形式，必须有逗号；
```
>group：指定进程组，大多数情况下用不到

>target：如果传递了函数的引用，可以任务这个子进程就执行这里的代码；

>args：给target指定的函数传递的参数，以元组的方式传递；

>kwargs：给target指定的函数传递命名参数；

>name：给进程设定一个名字，可以不设定；
```
---
Process创建的实例对象的常用方法：
```
 .start()：启动子进程实例（创建子进程）;

.run():进程启动时运行的方法，正是它去调用target指定的函数，我们自定义类的类中一定要实现该方法  ;

.is_alive()：判断进程子进程是否还在活着;

p.join([timeout]):主线程等待 p 终止（强调：是主线程处于等的状态，而p是处于运行的状态）;

timeout是可选的超时时间，需要强调的是，p.join只能join住start开启的进程，而不能join住run开启的进程;
```
Process创建的实例对象的常用属性：
```
.daemon：默认值为False，如果设为True，代表p为后台运行的守护进程，当p的父进程终止时，p也随之终止，并且设定为True后，p不能创建自己的新进程，必须在p.start()之前设置
name：当前进程的别名，默认为Process-N，N为从1开始递增的整数;
pid：当前进程的pid（进程号）;
```
【补充】可以同时启动多个进程，进程内部可以再起线程。例子如下：
```
import multiprocessing
import time
import threading


def thread_run():
    print(threading.get_ident())


def run(name):
    time.sleep(2)
    print(name, " 进程启动")
    # 在进程中再启动线程
    t = threading.Thread(target=thread_run, )
    t.start()


if __name__ == '__main__':
    # 生成多个进程
    for i in range(10):
        p = multiprocessing.Process(target=run, args=('hello %s' % i,))
        p.start()
```
【注意】：`在windows中process()必须放到 #  if __name__ == '__main__'：下`

---

#### 关于进程中 pid 和 ppid 的用法
```
from multiprocessing import Process
import os
import time

def run_proc():
    """子进程要执行的代码"""
    print('子进程运行中，pid=%d...' % os.getpid())  # os.getpid获取当前进程的进程号
    print('子进程将要结束...')
    print('我的爸爸是，ppid=%d...' % os.getppid())  # os.getpid获取当前进程的进程号

if __name__ == '__main__':
    print('父进程pid: %d' % os.getpid())  # os.getpid获取当前进程的进程号
    p = Process(target=run_proc)
    p.start()
```
【进程中间，全局变量不共享】进程间内存是独立的
```
import random
from multiprocessing import Process
import os
import time

nums = [11, 22]

def work1():
    """子进程要执行的代码"""
    print("in process1 pid=%d ,nums=%s" % (os.getpid(), nums))
    for i in 'baidu':
        nums.append(i)
        time.sleep(random.random())
        print("in process1 pid=%d ,nums=%s" % (os.getpid(), nums))

def work2():
    """子进程要执行的代码"""
    print("in process2 pid=%d ,nums=%s" % (os.getpid(), nums))
    for i in 'neuedu':
        nums.append(i)
        time.sleep(random.random())
        print("in process2 pid=%d ,nums=%s" % (os.getpid(), nums))

if __name__ == '__main__':
    p1 = Process(target=work1)
    p1.start()


    p2 = Process(target=work2)
    p2.start()

    p1.join()
    p2.join()
```

输出：
```
in process1 pid=7536 ,nums=[11, 22]
in process2 pid=7180 ,nums=[11, 22]
in process2 pid=7180 ,nums=[11, 22, 'n']
in process2 pid=7180 ,nums=[11, 22, 'n', 'e']
in process1 pid=7536 ,nums=[11, 22, 'b']
in process2 pid=7180 ,nums=[11, 22, 'n', 'e', 'u']
in process1 pid=7536 ,nums=[11, 22, 'b', 'a']
in process1 pid=7536 ,nums=[11, 22, 'b', 'a', 'i']
in process1 pid=7536 ,nums=[11, 22, 'b', 'a', 'i', 'd']
in process2 pid=7180 ,nums=[11, 22, 'n', 'e', 'u', 'e']
in process1 pid=7536 ,nums=[11, 22, 'b', 'a', 'i', 'd', 'u']
in process2 pid=7180 ,nums=[11, 22, 'n', 'e', 'u', 'e', 'd']
in process2 pid=7180 ,nums=[11, 22, 'n', 'e', 'u', 'e', 'd', 'u']
```

---

##### multiprocessing模块介绍

python中多线程无法利用多核优势，如果想要充分地使用多核cpu的资源，可以使用`os.cpu_count()`
在python中大部分情况需要使用多进程，python提供了multiprocessing

multiprocessing模块用来开启子进程，并在子进程中执行我们定制的任务（比如函数），该模块与多线程模块threading的编程接口类似。

【反复强调】与线程不同，进程没有任何共享状态，进程修改的数据，改动仅限与该进程内。

---


#### 关于concurrent.futures模块

【背景】Python标准库为我们提供了threading和multiprocessing模块编写相应的异步多线程/多进程代码。
从Python3.2开始，标准库为我们提供了concurrent.futures模块，它提供了ThreadPoolExecutor和ProcessPoolExecutor两个类

【ThreadPoolExecutor和ProcessPoolExecutor继承了Executor，分别被用来创建**线程池和进程池**的代码。】
Executor实现了对threading和multiprocessing的更高级的抽象，对编写线程池/进程池提供了直接的支持。 

##### 关于concurrent.futures的基础模块executor和future

**Executor**是一个抽象类，它不能被直接使用。但是它提供的两个子类ThreadPoolExecutor和ProcessPoolExecutor却是非常有用，顾名思义两者分别被用来创建线程池和进程池的代码。我们可以将相应的tasks直接放入线程池/进程池，不需要维护Queue来操心死锁的问题，线程池/进程池会自动帮我们调度。

**Future**这个概念，你可以把它理解为一个在未来完成的操作，这是异步编程的基础，传统编程模式下，比如我们`操作queue.get的时候，在等待返回结果之前会产生阻塞`，cpu不能让出来做其他事情，而Future的引入帮助我们在等待的这段时间可以完成其他的操作。
