@Time    : 19-5-11
@Author  : Zurich
@Email   : 1178824808@qq.com
@Software: PyCharm

#创建函数
```
def 函数名（参数列表）：
  函数体
  pass
函数名（num）
```
举个栗子：
```py
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
输出：5050
当然，使用列表推导式更简单哦
```py
def caculateNum(num):
    '''
    计算1~num 之间的累加和
    :param num: 累加和的末尾  #函数文档（标注函数用途）
    :return: 返回累加和
    '''
    return sum([i for i in range(1,num+1)])
```
study more：将上面的那个函数封装成一个模块，然后再新建一个 .py 文件导入此模块，**用函数实现模块化程序设计**
```py
import caculate
res = caculate.caculateNum(100)
print(res)
```
输出：5050
# 参数传递
- 必须参数和关键字参数
```
# 必须参数正确的顺序传入，调用的时候必须和申明的时候保持一致
def f(name, age):
    print('I am %s , I am %d years old'%(name, age))
f('xuance',18)
# 关键字参数   使用关键字参数可以允许函数调用时顺序不一致
f(age=20, name='xuance')
```
输出：
```py
I am xuance , I am 18 years old
I am xuance , I am 20 years old
```
 - 默认参数
> 缺省参数没有传入，默认值失效
```py
def f(name, age, sex='male'):
    print('I am %s , I am %d years old'%(name, age))
    print('sex: %s'%sex)
f('shouyue', age = 21)
f('xiaoyun', 66, sex='female')
# # 是否显示指定参数，方便自己阅读
```
为了使得代码便于阅读，最好显示制定参数。
输出：
```py
I am shouyue , I am 21 years old
sex: male
I am xiaoyun , I am 66 years old
sex: female
```
# 匿名函数
语法  ：
>lambda 参数:表达式

冒号前面的参数，可以有多个
冒号后面的表达式，只能有一个表达式， 不写return 返回值就是表达式的结果
**优点**： 减少代码量， 代码看起来‘优雅’
- 例一
```py
def rect(x, y):
    return x * y
area = rect(3, 5)
print(area)
# 使用lambda 表达式
res = lambda x, y:x*y
print(res(4, 6))
```
输出：
```
15
24
```
- 例二
```py
def cal(x, y):
    if x > y:
        return x*y
    else:
        return x/y
c = cal(5,6)
print('普通函数：',c)

calc = lambda x, y: x*y if x > y else x/y
print('使用lambda：',calc(5,6))
```
输出：
```
普通函数： 0.8333333333333334
使用lambda： 0.8333333333333334
```
#### 列表的排序中使用lambda
```py
stus = [
    {'name':'baili', 'age':33},
    {'name':'luban', 'age':4},
    {'name':'diaochan', 'age':18},
    {'name':'xuance', 'age':77},
]
# for i in range(stus['age']):
#     print(i)
# Key值： 是按照那个元素为依据进行排序
# sorted(stus,key='')
print('排序前：', stus)
r = sorted(stus, key=lambda x:x['age'])
print('排序后：', r)
rr = sorted(stus, key=lambda x:x['age'], reverse = True)
print('年龄从大到小排序：',rr)
```
输出：
```
排序前： [{'name': 'baili', 'age': 33}, {'name': 'luban', 'age': 4}, {'name': 'diaochan', 'age': 18}, {'name': 'xuance', 'age': 77}]
排序后： [{'name': 'luban', 'age': 4}, {'name': 'diaochan', 'age': 18}, {'name': 'baili', 'age': 33}, {'name': 'xuance', 'age': 77}]
年龄从大到小排序： [{'name': 'xuance', 'age': 77}, {'name': 'baili', 'age': 33}, {'name': 'diaochan', 'age': 18}, {'name': 'luban', 'age': 4}]

```
