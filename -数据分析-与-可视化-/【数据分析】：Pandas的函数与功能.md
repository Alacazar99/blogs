###数据的取值与选择

#####series 数据选择方式
- 1、将series看做字典
```
# 1、将series看做字典
data = pd.Series([0.25,0.5,0.75,1],index = ['a','b','c','d'])
data['c']

## 判断键a 是否存在
'a' in data 
# 获取所有的键
data.keys()

##调用items()方法
list(data.items())

## 动态的添加数据
data['e'] = 1.25
data
```
- 2、将series当做一维数组
【解释】：series 拥有和Numpy数组一样的功能，包括：索引、切片、掩码、花哨索引
![](https://upload-images.jianshu.io/upload_images/17476267-20ac8f9c8ef0b6e2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

【注意】：显示索引选择时，包括最后一个索引。隐式索引则不包括。

#### 索引器
索引器包括：loc、iloc、ix
函数|	说明
|:-:|:-:|
.loc()	|基于标签，参数可为单个标签、标签列表、切片对象或者布尔数组	
.iloc()	|基于索引，参数可为整数、整数列表、系列值
```
data = pd.Series(['a','b','c'],index=[1,4,6])
data

[Out]:
1    a
4    b
6    c
dtype: object
```
- loc:表示使用显示索引
```
data.loc[1]

[Out]:'a'

data.loc[1:3]
---
[Out]:1    a
dtype: object
```
- iloc :表示隐式索引
```
data.iloc[1]
data.iloc[1:3]
```
[Out]:
```
4    b
6    c
dtype: object
```

### DataFrame 数据选择方式
```
population_dict={'Californis':345987634,'New York':40768934,'Florida':76543212,'Illinois':156898456}
area_dict= {'Californis':336784,'New York':4908874,'Florida':43212,'Illinois':12986}
pop = pd.Series(population_dict)
area = pd.Series(area_dict)

data = pd.DataFrame({'area':area,'pop':pop})
data
```
[Out]:
```

             area	   pop
Californis	336784	345987634
New York	4908874	40768934
Florida	43212	76543212
Illinois	12986	156898456
```
- 将DataFrame 看做字典
![DataFrame- 字典 ](https://upload-images.jianshu.io/upload_images/17476267-a3a099b7051546e9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- 将DataFrame看做二维数组
   -  1、按行查看数组数据
   -  2、转置操作
![DataFrame-二维数组 ](https://upload-images.jianshu.io/upload_images/17476267-4de58d797b5bbd2e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

   -  3、用索引器来取值

![](https://upload-images.jianshu.io/upload_images/17476267-6688a6998f905e22.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
【技能提升】:ix 索引器，可以将显示索引和隐式索引混合使用。但是 ix不被新版本支持了，所以作为了解就好。
```
data.ix[:'Florida',:2] 

---【警告】：
<pre style="box-sizing: border-box; overflow: auto; font-family: monospace; font-size: 14px; display: block; padding: 1px 0px; margin: 0px; line-height: inherit; color: rgb(0, 0, 0); word-break: break-all; overflow-wrap: break-word; background-color: transparent; border: 0px; border-radius: 0px; white-space: pre-wrap; vertical-align: baseline; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: left; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">C:\Users\admin\Anaconda3\lib\site-packages\ipykernel_launcher.py:2: DeprecationWarning: 
.ix is deprecated. Please use
.loc for label based indexing or
.iloc for positional indexing

See the documentation here:
[http://pandas.pydata.org/pandas-docs/stable/indexing.html#ix-indexer-is-deprecated](http://pandas.pydata.org/pandas-docs/stable/indexing.html#ix-indexer-is-deprecated)</pre>

```
【】：
任何处于处理Numpy形式数据的方法 ，都可以用于这些索引器。比如：掩码、花式索引；
![](https://upload-images.jianshu.io/upload_images/17476267-09e4f78141d49529.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
【】：任何一种取值方法，都可以用于调整数据。
![](https://upload-images.jianshu.io/upload_images/17476267-807159abba2f4ea1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#####其他的一些选值操作
![](https://upload-images.jianshu.io/upload_images/17476267-57b0f88bee6cafc3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
【解释】：
- 对单个标签取值，选择的是列；
- 对多个标签用切片操作取值，选择的是行；
- 【隐式索引编号】：切片也可以直接用行数实现；
- 【掩码操作】：可以直接对一行过滤
---
###Pandas 的数值运算方法

#####通用函数：保留索引
![](https://upload-images.jianshu.io/upload_images/17476267-4b99ebcf41a0608a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#####通用函数：对齐索引
```
area = pd.Series({'Californis':336784,'Florida':43212,'Illinois':12986})
population = pd.Series({'Californis':345987634,'New York':40768934,'Illinois':156898456})
population / area
```
[Out]:
```
Californis     1027.328003
Florida                NaN
Illinois      12082.123518
New York               NaN
dtype: float64
```
【解释】：
- NaN：表示“此处没有数据”；
-  任何缺失值都会用NaN来填充；
- 如果NaN不是我们想要的数据结果，可以用通用函数的fill_value来设定
- DataFrame在计算时，也会在列上**索引对齐**，并且计算结果会对索引自动排序；
![](https://upload-images.jianshu.io/upload_images/17476267-089520c2c396c4e9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 使用fill_value 来填充缺失值：

![](https://upload-images.jianshu.io/upload_images/17476267-a792d429400ca525.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---
### DataFrame 和Series的运算

![](https://upload-images.jianshu.io/upload_images/17476267-8b513c7426891c09.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

【注释】：
- 通过axis参数，可以指定计算的方式
- DataFrame 和Series 运算时，结果的索引也会自动对齐；
```
df -df.iloc[0]

---[Out]:
	Q	E	S	T
0	0	0	0	0
1	-1	-2	0	7
2	-9	-1	-1	2
```
```
df.subtract(df['E'],axis=0)  # (按列计算)

---[Out]:
    Q	E	S	T
0	1	0	0	-8
1	2	0	2	1
2	-7	0	0	-5

halfrow = df.iloc[0,::22]
halfrow

---[Out]:
Q    9
Name: 0, dtype: int32
```
###### 缺失值处理
 Pandas 采用标签法来表示缺失值，有两种方式：
- 浮点数据类型的NaN值;
- pyton对象类型：None;
【备注】：
- Python对象类型的缺失值，只能用于Numpy; Pandas:中的‘objects’类型数据；
- object 类型数据会消耗更多的资源；

![](https://upload-images.jianshu.io/upload_images/17476267-21e633ca419f90fc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

【注意】：
- 无法对包含None的数组进行累计操作；（例如：`vals1.sum()`）
- NaN：数值类型的缺失值;
- 任何数字与NaN进行运算，结果都是NaN;
-【例外】：Numpy中 提供了忽视Nan缺失值影响的函数（`nansum、nanmax、nanmin`）；

![](https://upload-images.jianshu.io/upload_images/17476267-9e9be9a5e9a4d043.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#####  Pandas 中None与NaN的比较

 pandas 将None与NaN看成是可等价交换的，在适当的时候，会将两者进行替换，除此之外，Pandas 会将没有标签值的数据，自动转换成NaN。实例如下：
```
pd.Series([1,np.nan,2,None])

输出:
0    1.0
1    NaN
2    2.0
3    NaN
dtype: float64
```
```
x[0] = None
x
输出：
0    NaN
1    1.0
dtype: float64

```
andas：对不同类型的缺失值的转换规则
类型    |  缺失值 | NaN标签值
|:-:|:-:|:-:|
floating（浮点型）| 无变化 | np.nan
object(对象类型) |  无变化 | None，Np.nan
iteger(整数类型)|强制转换为float64 | np.nan
boolean(布尔型) | 强制转换为object | None或者np.nan
【注意】：Pandas 中，字符串使用object类型存储。

### 缺失值处理
函数|	说明
|:-:|:-:|
isnull()|	如果为NA，返回布尔值True，否则为False
notnull()	|与isnull()相反
fillna()	|寻找NA值，替换为value，参数method填充方式:pad/ffill向前填充，bfill/backfill向后填充
ffill()	|等同于fillna(method=’ffill’)
bfill()	|等同于fillna(method=’bfill’)
dropna()	|丢弃包含NA值的行或者列，axis默认为0，即丢弃行
replace()	|替换，用标量值替换NA则等同于 fillna()函数
```
data = pd.Series([1, np.nan,'hello',None])
### 创建一个布尔类型的掩码标签缺失值；；
data.isnull()
"""输出
0    False
1     True
2    False
3     True
dtype: bool
"""
# 与isnull 操作相反
data.notnull()
"""输出
0     True
1    False
2     True
3    False
dtype: bool
"""
# 返会一个除去缺失值的数据
data.dropna()
"""输出
0        1
2    hello
dtype: object
"""
df = pd.DataFrame([[1,np.nan,2],[2,3,5],[np.nan,4,6]])
df
"""输出
	0	1	2
0	1.0	NaN	2
1	2.0	3.0	5
2	NaN	4.0	6
"""
#默认删除包含缺失值的正行数据
df.dropna()
"""输出
    0	1	2
1	2.0	3.0	5
"""
# 删除包含缺失值的整列数据
df.dropna(axis=1)
"""输出
    2
0	2
1	5
2	6
"""
```
```
df[3] = np.nan
df
"""输出
    0	1	2	3
0	1.0	NaN	2	NaN
1	2.0	3.0	5	NaN
2	NaN	4.0	6	NaN
"""
# 只有全部是缺失数据才删除；
df.dropna(axis = 1,how = 'all')
"""输出
	0	1	2
0	1.0	NaN	2
1	2.0	3.0	5
2	NaN	4.0	6
"""
# 【thresh】
df.dropna(axis=1,thresh=3)
"""输出
	2
0	2
1	5
2	6
"""
```
关于 【thresh】：通过thresh 设置非缺失值的最小数量（thresh=n：表没有缺失值）

#####填充缺失值：fillna
- 使用0填充缺失值；
- 用缺失值前面的有效值来填充缺失值。
- 用缺失值后面的有效值来填充缺失值。

![](https://upload-images.jianshu.io/upload_images/17476267-cb453d62936c8cc5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
【备注】：# DataFrame 填充方法与Series相似，但是填充时，需要设定坐标轴参数：axis
例如：`df.fillna(method='ffill',axis=1)`

### 层级索引
```
index = [('California',2000),('California',2020),('New York',2000),('New York',2020),('Texas',2000),('Texas',2020),]
populations=[3564549,3965987,1984654,2484654,1566432,3564895]
pop = pd.Series(populations,index=index)
pop

"""输出
(California, 2000)    3564549
(California, 2020)    3965987
(New York, 2000)      1984654
(New York, 2020)      2484654
(Texas, 2000)         1566432
(Texas, 2020)         3564895
dtype: int64
"""
## 查出2000年所有州的数据
pop[[i for i in pop.index if i[1]==2000]]
"""输出
(California, 2000)    3564549
(New York, 2000)      1984654
(Texas, 2000)         1566432
dtype: int64
"""
```
【方法优化】：
- Pandas 多级索引；
- 通过元组的列表创建多级索引；

```
index = pd.MultiIndex.from_tuples(index)
index
"""输出
MultiIndex(levels=[['California', 'New York', 'Texas'], [2000, 2020]],
           labels=[[0, 0, 1, 1, 2, 2], [0, 1, 0, 1, 0, 1]])
"""

pop = pop.reindex(index)
pop
"""输出
California  2000    3564549
            2020    3965987
New York    2000    1984654
            2020    2484654
Texas       2000    1566432
            2020    3564895
dtype: int64
"""

## 选取2020年的所有数据
pop[:,2020]
"""输出
California    3965987
New York      2484654
Texas         3564895
dtype: int64
"""
```
##### 高级数据的多级索引
- unstack() : 可以讲一个拥有多级索引的Series转换为普通索引的DataFrame;
- stack() ： 可以将普通索引的DataFrame转换为拥有多级索引的Series
```
pop_df = pop.unstack()
pop_df
"""输出
            2000	2020
California	3564549	3965987
New York	1984654	2484654
Texas	1566432	3564895
"""
# stack() ： 可以将普通索引的DataFrame转换为拥有多级索引的Series
pop_df.stack()
"""输出
California  2000    3564549
            2020    3965987
New York    2000    1984654
            2020    2484654
Texas       2000    1566432
            2020    3564895
dtype: int64
"""
```
![](https://upload-images.jianshu.io/upload_images/17476267-ec2f76d90ce8b10e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 创建多级索引

将Series | DataFrame 的index参数设置为至少二维的索引数组
```
df = pd.DataFrame(np.random.rand(4,2),index=[['a','a','b','b'],[1,2,1,2]],columns=['data1','data2'])
df
"""输出
data1	data2
a	1	0.800254	0.222281
2	0.352115	0.467182
b	1	0.982438	0.396324
2	0.313206	0.891846
"""
data = {
    ('California',2000):3564549,
    ('California',2020):3965987,
    ('Texas',2000):1984654,
    ('Texas',2020):2484654,
    ('New York',2000):1566432,
    ('New York',2020):3564895
}
pd.Series(data)

"""输出
California  2000    3564549
            2020    3965987
Texas       2000    1984654
            2020    2484654
New York    2000    1566432
            2020    3564895
dtype: int64
"""
```
