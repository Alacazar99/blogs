
### 📖  一、 数据分析
【百度百科简介】：指用适当的[统计分析方法](https://baike.baidu.com/item/%E7%BB%9F%E8%AE%A1%E5%88%86%E6%9E%90%E6%96%B9%E6%B3%95/12143543)对收集来的大量数据进行分析，提取有用信息和形成结论而对数据加以详细研究和概括总结的过程。

【关于就业】：数据分析师（初级，高级，专家...）等。
![【图片来源】：Boss直聘](https://upload-images.jianshu.io/upload_images/17476267-586305901ddb598c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
【数据来源】：谈及大数据、数据分析，那么就不可避免的要谈到如何获取数据，这可是一个非常非常令人头疼闹热的问题，当前的信息时代数据量十分庞大，复杂，所以，如何获取有价值的数据，是一门值得考究的学问。数据的主要来源有以下几方面：

1、[搜索引擎-爬虫]()抓取数据；

2、网站[IP](https://baike.baidu.com/item/IP/224599)、[PV](https://baike.baidu.com/item/PV/402)等基本数据；

3、网站的[HTTP]()响应时间数据；

4、[网站](https://baike.baidu.com/item/%E7%BD%91%E7%AB%99)流量来源数据。

---

###📖    二、关于Jupyter Notebook
【简介】：upyter Notebook是基于网页的用于交互计算的应用程序。其可被应用于全过程计算：开发、文档编写、运行代码和展示结果。——[Jupyter Notebook官网](https://link.jianshu.com/?t=https%3A%2F%2Fjupyter-notebook.readthedocs.io%2Fen%2Fstable%2Fnotebook.html)

【特点】：以网页的形式打开，可以在网页页面中直接编写代码和运行代码，代码的运行结果也会直接在代码块下显示。

##### 1、安装
```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple juniper
```
##### 2、启动
在**命令提示符窗口**中输入`jupyter notebook`
如果你的jupyter notebook文件不在c盘根目录，那么可以通过
```
cd +文件路径
jupyter notebook
（运行）
```
![运行成功](https://upload-images.jianshu.io/upload_images/17476267-824c8addf0918ef3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
当出现如上图所示信息后，浏览器没有自动打开jupyter notebook，那么我么可以手动复制上图链接，在浏览器中打开。
【注释】：在运行过程中，不可以关闭**命令提示符窗口**。
【关于安装和配置可参考】：[https://www.jianshu.com/p/91365f343585](https://www.jianshu.com/p/91365f343585)

##### 3、jupyter notebook的主要优点与用途

- 编程时具有语法高亮、缩进、tab补全的功能。

- 可直接通过浏览器运行代码，同时在代码块下方展示运行结果。

- 以富媒体格式展示计算结果。富媒体格式包括：HTML，LaTeX，PNG，SVG等。
- 支持使用LaTeX编写数学性说明。
 
除此之外，根据 jupyter notebook的特点，它在以下几个板块的学习和开发中，都有不错的帮助：

- 数据处理和转换
- 数值模拟分析
- 统计建模
- 机器学习 

###📖  三、  jupyter notebook的简单使用
```
# 使用tab键来探索代码

# 使用tab键导包时，自动补全
from itertools import count

#结合通配符使用。例如：找到字符串对象中，包含find的方法
str.*find?
```
######jupyter notebook魔法命令 
魔法命令主要分为两大类：
- 行魔法 <sup>（Line magic）</sup> 前缀为 `%`；
- 单元魔法 <sup>（(Cell magic)</sup>  前缀为 `%%`； 

下面将介绍比较常用的一些函数：

魔法函数|函数说明
|:-:|:-:|
%run| 运行脚本文件（执行外部的代码）
%timeit|测试代码性能（测试一行Python语句的执行时间）
%%itmeit|测试代码性能（执行多行语句）
%lsmagic|列出所有魔法命令
%命令? |查看魔法命令详细说明
%history| 输入的历史记录
%xmode|【异常控制 】可以在轨迹追溯中找到错误的原因
%xmode Plain| 以紧凑的方式显示异常信息
%debug |用来在交互环境中，调试程序

详细介绍：
###### 1、**%timeit**   和   **%%timeit**
```jupyter notebook
%timeit L = [n ** 2 for n in range(100)]
```
【输出】：28.2 µs ± 2.24 µs per loop (mean ± std. dev. of 7 runs, 10000 loops each)
```
# %%timeit 用来执行多行语句
%%timeit
L = []
for n in range(100):
    L.append(n ** 2)
```
【输出】：33.7 µs ± 742 ns per loop (mean ± std. dev. of 7 runs, 10000 loops each)

【结论】：`%timeit `的执行更高效。

###### 2、%lsmagic
```
# 列出所有可用的魔法命令
%lsmagic
```
```
Available line magics:
%alias  %alias_magic  %autoawait  %autocall  %automagic  %autosave  %bookmark  %cd  %clear  %cls  %colors  %conda  %config  %connect_info  %copy  %ddir  %debug  %dhist  %dirs  %doctest_mode  %echo  %ed  %edit  %env  %gui  %hist  %history  %killbgscripts  %ldir  %less  %load  %load_ext  %loadpy  %logoff  %logon  %logstart  %logstate  %logstop  %ls  %lsmagic  %macro  %magic  %matplotlib  %mkdir  %more  %notebook  %page  %pastebin  %pdb  %pdef  %pdoc  %pfile  %pinfo  %pinfo2  %pip  %popd  %pprint  %precision  %prun  %psearch  %psource  %pushd  %pwd  %pycat  %pylab  %qtconsole  %quickref  %recall  %rehashx  %reload_ext  %ren  %rep  %rerun  %reset  %reset_selective  %rmdir  %run  %save  %sc  %set_env  %store  %sx  %system  %tb  %time  %timeit  %unalias  %unload_ext  %who  %who_ls  %whos  %xdel  %xmode

Available cell magics:
%%!  %%HTML  %%SVG  %%bash  %%capture  %%cmd  %%debug  %%file  %%html  %%javascript  %%js  %%latex  %%markdown  %%perl  %%prun  %%pypy  %%python  %%python2  %%python3  %%ruby  %%script  %%sh  %%svg  %%sx  %%system  %%time  %%timeit  %%writefile

Automagic is ON, % prefix IS NOT needed for line magics.
```
```
# 查看魔法命令的帮助文档，使用？
%timeit?
```
![%timeit?.png](https://upload-images.jianshu.io/upload_images/17476267-9906931c3d0c20ed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 3、 %xmode
【异常控制】:可以在轨迹追溯中找到错误的原因。

```
def func1(a,b):
    return a / b

def func2(x):
    a = x
    b = x - 1
    return func1(a,b)

func2(1)
```
【输出】：
```
---------------------------------------------------------------------------
ZeroDivisionError                         Traceback (most recent call last)
<ipython-input-7-3129750ef57b> in <module>
      7     return func1(a,b)
      8 
----> 9 func2(1)

<ipython-input-7-3129750ef57b> in func2(x)
      5     a = x
      6     b = x - 1
----> 7     return func1(a,b)
      8 
      9 func2(1)

<ipython-input-7-3129750ef57b> in func1(a, b)
      1 def func1(a,b):
----> 2     return a / b
      3 
      4 def func2(x):
      5     a = x

ZeroDivisionError: division by zero

```
###### 4、%xmode Plain 
```
%xmode Plain # 以紧凑的方式显示异常信息
func2(1)
```
【输出】：
```
Exception reporting mode: Plain
Traceback (most recent call last):

  File "<ipython-input-63-2c0c65acd0a8>", line 2, in <module>
    func2(1)

  File "<ipython-input-61-24291467c7c0>", line 7, in func2
    return func1(a,b)

  File "<ipython-input-61-24291467c7c0>", line 2, in func1
    return a / b

ZeroDivisionError: division by zero
```



























