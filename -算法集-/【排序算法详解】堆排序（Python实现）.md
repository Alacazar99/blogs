
代码自带解释，就不多赘述原理了...
- 💬 堆的向下调整性质；
- 💬 当根节点的左右子树都是堆时，可以通过一次向下的调整来将其变换成一个堆；
### 堆排序
```
def Sort(lis, low, high):         # low是根节点所在位置的下标，high是要调整的树的最后一个元素的下标（用于判断是否结束调整）
    tmp = lis[low]                  # 根节点的值存起来
    i = low                         # 根节点下标
    j = 2 * i + 1                     # 根节点左孩子下标
    while j <= high:                   # 判断j是否存在，不存在说明可以结束调整了
        if j + 1 <= high and lis[j+1] > lis[j]:  # 如果右孩子存在并且比左孩子大
            j += 1                      # j指向右孩子
        if lis[j] > tmp:                 # 如果孩子的值比父亲的大，需要进行调整，否则可以结束调整
            lis[i] = lis[j]
            i = j
            j = 2 * i + 1
        else:
            break
    lis[i] = tmp            # 调整结束，将tmp的值放到调整后的位置

def heap_sort(lis):
    n = len(lis)
    for i in range(n//2-1, -1, -1):              # i表示遍历的low, n//2-1是最后一个非叶子节点的下标
        Sort(lis, i, n-1)
    for i in range(n-1, 0, -1):                # i表示逻辑上当前堆的high
        lis[i], lis[0] = lis[0], lis[i]
        Sort(lis, 0, i-1)
```
【解释一句】：# 最后在输出数的时候为了节省空间，我们把输出的数放在队尾，`i`前面的是逻辑上的堆，后面则是有序区。

### 算法测试
在本例测试中，为了直观体现**堆排序的性质**，使用了一个打印树的函数。
```
def print_tree(array): #打印堆排序使用
    '''
    深度 前空格 元素间空格
    1     7       0
    2     3       7
    3     1       3
    4     0       1
    '''
    # first=[0]
    # first.extend(array)
    # array=first
    index = 1
    depth = math.ceil(math.log2(len(array))) # 因为补0了，不然应该是math.ceil(math.log2(len(array)+1))
    sep = '  '
    for i in range(depth):
        offset = 2 ** i
        print(sep * (2 ** (depth - i - 1) - 1), end='')
        line = array[index:index + offset]
        for j, x in enumerate(line):
            print("{:>{}}".format(x, len(sep)), end='')
            interval = 0 if i == 0 else 2 ** (depth - i) - 1
            if j < len(line) - 1:
                print(sep * interval, end='')
        index += offset
        print()
```
开始测试：
```
orignal_list=[0, 74, 73, 59, 72, 64, 69, 43, 36, 70, 61, 40, 16, 47, 67, 17, 31, 19, 24, 14, 20, 48, 5, 7, 3, 78, 84, 92, 97, 98, 99]
print(orignal_list)
print_tree(orignal_list)
heap_sort(orignal_list)
#打印树
print_tree(orignal_list)

print(orignal_list)
```
输出结果如下：
```
[0, 74, 73, 59, 72, 64, 69, 43, 36, 70, 61, 40, 16, 47, 67, 17, 31, 19, 24, 14, 20, 48, 5, 7, 3, 78, 84, 92, 97, 98, 99]
【初始树】：
                              74
              73                              59
      72              64              69              43
  36      70      61      40      16      47      67      17
31  19  24  14  20  48   5   7   3  78  84  92  97  98  99
【堆排序】后：
                               3
               5                               7
      14              16              17              19
  20      24      31      36      40      43      47      48
59  61  64  67  69  70  72  73  74  78  84  92  97  98  99
[0, 3, 5, 7, 14, 16, 17, 19, 20, 24, 31, 36, 40, 43, 47, 48, 59, 61, 64, 67, 69, 70, 72, 73, 74, 78, 84, 92, 97, 98, 99]
```
