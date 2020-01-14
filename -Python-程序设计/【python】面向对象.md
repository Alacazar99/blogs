# 面向对象(Object Oriented)
是软件开发方法。
> 面向对象的概念和应用已超越了程序设计和软件开发，扩展到如数据库系统、交互式界面、应用结构、应用平台、分布式系统、网络管理结构、CAD技术、人工智能等领域。面向对象是一种对现实世界理解和抽象的方法，是计算机编程技术发展到一定阶段后的产物。
# 类
定义：类（Class）是面向对象程序设计（Object-Oriented Programming）实现信息封装的基础。类是一种用户定义类型。
- User类: 把用户相关信息抽象联系在一起
- 类名: 驼峰结构,并且首字母大写.
- 一般来说,类由属性和行为(方法)组成。
> 属性：表示类的特点,
> (方法)：规定这个类的功能.

> 程序 = 数据结构 + 算法
  类 = 属性 ＋ 方法(行为)
- 如果函数定义在类中,称为**方法**.
- 类 -> 实例:' 实例是类的具体化.'
-  类也是对象
- 类的所有属性存放在一个字典里
- `self`指的是:当前对象(方法的调用者)
##### 两者的区别：

####### __ new__()方法:
`__new__()`方法是在类准备将自身实例化时调用。
`__new__()`方法始终都是类的静态方法，即使没有被加上静态方法装饰器。
`__new__()`方法负责开发地皮，打下地基，并将原料存放在工地。

- `__init__()`方法:
`__init__()`方法在创建对象后，就会被默认调用，不需要手动调用；
`__init__()`方法默认有一个参数为self，python解释器会自动将当前对象引用传递进去，也可以传入其他参数
- 以建房子为例：
` __new__()`方法: 负责开发地皮，打下地基，并将原料存放在工地。
` __init__()`方法: 负责从工地取材料建造出地皮开发招标书中规定的大楼，
`__init__()`负责: 大楼的细节设计，建造，装修使其可交付给客户

举个栗子：
```py
class Cat:

    def __new__(cls, *args, **kwargs):
        print('__new__')
        return super().__new__(cls)
    # 构造方法,该方法会在创建对象(实例)的时候,自动调用
    def __init__(self,color,name):
        self.color = color
        self.name = name

    def catch_rat(self):
        print(self.name + '抓住了老鼠jake')

# # 创建实例
tom = Cat('Blue','Tom')
tom.catch_rat()
# 实例化对象的过程
# 1.分配内存
# 2.分配内存(__new__)
# 3.初始化值(__init__)
print(tom.name)
tom.name = 'jiafei'
print(tom.name)
```
### 演绎实例化对象的过程
- 1.分配内存
- 2.分配内存(`__new__`)
- 3.初始化值(`__init__`)
- 私有属性

输出：
```py
__new__
Tom抓住了老鼠jake
Tom
jiafei
{'color': 'Blue', 'name': 'jiafei'}
```
##### 属性私有化(别人不能改)
- 1.两个下划线__开头；
- 2.当然也能自己约定 _单下划线；（默认这个规则比较好，不然就太吸引仇恨辽）
- __双下划线为什么能够隐藏变量?
  > 1、`_xxx `不能用于`from module import * `以单下划线开头的表示的是`protected`类型的变量。即保护类型只能允许其本身与子类进行访问。 
2、`__xxx `双下划线的表示的是私有类型的变量。只能是允许这个类本身进行访问了。**即使是子类也不可以**
3、`__xxx___` 定义的是特列方法。
- 双下划线开头的属性,会被名称转换:
__类名 + 属性名;
 - `__dict__()`里面装有实例的所有属性;


来看看以下实例：
```py
class Student:
    '''
    学生类
    '''
    def __init__(self,name,age):
        self.__name = name;
        self.__age = age

    def set_name(self,name):
        if not len(name) < 3:
            self.__name = name

    def get_name(self):
        return self.__name

    def age(self):
        return self.__name

    @property
    # 把get方法变为属性只需要加上 @property装饰器即可
    def age(self):
        print('年龄:')
        return self.__age

    @age.setter
    # @property本身又会创建另外一个装饰器 @score.setter
    # @score.setter负责把set方法变成给属性赋值
    def age(self,age):
        if age >0 and age <100:
            self.__age = age


s = Student('Zurich',36)
# 提供了get\set 方法
print(s.get_name())
s.set_name('Alcazar')
print(s.get_name())

s.age = 27
print(s.age)
```
内存分析结果示例：
![内存分析](https://upload-images.jianshu.io/upload_images/17476267-37311b650b2ca7de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

执行结果：
```
Zurich
Alcazar
年龄:
27
```
##### 代码块中涉及的 `@property`和`@score.setter`
**@property**：
- 把get方法变为属性只需要加上@property装饰器即可。
- @property本身又会创建另外一个装饰器 @score.setter
**@score.setter**：
- 负责把set方法变成给属性赋值

----------------------
一般来说，要使用某个类的方法，需要先实例化一个对象，然后调用方法。 
而使用@staticmethod或@classmethod，就可以不需要实例化，直接类名.方法名()来调用。
##### 如何区分 @classmethod 和@staticmethod
- **@classmethod**
> @classmethod 修饰的方法是类方法,类方法可以通过类名调用.
@classmethod也不需要`self`参数，但第一个参数需要是表示自身类的`cls参数`
- **@staticmethod**
>  @staticmethod 修饰的方法`:静态方法`.可以通过类名直接访问.可当做普通函数.
 可以用来写工具类
> @staticmethod不需要表示自身对象的self和自身类的cls参数，就跟使用函数一样

[更多关于@staticmethod和@classmethod的更多知识点](https://blog.csdn.net/polyhedronx/article/details/81911548)
```py
class User:
    num = 0
    def __init__(self,name):
        self.name = name
        User.num +=1

    def print_name(self):
        print(self.name,User.num)

    @classmethod
    def create_user(cls,name,age):
        # user = cls(name)
        # user.name = name
        print(cls)
        user = cls(name)
        user.age = age
        return user

    @staticmethod
    def sum(a,b):
        return a+b

    def __str__(self):
        _str = ''
        for k,v in self.__dict__.items():
            _str += str(k)
            _str += ':'
            _str += str(v)
            _str += ','
        return _str

u1 = User('Zurich')
u1.print_name()

u2 = User('Alcazar')
u2.print_name()

User.num += 1
u1.print_name()

u3 = User.create_user('Zurich',18)
print(u3.age)
print(u3.name)
#@classmethod 修饰的方法是类方法,类方法可以通过类名调用.
# 类方法必须有一个类型参数:cls

print(User.sum(3,5))
# @staticmethod 修饰的方法:静态方法.可以通过类名直接访问.可当做普通函数.
# 可以用来写工具类

u4 = u1.create_user('Zurich',21)
print(u4.age)
print(u4.sum(6,6))
print(u4)
print(u2)
```
输出：
```
Zurich 1
Alcazar 2
Zurich 3
<class '__main__.User'>
18
Zurich
8
<class '__main__.User'>
21
12
name:Zurich,age:21,
name:Alcazar,
```
