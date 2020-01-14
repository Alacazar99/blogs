###@Time    : 2019/4/21 
### @Author  : Zurich.alcazar
### @Software: PyCharm

![python](https://upload-images.jianshu.io/upload_images/17476267-ce266cecf25423ae.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#首先介绍：Python中的数据类型转换
![比较常用的一些函数](https://upload-images.jianshu.io/upload_images/17476267-e115ba8a6b2b9bdb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# python中交换两个变量的值
代码块：
```
a = 1
b = 2
print(a,b)
a,b = b, a
print(a,b)  
```
输出：
```
1 2
2 1
```
#标识符
  Python中标识符由字母，下划线(_)，以及数字组成，且数字不能开头，字母区分大小写
代码块：
```
hero_name = "鲁班七号"
level = 15
print('您选择的英雄是%s\n 当前等级为%d'%(hero_name,level)'级')          # /n表示换行
```
输出：
```
您选择的英雄是鲁班七号
当前等级为15级
```
#多行赋值操作
代码块：
```
name,age,sex = '黄先生',20,'男'
print(name, age, sex)
```
#判断和循环操作
##if 和 else
- if 要判断的条件：
      条件成立时执行某事件（语句） 
代码块：
```
age = input('请输入您的年龄：')
age = int(age)
if age >= 18:
  print('你可以去网吧上网了~')
if age == 20:
  print('网吧充会员半价~')
else:
  print('未成年的弟弟啊，你还不能去网吧哦@_@')
```
输出：
```
请输入您的年龄：>？16
未成年的弟弟啊，你还不能去网吧哦@_@
```
## elif条件语句
if XXX1:
  XXX1
elif XXX2:
  XXX 2
elif XXX3:
  XXX3
  else XXX4
代码块：
```
score = input('请输入您的成绩：')
score = int(score)
if score >=90 and score <= 100:
  print('考试等级为：A')
elif score >= 80 and score <= 90:
  print ('考试等级为：B')
elif score >= 70 and score <= 80:
  print('考试等级为：C')
elif score >= 60 and score <= 70:
  print('考试成绩为：D')
else:
  print('本次考试您未及格，请准时参加补考！')
```
输出：
```
请输入您的成绩：>？59
本次考试您未及格，请准时参加补考！
 ```
##python 中独特的用法
代码块：
```
score = 77
score = '你的成绩为77'if score == 77 else score
print(score,'分')
```
输出：
```
你的成绩为77分
```
#拓展 猜拳游戏
代码块：
```
import random
player = input('请输入：剪刀(0), 石头(1), 布(2)')
player = int(player)
#计算机生成[0~2]的随机数
computer = random.randint(0,2)
#胜负情况
if (player == 0 and computer == 2) or (player == 1 and computer == 0) or (player == 2 and computer == 1):
  print('恭喜您获胜')
#考虑是平局
elif (player == 0 and computer == 0) or (player == 1 and computer == 1) or (player == 2 and computer == 2):
  print('想到一块儿去了哇，要不再来一局')
#输
else:
  print('你输咯~')
```
#while循环语句
while 条件：
条件满足时执行某事件
```
i = 1
while j <= 10:
  print('这是我第{}次向您致歉，对不起'.format(i))
i += 1
```
输出：
```
这是我第1次向您致歉，对不起
这是我第2次向您致歉，对不起
这是我第3次向您致歉，对不起
这是我第4次向您致歉，对不起
这是我第5次向您致歉，对不起
这是我第6次向您致歉，对不起
这是我第7次向您致歉，对不起
这是我第8次向您致歉，对不起
这是我第9次向您致歉，对不起
这是我第10次向您致歉，对不起
```
- 计算1 ~  100之间的偶数的累加和（包含1和100）
代码块：
```
i = 1
sum = 0
while i <= 100:
  if i % 2 == 0：
    sum += 1
  i += 1
print(sum)
```
结果：
```
2550
```
#练习：打印九九乘法表
代码块：
```
i = 1
while i <= 9：
  j = 1
  while j <= i:
    result = i* j
    print(i,'*',j,'=',result,end = '  ')
  j += 1
print('\n')
i += 1
```
# for循环语句
for 临时变量 in 可迭代对象：
循环满足时执行的事件
代码块：
```
company = 'neusoft'
for i in company:
    print(i)
    if i == 's':
        print('看到这篇简书的同学，都帅爆了')
```
```
n
e
u
s
看到这篇简书的同学，都帅爆了
o
f
t
```
- range (起始值， 终止值， 步长)
```
for i in range(1, 101,2):
  print(i)
```
-break 语句：跳出当前循环
-continue语句：跳出当前循环，且执行下一次循环
代码块：
```
company = 'neusoft'
for i in company:
  print('- - - - - - - -')
  if i == 's':
    continue
  print(i)
else:
    print('没有执行break语句')
```
  输出：
```
- - - - - - - -
n
- - - - - - - -
e
- - - - - - - -
u
- - - - - - - -
s
- - - - - - - -
o
- - - - - - - -
f
- - - - - - - -
t
没有执行break语句
```
#字符串
- 访问访问字符串
代码块：
```
word = ''hello 'word!'''
print(word)
print(word[2])
```
输出：
```
hello 'word!'
l
```
- 切片
```
name = 'abc'
print('name*3')
```
输出：
```
abcabcabc
```
- 截取一部分的操作
- 对象[起始: 终止: 步长]
```
name = 'abcdefghjk'
print(name[0:3])
print(name[0:-1])
print(name[::2])
print(name[::-1])
```
```
abc
abcdefghj
acegj
kjhgfedcba
```
### 字符串(str)常见操作
- find()的用法
检查str中是否包含在my_str的前十个字符中，如果存在，则返回起始值的索引值，否则返回-1
```
my_str = 'hello world neuedu and neueducpp'
index = my_str.find('neuedu',0,10)
print(index)
```
输出：
```
-1
```
比较上下两个代码块
```
my_str = 'hello world neuedu and neueducpp'
index = my_str.index('neuedu')
print(index)
```
输出：
```
12
```
- count 返回目标字符串出现的次数
此处不再举例
![希望各位关注哦 会持续更新哒](https://upload-images.jianshu.io/upload_images/17476267-6e738ed3de727de0.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
注释：图片均来源网络，侵权请联系作者
