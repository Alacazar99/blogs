##### 描述符-文字介绍
Python为开发者提供了一个非常强大的功能——描述符。那什么是描述符呢？通过查看Python的官方文档，我们知道把实现了`__get__()、__set__()和__delete__()`中的其中任意一种方法的类称之为描述符，描述符的本质是新式类，并且被代理的类（即应用描述符的类）也是新式类。`描述符的作用是用来代理一个类的属性`，需要注意的是描述符不能定义在类的构造函数中，只能定义为类的属性，它只属于类的，不属于实例，我们通过查看实例和类的字典即可知晓。

 　　描述符是可以实现大部分Python类特性中最底层的数据结构的实现手段，我们常使用的`@classmethod、@staticmethd、@property`、甚至是 `__slots__ ()`等属性都是通过描述符来实现的。它是很多高级库和框架的重要工具之一，是使用到装饰器或者元类的大型框架中的一个非常重要组件。在一般的开发中我们可能用不到描述符，但是我们如果想要开发一个大型的框架或者大型的系统，那使用描述符会起到如虎添翼的作用。它的加盟将会使得系统更加完美。

##### 描述符协议：
实现了 __get__()、__set__()、__delete__() 其中至少一个方法的类，就是一个描述符。
- `__get__：`用于访问属性。它返回属性的值，若属性不存在、不合法等都可以抛出对应的异常。
- `__ set__：`在属性分配操作中调用。不会返回任何内容。
- `__ delete__：`制删除操作。不会返回内容。
---

描述符的特点
- 是一个类，定义了访问另一个类属性的方式；
- 把至少实现了内置属性 `__set__()和__get__() `方法的描述符称为数据描述符；
- 把实现了除 `__set__() `以外的方法的描述符称为非数据描述符;
- 访问对象属性的时候：obj.attr，一般会先调用`__getattribute__()`方法，查找顺序如下：
（1）数据描述符   （2） `__dict__`属性   （3）非数据描述符

- 描述符的优先级的高低如下：

 类属性 > 数据描述符 > 实例属性 > 非数据描述符 > 找不到的属性触发`_getattr__()`

---
##### 数据描述符
举例来看:
```
class RevealAccess:
    def __init__(self,initval = None,name='var'):
        self.val = initval
        self.name = name

    def __get__(self, instance, owner):
        print("获取..",self.name)
        return self.val

    def __set__(self, instance, value):
        print("设置值：",self.name)
        self.val = value

class MyClass:
    x = RevealAccess(10,"var 'x'")
    y = 5

    def __init__(self):
       self.x = 'self x'
        # pass
m = MyClass()
print(m.x)
print("-----------------")
m1 = MyClass()
m1.x = 20
print(m1.x)
```
output：
```
设置值： var 'x'
获取.. var 'x'
self x
-----------------
设置值： var 'x'
设置值： var 'x'
获取.. var 'x'
20
```
分析一下内存：
![](https://upload-images.jianshu.io/upload_images/17476267-400ebd8abd9f50ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
---
非数据描述符

```
class InitOnAccess:
    def __init__(self, klass, *args, **kwargs):
        self.klass = klass
        self.args = args
        self.kwargs = kwargs
        self._initialized = None

    def __get__(self, instance, owner):
        if self._initialized is None:
            print("_initialized is None")
            self._initialized = self.klass(*self.args, **self.kwargs)
        else :
            print("cached")

        return self._initialized

class TestClass:
    lazy_initialized =InitOnAccess(list, "arguments")


t = TestClass()
t.lazy_initialized
t.lazy_initialized
```
output:
```
_initialized is None
cached
```
---
