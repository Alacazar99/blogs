#####asyncio协程如何实现并发
###### 1、先介绍一下 并发和并行以及高并发
【并发】<sup>（Concurrent）</sup>：同时拥有**两个或多个**线程，如果程序在**单核**处理器上运行，多个线程将**交替地换入或者换出**内存，这些线程是**同时存在**的，每个线程都处于执行过程中的某个状态；如果运行在多核处理器上，此时，程序中每个线程都将分配到一个处理器核上，然后能够同时运行。

在[操作系统](https://baike.baidu.com/item/%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F/192)中，指一个时间段中有几个程序都处于已启动运行到运行完毕之间，且这几个程序都是在同一个[处理机](https://baike.baidu.com/item/%E5%A4%84%E7%90%86%E6%9C%BA/128842)上运行，但任一个时刻点上只有一个程序在处理机上运行。
-------【百度百科】

【总结】：并发**就是多个线程操作相同的处理机中的资源，保证线程安全的同时，对资源进行合理的利用**。


【并行】<sup>（High Concurrency）</sup>：当系统有**一个以上CPU**时,则线程的操作有可能非并发。当一个CPU执行一个线程时，另一个CPU可以执行另一个线程，**两个线程互不抢占CPU资源**，可以同时进行。(宏观上，时间有重叠)。

---
二者的区别：
- 【并行】是指两个或者多个事件在**同一时刻发生**；
- 【并发】是指两个或多个事件在**同一时间间隔**内发生；
在[多道程序](https://baike.baidu.com/item/%E5%A4%9A%E9%81%93%E7%A8%8B%E5%BA%8F)环境下，[并发性](https://baike.baidu.com/item/%E5%B9%B6%E5%8F%91%E6%80%A7)是指在一段时间内宏观上有多个程序在同时运行，但在[单处理机系统](https://baike.baidu.com/item/%E5%8D%95%E5%A4%84%E7%90%86%E6%9C%BA%E7%B3%BB%E7%BB%9F)中，每一时刻却仅能有一道程序执行，故微观上这些程序只能是分时地交替执行。

---

【高并发】<sup>（High Concurrency）</sup>：是现在互联网设计系统中需要考虑的一个重要因素之一，通常来说，就是通过严谨的设计来保证系统能够同时并行处理很多的请求。这就是大家常说的「 高并发 」。就是说系统能也够在某一时间段内提供很多请求，但是不会影响系统的性能。如果想设计出高可用和高性能的系统，就应该从很多的方面来考虑，例如应该从*硬件、软件、编程语言的选择、网络方面的考虑、系统的整体架构、数据结构、算法的优化、数据库的优化`等等多方面。

asyncio想要实现并发，就需要多个协程来完成任务，每当有任务阻塞的时候就await，然后其他协程继续工作，这需要创建多个协程的列表，然后将这些协程注册到事件循环中。

看例子说话
```py
import asyncio

async def test1():

    print("1")
    await asyncio.sleep(1)
    print("2")
    return "stop"

a = test1()
b = test1()
c = test1()

tasks = [
    asyncio.ensure_future(a),
    asyncio.ensure_future(b),
    asyncio.ensure_future(c),
]

loop = asyncio.get_event_loop()
loop.run_until_complete(asyncio.wait(tasks)) # 注意asyncio.wait方法
for task in tasks:
    print("task result is ",task.result())
```
【解释一下】：代码先是定义了三个协程对象，然后通过asyncio.ensure_future方法创建了三个task，并且将所有的task加入到了task列表，最终使用loop.run_until_complete将task列表添加到事件循环中。


---

##### 基于asyncio实现的协程爬虫

上文中已经介绍了如何使用`async与await`创建协程函数，以及如何使用asyncio.get_event_loop创建事件循环并执行协程函数。在这里主要讲一讲涉及到**asyncio库**的异步爬虫如何实现。

【存在的问题】：requests模块、urllib模块写异步爬虫，但实际操作发现并不支持asyncio异步；
【解决思路】：使用aiohttp模块编写异步爬虫；
```py
import asyncio
import aiohttp

async def run(url):
    print("start spider ",url)
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as resp:
            print(resp.url)

url_list = ["https://www.baidu.com","https://home.com","https://movie.com","https://taobai.com"]

tasks = [asyncio.ensure_future(run(url)) for url in url_list]
loop = asyncio.get_event_loop()
loop.run_until_complete(asyncio.wait(tasks))
```
---

##### 关于阻塞函数在asyncio中使用的问题以及解决方案

【客观背景】：虽然目前有异步函数支持asyncio，但**实际问题是大部分IO模块还不支持asyncio。**
【问题】：如果想在asyncio中使用其他阻塞函数，该怎么实现呢？

###### 阻塞函数在asyncio中使用的问题

【】阻塞函数(例如io读写，requests网络请求等)，阻塞了客户程序与asycio事件循环的唯一线程，因此在执行调用时，整个应用程序都会冻结。


【解决方案】：使用事件循环对象的**run_in_executor方法。**
【详细解释】：syncio的事件循环在背后维护着一个ThreadPoolExecutor对象，我们可以调用run_in_executor方法，把可调用对象发给它执行，即可以通过run_in_executor方法来新建一个线程来执行耗时函数。

---

【run_in_executor方法】
```
AbstractEventLoop.run_in_executor(executor, func, *args)
```
【组成解释】:
- executor： 参数应该是一个 Executor 实例。如果为 None，则使用默认 executor。（区分大小写）
- func ：就是要执行的函数。
- args： 就是传递给 func 的参数。
看个实例：

```
import asyncio
import time

async def run(url):
    print("start ",url)
    loop = asyncio.get_event_loop()
    try:
        await loop.run_in_executor(None,time.sleep,1)
    except Exception as e:
        print(e)
    print("stop ",url)

url_list = ["https://www.baidu.com","https://home.com","https://movie.com","https://taobai.com"]

tasks = [asyncio.ensure_future(run(url)) for url in url_list]
loop = asyncio.get_event_loop()
loop.run_until_complete(asyncio.wait(tasks))
```
【好处】run_in_executor方法：可以借此创建协称并发；避免使用特定的模块来实现IO异步开发。


【参考】：https://blog.csdn.net/sinat_34082752/article/details/80680348
