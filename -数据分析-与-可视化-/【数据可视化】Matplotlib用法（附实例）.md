###📖  一、Matplotlib库相关介绍
【介绍】：Matplotlib 是建立在Numpy数组基础上的多平台数据可视化程序库。

###📖  二、设置图形标签

######【第一种】：MATLIB 风格

```python
plt.plot(x,np.sin(x))
plt.title("A Sine Curve")
plt.xlabel("x")
plt.ylabel("sin(x)")
```

![MATLIB 风格](https://upload-images.jianshu.io/upload_images/17476267-2d8a75b91282cbe2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######【第二种】：面向对象风格 

```python
# 设置时，要做如下形式的变换
#plt.xlabel => ax.set_xlabel
ax = plt.axes()
ax.plot(x,np.sin(x))
ax.set_xlabel("x")
ax.set_ylabel("sin(x)")
ax.set_title('A Sine Curve')
```
![面向对象风格](https://upload-images.jianshu.io/upload_images/17476267-2d8a75b91282cbe2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 📖 三、设置图例

######【第一步】：绘制图例（参数为：label）

```python
plt.plot(x,np.sin(x),'-g',label="sin(x)")
plt.plot(x,np.cos(x),':b',label="cos(x)")

#显示图例
plt.legend()
```


![显示图例](https://upload-images.jianshu.io/upload_images/17476267-6da6c0a9b200d219.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######【第二步】：配置图例

```python
x = np.linspace(0,10,1000)
fig,ax = plt.subplots()
ax.plot(x,np.sin(x), '-b', label="Sine")
ax.plot(x,np.cos(x),'--r', label="Cosine")
ax.axis('equal')

leg = ax.legend(loc='upper left',frameon=False, ncol=2)
```
【注释】：Matplotlib中显示图形时调用`legend()`函数，其中有以下几个较为常用的参数
- loc： 位置参数；
- frameon： 去掉边框；
- ncol： 图例的列数；


![配置图例](https://upload-images.jianshu.io/upload_images/17476267-5370735caf677118.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
######【第三步】：其他配置

```python
x = np.linspace(0,10,1000)
fig,ax = plt.subplots()
ax.plot(x,np.sin(x), '-b', label="Sine")
ax.plot(x,np.cos(x),'--r', label="Cosine")
ax.axis('equal')
- fancybox：圆角边框
- framealpha: 边框的透明度
- shadow:阴影
- borderpad：边距
leg = ax.legend(fancybox=True, framealpha=0.5, shadow=True,borderpad=1)
```
【注释】：Matplotlib中显示图形时调用`legend()`函数，其中有以下几个较为常用的参数：
- fancybox：圆角边框；
- framealpha: 边框的透明度；
-  shadow:阴影；
- borderpad：边距；

![output_相关配置](https://upload-images.jianshu.io/upload_images/17476267-fdbfe7b0785b34e7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######【第四步】：设置多个图例

```python
fig,aix = plt.subplots()
styles = ['-','--','-.',':']
lines = []

x = np.linspace(0,10,1000)
for i in range(4):
    lines += aix.plot(x,np.sin(x - i * np.pi / 2),styles[i],color='black')

# 设置第一个图的图例
aix.legend(lines[:2],['Line A','Line B'], loc='upper right', frameon=False)

# 设置第二个图例
from matplotlib.legend import Legend
leg = Legend(aix,lines[2:],['LineC','LineD'],loc='lower right',frameon=False)
aix.add_artist(leg)

aix.axis('equal')
plt.show()
```

![output_多个图例](https://upload-images.jianshu.io/upload_images/17476267-3e9f6703f31fa2cf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 📖 四、绘制简易散点图

######【第一步】：基本绘制

```python
x = np.linspace(0,10,30)
y = np.sin(x)
# 参数o用来表示图形符号
plt.plot(x,y,'o',color='black')
```
【解释】：这里使用参数o用来表示图形符号。

![output_基本绘制](https://upload-images.jianshu.io/upload_images/17476267-2e43e57605541217.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


######【第二步】：设置样式
样式种类有许多，在绘图过程中，可以更具自己的偏好选择样式，主要样式如下：
```python
rng = np.random.RandomState(0)
for marker in ['o','.',',','x','+','v','^','<','>','s','d']:
    plt.plot(rng.rand(5),rng.rand(5),marker, label="marker='{0}'".format(marker))
    plt.legend(numpoints=1)
    plt.xlim(0,1.8)
```


![output_设置样式](https://upload-images.jianshu.io/upload_images/17476267-59ea884e1efc5f43.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######【第三步】：同时画出点线和折线

```python
plt.plot(x,y,'-ok')
```


![output_点线和折线](https://upload-images.jianshu.io/upload_images/17476267-f8b4b984f109af6b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######【第四步】：散点图的其他属性

```python
plt.plot(x,y,'-p',color='gray',markersize=15,linewidth=4,markerfacecolor='white',markeredgecolor='gray',markeredgewidth=2)
```
- markersize：对应坐标图形尺寸，这里设置为15px；
- linewidth：线条尺寸（粗细）：这里设置为4px；
- markerfacecolor：图形颜色；
- markeredgecolor：图形边框以及线条颜色；
- markeredgewidth：边框线尺寸（粗细），这里设置为2px；

![output_其他属性](https://upload-images.jianshu.io/upload_images/17476267-ec7396cf7a095f2e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


######【第五步】：使用 plt.scatter 绘制散点图
（特点）： scatter 与 plot 相比，拥有更高的灵活性，可以控制每个散点。
```python
# scatter 与 plot 相比，拥有更高的灵活性，可以控制每个散点。
plt.scatter(x,y,marker='o')
```

![output_scatter绘图](https://upload-images.jianshu.io/upload_images/17476267-ae47979490de1bdb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######【第六步】：通过 scater 控制每个点

```python
rng = np.random.RandomState(0)
x = rng.randn(100)
y = rng.randn(100)
colors = rng.rand(100)
sizes = 1000 * rng.rand(100)
plt.scatter(x,y,c=colors,alpha=0.3,s=sizes,cmap='viridis')

# 显示颜色条
plt.colorbar()
```



![output_26_1.png](https://upload-images.jianshu.io/upload_images/17476267-b3f9c6c26e8484d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


###📖  五、基本误差线图

```python
x = np.linspace(0,10,50)
dy = 0.8
y = np.sin(x) + dy * np.random.randn(50)
plt.errorbar(x,y,yerr=dy,fmt='.k')
```
![output_误差线图](https://upload-images.jianshu.io/upload_images/17476267-eb46f854f2c0ee5c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 📖 六、等高线图

######【第一步】：绘制基本等高线图

```python
# 生成坐标
def f(x,y):
    return np.sin(x) ** 10 + np.cos(10 + y * x) * np.cos(x)
x = np.linspace(0,5,50)
y = np.linspace(0,5,40)

# 生成x，y组成坐标的集合
X,Y = np.meshgrid(x,y)
z = f(X,Y)
```

```python
# 绘制图形
# 当只有一种颜色时，实线表示正数，虚线表示负数
plt.contour(X,Y,z,colors='black')
```

![output_基本等高线图](https://upload-images.jianshu.io/upload_images/17476267-8c5954863b09919f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######【第二步】：使用预定义的配色方案

`plt.cm.<Tab>`：可以查看有一共有多少种预定义的配色方案。

```python
# 使用预定义的配色方案：RdGy红灰方案
#可以通过plt.cm.<Tab>，来查看一共有哪些方案
plt.contour(X,Y,z,cmap='RdGy')
```

![output_配色](https://upload-images.jianshu.io/upload_images/17476267-7df56252e7b403e2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######【第三步】：生成带填充色的等高线图

```python
# plt.contourf(X,Y,z,cmap='RdGy')
# （参数）10 : 表示等高线的密集程度
plt.contourf(X,Y,z,10,cmap='RdGy')
plt.colorbar()
```

![output_填充色](https://upload-images.jianshu.io/upload_images/17476267-d600a5c40e21b7a6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


######【第四步】：将等高线渲染成渐变图
感觉是通过打马赛克的方式，来实现模糊化渲染。
```python
# 将数组z对应的点渲染为图象
plt.imshow(z,extent=[0,5,0,5],origin='lower',cmap="RdGy")
plt.colorbar()
plt.axis(aspect='image')
```


![output_渐变图](https://upload-images.jianshu.io/upload_images/17476267-8810aee922c68622.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


######【第五步】：将等高线图和渐变图叠加一起
我们看见的等高线图当然不象这么简陋，但是通过Matplotlib可以实现类似的功能。
```python
contours = plt.contour(X,Y,z,3,colors='black')
plt.clabel(contours,inline=True, fontsize=8)

plt.imshow(z,extent=[0,5,0,5],origin='lower',cmap='RdGy',alpha=0.3)
plt.colorbar()
```

![output_33_1.png](https://upload-images.jianshu.io/upload_images/17476267-93ca5551ebad9460.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 📖 七、频次直方图

```python
data = np.random.randn(1000)
plt.hist(data)
plt.show()
```


![output_基本直方图](https://upload-images.jianshu.io/upload_images/17476267-b984d21ea0899de4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######【第一步】：设置直方图的样式

```python
plt.hist(data,bins=30,alpha=0.5,histtype='stepfilled',color='steelblue',edgecolor='black')
plt.show()
```


![output_直方图样式](https://upload-images.jianshu.io/upload_images/17476267-04a4ab6075c92036.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

案例：

```python
x1 = np.random.normal(0,0.8,1000)
x2 = np.random.normal(-2,1,1000)
x3 = np.random.normal(3,2,1000)

kwargs = dict(histtype="stepfilled",alpha=0.3,bins=40)
plt.hist(x1,**kwargs)
plt.hist(x2, **kwargs)
plt.hist(x3, **kwargs)
plt.show()
```

![output_案例](https://upload-images.jianshu.io/upload_images/17476267-f8da4f3a3a883ad4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######【第二步】：使用 `plt.hist2d() `生成二维频次直方图
生成两组符合正太分布、协方差矩阵为cov的数据

```python
# 生成两组符合正太分布、协方差矩阵为cov的数据
mean = [0,0]
cov = [[1,1],[1,2]]
x, y = np.random.multivariate_normal(mean,cov,10000).T
```


```python
# 使用hist2d绘制二维频次直方图
plt.hist2d(x,y,bins=30,cmap='Blues')
cb = plt.colorbar()
cb.set_label("counts in bins")
```

![output_二维频次直方图](https://upload-images.jianshu.io/upload_images/17476267-0aa472942118255e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######【第三步】： 生成六边形区间划分的二维频次直方图

默认情况下，二维频次直方图是由正方形组成的，可以使用plt.hexbin() 方法绘制六边形组成的二位频次直方图。

```python
# 六边形区间划分
plt.hexbin(x,y,gridsize=30,cmap='Blues')
cb = plt.colorbar(label = 'Counts in bins')
```

![output_六边形区间划分](https://upload-images.jianshu.io/upload_images/17476267-52b5609193f0496f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 📖 八、多子图

```python
# 新建一个子图
ax1 = plt.axes()
ax2 = plt.axes([0.65,0.65,0.2,0.2])
```

![output_新建一个子图](https://upload-images.jianshu.io/upload_images/17476267-78dbf75a24412b55.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######【第一步】：通过 `fig.add_axes([xpos,ypos,width,height]) `创建子图

```python
fig = plt.figure()
ax1 = fig.add_axes([0.1,0.5,0.8,0.4])
ax2 = fig.add_axes([0.1,0.1,0.8,0.4])
x = np.linspace(0,10)
ax1.plot(np.sin(x))
ax2.plot(np.cos(x))
```

![output_多子图](https://upload-images.jianshu.io/upload_images/17476267-a68a74cc4cc1e59c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######【第二步】：通过 plt.subplot() 方法创建子图

```python
# 创建网格子图
for i in range(1,7):
    plt.subplot(2,3,i)
    plt.text(0.5,0.5,str((2,3,i)),fontsize=18, ha='center')
```


![output_45_0.png](https://upload-images.jianshu.io/upload_images/17476267-17faea3db208f9de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######【第二步】：使用 `fig.subplots_adjust()` 调整子图之间的间隔

```python
fig = plt.figure()
fig.subplots_adjust(hspace=0.4,wspace=0.4)
for i in range(1,7):
    plt.subplot(2,3,i)
    plt.text(0.5,0.5,str((2,3,i)),fontsize=18, ha='center')
```


![output_46_0.png](https://upload-images.jianshu.io/upload_images/17476267-b805ba1a77801f58.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######【第二步】：使用 `plt.subplots() `创建子图

```python
# plt.subplots()  一行代码，创建网格
fig,ax = plt.subplots(2,3,sharex='col',sharey='row')
```

![output_47_0.png](https://upload-images.jianshu.io/upload_images/17476267-b9a9ed46a4c31505.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```python
# 绘制子图内容
for i in range(2):
    for j in range(3):
        ax[i,j].text(0.5, 0.5, str((i,j)),fontsize=18,ha='center')
fig
```

![output_子图内容](https://upload-images.jianshu.io/upload_images/17476267-ed55b6cc8ba0b8b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######【第二步】：使用 `plt.GridSpec()` 创建不规则网格布局

```python

grid = plt.GridSpec(2,3,wspace=0.4, hspace=0.3)
plt.subplot(grid[0,0])
plt.subplot(grid[0,1:])
plt.subplot(grid[1,:2])
plt.subplot(grid[1,2])
```


![output_规则网格布局](https://upload-images.jianshu.io/upload_images/17476267-f10d995a75861300.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

实例：绘制多轴频次直方图

```python
# 创建正太分布的数据
mean = [0,0]
cov = [[1,1],[1,2]]
x, y = np.random.multivariate_normal(mean, cov,200).T

# 设置坐标轴和网格的配置方式
fig = plt.figure(figsize=(6,6))
grid = plt.GridSpec(4,4,hspace=0.2, wspace=0.2)
main_ax = fig.add_subplot(grid[:-1,1:])
y_hist = fig.add_subplot(grid[:-1,0],xticklabels=[],sharey=main_ax)
x_hist = fig.add_subplot(grid[-1,1:],xticklabels=[],sharex=main_ax)

# 主坐标轴画散点图
main_ax.plot(x,y,'ok',markersize=3,alpha=0.2)

# 次坐标的频次直方图
x_hist.hist(x,40,histtype='stepfilled',orientation='vertical', color='gray')
# 反转y轴
x_hist.invert_yaxis()

y_hist.hist(y,40,histtype='stepfilled',orientation='horizontal', color='gray')
# 反转x轴
y_hist.invert_xaxis()
```

![output_多轴频次直方图](https://upload-images.jianshu.io/upload_images/17476267-62293eb3899f60f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 📖 九、给图形添加箭头和文本

```python
fig,ax = plt.subplots()
x = np.linspace(0,20,1000)
ax.plot(x,np.cos(x))
ax.axis('equal')


ax.annotate('local maximum',xy=(6.28,1),xytext=(10,4),arrowprops=dict(facecolor='black',shrink=0.05))
ax.annotate('local minimum',xy=(5*np.pi,-1),xytext=(2,-6),arrowprops=dict(arrowstyle="->",connectionstyle="angle3,angleA=0,angleB=-90"))
```

![output_箭头和文本](https://upload-images.jianshu.io/upload_images/17476267-80dc2c2b3559712e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

