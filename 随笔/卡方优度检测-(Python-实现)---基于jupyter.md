## 卡方独立性检验

```
import numpy as np
from scipy.stats import chi2_contingency   # 列联表分析
from scipy.stats import chi2               # 卡方分布
```
##### （2）参数说明[](http://localhost:8888/notebooks/%E5%8D%A1%E6%96%B9%E4%BC%98%E5%BA%A6%E6%A3%80%E6%B5%8B%20(Python%20%E5%AE%9E%E7%8E%B0)%20%20--%E5%9F%BA%E4%BA%8Ejupyter%E5%B9%B3%E5%8F%B0.ipynb#%EF%BC%882%EF%BC%89%E5%8F%82%E6%95%B0%E8%AF%B4%E6%98%8E)

###### 【输入】：[](http://localhost:8888/notebooks/%E5%8D%A1%E6%96%B9%E4%BC%98%E5%BA%A6%E6%A3%80%E6%B5%8B%20(Python%20%E5%AE%9E%E7%8E%B0)%20%20--%E5%9F%BA%E4%BA%8Ejupyter%E5%B9%B3%E5%8F%B0.ipynb#%E3%80%90%E8%BE%93%E5%85%A5%E3%80%91%EF%BC%9A)

alpha --- 置信度，用来确定临界值;

data --- 数据，请使用numpy.array数组;

###### 【输出】：[](http://localhost:8888/notebooks/%E5%8D%A1%E6%96%B9%E4%BC%98%E5%BA%A6%E6%A3%80%E6%B5%8B%20(Python%20%E5%AE%9E%E7%8E%B0)%20%20--%E5%9F%BA%E4%BA%8Ejupyter%E5%B9%B3%E5%8F%B0.ipynb#%E3%80%90%E8%BE%93%E5%87%BA%E3%80%91%EF%BC%9A)

参数 | 参数描述

CMIN --- 卡方值，也就是统计量;

p --- P值(统计学名词)，与置信度对比，也可进行假设检验，P值小于置信度，则拒绝原假设;

freedom --- 自由度;

re --- 判断变量，1表示拒绝原假设，0表示接受原假设;

prediction_value --- 原数据数组同维度的对应理论预测值(预测结果);


```
def chi2_independence(alpha, data):
    CMIN, p, dof, prediction_value = chi2_contingency(data)
    
    if dof == 0:
        print('自由度应该大于等于1')
    elif dof == 1:
        cv = chi2.isf(alpha * 0.5, dof)
    else:
        cv = chi2.isf(alpha * 0.5, dof-1)
    if CMIN > cv:
        re = 1    # 表示拒绝原假设
    else:
         re = 0   # 表示接受原假设
    return CMIN, p, dof, re, prediction_value
```


```
# 测试


alpha1 = 0.005    # 置信度，常用0.01，0.05，用于确定拒绝域的临界值

data1 = np.array([[43, 49,22,114], [8, 2,5,15],[47,44,30,121]]) 

data2 = np.array([[43, 49,22], [8, 2,5],[47,44,30]]) # # 插入数据（用于测试）


CMIN, p, freedom, re, prediction_value = chi2_independence(alpha1, data2)


print("卡方值:\n",CMIN)
print("P值:\n",p)
print("自由度:\n",freedom)
print("判断变量:\n",re)
print("原数据数组同维度的理论预测值(预测结果):\n",prediction_value)
```
![1.png](https://upload-images.jianshu.io/upload_images/17476267-8f44085beff8d89e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


---

# 卡方拟合性检验[¶](http://localhost:8888/notebooks/%E5%8D%A1%E6%96%B9%E4%BC%98%E5%BA%A6%E6%A3%80%E6%B5%8B%20(Python%20%E5%AE%9E%E7%8E%B0)%20%20--%E5%9F%BA%E4%BA%8Ejupyter%E5%B9%B3%E5%8F%B0.ipynb#%E5%8D%A1%E6%96%B9%E6%8B%9F%E5%90%88%E6%80%A7%E6%A3%80%E9%AA%8C)

卡方检验能检验单个多项分类名义型变量各分类间的实际观测次数与理论次数之间是否一致的问题，这里的观测次数是根据样本数据得多的实计数，理论次数则是根据理论或经验得到的期望次数。

这一类检验称为拟合性检验。 其自由度通常为分类数减去1，理论次数通常根据某种经验或理论。

总而言之，卡方拟合度检验用于判断不同类型结果的比例分布相对于一个期望分布的拟合程度。

卡方拟合性检验适用于变量为**类别型变量**的情况。

```
# 导如相关库

import numpy as np
from scipy.stats import chisquare  
from scipy.stats import chi2        # 卡方分布
```


### （1）假设检验重要知识

H0: 类别A与B的比例没有差异；

H1：类别A与B的比例有差异；

若卡方值大于临界值，拒绝原假设，表示A与B不相互独立，A与B相关；
函数中re返回为1表示拒绝原假设，0表示接受原假设；

### （2）参数说明

#### 【输入】：
**alpha** --- 置信度，用来确定临界值；

**data**  --- 数据，使用numpy.array数组；

**sp**  --- 表示输入数组的形状参数，默认为一维；

#### 【输出】：

**chis**  --- 卡方值，也就是统计量；

**p_value** --- P值（统计学名词），与置信度对比，也可进行假设检验，P值小于置信度，即可拒绝原假设；

**critical_value** --- 拒绝域临界值；

**freedom** --- 自由度；

**result** --- 判断变量，1表示拒绝原假设，0表示接受原假设；


 （3）应用场景
 
要求样本含量应大于40，且每个格子中的理论频数最好大于5;


```
def chi2_fitting(data, alpha, sp=None):
    
    chis,p_value = chisquare(data, axis=sp)
    i, freedom = data.shape  # freedom为自由度
    
    if freedom == 0:
        print('自由度应该大于等于1')
    elif freedom == 1:
        critical_value = chi2.isf(alpha * 0.5, freedom)
    else:
        critical_value = chi2.isf(alpha * 0.5, freedom - 1)
 
    if chis > critical_value:
        result = 1  # 表示拒绝原假设
    else:
        result = 0  # 表示接受原假设
    return chis, p_value, critical_value, freedom-1, result

```

```
# 拟合测试

data1 = np.array([[43, 49,22], [8, 2,5],[47,44,30]]) # 插入数据
alpha1 = 0.05

chis1, p_value1, critical_value, dof, result1 = chi2_fitting(data1, alpha1)


print("卡方值:  ",chis1)
print("P值:     ",p_value1)
print("拒绝域临界值:",critical_value)
print("自由度:  ",dof)
print("判断变量（1表示否定原假设，0表示肯定原假设）: ",result1)
```
![2.png](https://upload-images.jianshu.io/upload_images/17476267-cf6753aea2625b36.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



---
## 导入房价数据集
使用卡方独立性检验和卡方拟合性检验进行相关预测。
```
### 导入数据集

# 读取房价数据
import pandas as pd

def load_housing_data():
    return pd.read_csv('./housing.csv',encoding='gbk')

housing = load_housing_data()
# # 查看前五行数据
housing.head()
```

```
housing.describe()   ## 数据集描述；
```

```
## 居民收入: income

income = housing['median_income']
income

## 房屋价值：house_value
## 房屋年龄：housing_median_age
## 房屋面积 ：total_rooms

house_value =  housing['median_house_value']
house_value

housing_median_age =  housing['housing_median_age']
housing_median_age


total_rooms = housing['total_rooms']
total_rooms
```

### （1） 探究收入与房价的关系
```
# 数据: 探究收入与房价的关系

data2 = np.array([income,house_value]) # 数据: 探究收入与房价的关系

CMIN, p, freedom, re, prediction_value = chi2_independence(alpha1, data2)

print("卡方值:  ",CMIN)
print("P   值:  ",p)
print("自由度:  ",freedom)   # 
print("判断变量（1表示否定原假设，0表示肯定原假设）: \n",re)
print("原数据数组同维度的理论预测值(预测结果):\n",prediction_value)

print("------------分隔线----------------")
print("【卡方拟合性检验】：")

alpha1 = 0.05
chis1, p_value1, critical_value, dof, result1 = chi2_fitting(data2, alpha1)

print("卡方值:  ",chis1)
print("P值:     ",p_value1)
print("拒绝域临界值:",critical_value)
print("自由度:  ",dof)
print("判断变量（1表示否定原假设，0表示肯定原假设）: ",result1)
```

![3.png](https://upload-images.jianshu.io/upload_images/17476267-2681fc7cfcc01e49.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


---
###  探究房龄与房价的关系
```
# 数据: 探究房龄与房价的关系

data2 = np.array([housing_median_age,house_value]) # 数据: 探究收入与房价的关系

CMIN, p, freedom, re, prediction_value = chi2_independence(alpha1, data2)


print("卡方值:  ",CMIN)
print("P   值:  ",p)
print("自由度:  ",freedom)   # 
print("判断变量（1表示否定原假设，0表示肯定原假设）: \n",re)
print("原数据数组同维度的理论预测值(预测结果):\n",prediction_value)

print("------------分隔线----------------")
print("【卡方拟合性检验】：")

alpha1 = 0.05
chis1, p_value1, critical_value, dof, result1 = chi2_fitting(data2, alpha1)

print("卡方值:  ",chis1)
print("P值:     ",p_value1)
print("拒绝域临界值:",critical_value)
print("自由度:  ",dof)
print("判断变量（1表示否定原假设，0表示肯定原假设）: ",result1)
```

![4.png](https://upload-images.jianshu.io/upload_images/17476267-23a48588f186a4a1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



---

###  探究房屋面积与房价的关系
```
# 数据: 探究房屋面积与房价的关系

data2 = np.array([total_rooms,house_value]) # 数据: 探究收入与房价的关系

CMIN, p, freedom, re, prediction_value = chi2_independence(alpha1, data2)

print("卡方值:  ",CMIN)
print("P   值:  ",p)
print("自由度:  ",freedom)   # 
print("判断变量（1表示否定原假设，0表示肯定原假设）: \n",re)
print("原数据数组同维度的理论预测值(预测结果):\n",prediction_value)

print("------------分隔线----------------")
print("【卡方拟合性检验】：")

alpha1 = 0.05
chis1, p_value1, critical_value, dof, result1 = chi2_fitting(data2, alpha1)

print("卡方值:  ",chis1)
print("P值:     ",p_value1)
print("拒绝域临界值:",critical_value)
print("自由度:  ",dof)
print("判断变量（1表示否定原假设，0表示肯定原假设）: ",result1)
```
![5.png](https://upload-images.jianshu.io/upload_images/17476267-28b6c3708ef13a54.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---

【拓展】



![拟合优度检验](https://upload-images.jianshu.io/upload_images/17476267-7860d171ec3c08ea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![独立性检验](https://upload-images.jianshu.io/upload_images/17476267-aa5314b193e8e81b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

