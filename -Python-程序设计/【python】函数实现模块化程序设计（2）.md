@Time    : 19-5-18        
@Author  : Zurich.alcazar            
@Email   : 1178824808@qq.com         
@Software: PyCharm   
```  
def get_fullname(last_name ,first_name):
    '''                                 
    求全名                                 
    :param last_name: 姓                 
    :param first_name: 名                
    :return: 全名                         
    '''                                 
    full_name = last_name + first_name  
    return full_name 
```  
>  两种传参方式：按位置，关键字            
> 如果有默认值则必须放在最后
```
full_name = get_fullname('Zurich','.alcazar') 
print(full_name)    
full_name1 = get_fullname(last_name='Zurich',first_name='alcazar') print(full_name1)    
```
### 变长参数
-` *`将参数都放置在一个元组中
举个栗子：
```
def self(*a):
    print(a)
    print(type(a))
# 将参数都放置在一个元组中
def self1(name,*a):
    print(name)
    print(a)
    print(type(a))
self(1,2,3,4)
self1(1,2,3,4)
```
输出：

(1, 2, 3, 4)
<class 'tuple'>
1
(2, 3, 4)
<class 'tuple'>

- `**` 将参数都传到字典中
```
def d_self(**kwargs):
    print(kwargs)
d_self(last_name='Zurich',first_name = "Alcazar")
```
输出：
```py
# 字典
{'last_name': 'Zurich', 'first_name': 'Alcazar'}
```
- *混合使用*
> 形参顺序：`位置参数 -> 元组 -> 字典`
```py
def mix(name, *t, **kwargs):
    print(name)
    print(t)
    print(kwargs)
mix('Zurich', 'age', 'adress', gender='女')
```
执行结果：
```py
Zurich
('age', 'adress')
{'gender': '女'}
```
##### 拆分
```
a= (1,2,3,4,5,6)
print(a)
print(*a)
```
执行结果：

(1, 2, 3, 4, 5, 6)
1 2 3 4 5 6

- 单`* `： 拆开元组、列表的作用
```
def f(*arg):
    print(arg)
# # *:拆开元组、列表的作用
f(*[1,2,3,4,5])
f(*[1,2,3,4,5])
```
- 双` **` ：拆开字典的作用
```
def function(**kwargs):
    print(kwargs)
function(**{'name':'Zurich','age':20,'id':'beijing'})
```
执行结果：
```
{'id': 'beijing', 'name': 'Zurich', 'age': 20}
```
### return 返回值
```
def sum_avg(*numbers):
    total = sum(numbers)
    avg_number = total/len(numbers)
    return total,avg_number

sum,avg = sum_avg(1,2,3,4,5,6,7,8,9,10)
print('总和：%f'% sum)
print('平均数：%f'%avg)
```
执行结果：
```
总和：55.000000
平均数：5.500000
```
*此处插播一条小练习*
 
```
# 课外练习
squares = [x **2 for x in [1,2,3,4,5,6,7,8,]]
print(squares)
s = []
for x in [1,2,3,4,5,6,7,8,9,11]:
    s.append(x**2)
print(s)
```
输出：
```
[1, 4, 9, 16, 25, 36, 49, 64]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 121]
```
### 函数的传参问题
- 数据分为：引用类型，普通类型
- python中的数据类型都是普通类型。数、布尔型、字符串型， 除了这些之外的类型都是引用类型
- 普通类型赋值时，传的是`值`
- 引用类型赋值时，传的是`地址`
- 传参的实质是`赋值操作`，如果传递的式引用类型数据，则需要注意是否在函数中对其做除了修改
在这里推荐一个网站，适用于python分析内存：http://www.pythontutor.com/visualize.html#mode=edit
举个栗子：
```
def power(numbers):
    # 创建数据副本
    numbers = numbers[:]
    numbers = list(numbers)
    numbers[3] = 11
    numbers = [x**2 for x in numbers] #列表解析表达式
    return numbers
nums = [1,2,3,4,5,6,7,8,9,10]
print(power(nums))
print(nums)
```
内存分析：
![内存分析](https://upload-images.jianshu.io/upload_images/17476267-458be56555f78339.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

输出：
```
[1, 4, 9, 121, 25, 36, 49, 64, 81, 100]
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```
### 闭包
> 1、闭包就是能够**读取其他函数内部变量的函数**。
 >2、 闭包可以理解成**“定义在一个函数内部的函数“**。
3、在本质上，闭包是将**函数内部**和**函数外部**连接起来的桥梁。
```
# 内层函数可以访问外层函数的变量，但是不能修改～～
# 内层函数访问变量时，会先从自己内部开始查找，如果找不到，就层层向上查找
def outter():
    aa = 10
    def inner():
    # 说明使用的是全局变量
    #     global aa
        nonlocal aa  #局部变量
        aa -= 1
        print(aa)
    return inner

fo = outter()
fo()
fo()
fo()
fo()
fo()
```
输出：
```py
9
8
7
6
5
```
##### 注释
- 在python 中，变量的作用域以`函数`为单位
- `global` 修饰变量，说明使用的是函数的全局变量
- `nonlocal`修饰变量，说明使用的是嵌套层函数变量
- 闭包的本质：函数嵌套函数，外层函数返回内层函数的地址
##### 递归问题
> *递归*：函数自己调用自己，编写递归或循环时一般先考虑出口（结束的条件）问题。
```
def factorial(n):
    mm = 1
    for num in range(1,n+1):
        mm *= num
    return mm
print(factorial(5))
```
输出：
```
120
```
```
# 递归调用
def factorial1(n):
    if n == 1:
        return 1
    return n*factorial1(n-1)
print(factorial1(8))
```
内存分析：
![递归调用内存分析.png](https://upload-images.jianshu.io/upload_images/17476267-c83830a34351a4d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 小练习：
使用多种方式输出 斐波那契数列：1 1 2 3 5 8 13 1 34 55 89 144 233 377 610 987 ···
- **方法一**：约束输出最大值
输出小于10000的斐波那契数列
```
def fb():
  a,b = 1,1
  while a < 10000:
      print(a,end=' ')
      a,b = b,a+b
fb()
```
输出：
```PY
1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597 2584 4181 6765 
```
- **方法二**：用递归实现
```
def fb1(n):
    if n == 1 or n == 2:
        return 1
    return fb1(n-1) + fb1(n-2)
for i in range(1,20):
    print(fb1(i))
```
输出：
```py
1
1
2
3
5
8
13
21
34
55
89
144
233
377
610
987
1597
2584
4181
```
- **方法三**：用循环实现
```
def fb_for(n):
    before = 0
    after = 1
    if n == 1:
        return 1
    for i in range(2,n+1):
        tmp = before + after
        before = after
        after = tmp
        # before,after = after,tmp
    return tmp
for i in range(1,20):
    print(fb_for(i))
```
--------------------------------------------
### 高阶函数：高阶函数的参数或者函数的返回值是函数
举个栗子：
```py
def handle(func,*param):
    return func(*param)

def my_sum(*param):
    sum = 0
    for i in range(len(param)):
        sum += param[i]
    return sum
print(my_sum(1,2,3,4,5,6))
# 函数的参数还是一个函数
print(handle(my_sum,1,2,3,4,5))

-----------
def my_mul(*param):
    '''
    累乘
    :param param:
    :return:
    '''
    mul = 1
    for v in param:
        mul *= v
    return mul
print(handle(my_mul,1,2,3,4,5,6))
```
输出：
```py
21
15
720
```
# map函数
map(func,inteable) 
该函数会把 inteable中的数据依次传递给func函数处理，最后把处理结果返回
```
def power(x):
    return x * x

result = map(power,[3,4,5,6])
for i in result:
    print(i)
result = map(lambda x: x * x, [ 4, 5, 6, 7, 8])
print(list(result))
```
输出：
```
9
16
25
36
[16, 25, 36, 49, 64]
```
### reduce函数
reduce(func,interable) 累计操作
func函数必须接收两个参数。ruduce 会把func的运行结果做一个参数，后从inteable中再取一个数据当另一个参数。
```py
from functools import  reduce
li = [1,2,3,4,5]
result = reduce(lambda x,y:x * y,li)
print(result)
```
输出：
```py
120
```
# filter函数
`filter(func,ointerable) `
使用函数 `func`来过滤`可迭代对象（interable）`将`interable`中的数据传入函数`func`中，如果函数返回Ture ，则保留该数据，否则就不保留。
##### 举个栗子：判断是否为奇数
```
li = [11,2,35,4,75,6,7,18]
result = list(filter(lambda x: x % 2 == 1,li))
print(result)
输出奇数：[11, 35, 75, 7]
### sort函数
sort（interable, key = None, reverse=False）对函数进行排序
- key: 可以用来指定排序的规则，key值是一个函数。
- reverse： 是用来指定排序的顺序-->升序或降序

li = [4,76,87,-10,66]
re = li.sort() #就地排序
```
-  `list `自带的`sort方法`会影响原始数据，系统的sorted函数不会影响！
```
print(re)
rs = sorted(li,key=abs,reverse = True)
print(rs)
print(li)
```


输出排序结果：
```
None
[87, 76, 66, -10, 4]
[-10, 4, 66, 76, 87]
```
-------------------------------------

实例：
```
# ------------
def ff():
    '''
    打印test!!
    :return:
    '''
    print('打印test!!')
print(ff.__doc__)
help(ff)
```
执行：
```
    打印test!!
    :return:
    
Help on function ff in module __main__:

ff()
    打印test!!
    :return:
```
-------------------
###  模块
就是`.py`文件，里面定义了一些函数和变量，需要的时候就可以导入这些模块。
- 模块方式：
1、from 模块名 import 函数名
 2、import 模块名
- 可以使用`as` 为模块或函数起一个别名
---------------
### 包
在模块之上的概念，为了方便管理而将文件进行打包。包目录下第一个文件便是 `__init__.py`，然后是一些`模块文件`和`子目录`
### 库
库：具有相关功能模块的集合。这也是`Python`的一大特色之一，即具有强大的`标准库`、`第三方库`以及`自定义模块`。
- 第三方库：就是由其他的第三方机构，发布的具有特定功能的模块。
- 自定义模块：用户自己可以自行编写模块，然后使用。
**这三个概念实际上都是模块，只不过是个体和集合的区别**
