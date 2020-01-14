#### 首先介绍一下 偏函数

如果需要减少某个函数的参数个数，你可以使用 
- functools.partial()

【作用一】:partial() 函数允许你给**一个或多个参数**设置固定的值，减少接下来被调用时的参数个数。


【作用二】:partial() 用于固定某些参数，并返回一个新的callable对象。

---

#### 关于协程的调用步骤
下面将简单介绍asyncio的使用。
- 🌰**event_loop 事件循环**：程序开启一个无限的循环，程序员会把一些函数注册到事件循环上。当满足事件发生的时候，调用相应的协程函数。
- 🌰**coroutine 协程**：协程对象，指一个使用async关键字定义的函数，它的调用不会立即执行函数，而是会返回一个协程对象。协程对象需要注册到事件循环，由事件循环调用。
- 🌰**task 任务**：一个协程对象就是一个原生可以挂起的函数，任务则是对协程进一步封装，其中包含任务的各种状态。
- 🌰**future对象**： 代表将来执行或没有执行的任务的结果。它和task上没有本质的区别；

- 🌰**async/await 关键字**：python3.5 用于定义协程的关键字，async定义一个协程，await用于挂起阻塞的异步调用接口。

###### 重点注意:
1.当我们给一个函数添加了async关键字，或者使用asyncio.coroutine装饰器装饰，就会把它变成一个异步函数。 
2.每个线程有一个事件循环，主线程调用asyncio.get_event_loop时会创建事件循环；

3.将任务封装为集合asyncio.gather(*args),之后一起传入事件循环中；

4.要把异步的任务丢给这个循环的run_until_complete方法，事件循环会安排协同程序的执行。和方法名字一样，该方法会等待异步的任务完全执行才会结束。

【注释】实现协程的不仅仅是asyncio，**tornado和gevent**都能实现类似的功能。

来看一个实例🌰：
```
async def test1():
    print("1")

async def test2():
    print("2")

a = test1()
b = test2()

try:
    a.send(None) # 可以通过调用 send 方法，执行协程函数
except StopIteration as e:
    print(e.value)
    # 协程函数执行结束时会抛出一个StopIteration 异常，标志着协程函数执行结束，返回值在value中
    pass
try:
    b.send(None) # 可以通过调用 send 方法，执行协程函数
except StopIteration:
    print(e.value)
    # 协程函数执行结束时会抛出一个StopIteration 异常，标志着协程函数执行结束，返回值在value中
    pass

---
输出：
1
2
```
【解释】程序先执行了test1函数，等到test1函数执行完后再执行test2函数。从执行过程上来看目前协程函数与普通函数没有区别，并没有实现异步函数。

---
#### 关于阻塞和await
【概念】使用`async`关键字定义的协程对象，使用await可以针对耗时的操作进行挂起（是生成器中的yield的替代，但是本地协程函数不允许使用），让出当前控制权。协程遇到await，事件循环将会挂起该协程，`执行别的协程，直到其他协程也挂起`，或者执行完毕，在进行下一个协程的执行。
```
import asyncio

async def test1():
    print("1")
    await asyncio.sleep(1) # asyncio.sleep(1)返回的也是一个协程对象
    print("2")

async def test2():
    print("3")
    print("4")

a = test1()
b = test2()

try:
    a.send(None) # 可以通过调用 send 方法，执行协程函数
except StopIteration:
    # 协程函数执行结束时会抛出一个StopIteration 异常，标志着协程函数执行结束
    pass

try:
    b.send(None) # 可以通过调用 send 方法，执行协程函数
except StopIteration:
    pass

---
输出：
1
3
4
```
【解释】程序先执行test1协程函数，在执行到await时，test1函数停止了执行（阻塞）；接着开始执行test2协程函数，直到test2执行完毕。从结果中，我们可以看到，直到程序运行完毕，test1函数也没有执行完（没有执行print(“2”)）

---
【对上面的代码块进行修改】，使得test1函数完全执行
```
import asyncio
async def test1():
    print("1")
    await test2()
    print("2")

async def test2():
    print("3")
    print("4")

loop = asyncio.get_event_loop()
loop.run_until_complete(test1())

---
输出：
1
3
4
2
```
【事件循环方法】**asyncio.get_event_loop方法**可以创建一个事件循环，然后使用 **run_until_complete** 将协程注册到事件循环，并启动事件循环。

【技能升级】使用**async可以定义协程对象**，使用**await可以针对耗时的操作进行挂起**，就像生成器里的yield一样，函数让出控制权。协程遇到await，事件循环将会挂起该协程，执行别的协程，直到其他的协程也挂起或者执行完毕，再进行下一个协程的执行，**协程的目的也是让一些耗时的操作异步化**。

---

#### 关于task任务

由于协程对象不能直接运行，在注册事件循环的时候，其实是**run_until_complete方法**将协程包装成为了一个任务（task）对象。所谓**task对象是Future类的子类**，保存了协程运行后的状态，用于未来获取协程的结果。我们也可以手动将协程对象定义成task，改进代码如下：

【对上面的代码块再一次修改】
```
import asyncio

async def test1():
    print("1")
    await test2()
    print("2")

async def test2():
    print("3")
    print("4")

loop = asyncio.get_event_loop()
task = loop.create_task(test1())
loop.run_until_complete(task)
```
【重点回顾】前面说到task对象保存了协程运行的状态，并且可以获取协程函数运行的返回值；

###### 关于task对象那么具体该如何获取呢？
- 需要绑定回调函数

- 直接在运行完task任务后输出

【提升】如果使用send方法执行函数，则返回值可以通过捕捉StopIteration异常，利用StopIteration.value获取。

【举个例子】：🌰
  ```
import asyncio

async def test_one():
    print("Zurich")
    await test_two()
    print("Alzacar")
    return "stop"

async def test_two():
    print("--")
    print("$$$")

loop = asyncio.get_event_loop()
task = asyncio.ensure_future(test_one())
loop.run_until_complete(task)
print(task.result())

---
【Output】：
Zurich
Alzacar
--
$$$
```

---
##### 使用future对象

【解释】：future对象有几个状态：**Pending、Running、Done、Cancelled**。创建future的时候，task为pending，事件循环调用执行的时候当然就是running，调用完毕自然就是done，如果需要停止事件循环，就需要先把task取消，可以使用**asyncio.Task**获取事件循环的task。

【程序：关于future对象的使用】
```
import asyncio
import functools

async def test1():
    print("1")
    await test2()
    print("2")
    return "stop"

async def test2():
    print("3")
    print("4")

def callback(param1,param2,future):
    print(param1,param2)
    print('Callback:',future.result())

loop = asyncio.get_event_loop()
task = asyncio.ensure_future(test1())
task.add_done_callback(functools.partial(callback,"param1","param2"))# 绑定回调函数
loop.run_until_complete(task)
```
【说明】通过future对象的result方法可以获取协程函数的返回值，创建task。**test1()是一个协程对象**。
回调函数中的**future对象**就是创建的**task对象**。
【多插播一句】：回调函数如果需要接受多个参数，可以通过**偏函数**导入。

---

【关于协程的停止】

怎么停止执行协程呢？
【第一步】：需要先取消task
【第二步】：停止loop事件循环。

```
import asyncio

async def test1():
    print("1")
    await asyncio.sleep(3)
    print("2")
    return "stop"

tasks = [
    asyncio.ensure_future(test1()),
    asyncio.ensure_future(test1()),
    asyncio.ensure_future(test1()),
]

loop = asyncio.get_event_loop()
try:
    loop.run_until_complete(asyncio.wait(tasks))
except KeyboardInterrupt as e:
    for task in asyncio.Task.all_tasks():
        task.cancel()
    loop.stop()
    loop.run_forever()
finally:
    loop.close()
```
---



【参考资料】[https://thief.one/2018/06/21/1/](https://thief.one/2018/06/21/1/ "Python3.5协程学习研究")
