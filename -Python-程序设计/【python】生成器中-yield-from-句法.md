###### yield from 句法
在生成器中使用yield from subgen()时，subgen会获得控制权，把产出的值传给生成器的调用方，即调用方可以直接控制subgen。与此同时，gen会阻塞，等待subgen终止。

###### 【简单应用】用来简化for循环中的yield表达式

举个栗子：
```
def gen():
    for c in 'AB':
        yield c

    for i in range(1,3):
        yield i

print(list(gen()))
```
前后对比  
```
def gen():
    yield from 'AB'
    yield from range(1,3)
    
print(list(gen()))
```
【解释】yield from x表达式对x对象所做的第一件事是，调用iter(x)，从中获取迭代器。因此，x可以是任何可迭代的对象。

---
###### 【应用升级】把职责委托给子生成器

yield from 的主要功能是打开双向通道，把最外层的调用方与最内层的子生成器连接起来，这样二者可以直接发送和产出值，还可以直接传入异常，而不用在位于中间的协程中添加大量处理异常的样板代码。通过这个结构，协程可以把功能委托给子生成器。

主要术语：
- 委派生成器:
 包含 yield from <iterable> 表达式的生成器函数。

- 子生成器:
从 yield from 表达式中 <iterable>部分获取的生成器函数。

- 调用方
调用委派生成器的客户端（调用方）代码。

> graph LR
调用方  ==>  委派生成器
委派生成器  ==>  子生成器

######

【店小二】：各位看官，下面的代码请查收@ @
```
from collections import namedtuple

Result = namedtuple('Result','count average')

# 子生成器
def averager():
    total = 0.0
    count = 0
    average = None

    while True:
        # main 方法中发送的各种值，会绑定到term变量上
        term = yield
        
        # 子生成器终止的条件
        if term is None:
            break

        total += term
        count += 1
        average = total / count
    
    # 返回值会成为grouper中 yield from表达式的值
    return Result(count,average)

# 委派生成器
def grouper(results,key):
    while True:
        # 每次迭代都会生成一个averager实例。每个生成器都是本协程（grouper）使用的生成器对象。
        results[key] = yield from averager()

# 客户端代码
def main(data):
    results = {}
    for key,values in data.items():
        # results 用来存储结果
        group = grouper(results, key)
        # 预激活协程
        next(group)
        for value in values:
            # 发送的每个值都会经由grouper的yield from处理，通过管道传给averager实例。同时，当前的grouper实例，会在yield from 处暂停。
            group.send(value)
        # 把None值传入grouper，导致当前的averager实例终止，并让grouper继续运行，再创建一个aveager实例，处理下一组值。
        group.send(None)
    print(results)

data = {
    'girls;kg':[40.9,38.5,44.3,42.2,45.2,41.7,44.5,38.0,40.6,44.5],
    'girls;m':[1.6,1.51,1.4,1.3,1.41,1.39,1.33,1.46,1.45,1.43],
    'boys;kg':[39.0,40.8,43.2,40.8,43.1,38.6,41.4,40.6,36.3],
    'boys;m':[1.38,1.5,1.32,1.25,1.37,1.48,1.25,1.49,1.46]
}


main(data)
```
Output:
```
{'girls;kg': Result(count=10, average=42.040000000000006), 'girls;m': Result(count=10, average=1.4279999999999997), 'boys;kg': Result(count=9, average=40.422222222222224), 'boys;m': Result(count=9, average=1.3888888888888888)}
```

【提示】委派生成器在yield from 表达式处暂停时，调用方可以直接把数据发给子生成器，子生成器再把产出的值发给调用方。子生成器返回之后，解释器会抛出StopIteration异常，并把返回值附加到异常对象上，此时委派生成器会恢复运行。

---
###【重点】
###### yield from 句法的注意点
- 如果子生成器不终止，委派生成器会在yield from处永远暂停。

- 因为委派生成器相当于管道，所以可以把任意数量个委派生成器连接在一起：一个委派生成器使用yield from调用一个子生成器，而那个子生成器本身也是委派生成器，使用yield from调用另一个子生成器，以此类推。最终，这个链条要以一个只使用yield表达式的简单生成器结束；不过，也能以任何可迭代的对象结束。
- 任何yield from链条都必须由客户驱动，在最外层委派生成器上调用next(...)函数或.send(...)方法。


###### yield from 句法的意义
【写在前面】下面的几点知识...有点复杂，考虑的如果不够全面的话，可能写的程序会崩。

- 子生成器产出的值都直接传给委派生成器的调用方（即客户端代码）。

- 使用send()方法发给委派生成器的值都直接传给子生成器。如果发送的值是None，那么会调用子生成器的next()方法。

> 如果发送的值不是None，那么会调用子生成器的send()方法。如果调用的方法抛出StopIteration异常，那么委派生成器恢复运行。任何其他异常都会向上冒泡，传给委派生成器。

- 生成器退出时，生成器（或子生成器）中的return expr表达式会触发StopIteration(expr)异常抛出。

- yield from表达式的值是子生成器终止时传给StopIteration异常的第一个参数。

- 传入委派生成器的异常，除了GeneratorExit之外都传给子生成器的throw()方法。

如果调用throw()方法时抛出StopIteration异常，委派生成器恢复运行。StopIteration之外的异常会向上冒泡，传给委派生成器。

- 如果把GeneratorExit异常传入委派生成器，或者在委派生成器上调用close()方法，那么在子生成器上调用close()方法，如果它有的话。如果调用close()方法导致异常抛出，那么异常会向上冒泡，传给委派生成器；否则，委派生成器抛出GeneratorExit异常。

