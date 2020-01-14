##### 关于迭代

迭代是访问集合元素的一种方式。迭代器是一个可以记住遍历的位置的对象。迭代器对象从集合的第一个元素开始访问，直到所有的元素被访问完结束。迭代器只能往前不会后退。

#### 迭代器本质
我们分析对可迭代对象进行迭代使用的过程，发现每迭代一次（即在for...in...中每循环一次）都会返回对象中的下一条数据，一直向后读取数据直到迭代了所有数据后结束。那么，在这个过程中就应该有一个“人”去记录每次访问到了第几条数据，以便每次迭代都可以返回下一条数据。我们把这个能帮助我们进行数据迭代的“人”称为迭代器(Iterator)。

###### 迭代器规定的两个方法：
- `__iter__`:返会迭代器本身。(要返回一个迭代器，迭代器自身正是一个迭代器，所以迭代器的__iter__方法返回自身即可)
- `__next__`:返会下一个元素。（对迭代器使用next()函数的时候，迭代器会向我们返回它所记录位置的下一个位置的数据。）
读取完毕，触发一个StopTteration异常。
**迭代器  ：节约内存   ；读取数据的一种方式（读）**

>> 迭代器：当需要迭代对象时，会自动调用系统函数iter()函数，如下：
检查对象是否实现了`__iter__`方法，如果实现了，则调用，返会一个迭代器。
如果没有实现`__iter__`方法,但是实现了`__getitem__()`方法，python会创建一个迭代器，尝试按顺序（从0 开始）获取元素
如果前两者都无 则抛出异常
##### iter()函数与next()函数
list、tuple等都是可迭代对象，我们可以通过iter()函数获取这些可迭代对象的迭代器。然后我们可以对获取到的迭代器不断使用next()函数来获取下一条数据。iter()函数实际上就是调用了可迭代对象的`__iter__`方法。
举个栗子：
```
import reprlib
class Sentence:
    def __init__(self,text):
        self.text = text
        self.words = self.text.split()

    def __getitem__(self, item):
        return self.words[item]

    def __len__(self):
        return len(self.words)

    def __repr__(self):
        return "Sentence(%s)" % reprlib.repr(self.text)


s = Sentence('Zurich Alcazar love beautiful girl ')
print(s)
for word in s:
    print(word)
```
简单的看一下内存的占用情况：
![内存分析](https://upload-images.jianshu.io/upload_images/17476267-808af939460079d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Output：
```
Sentence('Zurich Alcaz...autiful girl ')
Zurich
Alcazar
love
beautiful
girl
```
【注意】 reprlib模块 提供了对表示对象的字符串大小的限制。它提供的功能是内建函数repr（）的加强版。

---
##### 生成器（Generators）

 生成器是构造迭代器的最简单有力的工具，与普通函数不同的只有在返回一个值的时候使用yield来替代return，然后yield会自动构建好next()和iter()。

生成器：用于生成数据（写）

>>生成器最佳应用场景是：
你不想同一时间将所有计算出来的大量结果集分配到内存当中，特别是结果集里还包含循环。比方说，循环打印1000000个数，我们一般会`使用xrange()而不是range()`，因为前者返回的是生成器，后者返回的是列表（列表消耗大量空间）。

举例来看。。。
```

class Count:
    def __init__(self,step):
        self.step = step

    def __next__(self):
        if self.step <= 0:
            raise StopIteration

        self.step -=1
        return self.step
    #调用时生成

    def __iter__(self):
        return self

```
output：
```
7
6
5
4
3
2
1
0
```
迭代的方式生成斐波那契数列：
参考文档：https://www.jianshu.com/writer#/notebooks/36309588/notes/47910634
```
def fib(n):
    if n == 1  or n == 2:
        return 1
    else:
        return fib(n-2) + fib(n-1)
print(fib(20))

--------
output:
        6765
```

---
【总结】
- next 方法，会在yield关键字处停止，并返会yield后的值。
- 只要python函数的定义中有`yield`关键字，则该函数就是生成器。调动时会返会一个生成器对象。
也就是说`生成器函数是用来生成生成器的`。
- `生成器也是迭代器`，会生成yield关键字后面表达式的值
- 一般函数使用return 返回，而生成器是由yield返回数据









