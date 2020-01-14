# 继承
**继承** 是[面向对象](https://baike.baidu.com/item/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1/2262089)软件技术当中的一个概念，与多态、封装共为面向对象的三个基本特征。 继承可以使得子类具有父类的属性和方法或者重新定义、追加属性和方法等。
附上一个使用面向对象编程，并使用继承方式实现的模拟打怪游戏
## 注意
- 1、子类拥有父类得特征，而父类没有，父类更通用，子类更具体，
> （特征包括属性和方法，自身的特性，拥有父类没有的）
- 2、使用`extends`继承父类，语句格式：`class 子类名 extends 父类名{}`
- 3、父类中一般只定义`一般属性`和`方法`
> （这个一般可以理解为是子类共有的，这就是父类更通用，而子类拥有其他的，所以子类更具体）
- 4、子类中通过`super`关键字来调用父构造方法
- 5、在子类中可以继承父类得那些东西，哪些不可以继承：
> - 父类中`public`，`protected`修饰的属性，方法可以继承，`private`修饰的属性和方法不能被继承

- 6、规则： 创建子类对象的时候，首先调用的是父类的`无参构造方法`创建一个父类对象
- 7、可以在子类中显示调用父类的有参构造方法
- 8、如果父类的属性均为`private`修饰，则可以通过共有的`getter，setter`方法来调用
>-  Python 支持多继承 ,继承内容和继承循序相关.
所有的类，都会默认继承object类

举个栗子：

```py
class A:
    def __init__(self):
        self.name = 'A'

    def print_test(self):
        print('￥￥￥￥￥￥￥')

class B:
    def __init__(self):
        self.name = 'B'

# class c(A,B):
class C(B,A):
    name = 'C'
    def __init__(self):
        super(C, self).__init__()
        self.age = 20

    def print_test(self):
        super(C, self).print_test()
        print('$$$$$$$$$$$$')
c = C()
print(c.name)
print(c.age)
# # 调用属性或方法的查找顺序
print(C.__mro__)
c.print_test()
```
内存分析：
![内存分析](https://upload-images.jianshu.io/upload_images/17476267-eff598a50b915108.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

输出：
```
B
20
(<class '__main__.C'>, <class '__main__.B'>, <class '__main__.A'>, <class 'object'>)
￥￥￥￥￥￥￥
$$$$$$$$$$$$
```
如果
```
__mro__方法：

python中支持多重继承，在解析父类的__init__时，
定义解析顺序的是子类的__mro__属性，内容为一个存储要解析类顺序的元组。
```
### 组合优于继承
```
class A:
    def __init__(self):
        self.name = 'A'

    def print_test(self):
        print('￥￥￥￥￥￥￥')

class D:
    def __init__(self):
        self.a = A()

    def print_test(self):
        self.a.print_test()
        print('= = = = = = = ')

d = D()
d.print_test()
```
```
￥￥￥￥￥￥￥
= = = = = = = 
```
##### 判断实例：
```
print(isinstance(C,B))
print(issubclass(B,A))
```
```
False
False
```
