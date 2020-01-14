
##### 基于生成器的协程

生成器可以作为协程（coroutine）使用，称为 "基于生成器的协程"。
协程和生成器类似，都是定义体中包含 yield 关键字的函数。但它们也有本质区别：
- 生成器用于生成 供迭代的数据，next()方法只允许调用方从生成器中获取数据； 
- 而协程与迭代无关，协程是数据的消费者，调用方会把数据推送给协程。

---

【知识拓展】：
 2001年，Python 2.2 通过了 PEP 255 -- Simple Generators ，**引入了yield 关键字实现了生成器函数**，yield 包含 `产出 和 让步 `两个含义： 生成器中` yield x `这行代码会 产出 一个值，提供给 next(...) 的调用方； 此外,还会作出让步，暂停执行生成器，让调用方继续工作，直到需要使用另一个值时再调用 next(...);

2005年，Python 2.5 通过了 PEP 342 - "Coroutines via Enhanced Generators"，给生成器增加了**.send()、.throw()和.close()方法**，第一次实现了`基于生成器的协程函数`（generator-based coroutines）

---
##### 协程的简单使用
- （coroutine function）：协程函数；
- （coroutine object）：协程对象；

生成器的调用方可以使用.send(...)方法发送数据，发送的数据会成为生成器函数中yield表达式的值。因此，生成器可以作为协程使用。协程是指一个过程，这个过程与调用方协作，产出由调用方提供的值。
```
def simple_coroutine():
    print("->coroutine started")
    x = yield # ①
    print("-> coroutine received:", x)

my_coro = simple_coroutine()
next(my_coro)  # ②
my_coro.send(42) # ③
```
OUTPUT:
```
->coroutine started
Traceback (most recent call last):
-> coroutine received: 42
  File "C:/Users/admin/Desktop/untitled1/6-30/coroutine.6-30.py", line 129, in <module>
    my_coro.send(42) # ③
StopIteration
```
【解释一下】：
- ① yield在表达式中使用；如果协程只需从客户那里接收数据，那么产出的值是None——这个值是隐式指定的，因为yield关键字右边没有表达式。

- ② 首先要调用next(...)函数，因为生成器还没启动，没在yield语句处暂停，所以一开始无法发送数据。
- ③ 调用这个方法后，协程定义体中的yield表达式会计算出42；现在，协程会恢复，一直运行到下一个yield表达式，或者终止。

---
###### 协程与生成器的对比

- （生成器）generators are data producers：

生成器用于生成供迭代的数据，next()方法只允许调用方从生成器中获取数据；

- （协程）coroutines are data consumers：

协程是数据的消费者，调用方会把数据推送给协程。send()方法允许调用方和协程之间双向交换数据。


【注意】： 协程也可以产出值，但这与迭代无关；

---
###### 协程的状态
- **'GEN_CREATED'**： 等待开始执行；

- **'GEN_RUNNING'**： 正在被解释器执行。只有在多线程应用中才能看到这个状态；
- '**GEN_SUSPENDED'**： 在yield表达式处暂停；
- **'GEN_CLOSED'**： 执行结束；

【注释】：可以使用 **inspect.getgeneratorstate(...) 函数**查看协程的当前状态

举例来看：
```
def simple_coroutine():
    print("->coroutine started")
    x = yield
    print(getgeneratorstate(my_coro))
    print("-> coroutine received:", x)

my_coro = simple_coroutine()
print(getgeneratorstate(my_coro))
next(my_coro)
print(getgeneratorstate(my_coro))
my_coro.send(27)
print(getgeneratorstate(my_coro))
```
Output:
```
GEN_CREATED
Traceback (most recent call last):
->coroutine started
GEN_SUSPENDED
  File "C:/Users/admin......(嘻嘻,保密)......py", line 144, in <module>
GEN_RUNNING
    my_coro.send(27)
-> coroutine received: 27
StopIteration
```
【注意点】

- 因为send方法的参数会成为暂停的yield表达式的值，所以，仅当协程处于暂停状态时才能调用send方法。

- 如果给未激活的协程对象发送None以外的值，会引发错误 (避免小错误哦)。

---
##### 激活协程的方式
- next(...)方法；
- my_coro.send(None)；

小二...给朕上代码:
```
def simple_coro2(a):
    print('->Started :a = ' ,a)
    b = yield a
    print('->Started :b = ' ,b)
    c = yield a + b
    print('->Received: c=',c)

my_coro2 = simple_coro2(14)
next(my_coro2)
rs1 = my_coro2.send(28)
print(rs1)
rs2 = my_coro2.send(99)
print(rs2)
```
计算移动平均值
```
def averager():
    total = 0.0
    count = 0
    average = 0.0

    while True:
        term = yield average
        total += term
        count += 1
        average = total /count

cor_avg = averager()
next(cor_avg)
cor_avg.send(20)
cor_avg.send(5)
cor_avg.send(27)
----
输出：
20.0
12.5
17.333333333333332
```

【知识拓展】：关于wraps（写的很详细）：
https://blog.csdn.net/hqzxsc2006/article/details/50337865 

---

###### Python装饰器

在实现的时候，被装饰后的函数其实已经是另外一个函数了（函数名等函数属性会发生改变），为了不影响，Python的`functools包`中提供了一个叫wraps的decorator来消除这样的副作用。

写一个decorator的时候，最好在实现之前加上functools的wrap，它能保留原有函数的名称和docstring。
```
#不加wraps
def my_decorator(func):
    
    def wrapper(*args, **kwargs):
        """decorator"""
        print("Calling decorated function...")
        return func(*args,**kwargs)

    return wrapper

@my_decorator
def example():
    """Docstring"""
    print('Called example function')
    
print(example.__name__,example.__doc__)

#输出：
wrapper decorator


#加wraps

from functools import wraps
def my_decorator(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        """decorator"""
        print("Calling decorated function...")
        return func(*args,**kwargs)

    return wrapper

@my_decorator
def example():
    """Docstring"""
    print('Called example function')
    
print(example.__name__,example.__doc__)
```
Output:
example Docstring
---
【预激协程】
调用send()方法之前，需要先调用next(my_coro)方法进行激活。有时候为了简化协程的用法，有时会使用一个预激装饰器。

一起来看一个例子：
```
from functools import wraps

def coroutine(func):
    @wraps(func)
    def primer(*args,**kwargs):
        gen = func(*args,**kwargs)
        next(gen)
        return gen

    return primer


@coroutine
def averager():
    total = 0.0
    count = 0
    average = None

    while True:
        term = yield average
        total += term
        count += 1
        average = total / count

from inspect import getgeneratorstate
coro_avg = averager()
print(getgeneratorstate(coro_avg))
print(coro_avg.send(10))
print(coro_avg.send(30))
```
Output：
```
GEN_SUSPENDED
10.0
20.0
```
【解释】：如果使用yield from 语法，会自动预激。

asyncio.coroutine装饰器不会预激活协程，因此能兼容yield from 句法。

---
##### 终止协程和异常处理

协程中未处理的异常会向上冒泡，传给next函数或send方法的调用方(`触发协程的对象`)。
- 由于第三行发送的不是数据，导致协程内部抛出异常；
- 由于协程内部没有处理异常，协程会终止。如果试图重新激活协程，会抛出StopIteration异常；
##### 异常处理的两种方式
- ###### generator.throw(exec_type[,exc_value[,traceback]])

- 会使生成器在暂停的yield表达式处抛出指定的异常；
- 如果生成器处理了抛出的异常，代码会向前执行到下一个yield表达式，而产出的值会成为调用generator.throw方法得到的返回值；
- 如果生成器没有处理抛出的异常，异常会向上冒泡，传到调用方的上下文中；
---
- ###### generator.close()

 - 致使生成器在暂停的yield表达式处抛出GeneratorExit异常（正常终止协程）。

- 如果生成器没有处理这个异常，或者抛出了StopIteration异常（通常是指运行到结尾），调用方不会报错；

- 如果收到GeneratorExit异常，生成器一定不能产出值，否则解释器会抛出RuntimeError异常。生成器抛出的其他异常会向上冒泡，传给调用方。

【注意1】如果收到GeneratorExit异常，生成器一定不能产出值，否则解释器会抛出RuntimeError异常；
生成器抛出的其他异常会向上冒泡，传递给调用方；
【注意2】但是，如果传入协程的异常没有处理，协程会终止，即状态变成 'GEN_CLOSED'：

呈上一份代码：
```
class DemoException(Exception):
    pass

def demo_exc_handing():
    print('==>coroutine strated')

    while True:
        try:
            x = yield       # 产生异常！
        except DemoException:  # 特别处理 DemoException 异常
            print('==> DemoException handled.Continuing...')
        else:         # 如果没有异常，那么显示接收到的值
            print('==>coroutine received:{!r}'.format(x))
        finally:
            print('==>Ending...')
exc_coro = demo_exc_handing()
next(exc_coro)
exc_coro.send(11)
exc_coro.send(22)

print(inspect.getgeneratorstate(exc_coro))
```
输出：
```
==>Ending...
==>coroutine strated
==>coroutine received:11
==>Ending...
==>coroutine received:22
==>Ending...
GEN_SUSPENDED
```
![基于jupyter-notebook](https://upload-images.jianshu.io/upload_images/17476267-096c46fa698f66a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

【解释】如果把DemoException异常传入demo_finally协程，它会处理，然后继续运行
```
exc_coro = demo_exc_handling()
next(exc_coro)
exc_coro.send(11)
exc_coro.send(22)
# 将 DemoException 传入
exc_coro.throw(DemoException)
from inspect import getgeneratorstate
print(getgeneratorstate(exc_coro))
```
仔细看上述两者的区别
【下一步】：如果传入的异常没有处理，协程会立即停止，变成'GEN_CLOSED'状态。
例如：
```
exc_coro = demo_exc_handling()
next(exc_coro)
exc_coro.send(11)
exc_coro.send(22)
# 将 DemoException 传入
exc_coro.throw(ZeroDivisionError)
from inspect import getgeneratorstate
print(getgeneratorstate(exc_coro))
```
【技能强化】：如果协程必须做一些清理工作，则可以在协程体中放入` try/finally 代码块`
例如：
```
def demo_finally():
    print('-> coroutine started')
    try:
        while True:
            try:
                x = yield
            except DemoException:
                print('*** DemoException handled. Continuing...')
            else:
                print('-> coroutine received: {!r}'.format(x))
    finally:
        print("->coroutine ending...")

demo_coro = demo_finally();
next(demo_coro)
demo_coro.send(11)
demo_coro.send(ZeroDivisionError)
```
---

###### 让协程return值

Python 3.3以后，允许在协程中有return expr表达式，如果执行到该语句，则协程运行结束。同时会抛出StopIteration异常，而返回值就在该异常对象的value属性上。
【重点】**如果协程中有return语句，则返回return后面的表达式的值，否则返回None**
有些协程在被激活后，每次驱动（drive）协程时，不会产出值，而是在最后（协程正常终止时）返回一个值（通常是某种累加值）

还是上述讲过的例子，在这里会返回平均值：
```
from collections import namedtuple
Result = namedtuple('Result','count average')

def averager():
    total = 0.0
    count = 0
    average = None

    while True:
        term = yield
        if term is None:
            break

        total += term
        count += 1
        average = total / count

    return Result(count,average)

coro_avg = averager();
next(coro_avg)
coro_avg.send(10)
coro_avg.send(30)
coro_avg.send(20)
coro_avg.send(None)
```
Output：
```
Traceback (most recent call last):
  File "C:/Users/...(保密，嘻嘻)...py", line 25, in <module>
    coro_avg.send(None)
StopIteration: Result(count=3, average=20.0)
```
生成器对象会抛出StopIteration异常。异常对象的value属性保存着返回的值.....
【yield from结构】：在内部自动捕获StopIteration异常。对yield from结构来说，解释器不仅会捕获StopIteration异常，还会把value属性的值变成yield from表达式的值。

###### 使用协程的优点
- 协程最大的优势就是协程极高的执行效率。子程序切换由程序自身控制，因此，没有线程切换的开销；
- 不需要多线程的锁机制，阻塞自动切换；
- 程即是生成器对象的yield和send方法的配合使用；

【关于python中的 yield from 句法】：https://www.jianshu.com/p/6c6761a407c5

---
