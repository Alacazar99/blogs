### Matplotlib绘制3D图形

######【第一步】：引入相关库
```
from  mpl_toolkits import mplot3d
%matplotlib inline

import numpy as np
import matplotlib.pyplot as plt

```
######【第二步】：构建3d坐标系

```
fig = plt.figure()
ax = plt.axes(projection='3d')
```

![output_3d坐标系](https://upload-images.jianshu.io/upload_images/17476267-6a1b037fd63c4ccf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######绘制线图和点图
```
fig = plt.figure()
ax = plt.axes(projection='3d')
# 三维数据的点、线
zline = np.linspace(0,15,1000)
xline = np.sin(zline)
yline = np.cos(zline)

# 绘制线图
ax.plot3D(xline,yline,zline,'gray')

#绘制散点图
zdata = 15 * np.random.randn(100)
xdata = np.sin(zdata) + 0.1 * np.random.randn(100)
ydata = np.cos(zdata) + 0.1 * np.random.randn(100)
ax.scatter3D(xdata,ydata,zdata,c=zdata,cmap='Greens')
```


![Image](https://upload-images.jianshu.io/upload_images/17476267-7b9a507b14e6fc82.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


######绘制三维等高线图
具体的参数描述如下表所示：
参数 	|描述
|:-:|:-:|
X, Y，Z|	坐标点
rcount,ccount,rstride,cstride|	坐标点
color	|定义surface patch的颜色,`type`:color-like
cmap	|定义surface patch的颜色，只不过是colorMap,`type`:colormap
facecolors	|指定单个patch的颜色, `type`:array-like of colors
norm	|colormap的normalization， 
shade	|阴影效果，`type`:boolean
vmin, vmax	|normalization的边界
**kwargs	|向下传递到Poly3DCollection
antialiased	|抗锯齿，`type`:boolean

【程序】：
```
# 模拟数据
def f(x,y):
    return np.sin(np.sqrt(x ** 2 + y ** 2))

x = np.linspace(-6,6,30)
y = np.linspace(-6,6,30)
X,Y = np.meshgrid(x,y)
Z = f(X,Y)

# 绘制
fig = plt.figure()
ax = plt.axes(projection='3d')
ax.contour3D(X,Y,Z,50,cmap='RdGy')
ax.set_xlabel("x")
ax.set_ylabel("y")
ax.set_zlabel("z")

# 调整图形的角度
ax.view_init(60,30)
```
【可视化】：


![untitled.png](https://upload-images.jianshu.io/upload_images/17476267-981d76089916c151.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


######绘制线图和点图 绘制线框图
调用函数：`plot_wireframe()`
```
plot_wireframe(X, Y, Z, *args, **kwargs)
```
具体的参数描述如下表所示：
参数 	|描述
|:-:|:-:|
X, Y,Z|	坐标点
rcount,ccount	|采样数，越大采样越多，默认50
rstride,cstride|	采样步长，越小采样越多
**kwargs	|其他参数向下传入Line3DCollection

【程序】：
```
fig = plt.figure()
ax = plt.axes(projection='3d')
ax.plot_wireframe(X,Y,Z,color='green')
ax.set_title("wireframe")
```
【可视化】：

![output_线框图](https://upload-images.jianshu.io/upload_images/17476267-ccc4f36c94f57aa2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
