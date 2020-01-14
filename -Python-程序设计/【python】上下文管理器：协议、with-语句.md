
##### 上下文管理器
概念：实现了上下文协议的对象即为上下文管理器。
```
1. 上下文表达式：with open('test.txt') as f:
2. 上下文管理器：open('test.txt')
3. f 不是上下文管理器，应该是资源对象。
```
##### 上下文管理器的协议：
`__enter__`进入的方法
`__exit__`退出的方法

【作用】：一种更加优雅的方式，操作（创建/获取/释放）资源，如文件操作、数据库连接;可以以一种更加优雅的方式，处理异常；

##### with 语句
谈及上下文管理器，就不得不提一下`with 语句`，with 语句是 Pyhton 提供的一种简化语法，适用于对资源进行访问的场合，确保不管使用过程中是否发生异常都会执行必要的“清理”操作，释放资源，with 语句主要是为了简化代码操作。
语法如下：
```
with context as var：
    statements（语句）
```
【知识拓展】：线程中的锁其实也实现了上下文管理协议：
```
import threading

withthreading.Lock():

    # 关键部分 

    statements

    # 关键部分结束 
```
- with：当文件操作执行完成后， with语句会自动调用上下文管理器里的关闭语句来关闭文件资源。
- with 语句的实现原理建立在上下文管理器之上

【注释】Python 提供了 with 语法用于简化资源操作的后续清除操作，是`  try / finally  `的替代方法。Python 还提供了一个 contextmanager 装饰器，更进一步简化上下管理器的实现方式。
【耿直插播】`contextlib模块`的`contextmanager装饰器`可以更方便的实现上下文管理器。（使用装饰器在一定程度上也简化了代码）
任何能够被yield关键词分割成两部分的函数，都能够通过装饰器装饰的上下文管理器来实现。任何在yield之前的内容都可以看做在`__enter__`中的代码执行前的操作，而任何yield之后的操作都可以放在`__exit__ `函数中。

###### 下面讲讲关于上下文管理器的协议的用法
`__enter__ `方法会在执行 with 后面的语句时执行，一般用来处理操作前的内容。比如一些创建对象，初始化等；
`__exit__ `方法会在 with 内的代码执行完毕后执行，一般用来处理一些善后收尾工作，比如文件的关闭，数据库的关闭等。

###### `__exit__`方法的参数

`__exit__ `方法中有三个参数，用来接收处理异常，如果代码在运行时发生异常，异常会被保存到这里。  `__exit__`函数就能够拿到关于异常的所有信息(异常类型，异常值以及异常追踪信息)，这些信息将帮助异常处理操作。

- `exc_type` : 【异常类型】

- `exc_val` : 【异常值】

- `exc_tb` : 【异常回溯追踪】

---
关于上下文管理器的用法，举个实际的例子来看看：
```
import sys
class lookingGlass:
    def __enter__(self):
        self.original_write = sys.stdout.write
        sys.stdout.write = self.reverse_write
        return "ASDFGHJKL"

    def reverse_write(self, text):
        self.original_write(text[::-1])

    def __exit__(self, exc_type, exc_val, exc_tb):
        """
        :param exc_type: 异常的类型
        :param exc_val: 异常实例  except ZeroDivisionError as data
        :param exc_tb: traceback对象
        :return:
        """
        sys.stdout.write = self.original_write
        return True  
       # 如果返会True 则代表已正确处理异常；如果返会TRUE以外的其他值，则代表异常未处理，异常会层层上抛

with lookingGlass() as what:

    print("ABC$$$$HJK")
    print(what)

import contextlib

@contextlib.contextmanager
def looking_glass():
    original_write = sys.stdout.write

    def reverse_write(text):
        # original_write(text[1:10:-1])
        original_write(text[::-1])

    sys.stdout.write = reverse_write
    yield 'AAA￥￥￥SSS'
    # 以yield为分界点， yield之后的代码相当于__exit__中的代码；yield之前的代码，相当于__enter__中的代码
    # yield 的值相当于__exit__中的返回值；
    sys.stdout.write = reverse_write
print('-------$$$$------')
with looking_glass() as ABC:
    print('Zurich')
    print(ABC)
```

![输出结果如图](https://upload-images.jianshu.io/upload_images/17476267-b1cc772aa2d01ebf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

下面，我们来分析一下实现过程。
