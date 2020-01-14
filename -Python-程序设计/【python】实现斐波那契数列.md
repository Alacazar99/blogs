使用多种方式实现 斐波那契数列
：1 1 2 3 5 8 13 1 34 55 89 144 233 377 610 987 ···
- **方法一**：约束输出最大值
输出小于`10000`的斐波那契数列
```
def fb():
  a,b = 1,1
  while a < 10000:
      print(a,end=' ')
      a,b = b,a+b
fb()
```
内存分析：
![内存分析](https://upload-images.jianshu.io/upload_images/17476267-fa6323fd4aeccc0f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

执行结果：
```PY
1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597 2584 4181 6765 
```
------------------------------------
- **方法二**：用递归实现
```
def fb1(n):
    if n == 1 or n == 2:
        return 1
    return fb1(n-1) + fb1(n-2)
for i in range(1,20):
    print(fb1(i))
```
执行结果：
```py
1
1
2
3
5
8
13
21
34
55
89
144
233
377
610
987
1597
2584
4181
```
-------------------------
- **方法三**：用循环实现
```
def fb_for(n):
    before = 0
    after = 1
    if n == 1:
        return 1
    for i in range(2,n+1):
        tmp = before + after
        before = after
        after = tmp
        # before,after = after,tmp
    return tmp
for i in range(1,20):
    print(fb_for(i))
```
内存分析:
![内存分析](https://upload-images.jianshu.io/upload_images/17476267-f115d16f71c7fbcf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/640)
