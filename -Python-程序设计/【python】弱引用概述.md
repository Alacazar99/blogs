###### 创建弱引用

可以通过调用`weakref模块`的 ` ref(obj[ , callback] )`来创建一个弱引用，obj是你想弱引用的对象，callback是一个可选的函数，当因没有引用导致Python要销毁这个对象时调用。回调函数callback要求单个参数（弱引用的对象）。


【注意】emmmmm....如果项目用到了循环引用，那么就用`弱引用`，对象才能顺利被垃圾回收。

举个栗子：
创建一个银行账户，如果账户余额减少，则发送信息提醒户主。。。
```

class Account:

    def __init__(self,name,balance):
        self.name = name
        self.balance = balance
        self.observers = set()

    def register(self,observer):
        self.observers.add(observer)

    def unregister(self,observer):
        self.observers.remove(observer)

    def notify(self):
        for ob in self.observers:
            ob.update()

    def withdraw(self,amt):
        self.balance -= amt
        self.notify()

    def __del__(self):
        for ob in self.observers:
            ob.close()


class AccountObserver:
    def __init__(self,name,account):
        self.name = name
        self.account = account
        # 将自己加入到accound的观察者列表中
        self.account.register(self)

    def update(self):
        print("{} ,Balance is {}".format(self.name,self.account.balance))

    def close(self):
        print("关闭账户")

    def __del__(self):
        self.account.unregister(self)
        del self.account

a = Account("zs", 1000)
a_ob = AccountObserver("aob",a)
b_ob = AccountObserver("bob",a)
c_ob = AccountObserver("cob",a)
print(a_ob)
print(b_ob)
print(c_ob)
#
a.withdraw(100)

```
```
<__main__.AccountObserver object at 0x000002924356A080>
<__main__.AccountObserver object at 0x000002924356A0B8>
<__main__.AccountObserver object at 0x000002924356A0F0>
aob ,Balance is 900
bob ,Balance is 900
cob ,Balance is 900
```
---
弱引用，新增变量时，变量计数器不会增加

```
import weakref
from sys import getrefcount


class A:
    def __init__(self):
        pass
    def p(self):
        print("$$$$$$$$$$$$")

a = A()
b = a
c = weakref.ref(a)
print(getrefcount(a))
d = c()
d.p()
print(getrefcount(a))
```
```
3
$$$$$$$$$$$$
4
```
分析一下内存，思考为啥输出从3变成了4...
c是a的弱引用，而，d由c赋值，所以再一次调用getrefcount函数统计时，多调用了一次。
----
##### 垃圾回收
```
import gc
print(gc.get_threshold())
```
Output：(700, 10, 10)
【注释】返回(700, 10, 10)，后面的两个10是与分代回收相关的阈值，后面可以看到。700即是垃圾回收启动的阈值（当垃圾积累到700时开始第一次回收，累计10次清理依旧无法清除的垃圾，传入后面分代回收）。阈值可以通过`gc`中的`set_threshold()`方法重新设置。
当然，也可以使用 `gc.collect()` 手动启动垃圾回收
        
