#### 题目描述
给定 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

【说明】：你不能倾斜容器，且 n 的值至少为 2。
```
class Solution:
    def maxArea(self, height: List[int]) -> int:
        area = 0
        left = 0
        right = len(height) -1
        
        while left < right:
            if height[left] <= height[right]:
                area = max((right-left)*height[left],area)
                left += 1
            else:
                area = max((right-left)*height[right],area)
                right -= 1
        return area
```
时间复杂度 O(N)，双指针遍历一次底边宽度 NN。
空间复杂度 O(1)，指针使用常数额外空间。

### 解题思路

- 矩阵的长度：两条垂直线的距离
- 矩阵的宽度：两条垂直线其中较短一条的长度

矩阵面积最大化：**两条垂直线的距离越远越好，两条垂直线的最短长度也要越长越好。**

#### 双指针解题

【第一步】：假设两个指针left 和 right，分别指向数组的头和尾。秉承要求最大的面积的目的。我们可以知道：此时有最大的**长度（right-left是最大的）**但是面积是长与宽的最大乘积；

【下一步】：那么我们可以通过移动指针来**宽度**，如何调整？

**将当前两个指针指向的值进行比较，较小值对应的指针向中间靠拢（即左指针向右移动；右指针向左移动），直到两个指针重合，跳出循环，输出结果。**

![时间还行...](https://upload-images.jianshu.io/upload_images/17476267-8bf261e4fdd61093.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


