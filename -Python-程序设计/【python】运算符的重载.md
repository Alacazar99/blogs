######什么是运算符重载：

让自定义的类生成的对象(实例)能够使用运算符进行操作
######运算符重载的作用:
- 让自定义的实例像内建对象一样进行运算符操作 
- 让程序简洁易读
- 对自定义对象将运算符赋予新的规则

**算术运算符的重载:**

              方法名                  运算符和表达式       说明
           __add__(self,rhs)        self + rhs        加法
            __sub__(self,rhs)        self - rhs         减法
            __mul__(self,rhs)        self * rhs         乘法
            __truediv__(self,rhs)   self / rhs          除法
            __floordiv__(self,rhs)  self //rhs          地板除
            __mod__(self,rhs)       self % rhs       取模(求余)
            __pow__(self,rhs)       self **rhs         幂运算

举个栗子：
定义一个二维函数类：用于实现加减乘除等运算。
```
class Vector2D:

    def __init__(self,x,y):
        self.x = x
        self.y = y

    # a + b
    def __add__(self, other):
        x = self.x + other.x
        y = self.y + other.y
        return Vector2D(x,y)

    def __str__(self):
        return "Vector2D(x={},y={})".format(self.x,self.y)

    # a - b
    def __sub__(self, other):

        return Vector2D(self.x - other.x,self.y - other.y)
    # a * b
    def __mul__(self, other):
        return Vector2D(self.x * other.x, self.y * other.y)

    # b / b
    def __truediv__(self, other):
        return Vector2D(self.x / other.x,self.y / other.y)

    # a+=b
    def __iadd__(self, other):
        print("__iadd__")
        return self.__add__(other)

    # a的绝对值
    def __abs__(self):
        # return sqrt(self.x ** 2 + self.y ** 2)

        return hypot(self.x,self.y)


v1 = Vector2D(2,3)
v2 = Vector2D(3,4)
print(abs(v1))
# len(v1)

v3 = v1 + v2
print(v3)

print(v1 - v2)

print(v1 * v2)

print(v1 / v2)

v1 += v2
print(v1)
```
输出：
```
Vector2D(x=5,y=7)
Vector2D(x=-1,y=-1)
Vector2D(x=6,y=12)
Vector2D(x=0.6666666666666666,y=0.75)
__iadd__
```

---------------
内存分析一下：
      看图叭，不想多BB...

![](https://upload-images.jianshu.io/upload_images/17476267-4bde697d867b08b0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
---------------------------------
比较`__add__` 、` __radd__`、`__iadd__`的区别
（1）如果a有`__add__`方法，并且返回值不是`NotImplemented`, 则调用`a.__add__(b)`，然后返回结构
（2）如果a没有`__add__`方法，或者返回了`NotImplemented`,检查b有没有`__radd__`方法，如果有， 且没有返回`NotImplemented`，则会调用`b.__radd__(a)`,然后返回结果
 3）如果b没有`__radd__`方法，或者调用`__radd__`返回`NotImplemented`，抛出错误。

例如：
```
class A:
    def __add__(self, other):
        print(type(other))
        print("A__add")
        return NotImplemented


    def __radd__(self, other):
        print(type(other))
        print("B__add")

class B:
    def __radd__(self, other):
        print("B_radd")

a = A()
b = B()
a + b
```
Output:
```
<class '__main__.B'>
A__add
B_radd
```
--------------------------------
关于`__str__` 与` __repr__`区别：
（1） ` __str__`不存在的时候，会尝试调用`__repr__`
（2）`__str__`是为了让人读；`__repr__ `是为了让机器读
  （3）`__str__`由 print函数 和 str()函数 调用，`__repr__`是由 repr() 函数调用

```
class Student:
    def __init__(self,name):
        self.name = name

    def __repr__(self):
        return "Student(%r)" % self.name

    def __str__(self):
        return "name:%s"%self.name
# 创建实例
s = Student("Zurich")
print(s)
str1 = repr(s)
print(str1)
print(eval(str1))
```
Output：
```
name:Zurich
Student('Zurich')
name:Zurich
```
特别安利一个网址：http://www.pythontutor.com/visualize.html#mode=display (分析内存太好用了哇)
![__str__由 print函数 和 str()函数 
调用; __repr__是由 repr() 函数调用](https://upload-images.jianshu.io/upload_images/17476267-c75fc27c229bfb28.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
