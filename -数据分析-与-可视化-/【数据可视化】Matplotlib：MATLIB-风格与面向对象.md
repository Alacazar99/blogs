### 📖 一、Matplotlib库相关介绍
【介绍】：Matplotlib 是建立在Numpy数组基础上的多平台数据可视化程序库。

##### 安装
```
pip install matplotlib
```
##### 两种使用方式
- 1、交互式图形： %matplotlib notebook
- 2、静态图形： %matplotlib inline


##### 导入相关库
```python
import matplotlib.pyplot as plt
import numpy as np
%matplotlib inline
```

######【第一步】：绘制正弦、余弦曲线
```python
x = np.linspace(-5,5,100)
fig = plt.figure()
plt.plot(x,np.sin(x),'-')
plt.plot(x,np.cos(x),'--')
plt.show()
```

![output_正弦、余弦曲线](https://upload-images.jianshu.io/upload_images/17476267-d394068e25c7a9a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######【第二步】：利用`savefig`将图形保存为文件
```python
fig.savefig('my.png')
```

######【第三步】：利用`from IPython.display import Image`显示图形

```python
from IPython.display import Image
Image("my.png")
```
![output_显示图形](https://upload-images.jianshu.io/upload_images/17476267-d394068e25c7a9a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######【第四步】：获取支持的图像格式
```python
fig.canvas.get_supported_filetypes()
```
    {'ps': 'Postscript',
     'eps': 'Encapsulated Postscript',
     'pdf': 'Portable Document Format',
     'pgf': 'PGF code for LaTeX',
     'png': 'Portable Network Graphics',
     'raw': 'Raw RGBA bitmap',
     'rgba': 'Raw RGBA bitmap',
     'svg': 'Scalable Vector Graphics',
     'svgz': 'Scalable Vector Graphics'}

###📖  二、画图的两种风格
######【第一种】：MATLIB 风格接口

```python
# 创建图形
plt.figure()

#(行、列、子图的编号)
plt.subplot(2,1,1)
plt.plot(x,np.sin(x))

plt.subplot(2,1,2)
plt.plot(x,np.cos(x))
```
【解释】：这种风格的重要特征是：有状态的。会持续跟踪当前的图形和坐标进行绘制。
- plt.gcf()  ：获取当前图形
- plt.gca() ： 获取当前坐标轴

![output_MATLIB 风格](https://upload-images.jianshu.io/upload_images/17476267-d22b6bf803217f76.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


######【第二种】：面向对象接口

```python
# ax 是包含了两个Axes对象的数组
fig, ax = plt.subplots(2)

# 在每个对象上调用plot()
ax[0].plot(x,np.sin(x))
ax[1].plot(x,np.cos(x))
```
【基本思路】：将画图实例化对象为`ax`,（ ax 是包含了两个Axes对象的数组），然后对每个对象直接调用plot()等相关函数进行绘图。
![output_面向对象](https://upload-images.jianshu.io/upload_images/17476267-d22b6bf803217f76.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###📖  三、绘制简易线型图
- figure:可以被看做是一个能够容纳各种坐标轴、图形、文字、和标签的容器
- axes： 是一个带标签和刻度的矩形。
```python
# figure:可以被看做是一个能够容纳各种坐标轴、图形、文字、和标签的容器
# axes： 是一个带标签和刻度的矩形。
fig = plt.figure()
ax = plt.axes()
```
######【第一步】：没错，先绘制一个空白的带标签和刻度的容器作为绘图的基础（就像画布那样）。

![output_画布](https://upload-images.jianshu.io/upload_images/17476267-4eb0bb151a6cb60a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


######【第二步】：通过不断的调用plot可以在一个画布上绘制多个图形。
```python
plt.plot(x,np.sin(x))
plt.plot(x,np.cos(x))
```
![output_绘制图形](https://upload-images.jianshu.io/upload_images/17476267-e18f1879b35917a6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


######【第三步】：调整图形的线条颜色与风格

```python
## 颜色 color参数
plt.plot(x,np.sin(x - 0), color='blue')  # 标准颜色名称
plt.plot(x,np.sin(x - 1), color='g')     # 缩写颜色的代码(rgbcmyk)
plt.plot(x,np.sin(x - 2), color='0.75')  # 范围在0-1直间的灰度值
plt.plot(x,np.sin(x - 3), color='#ff99dd') #十六进制
plt.plot(x,np.sin(x - 4), color=(1.0,0.2,0.3)) #RGB元组，范围0-1
```
【补充】：Matplotlib中关于颜色的表达形式，主要有以下几种。
- color='blue'：标准颜色名称；
- color='g'： 缩写颜色的代码(rgbcmyk)；
- color='0.75'：范围在0-1直间的灰度值；
- color='#ff99dd'：十六进制；
- color=(1.0,0.2,0.3)：RGB元组，范围(0,1)；

![output_颜色的表达形](https://upload-images.jianshu.io/upload_images/17476267-60f1b149b6f2c9ef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


######【第四步】： 使用 `linestyle` 参数调整线条风格
```python
plt.plot(x,x+0,linestyle='solid')
plt.plot(x,x+1,linestyle='dashed')
plt.plot(x,x+2,linestyle='dashdot')
plt.plot(x,x+3,linestyle='dotted')
```
![output_线条风格](https://upload-images.jianshu.io/upload_images/17476267-dc90268b680f4cc0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


######【第五步】：线条风格，简写形式

```python
plt.plot(x,x+0,linestyle='-')   # 实线
plt.plot(x,x+1,linestyle='--')  # 虚线
plt.plot(x,x+2,linestyle='-.') # 点划线
plt.plot(x,x+3,linestyle=':') # 实点线
```

![output_线条风格](https://upload-images.jianshu.io/upload_images/17476267-dc90268b680f4cc0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######【第六步】：（参数组合）将color 和linestyle 组合起来

```python
plt.plot(x,x+0,'-g')  # 绿色实线
plt.plot(x,x+1,'--c') # 青色虚线
plt.plot(x,x+3,'-.k') # 黑色点划线
plt.plot(x,x+4,':r') # 红色点实线
```

![output_参数组合](https://upload-images.jianshu.io/upload_images/17476267-25fc7f99d246cfa7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######【第七步】：调整坐标轴的上下限

- 方式1：两个基础方法：plt.xlim()  和 plt.ylim()
```python
# 方式1：两个基础方法：plt.xlim()  和 plt.ylim()
plt.plot(x,np.sin(x))
plt.xlim(-1,11)
plt.ylim(-1.5,1.5)
```

![output_参数组合](https://upload-images.jianshu.io/upload_images/17476267-3a7dd9347676fc3a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- 方式2：plt.axis([xmin,xmax,ymin,ymax])
```python
# 方式2：plt.axis([xmin,xmax,ymin,ymax])
plt.plot(x,np.sin(x))
plt.axis([-1,11,-1.5,1.5])
```
![output](https://upload-images.jianshu.io/upload_images/17476267-3a7dd9347676fc3a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


######【第八步】：坐标轴逆序显示，将 xlim() 或者 ylim()的参数设置为负值。

```python
plt.plot(x,np.sin(x))
plt.xlim(10,0)
plt.ylim(1.2,-1.2)
```
![output_坐标轴逆序](https://upload-images.jianshu.io/upload_images/17476267-c95869dde0f66bbb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######【第九步】：使用 plt.axis('tight') 按照图形内容，自动缩紧坐标轴，不留空白。

![output_缩紧坐标轴](https://upload-images.jianshu.io/upload_images/17476267-a0378c15c4a43bac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```python
plt.plot(x,np.sin(x))
plt.axis('tight')
```


######【第十步】：让x和y轴长度单位相同

```python
plt.plot(x,np.sin(x))
plt.axis('equal')
```
![output_单位等长](https://upload-images.jianshu.io/upload_images/17476267-943b1dead0f5b3de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


