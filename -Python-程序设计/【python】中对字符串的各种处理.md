
![图片来源网络](https://upload-images.jianshu.io/upload_images/17476267-dab684f3199ae3d4.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


>**replace()**
```py
name = 'hello world haha'
new_name = name.replace('ha','Ha')
print(new_name)

name = 'hello world haha'
new_name = name.replace('ha','Ha',1)
print(new_name)
```
输出：
```
hello world HaHa
hello world Haha
```
>**split()：**
以 **str** 为分隔符分割 **name**，如果 **maxsplit** 指定值，那么仅分割 **maxsplit** 个子串
```swift
name = 'hello world ha ha'
namr_list = name.split(" ")
print(namr_list)
name_list1 = name.split(" ", 2)
print(name_list1)
```
输出：
```
['hello', 'world', 'ha', 'ha']
['hello', 'world', 'ha ha']
```

>**capitalize()：**
把字符串的一个字符大写


>**title()：**
把每个单词首字母大写

>**startwith()：**
 检测字符串是否以S t r 为开头

>**endswith()：**
 检测字符串是否以 str 为结尾

>**upper()：**
把字符串变为大写


>**lower()：**
把字符串变为小写

代码块：
```py
my_str = 'hello world neuedu and neueducpp'
my_str2 = my_str.capitalize()
print(my_str2)

my_str2 = my_str.capitalize()  #capitalize()的使用
print(my_str2)

my_str3 = my_str.title()   # title()的使用
print(my_str3)

my_str4 = my_str.startswith('hello')   #  startwith()的使用
print(my_str4)
my_str5 = my_str.startswith('Hello')
print(my_str5)

my_str6 = my_str.endswith('cpp')   # endswith()的使用
print(my_str6)
my_str7 = my_str.upper()   # upper()的使用
print(my_str7)
my_str8 = my_str.lower()   # lower() 的使用
print(my_str8)
```
输出：
```
Hello world neuedu and neueducpp
Hello world neuedu and neueducpp
Hello World Neuedu And Neueducpp
True
False
True
HELLO WORLD NEUEDU AND NEUEDUCPP
hello world neuedu and neueducpp

```
>**rjust()** 
返回一个原字符串 右对齐 并用空格填充 w i d t h 长度的新字符串

>**ljust()**
返回一个原字符串 左对齐 并用空格填充 w i d t h 长度的新字符串

>**lstrip()**：
删除字符串左边的空白字符

>**rstrip()：**
删除字符串右边的空白字符

>**strip():**  
删除两端的空白字符

代码块：
~~~
my_str_space = 'hello'
# rjust()返回一个原字符串 右对齐 并用空格填充 w i d t h 长度的新字符串
new_my_str_space = my_str_space.rjust(10,'$')
print(len(new_my_str_space))
print(new_my_str_space)
# ljust()返回一个原字符串 左对齐 并用空格填充 w i d t h 长度的新字符串
new_my_str_space1 = my_str_space.ljust(50,'*')
print(new_my_str_space1)
print(len(new_my_str_space1))#不常用

new_my_str_space2 = my_str_space.center(50,'$')  # center()的使用
print(new_my_str_space2)
print(len(new_my_str_space2))
# lstrip() 除字符串左边的空白字符
new_str2 = new_my_str_space2.lstrip()
print(new_str2)
print(len(new_str2))
# rstrip()字符串右边的空白字符
new_str3 = new_str2.rstrip()
print(len(new_str3))
print(new_str3)
#strip()  删除两端的空白字符
print(len(new_my_str_space2))
new_str4 = new_my_str_space2.strip()
print(new_str4)
~~~
运行结果：
```python
10
$$$$$hello
hello*********************************************
50
$$$$$$$$$$$$$$$$$$$$$$hello$$$$$$$$$$$$$$$$$$$$$$$
50
**********************hello***********************
50
**********************hello***********************
50
50
**********************hello***********************
50
**********************hello***********************
```
>**find()**相似用法 ——rfind  , lfind
```swift
my_str = 'hello world neuedu and neueducpp'
index5 = my_str.rfind('neuedu')
print(index5)
```
输出：
```
23
```
>**index()**相似用法——rindex 
```swift
my_str = 'hello world neuedu and neueducpp'
index6 = my_str.index('neuedu')
print(index6)
```
输出：
```
12
```
>**partition()**相似用法——rpartition
  把 mystr 以 str 分割成三部分 （str前,str,str后）
```swift
my_str = 'hello world neuedu and neueducpp'
print(my_str)
t_mystr = my_str.partition('neuedu')
print(t_mystr)     #元组
```
输出：
```
hello world neuedu and neueducpp
('hello world ', 'neuedu', ' and neueducpp')
```
>**splitlines():**
```swift
line = 'hello\nworld'
print(line)
list_line = line.splitlines()
print(list_line)
```
输出：
```
hello
world
['hello', 'world']
```
>**isalpha():**
 判断字符串是否都是字母 Ture
```swift
my_str = 'hello world neuedu and neueducpp'
alpha = my_str.isalpha()
print(alpha)
alpha2 = 'wuyanzu'   # 不能含有空格
alpha3 = alpha2.isalpha()
print(alpha3)
```

>**isdigit():**
 判断判断字符串是否都是数字 Ture
>**isalnum():**
判断是否只有字母或者数字
```swift
py_str1 = '1178824808@qq.com'
py_str2 = '1178824808qqcom'
py_str3 = '1178824808'
Judge1 = py_str1.isdigit()   # 运用isdight()
print(Judge1)
Judge2 = py_str3.isdigit()   # 运用isdight()
print(Judge2)
Judge3 = py_str1.isalnum()   #运用 isalnum()
print(Judge3)
Judge4 = py_str2.isalnum()   #运用isalnum()
print(Judge4)
```
输出：
```
False
True
False
True
```
>**isspace():**
>**join():**
```python
str4 = " "
list1 = ['my','name','is','wuyanzu']
# my_name = str4.join(list1)
# print(my_name)
my_name = "_".join(list1)
print(my_name)
my_name = "/".join(list1)
print(my_name)
```
输出：
```
my_name_is_wuyanzu
my/name/is/wuyanzu
```
# 常用的字符串操作符：
**find  count  repalce  split  join strip**
# 内容拓展：价值一个亿的 AI 核心代码
```py
print('请输入：')
while True:
    print('AI智能说：'+input().strip('吗？')+'!')
```
输出：
```py
请输入：
在吗？
AI智能说：在!
午饭吃了吗
AI智能说：午饭吃了!
这篇文章赞了吗？
AI智能说：这篇文章赞了!
```
#列表
- **列表**和**数组** 很像，但是数组可以储存不同类型的数据
```python
name_list = ['这个讲的什么不重要！','重点是：shixiang Huang henshuai!',1024]
 print('添加前:',name_list)
print(type(name_list))
#访问
print(name_list[0])
# 遍历
for name in name_list:       # for循环特别重要
   print(name)
```
输出：
```py
添加前: ['这个讲的什么不重要！', '重点是：shixiang Huang henshuai!', 1024]
<class 'list'>
这个讲的什么不重要！
这个讲的什么不重要！
重点是：shixiang Huang henshuai!
1024
```
- ###添加操作
>**append():**
```py
name_list = ['这个讲的什么不重要！','重点是：shixiang Huang henshuai!',1024]
str = input('请输入您要添加的内容:')
name_list.append(str)
print('添加后:',name_list)
```
```
添加后: ['这个讲的什么不重要！', '重点是：shixiang Huang henshuai!', 1024, '朋友，你好']
```
```py
list1 = []
for i in range(10):
    list1.append(i)
print(list1)
```
```
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```
>**insert():** 
在指定位置index 前插入 元素 object；’
```py
a = [0,1,2]
a.insert(1,10)
print(a)
```
输出：
```
[0, 10, 1, 2]
```
>**extend():** 
可以将另外一个集合整体添加到列表中；
```python
a = [1,2]
b = [3,4]
a.append(b)
print(a)
a.extend(b)
print(a)
```
输出：
```
[1, 2, [3, 4]]
[1, 2, [3, 4], 3, 4]
```
- ###修改
>**update():**
修改操作 
```py

nname_list = ['这个讲的什么不重要！','重点是：shixiang Huang henshuai!',1024]ame_list[1] = '我玩貂蝉很厉害~'
print(name_list)
```
输出：
```
['这个讲的什么不重要！', '我玩貂蝉很厉害~', 1024]
```
###查找
>**in  or  not in**
```py
name_list = ['这个讲的什么不重要！','重点是：shixiang Huang zeishuai!',1024]
find_name = '这个讲的什么不重要'
if find_name in name_list:
    print('在列表中')
else:
    print('不在列表中')
```
```
不在列表中
```
###删除
>del :根据下标进行删除
>pop :删除最后一个元素
>remove：根据元素的值进行删除
```py
name_list = ['这个讲的什么不重要！','重点是：shixiang Huang zeishuai!',1024]
name_list.pop()
print('删除后：',name_list)
```
```
删除后： ['这个讲的什么不重要！', '重点是：shixiang Huang zeishuai!']
```
#列表的排序

>语法：form modename import name1,name2   
```python
from random import randint
#num = randint(-10,10)     #[-10,10]
#print(num)
num_list = []
for i in range(10):
    num_list.append(randint(1,20))
print(num_list)
num_list.sort()
print('正序排序：',num_list)
num_list.sort(reverse = True)
print('逆序排序：',num_list)
new_list = sorted(num_list)
print(num_list)
print(new_list)
```
输出：
```
[10, 1, 18, 20, 11, 16, 13, 9, 18, 18]
正序排序： [1, 9, 10, 11, 13, 16, 18, 18, 18, 20]
逆序排序： [20, 18, 18, 18, 16, 13, 11, 10, 9, 1]
[20, 18, 18, 18, 16, 13, 11, 10, 9, 1]
[1, 9, 10, 11, 13, 16, 18, 18, 18, 20]

```
#sort() 与sorted()的区别

1） sort() 对原来的列表进行修改排序；sorted() 返回新的，原来的没有；
 2） sort()  属于列表的成员方法；sorted()对所有可迭代对象进行操作；
 3）  ls.sort(key  reverse)； sorted(ls)；
### 字符串的常用操作
```py
print(dir(''))
print(dir([]))#  查找列表
```
##### python中的列表如下：
['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'capitalize', 'casefold', 'center', 'count', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map', 'index', 'isalnum', 'isalpha', 'isascii', 'isdecimal', 'isdigit', 'isidentifier', 'islower', 'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
['__add__', '__class__', '__contains__', '__delattr__', '__delitem__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'clear', 'copy', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']

###列表的嵌套  列表里面还有列表
```py
school_name = [['qinghua','beida'],['nankai','tianda'],['dongda','yanshan']]
print(school_name)
print(school_name[0][1])
print('===========')
#print(school_name[0,1])   #此种写法会报错！
```
输出：
```
[['qinghua', 'beida'], ['nankai', 'tianda'], ['dongqin', 'yanshan']]
beida
===========
```
# 列表推导式：
指轻量级的循环创建列表
```py
list1 = []
for i in range(10):
    list1.append(i)
print(list1)
from random import randint
list2 = [i for i in range(2,18,2)]
print(list2)
```
```
# 随机创建一个列表
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
[2, 4, 6, 8, 10, 12, 14, 16]
```
```python
list3 = []
for _ in range(5):
    list3.append("不给我鲁班，我就送！")
print(list3)
list4 = ["不给我鲁班，我就送！" for _ in range(3)]
print(list4)
```
输出：
```
['不给我鲁班，我就送！', '不给我鲁班，我就送！', '不给我鲁班，我就送！', '不给我鲁班，我就送！', '不给我鲁班，我就送！']
['不给我鲁班，我就送！', '不给我鲁班，我就送！', '不给我鲁班，我就送！']
```
```python
from random import randint
#生成10个元素,范围在【-10,10】区间的列表
l = [randint(-10,10) for _ in range(10)]
print(l)
# 选出大于等于0的 数据
#选出大于等于0的 数据
res = []
for x in l:
    if x >= 0:
        res.append(x)
print('使用 for 循环筛选：',res)
# 循环的过程中使用 if
res2 = [x for x in l if x >= 0]
print('使用列推导式解析筛选：',res2)
```
输出：
```
[-2, -8, 2, 6, -5, -6, -6, 6, -6, 4]
使用 for 循环筛选： [2, 6, 6, 4]  # 这里介绍的两种方法都十分重要，重点掌握
使用列推导式解析筛选： [2, 6, 6, 4]
```
- 列表转化成字符串
```py
my_list = ['Welcome','to','ML','world']
my_list_to = str(my_list)
print(my_list_to)
print(' '.join(my_list))
```
输出：
```
['Welcome', 'to', 'ML', 'world']
Welcome to ML world
```
- 列表和字符串的 *
```swift
str1 = 'hehe'*3
print(str1)
list4 = ['6','9',0,3,14]*2
print(list4)
```
输出：
```
hehehehehehe
['6', '9', 0, 3, 14, '6', '9', 0, 3, 14]
```
练习(1)：使用for循环和列表推广式筛选出偶数的数据
```py
number = [i for i in range(11)]
print(number)
res3 = []
for x in number:
    if x%2 == 0:
        res3.append(x)
print('使用 for 循环筛选：',res3)

res4 = [x for x in number if x%2 == 0]
print('使用列推导式解析筛选：',res4)
```
输出：
```
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
使用 for 循环筛选： [0, 2, 4, 6, 8, 10]
使用列推导式解析筛选： [0, 2, 4, 6, 8, 10]
```
练习（2）：一个学校，有3个办公室，现有8个老师等待工位分配，请编写程序，完成随机分配
```python
from random import randint
import string
#定义3个办公室
offices = [[],[],[]]
#定义8个老师
names = list(string.ascii_uppercase[:8])
#names = list(names)
print('八位老师的代号为：',names)
for name in names:
    #[0~2]  产生随机数
    index = randint(0,2)
    offices[index].append(name)
print('办公室的分配情况：',offices)
i = 1
for tempNames in offices:
    print('办公室{}人数为{}'.format(i,len(tempNames)))
    i += 1
    print('成员为：')      # 遍历
    for name in tempNames:
        print('{}'.format(name),end=' ')
    print('\n')
    print('￥ '*10)
```
输出：
```
八位老师的代号为： ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']
办公室的分配情况： [['C'], ['A', 'E'], ['B', 'D', 'F', 'G', 'H']]
办公室1人数为1
成员为：
C 

￥ ￥ ￥ ￥ ￥ ￥ ￥ ￥ ￥ ￥ 
办公室2人数为2
成员为：
A E 

￥ ￥ ￥ ￥ ￥ ￥ ￥ ￥ ￥ ￥ 
办公室3人数为5
成员为：
B D F G H 

￥ ￥ ￥ ￥ ￥ ￥ ￥ ￥ ￥ ￥ 
```
#元组  tuple
代码块:
```py
a = ('wuyanzu',111,0.88)
print(type(a))  # 查看 a 的类型
print(a[0])
# 修改 : 不能修改  也不能删除
# a[1] = 99.9
# index count
a = ('a','b','c','b','a')
index1 = a.index('a')
print(index1)
c = a.count('b')
print(c)
```
输出：
```
<class 'tuple'>
wuyanzu
0
2
```
注意：单个元素的元组后面有逗号隔开
```py
b = (100,)
print(type(b))
```
```
<class 'tuple'>
```
同时遍历两个列表
>zip() : 用于将可迭代对象作为参数，将对象中的对应元素压缩；
>一个元组，返回这些元组对象，节约内存；
```py
a = [1, 2, 3]
b = [4, 5, 6]
c = [4, 5, 6, 7, 8]
zipped = zip(a,b)
print(zipped)
print(list(zipped))
zipped1 = zip(a,c)
print(list(zipped1))
   长度不一致时，与最短的对象相同
```
输出：
```
<zip object at 0x000001B33FD37B88> #zip 不能直接输出
[(1, 4), (2, 5), (3, 6)]
[(1, 4), (2, 5), (3, 6)]

```
#字典
>字典 key ==> value
```python
info = {'name':'刘强东','age': 45, 'id': 2134254656,'address':'北京'}
#访问 根据键进行访问；
print(type(info))
print(info['name'])
# print(info['sex'])   #访问不存在的键，会报错！
age = info.get('age',30) 
 #当我们不确定字典中是否存在某个key ，而且还要获得其 value 可以使用get方法
print(age)
mail = info.get('mail','12qiangdong@jingdong.com')
print(mail)
#修改
info['name'] = '马云'
print('修改后：',info)
#添加
info['sex'] = 'man'
print('添加后：',info)
#删除 del 根据 key 删除
del info['name']
print('删除后:',info)
#完全删除
# del info
# print(info)   #会报错！
info.clear()
print('after clear:',info)    # 输出空字典
```
输出：
```
<class 'dict'>
刘强东
45
12qiangdong@jingdong.com
修改后： {'name': '马云', 'age': 45, 'id': 2134254656, 'address': '北京'}
添加后： {'name': '马云', 'age': 45, 'id': 2134254656, 'address': '北京', 'sex': 'man'}
删除后: {'age': 45, 'id': 2134254656, 'address': '北京', 'sex': 'man'}
after clear: {}
```
### 字典常用操作

代码块：
```python
info = {'name':'刘强东','age': 45, 'id': 2134254656,'address':'北京'}
print(len(info))    # key 的个数
#keys
keys = info.keys()
print(keys)
#values
values = info.values()
print(values)
#items => (keys,values)
items = info.items()
print(items)
#遍历
for key, value in info.items():
    print(key,'==>>',value)
```
输出：
```
4
dict_keys(['name', 'age', 'id', 'address'])
dict_values(['刘强东', 45, 2134254656, '北京'])
dict_items([('name', '刘强东'), ('age', 45), ('id', 2134254656), ('address', '北京')])
name ==>> 刘强东
age ==>> 45
id ==>> 2134254656
address ==>> 北京

```
### 集合
特点：无序，元素唯一
用途：**元组** 或 **列表** 去重
```python
info = {'name':'刘强东','age': 45, 'id': 2134254656,'address':'北京'}
set1 = set()
set1 = {1, 2, 4, 5}
print(type(set1))
#添加 add
set1.add(8)
print(set1)
# 删除  若不存在，会报错
set1.remove(4)
print(set1)
# pop  :随机删除集合中的元素
set1.pop()
print(set1)
# discard : 存在则直接删除；不存在，也不会报错
set1.discard(3)
print(set1)
```
输出：
```
<class 'set'>
{1, 2, 4, 5, 8}
{1, 2, 5, 8}
{2, 5, 8}
{2, 5, 8}
name ==>> 刘强东
age ==>> 45
id ==>> 2134254656
address ==>> 北京
```
- 字典解析
*创建一个班级的分数的随机分布情况*
```python
from random import randint  #使用随机函数
grades = {'sutdent{}'.format(i):randint(50,100) for i in range(1,21)}
print(grades)
# 筛选数据，选出高于90分的人
data = {k:v for k, v in grades.items() if v >=90}
print('10个人中成绩 >= 90的人数：',len(data),'个')
print(data)
# 学生成绩随机生成，可用于成绩模拟
```
输出：
```
# 随机生成的结果
{'sutdent1': 75, 'sutdent2': 64, 'sutdent3': 91, 'sutdent4': 72, 'sutdent5': 79, 'sutdent6': 93, 'sutdent7': 80, 'sutdent8': 79, 'sutdent9': 68, 'sutdent10': 68, 'sutdent11': 65, 'sutdent12': 81, 'sutdent13': 93, 'sutdent14': 69, 'sutdent15': 79, 'sutdent16': 79, 'sutdent17': 95, 'sutdent18': 78, 'sutdent19': 56, 'sutdent20': 53}
10个人中成绩 >= 90的人数： 4 个
{'sutdent3': 91, 'sutdent6': 93, 'sutdent13': 93, 'sutdent17': 95}
```

集合解析
```
from random import randint  #使用随机函数
set1 = {randint(0,20) for _ in range(20)}
print('随机生成集合：',set1)
# 找到能被3 整除 的
res = { x for x in set1 if x%3 == 0}
print('其中能被3整除的：',res)
```
输出：
```
随机生成集合： {0, 1, 2, 3, 5, 6, 8, 12, 13, 15, 16, 17, 18}
其中能被3整除的： {0, 3, 6, 12, 15, 18}
```
#函数

> def 函数名（）:
  pass
```py
# def 函数名（）：
#   pass
#函数名（num）
def caculateNum(num):
    '''
    计算1~num 之间的累加和
    :param num: 累加和的末尾  #函数文档（标注函数用途）
    :return: 返回累加和
    '''
    res = 0
    for i in range(1, num+1):
        res += i
    return res
res = caculateNum(100)
print(res)
```
```
5050

```


![喜欢就点个赞再走吧@_@](https://upload-images.jianshu.io/upload_images/17476267-96e264a31fb4ca27.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
