在Python中，如果我们想实现创建类似于序列和映射的类（可以迭代以及通过[下标]返回元素），可以通过重写魔法方法`__getitem__、__setitem__、__delitem__、__len__`方法去模拟。
###### 魔术方法的作用：
`__getitem__(self,key):`返回键对应的值。
`__setitem__(self,key,value)：`设置给定键的值
`__delitem__(self,key):`删除给定键对应的元素。
`__len__():`返回元素的数量

【注释】只要实现了`__getitem__ `和  `__len__`方法，就会被认为是序列。
- 可以用`__len__():`函数来查看对象长度
- `__getitem__(self,key):` 可以对对象进行[]操作，如切片，索引，iterd等高级操作。
-  如果在类中定义了`__getitem__()`方法，那么他的实例对象（假设为P）就可以这样P[key]取值。当实例对象做P[key]运算时，就会调用类中的`__getitem__()`方法。

这些魔术方法的原理就是：当我们对类的属性item进行下标的操作时，首先会被`__getitem__()、__setitem__()、__delitem__()`拦截，从而执行我们在方法中设定的操作，如赋值，修改内容，删除内容等等。
这个方法应该以与键相关联的方式存储值，以便之后能够使用`__setitem__`来获取。当然，这个对象可变时才需要实现这个方法。

举个栗子：
定义一副扑克牌（不包括大小王），对牌进行洗牌，然后发牌。
```
mport collections
Card = collections.namedtuple('Card',['rank','suit'])
#也可以使用一个类来定义Card
# class Card:
#     def __init__(self,rank,suit):
#         self.rank = rank
#         self.suit = suit


class Puke:
    ranks = [str(n) for n in range(2,11) ] + list('JQKA')
    suits = "黑桃 方块 梅花 红心".split()

    def __init__(self):
        self._cards = [Card(rank,suit) for suit in self.suits for rank in self.ranks]

    def __len__(self):
        return len(self._cards)

    def __getitem__(self, item):
        return self._cards[item]

    def __setitem__(self, key, value):
        print(key,value)
        self._cards[key] = value

pk = Puke()

# print(pk._cards)
# for card in pk:
    print(card)
print(pk[2:6])
print(pk[12::13])
pk[1:3] = [Card(rank='A',suit='红桃')] * 3
print(pk[1:3])
```
Output：
```

[Card(rank='2', suit='黑桃'), Card(rank='3', suit='黑桃'), Card(rank='4', suit='黑桃'), Card(rank='5', suit='黑桃'), Card(rank='6', suit='黑桃'), Card(rank='7', suit='黑桃'), Card(rank='8', suit='黑桃'), Card(rank='9', suit='黑桃'), Card(rank='10', suit='黑桃'), Card(rank='J', suit='黑桃'), Card(rank='Q', suit='黑桃'), Card(rank='K', suit='黑桃'), Card(rank='A', suit='黑桃'), Card(rank='2', suit='方块'), Card(rank='3', suit='方块'), Card(rank='4', suit='方块'), Card(rank='5', suit='方块'), Card(rank='6', suit='方块'), Card(rank='7', suit='方块'), Card(rank='8', suit='方块'), Card(rank='9', suit='方块'), Card(rank='10', suit='方块'), Card(rank='J', suit='方块'), Card(rank='Q', suit='方块'), Card(rank='K', suit='方块'), Card(rank='A', suit='方块'), Card(rank='2', suit='梅花'), Card(rank='3', suit='梅花'), Card(rank='4', suit='梅花'), Card(rank='5', suit='梅花'), Card(rank='6', suit='梅花'), Card(rank='7', suit='梅花'), Card(rank='8', suit='梅花'), Card(rank='9', suit='梅花'), Card(rank='10', suit='梅花'), Card(rank='J', suit='梅花'), Card(rank='Q', suit='梅花'), Card(rank='K', suit='梅花'), Card(rank='A', suit='梅花'), Card(rank='2', suit='红心'), Card(rank='3', suit='红心'), Card(rank='4', suit='红心'), Card(rank='5', suit='红心'), Card(rank='6', suit='红心'), Card(rank='7', suit='红心'), Card(rank='8', suit='红心'), Card(rank='9', suit='红心'), Card(rank='10', suit='红心'), Card(rank='J', suit='红心'), Card(rank='Q', suit='红心'), Card(rank='K', suit='红心'), Card(rank='A', suit='红心')]
[Card(rank='4', suit='黑桃'), Card(rank='5', suit='黑桃'), Card(rank='6', suit='黑桃'), Card(rank='7', suit='黑桃')]
[Card(rank='A', suit='黑桃'), Card(rank='A', suit='方块'), Card(rank='A', suit='梅花'), Card(rank='A', suit='红心')]
slice(1, 3, None) [Card(rank='A', suit='红桃'), Card(rank='A', suit='红桃'), Card(rank='A', suit='红桃')]
[Card(rank='A', suit='红桃'), Card(rank='A', suit='红桃')]
```
`【注意】`:我们会发现output中，输出了：`slice(1, 3, None)`，下面给出解释。

-----------------------------------

###### 切片原理
语法：
>class slice(stop)
class slice(start, stop[, step])

参数说明：

- start -- 起始位置
- stop -- 结束位置
- step -- 间距

slice() 函数实现切片对象，主要用在切片操作函数里的参数传递。
> slice用于规定序列的选取规则

举两个栗子来看看：

```
step = slice(0,5,2)
components = [11, 22, 66, 88, 99, 00, 123]
print(components[step])
```
Output:
```py
[11, 66, 99]
```
切片原理
```
class MySeq:
    def __getitem__(self, item):
        return item

s = MySeq()
print(s[1])
print(s[1:4])
print(s[1:4:2])
print(s[1:4:2,7:9])
```
output
```
1
slice(1, 4, None)
slice(1, 4, 2)
(slice(1, 4, 2), slice(7, 9, None))
```
-----------------------------------
(程序员必会的  hhhhh.....)
看看slice在python3.7中是怎么描述的:
```
help(slice)
```
```
Help on class slice in module builtins:

class slice(object)
 |  slice(stop)
 |  slice(start, stop[, step])
 |  
 |  Create a slice object.  This is used for extended slicing (e.g. a[0:10:2]).
 |  
 |  Methods defined here:

 |  有点长。。。忽略  ********

 |  ----------------------------------------------------------------------
 |  Static methods defined here:
 |  
 |  __new__(*args, **kwargs) from builtins.type
 |      Create and return a new object.  See help(type) for accurate signature.
 |  
 |  ----------------------------------------------------------------------
 |  Data descriptors defined here:
 |  
 |  start
 |  
 |  step
 |  
 |  stop
 |  
 |  ----------------------------------------------------------------------
 |  Data and other attributes defined here:
 |  
 |  __hash__ = None
```
