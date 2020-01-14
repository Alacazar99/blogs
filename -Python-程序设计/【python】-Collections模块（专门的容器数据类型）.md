##### Collections模块

此模块实现了专门的容器数据类型，为Python的通用内置容器提供了替代方案。 以下几种类型用的很多：
- defaultdict (dict子类调用工厂函数来提供缺失值)
- counter (用于计算可哈希对象的dict子类)
- deque (类似于列表的容器，可以从两端操作)
- namedtuple (用于创建具有命名字段的tuple子类的工厂函数)
- OrderedDict (记录输入顺序的dict)

1.1. defaultdict
 > 其实就是一个查不到key值时不会报错的dict
```
person = {'name':'xiaobai','age':18}
print ("The value of key  'name' is : ",person['name'])
print ("The value of key  'city' is : ",person['city'])

Out: The value of key  'name' is :  xiaobai
Traceback (most recent call last):
  File "C:\Users\E560\Desktop\test.py", line 17, in <module>
    print ("The value of key  'city' is : ",person['city'])
KeyError: 'city'
```
【对比】现在如果用defaultdict再试试呢？
```
from collections import defaultdict
person = defaultdict(lambda : 'Key Not found') # 初始默认所有key对应的value均为‘Key Not Found’

person['name'] = 'xiaobai'
person['age'] = 18
print ("The value of key  'name' is : ",person['name'])
print ("The value of key  'adress' is : ",person['city'])
```
大家可以发现，这次没有问题了，其实最根本的原因在于当创建defaultdict时，首先传递的参数是所有key的默认value值，之后添加name，age进去的时候才会有所改变，当最终查询时，如果`key存在`，那就输出对应的value值，如果`key不存在`，就会输出事先规定好的值`‘Key Not Found’`
除此之外外，还可以利用defaultdict创建时，传递参数为所有key默认value值这一特性，实现一些其他的功能.
-------------------------------------------
1.2. counter
> Counter是dict的子类。因此，它是一个无序集合，其中元素及其各自的计数存储为字典。
就是一个计数器，**一个字典，key就是出现的元素，value就是该元素出现的次数**

举个栗子：
```
from collections import Counter

count_list = Counter(['B','B','A','B','C','A','B','B','A','C'])  #计数list
print (count_list)

count_tuple = Counter((2,2,2,3,1,3,1,1,1))  #计数tuple
print(count_tuple)

------------------------------
输出：Counter({'B': 5, 'A': 3, 'C': 2})
     Counter({1: 4, 2: 3, 3: 2})
```
`Counter一般不会用于dict和set的计数，因为dict的key是唯一的，而set本身就不能有重复元素`

1.3. deque
> 在需要在容器两端的更快的添加和移除元素的情况下，可以使用deque.
deque就是一个可以两头操作的容器，类似list但比列表速度更快

- deque的方法有很多，很多操作和list类似，也支持切片

- deque最大的特点在于可以从两端操作：
```
d = deque([i for i in range(5)])
print(len(d))
# Output: 5

d.popleft()   # 删除并返回最左端的元素

d.pop()       # 删除并返回最右端的元素
# Output: 4

print(d)
# Output: deque([1, 2, 3])

d.append(100)  # 从最右端添加元素

d.appendleft(-100) # 从最左端添加元素

print(d)
# Output: deque([-100, 1, 2, 3, 100])
```
- 首先定义一个deque时可以规定它的最大长度，deque和list一样也支持extend方法，方便列表拼接，但是deque提供双向操作：
```
from collections import deque
d = deque([1,2,3,4,5], maxlen=9)  #设置总长度不变
d.extendleft([0])  # 从左端添加一个list
d.extend([6,7,8])   # 从右端拓展一个list
print(d)
```
```
deque([0, 1, 2, 3, 4, 5, 6, 7, 8], maxlen=9)
```
现在d已经有9个元素了，而规定的maxlen=9，这个时候如果从左边添加元素，会自动移除最右边的元素，反之也是一样：
```
d.append(100)
print(d)
d.appendleft(-100)
print(d)

----------------------
输出：deque([1, 2, 3, 4, 5, 6, 7, 8, 100], maxlen=9)
     deque([-100, 1, 2, 3, 4, 5, 6, 7, 8], maxlen=9)
```
------------------------------------------------
1.4. namedtuple
> **命名元组**。它是元组的强化版。namedtuple可以将元组转换为方便的容器。`使用namedtuple，不必使用整数索引来访问元组的成员。`
可以把namedtuple 视为 不可变的 字典
```
from collections import namedtuple

Person = namedtuple('Person', 'name age city')        # 类似于定义class
xiaobai = Person(name="xiaobai", age=18, city="paris") # 类似于新建对象
print(xiaobai)
```
```
Person(name='xiaobai', age=18, city='paris')
```
创建namedtuple时非常像定义一个class，这里Person好比是类名，第二个参数就是namedtuple的值的名字了，很像class里的属性，不过这里不用加逗号分离。

-----------------------------------
1.5. OrderedDict
> “OrderedDict” 本身就是一个dict，但是它的特别之处在于会记录插入dict的key和value的顺序

举个栗子：
```
from collections import OrderedDict
d = {}
d['a'] = 1
d['b'] = 2
d['c'] = 3
d['d'] = 4
print(d)
```
```
Out:{'a': 1, 'c': 3, 'b': 2, 'd': 4}
```
这是一个普通的dict，因为无序，即使依次添加了a,b,c,d 四个键并赋予value，但是输出的顺序并不可控
OrderedDict的出现就是为了解决这个问题：
```
from collections import OrderedDict
d = OrderedDict()
d['a'] = 1
d['b'] = 2
d['c'] = 3
d['d'] = 4
print(d)

Out：OrderedDict([('a', 1), ('b', 2), ('c', 3), ('d', 4)])
```
因为会自动记录插入的顺序，同理，如果删除一个key, OrderedDict的顺序不会发生变化：
```
from collections import OrderedDict

print("Before deleting:\n")
od = OrderedDict()
od['a'] = 1
od['b'] = 2
od['c'] = 3
od['d'] = 4

for key, value in od.items():
    print(key, value)

print("\nAfter deleting:\n")
od.pop('c')
for key, value in od.items():
    print(key, value)

print("\nAfter re-inserting:\n")
od['c'] = 3
for key, value in od.items():
    print(key, value) 
```
