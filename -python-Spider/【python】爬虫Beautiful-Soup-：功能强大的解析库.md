###📖 关于Beautiful Soup
【简介💬】：Beautful Soup 就是Python 的一个HTML或者 XML 的解析库，可以用它来方便地从网页中提取数据。

Beautiful Soup 提供一些简单的、python 式的函数来处理导航、搜索、修改分析树等功能。它是一个工具箱，通过解析文档为用户提供需要抓取的数据，因为简单，所以不需要多少代码就可以写出一个完整的应用程序。

【评价💬】：一个灵活又方便的网页解析库，处理高效，支持多种解析器。
##### Beautiful Soup安装
![安装.png](https://upload-images.jianshu.io/upload_images/17476267-fa8d5b37181715af.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
或者：
```
pip install Beautiful Soup
```

###📖 Beautiful Soup的使用
Beautiful Soup支持lxml 、html5lib 、xml等第三方库，优缺点如下：
- **python内置标准库**：执行速度适中，文档容错能力强；
- **使用lxml 库解析**：执行速度快，文档容错能力强，但是要求安装lxml扩展；
- **使用 html5lib 库解析**：有最好的容错率，已浏览器的方式解析文档，生成HTML5格式的文档，但速度较慢；
- **解析库xml**：速度快，但是需要安装外部扩展；
```
from bs4 import BeautifulSoup
def init_soup():
    # python内置标准库：执行速度适中，文档容错能力强；
    soup = BeautifulSoup("<p>Hello</p>","html.parser")

    # 使用lxml 库解析，执行速度快，文档容错能力强，但是要求安装lxml扩展
    soup = BeautifulSoup("<p>Hello</p>","lxml")

    # 有最好的容错率，已浏览器的方式解析文档，生成HTML5格式的文档，但速度较慢；
    soup = BeautifulSoup("<p>Hello</p>","html5lib")

    # 支持解析xml，速度快，但是需要安装外部扩展；
    soup = BeautifulSoup("<p>Hello</p>","xml")
    print(soup.p.string)
```

通过下面这个实例，来具体的探知一下Beautiful Soup 的强大之处：
```
def basic():
    html = """
    <html lang="en">
        <head>
            <meta charset="UTF-8">
            <title>文集精选</title>
        </head>
        <body>
            <p class="title" name="dromouse">
                <b>Zurich_Alacazar</b>
            </p>
            <p class="story">
                从前有座山，山上有座
                <a href="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1565840005673&di=62c1016cfc220878086b827e609cffd9&imgtype=0&src=http%3A%2F%2Fnews.5068.com%2Fupfiles%2Fallimg%2F150131%2F8569_150131085538_1.jpg">城堡</a>，
                城堡里有个
                <a href="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1565840137537&di=d101dc5429cbe5dba1dc4e4273f3ba20&imgtype=0&src=http%3A%2F%2Fpic.58pic.com%2F58pic%2F17%2F54%2F88%2F058PICU58PICuCV_1024.jpg">国王</a>
                还有许多
                <a href="https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=3577049330,1894085967&fm=26&gp=0.jpg">王子公主</a>
            </p>
            <p class="story">
                故事待续...
            </p>
    """

    # 修复文档
    soup = BeautifulSoup(html,"lxml")
    print(soup.p.contents)

    # 节点选择

    # 打印
    print(soup.title)
    # 输出：
    print(type(soup.title))
    print(soup.title.string)
    # 打印head节点的html
    print(soup.head)
    # 获取第一个p标签；
    print(soup.p)

    ### 提取文档信息

    # 获取节点名称
    print(soup.title.name)
    # 获取属性值，字典类型；
    print(soup.p.attrs)
    print(soup.p.attrs["name"])

    # 获取文本内容，string属性；
    print(soup.p.b.string)

    # 嵌套选择
    print(soup.head.title)

    ### 关联选择

    # contents 属性：选取直接子节点
    print(soup.p.contents)

basic()
```
【💬输出】:
```
['\n', <b>Zurich_Alacazar</b>, '\n']
<title>文集精选</title>
<class 'bs4.element.Tag'>
文集精选
<head>
<meta charset="utf-8"/>
<title>文集精选</title>
</head>
<p class="title" name="dromouse">
<b>Zurich_Alacazar</b>
</p>
title
{'class': ['title'], 'name': 'dromouse'}
dromouse
Zurich_Alacazar
<title>文集精选</title>
['\n', <b>Zurich_Alacazar</b>, '\n']
```
####📖 四种对象
Beautiful Soup中，将复杂HTML文档转换成一个复杂的树形结构,每个节点都是Python对象,所有对象可以归纳为4种：
- Tag（ HTML 中的标签）
例如：
```
<title>文集精选</title>
```
【💬两个属性】：name 和 attrs。
- NavigableString（ 标签内部的文字）
例如：
```
print(soup.p.b.string)
```
【💬类型】：类型是一个 `NavigableString`，通俗的理解为**可以遍历的字符串**
- BeautifulSoup（一个文档的全部内容）
例如：
```
    # 获取节点名称
    print(soup.title.name)
    # 获取属性值，字典类型；
    print(soup.p.attrs)
    print(soup.p.attrs["name"])
```
- Comment（特殊类型的 NavigableString 对象）
例如：
```
  print(soup.p.contents)
```
####📖 关于节点
|描述|属性|返会类型
|:-:|:-:|:-:|
获取直接子节点|contents|返回列表
获取所有的子孙元素|descendants|返回类型为生成器；
获取直接父亲元素：|parent属性|仅指父亲节点
获取祖先元素：|parents属性|所有的祖先节点
获取兄弟元素：|next_sibling  、previous_sibling |分别获取下一个兄弟元素和上一个兄弟元素；
获取兄弟元素：|next_siblings 、 previous_siblings  |分别获取前面的兄弟元素和后面的兄弟元素；
【💬实例】如下：
```
def basic2():
    html = """
    <html lang="en">
        <head>
            <meta charset="UTF-8">
            <title>童话故事</title>
        </head>
        <body>
            <p class="title" name="dromouse">
                <b>Zurich_Alacazar</b>
            </p>
            <p class="story">
                从前有座山，山上有座
                <a href="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1565840005673&di=62c1016cfc220878086b827e609cffd9&imgtype=0&src=http%3A%2F%2Fnews.5068.com%2Fupfiles%2Fallimg%2F150131%2F8569_150131085538_1.jpg"><span>城堡</span></a>，
                城堡里有个
                <a href="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1565840137537&di=d101dc5429cbe5dba1dc4e4273f3ba20&imgtype=0&src=http%3A%2F%2Fpic.58pic.com%2F58pic%2F17%2F54%2F88%2F058PICU58PICuCV_1024.jpg"><span>国王</span></a>
                还有许多
                <a href="https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=3577049330,1894085967&fm=26&gp=0.jpg">王子公主</a>
            </p>
            <p class="story">
                故事待续...
            </p>
    """

    # 修复文档
    soup = BeautifulSoup(html,"lxml")
    # 获取直接子节点,返回列表
    print(soup.p.contents)
    # 获取直接子节点，返回生成器；
    for i ,child in enumerate(soup.p.contents):
        print(i,child)

    # 获取所有的子孙元素：descendants,返回类型为生成器；
    for descendants in soup.p.contents:
        print(descendants)
        print('------')

basic2()
```
【💬输出】:
```
['\n', <b>Zurich_Alacazar</b>, '\n']
0 

1 <b>Zurich_Alacazar</b>
2 



------
<b>Zurich_Alacazar</b>
------


------
```

###📖 标准选择器：find_all()函数的使用
```
 find_all(name,attrs,text ,**kwargs,recusive)
# 查找元素们
```

函数|功能
|:-:|:-:|
find()  |返回单个元素，即第一个匹配元素
find_parents()、find_parent  |用来搜索当前节点的父辈节点
find_next_sibling()|对当 tag 的所有后面解析的兄弟 tag 节点进行迭代，返回符合条件的后面的第一个tag节点
find_next_siblings()|对当 tag 的所有后面解析的兄弟 tag 节点进行迭代，返回所有符合条件的后面的兄弟节点
find_previous_sibling()| 返回第一个符合条件的前面的兄弟节点
find_previous_siblings()|返回所有符合条件的前面的兄弟节点
find_all_next() 、find_next()|返回节点后所有的（\第一个）符合条件的节点；
find_all_previous 、 find_previous()|返回节点前所有的（\第一个）符合条件的节点；

```
def build():
    html ="""
        <div class="panel">
            <div class="panel-heading">
                <h4>HELLO WORD</h4>
            </div>
            <div class="panel-body">
                <ul class="list" id="list" name='elements'>
                            <li class="element">11</li>
                            <li class="element">22</li>
                            <li class="element">33</li>
                            <li class="element">44</li>
                </ul>
                <ul class="list list-small" id="list2">
                            <li class="element">1111111</li>
                            <li class="element">666</li>
                </ul>
            </div>
        </div>
    """
    soup = BeautifulSoup(html,"lxml")
    return soup


def test_find_all():
    soup = build()
    # 通过name 属性查找
    print(soup.find_all(name='ul'))
    print(type(soup.find_all(name='ul')[0]))
    print(soup.find_all(name='ul')[0].find_all(name='li'))

    # 通过属性查找
    print(soup.find_all(attrs={'id':'list','name':'elements'}))
    print(soup.find_all(id='list2'))

    # class是关键字，需要给一个下划线；
    print(soup.find_all(class_='list'))

    import re
    # 通过文本查找
    print(soup.find_all(text=re.compile("^1+")))
    f = soup.find_all(text=re.compile("^1+"))
    print(type(f[0]))

# find()  返回单个元素，即第一个匹配元素
# find_parents()|find_parent  :
# find_next_sibling()| find_next_sibling():
# find_previous_sibling()| find previouos_sibling()：
# find_all_next() | find_next(): 返回节点后所有的（\第一个）符合条件的节点；
# find_all_previous | find_previous(): 返回节点前所有的（\第一个）符合条件的节点；

    ### css 选择器：select()
    print(soup.select(".panel-body li"))
test_find_all()
```
【💬输出】:
```
[<ul class="list" id="list" name="elements">
<li class="element">11</li>
<li class="element">22</li>
<li class="element">33</li>
<li class="element">44</li>
</ul>, <ul class="list list-small" id="list2">
<li class="element">1111111</li>
<li class="element">666</li>
</ul>]
<class 'bs4.element.Tag'>
[<li class="element">11</li>, <li class="element">22</li>, <li class="element">33</li>, <li class="element">44</li>]
[<ul class="list" id="list" name="elements">
<li class="element">11</li>
<li class="element">22</li>
<li class="element">33</li>
<li class="element">44</li>
</ul>]
[<ul class="list list-small" id="list2">
<li class="element">1111111</li>
<li class="element">666</li>
</ul>]
[<ul class="list" id="list" name="elements">
<li class="element">11</li>
<li class="element">22</li>
<li class="element">33</li>
<li class="element">44</li>
</ul>, <ul class="list list-small" id="list2">
<li class="element">1111111</li>
<li class="element">666</li>
</ul>]
['11', '1111111']
<class 'bs4.element.NavigableString'>
[<li class="element">11</li>, <li class="element">22</li>, <li class="element">33</li>, <li class="element">44</li>, <li class="element">1111111</li>, <li class="element">666</li>]

```

###📖  CSS选择器
在写 CSS 时，标签名不加任何修饰，类名前加点`.`，id名前加`#` 。同理，在这里我们也可以利用类似的方法来筛选元素，用到的方法是 **soup.select()**，返回类型是  ` list`。
```
print(soup.select("根据不同的需求，进行选择"))
```
方法和上述类似，主要查找方式分为以下五种：
- （1）通过标签名查找；
- （2）通过类名查找；
- （3）通过 id 名查找；
- （4）组合查找；
- （5）属性查找；
