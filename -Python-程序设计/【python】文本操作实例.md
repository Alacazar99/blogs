通过实例来了解文本操作：
创建一个文件:`data.txt`,
（1）假设此txt文件一共100000行，每行存放一个1~100之间的整数；
（2）找出文件中数字最多的10个数字，新建一个文本mostNum.txt，并将数字出现最多的10个元素和出现次数传入其中；
- 推荐使用collections模块
`from collections import Counter`
------------------------------------
### collections-Counter使用总结 
- Counter的使用
Counter 是一个有助于 hashable 对象计数的 dict 子类。它是一个无序的集合，其中 hashable 对象的元素存储为字典的键，它们的计数存储为字典的值，计数可以为任意整数，包括零和负数。
`在很多使用到dict和次数的场景下，Python中用Counter来实现会非常简洁，效率也会很高`
- defaultdict 的使用
是内建 dict 类的子类，它覆写了一个方法并添加了一个可写的实例变量。其余功能与字典相同。
> `defaultdict()` 第一个参数提供了` default_factory `属性的初始值，默认值为 `None`，`default_factory `属性值将作为字典的默认数据类型。所有剩余的参数与字典的构造方法相同，包括关键字参数。
- **most_common([n])的使用**
most_common(`n`) 中`n`为可选参数
>- 如果不输入n的值，则默认返回所有;
>- 输入-1则返回空;
>- 输入小于最长长度，则返回前n个数;
>- 输入等于最长长度，则返回所有

代码实现如下：
```
import random
from collections import Counter
with open("E://data.txt","w+") as f:
    counts = {}
    for i in range(100000):
        num = random.randint(1,101);
        f.write(str(num) + '\n')
        counts[num] = counts.get(num,0) + 1

with open("E://data.txt","r",encoding='utf-8') as s:
    txt = s.read()


roles = Counter(counts)
role = roles.most_common(10)
print(role)
for i in range(10):
    print(role[i])

with open("E://mostNum.txt",'w',encoding='utf-8') as m:
    # role = str(role[i][1])
    for i in range(10):
        m.write(str(role[i][0]) + " 出现：" + str(role[i][1]) + " 次\n")
```

执行结果：
```
[(42, 1056), (61, 1053), (28, 1043), (88, 1042), (47, 1037), (71, 1032), (48, 1032), (54, 1030), (2, 1030), (66, 1028)]
(42, 1056)
(61, 1053)
(28, 1043)
(88, 1042)
(47, 1037)
(71, 1032)
(48, 1032)
(54, 1030)
(2, 1030)
(66, 1028)
```
![结果和期望相同](https://upload-images.jianshu.io/upload_images/17476267-6eead6cd2a718914.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
