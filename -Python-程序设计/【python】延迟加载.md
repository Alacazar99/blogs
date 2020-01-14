##### 延迟加载的目的：
节省一些初始化所需要的时间和空间
Python里面的延迟加载用得非常多，其主要思想是延迟所要引入类的实例化。来达成上述的目的`（节省一些初始化所需要的时间和空间）`

##### 基本思路
我们创建一个类，我们将我们需要实例化的类传给他，这时该类都会变成一个延迟加载类，在应用的时候，虽然我实例化了这个延迟加载类，但是我们要引用的类就没有实例化。

---
参考文档：https://blog.csdn.net/hbnn111/article/details/80858889
思想路线：我们对于一个实例化的操作，无非最终会归结为`__getattr__`，`__setattr__`等运算符，因此只要我们定义好这些运算符就可以实现这些延迟，即只有执行这些操作的时候，才去真正实例化我们想要实例化的类： 
```
class Foo:
    x=1
    def __init__(self,y):
        self.y=y

    def __getattr__(self, item):
        print('----> from getattr:你找的属性不存在')

    def __setattr__(self, key, value):
        print('----> from setattr')
        # self.key=value #这就无限递归了,你好好想想
        self.__dict__[key]=value #应该使用它

    def __delattr__(self, item):
        print('----> from delattr')
        del self.item #无限递归了
        self.__dict__.pop(item)

    def __getattribute__(self, item):
        print('----> __getattribute__')
        return super().__getattribute__(item)

f = Foo(1)
print("---分割线--")
f.z = 10
```
执行`__setattr__`操作的时候，才去真正实例化我们想要实例化的类，在延迟函数`__delattr__(self, item)`中，没有实例化要引入的类，下面来分析一下内存的占用情况
![内存分析](https://upload-images.jianshu.io/upload_images/17476267-84e6baf47447ff70.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

output：
```
----> from setattr
----> __getattribute__
---分割线--
----> from setattr
----> __getattribute__
```

---

Example：
```
class Cache:
    _cache = dict()

    def __setattr__(self, key, value):
        if not Cache._cache.keys().__contains__(key):
            Cache._cache[key] = []

        Cache._cache[key].append(value)

    def __getattribute__(self, item):

        if not Cache._cache.keys().__contains__(item):
            return None

        return Cache._cache[item]

c = Cache()
c.user = "Zurich"
c.user = "MMMM"
print(c.user)
```
output:  ['Zurich', 'MMMM']

![](https://upload-images.jianshu.io/upload_images/17476267-4e5ee7a363e53bda.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我感觉自己都不是很懂这个..... 迷得很...
