通过学习排序算法，发现冒泡排序和选择排序在算法实现上，十分的近似，下面进行必要的一些讲解：
### 我们先来看看这两个算法的Python实现过程：
- 冒泡排序
时间复杂度（最好）：O(n)
平均时间复杂度：O(n²)
```
def bubble_sort(lis):
    for i in range(len(lis)):            
     # 这个循环负责设置冒泡排序进行的次数    
        for j in range(i+1,len(lis)):     # j为列表下标
            if lis[i] > lis[j]:
                lis[i], lis[j] = lis[j], lis[i]
    return lis
```
- 选择排序
时间复杂度：O(n²)
空间复杂度：O(1)
```
def selection_sort(lis):
  for i in range( len (lis)-1):
    minindex = i
    for j in range(i + 1, len(lis)):
      if lis[j] < lis[minindex]:
        minindex = j
    lis[i], lis[minindex] = lis[minindex], lis[i]
    return lis
```

---
### 原理分析
####- 【冒泡排序】
- 比较相邻的元素。如果第一个比第二个大，就交换他们两个。

- 对每一对相邻元素做同样的工作，从开始第一对到结尾的最后一对。

让数组当中相邻的两个数进行比较，**数组当中比较小的数值向下沉，数值比较大的向上浮！外层for循环控制循环次数，内层for循环控制相邻的两个元素进行比较。**

####- 【选择排序】

将一个序列分为两部分，**前面是有序序列，后面是无序序列，不断的将后面的无序序列中的最小值添加到前面的有序序列中，直到后面的无序序列中没有值,开始的时候将第一个值作为有序序列。**

---
由于冒泡排序中元素需要两两比较，所以要**遍历**所有元素，**冒牌排序算法，非常适用于寻找列表中最大值或者，最小值**。

在选择排序中，我们也需要一轮轮的选出剩余的无需元素中的最小值，所以也要一次次的遍历无序列表，**非常契合的使用冒泡的思想去选出最小值**。


![](https://upload-images.jianshu.io/upload_images/17476267-828f6c372f730ed4.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


【结论】：看这两个算法其实思维不同，但是实现的编码过程十分一致。如果看程序看蒙了，其实也不要紧，思维上能明白两者的区别就行...


