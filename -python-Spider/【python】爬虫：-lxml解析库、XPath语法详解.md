#### lxml解析库
【内容💬】： lxml是python的一个解析库，支持HTML和XML的解析，支持XPath解析方式，而且解析效率非常高。

![](https://upload-images.jianshu.io/upload_images/17476267-058f95f666196b5e.gif?imageMogr2/auto-orient/strip)

安装：在 teminal 中通过如下命令安装。
```
pip install lxml
```
---

######XPath语法
【简介💬】：XPath是一门在XML和HTML文档中查找信息的语言，可以用来在XML和HTML文档中对元素和属性进行遍历搜索。

![XPath（图片来源网络）](https://upload-images.jianshu.io/upload_images/17476267-2757df283766c368.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


XPath，全称XML Path Language，即**XML路径语言**，具体内容见下表：
表达式	|描述
|:-:|:-:|
nodename|	选取此节点的所有子节点
/	|从当前节点选取直接子节点
//	|从当前节点选取子孙节点
.	|选取当前节点
..	|选取当前节点的父节点
@	|选取属性
*	|通配符，选择所有元素节点与元素名
@*	|选取所有属性
[@attrib]	|选取具有给定属性的所有元素
[@attrib='value']|选取给定属性具有给定值的所有元素
[tag]|选取所有具有指定元素的直接子节点
[tag='text']|选取所有具有指定元素并且文本内容是text节点

下面将对表中的表达式做一些详细的使用介绍。具体的xpath函数可以参考：[https://docs.microsoft.com/zh-cn/previous-versions/ms256138(v=vs.120)](https://docs.microsoft.com/zh-cn/previous-versions/ms256138(v=vs.120))

XPath 函数分组|描述
|:-:|:-:|
[Node-Set](https://docs.microsoft.com/zh-cn/previous-versions/ms256482%28v%3dvs.120%29)| 接受 *node-set* 参数，返回参数集，或返回/提供有关节点集中特定节点的信息。
[String](https://docs.microsoft.com/zh-cn/previous-versions/ms256180%28v%3dvs.120%29) | 对字符串参数执行计算、格式化和处理。
[Boolean](https://docs.microsoft.com/zh-cn/previous-versions/ms256218%28v%3dvs.120%29) | 计算参数表达式，以获取布尔值结果。
[数字](https://docs.microsoft.com/zh-cn/previous-versions/ms256035%28v%3dvs.120%29)|计算参数表达式，以获取数字结果。
[Microsoft XPath 扩展函数](https://docs.microsoft.com/zh-cn/previous-versions/ms256453%28v%3dvs.120%29) | 提供按 XSD 类型选择节点的功能的 Microsoft XPath 扩展函数。 此外，还包括字符串比较函数、数字比较函数和日期/时间转换函数。
---
字符串函数 (XPath)	|描述
|:-:|:-:|
| [concat](https://docs.microsoft.com/zh-cn/previous-versions/ms256123%28v%3dvs.120%29) | 返回参数的串联。 
| [contains](https://docs.microsoft.com/zh-cn/previous-versions/ms256195%28v%3dvs.120%29) | 如果第一个参数字符串包含第二个参数字符串，则返回 true；否则，返回 false。
| [normalize-space](https://docs.microsoft.com/zh-cn/previous-versions/ms256063%28v%3dvs.120%29) | 返回去除了空白的参数字符串。
| [starts-with](https://docs.microsoft.com/zh-cn/previous-versions/ms256174%28v%3dvs.120%29) | 如果第一个参数字符串以第二个参数字符串开头，则返回 true；否则，返回 false。
[string](https://docs.microsoft.com/zh-cn/previous-versions/ms256133%28v%3dvs.120%29) | 将对象转换为字符串。
[string-length](https://docs.microsoft.com/zh-cn/previous-versions/ms256171%28v%3dvs.120%29) | 返回字符串中的字符数。
[substring](https://docs.microsoft.com/zh-cn/previous-versions/ms256054%28v%3dvs.120%29)| 返回第一个参数中从第二个参数指定的位置开始、第三个参数指定的长度的子字符串。
[substring-after](https://docs.microsoft.com/zh-cn/previous-versions/ms256455%28v%3dvs.120%29)| 返回第一个参数字符串中第一次出现第二个参数字符串之后的子字符串。
[substring-before](https://docs.microsoft.com/zh-cn/previous-versions/ms256071%28v%3dvs.120%29) | 返回第一个参数字符串中第一次出现第二个参数字符串之前的子字符串。
[translate](https://docs.microsoft.com/zh-cn/previous-versions/ms256119%28v%3dvs.120%29) | 返回第一个参数字符串，出现第二个参数字符串中的字符的位置替换为第三个参数字符串中对应位置的字符。

---

##### 💬构建etree 实例
- 文本构建
```
def load_text():
    text= """
        <div>
            <ul>
                <li class="item-0"><a href="link1.html">first item</a></li>
                <li class="item-1"><a href="link2.html">second item</a></li>
                <li class="item-inactive"><a href="link3.html">third item</a></li>
                <li class="item-1"><a href="link4.html">fourth item</a></li>
                <li class="item-0"><a href="link5.html">lxml是功能最丰富且易于使用的库，用于处理Python语言中的XML和HTML。</a>
            </ul>
        </div>
    """
    htmlcontent = etree.HTML(text)
    result = etree.tostring(htmlcontent)
    print(result.decode('utf-8'))
```
- 文件构建
```
def load_text2():
    html = etree.parse("./test.html",etree.HTMLParser())
    print(type(html))
    result = etree.tostring(html)
    print(result.decode('utf-8'))

load_text2()
```

【注意点💬】：
- 🔹etree.tostring()方法返会的是bytes类型，需要调用decode()方法来转换为sgr类型。
- 🔹经过处理后的Html的代码，会被自动修复，补全缺失的标签；

---

##### 节点：XML路径语言的使用范例
- ##### 选中所有节点
```
def select_nodes():
    # 选中所有节点
    html = etree.parse("./test.html",etree.HTMLParser())
    results = html.xpath("//*")
    print(results)

    # 选中所有的li节点
    results = html.xpath("//li")
    print(results)
    print(results[4])

select_nodes()
```
【注释💬】：xpath("//")  **双斜杠开头**即为**选中全部**。

- ##### 选中所有的子节点
```
def select_child_nodes():
    # 选中所有的子节点
    html = etree.parse("./test.html",etree.HTMLParser())
    results = html.xpath("//li/a")
    print(results)

    # 选中所有 子孙节点
    results = html.xpath("//ul//a")
    print(results)
select_child_nodes()
```
输出：
```
[<Element a at 0x347ec60>, <Element a at 0x347ec38>, <Element a at 0x347ec10>, <Element a at 0x347e940>, <Element a at 0x347e738>]
[<Element a at 0x347ec60>, <Element a at 0x347ec38>, <Element a at 0x347ec10>, <Element a at 0x347e940>, <Element a at 0x347e738>]
```

- ##### 选中父亲节点

```
def select_parent_nodes():
    html = etree.parse("./test.html",etree.HTMLParser())
    # 选中属性href='link4.html'的a标签的父亲
    results = html.xpath("//a[@href='link4.html']/..")
    print(results)

    # 选中属性href='link4.html'的a标签的父亲的class属性
    results = html.xpath("//a[@href='link4.html']/../@class")
    print(results)

    # 另一种方法：使用parent：：选中
    results = html.xpath("//a[@href='link4.html']/parent::*/@class")
    print(results)

select_parent_nodes()

————————
输出：
[<Element li at 0x398ed50>]
['item-1']
['item-1']
```

---
##### 根据属性解析
```
def select_node_by_attrs():
    # 根据属性解析
    html = etree.parse("./test.html",etree.HTMLParser())
    re = html.xpath("//li[@class='item-0']")
    print(re)
select_node_by_attrs()

----
输出：
[<Element li at 0x10ced28>, <Element li at 0x10ced00>]
````
##### 解析获取 文本内容

```
def get_text():
    html = etree.parse("./test.html",etree.HTMLParser())
    print(etree.tostring(html).decode("utf-8"))
    # 单斜杠：获取自身文本内容；
    re = html.xpath("//li[@class='item-0']/text()")
    print(type(re))
    print(re)
    # 双斜杠：获取自己以及子孙的 文本内容；
    re = html.xpath("//li[@class='item-0']//text()")
    print(re)

get_text()
```
【技能点💬】：
- 🔹单斜杠：获取自身文本内容；
- 🔹双斜杠：获取自己以及子孙的 文本内容；

##### 解析属性，获取值
```
def get_attr_content():
    html = etree.parse("./test.html",etree.HTMLParser())
    # 获取全部文本；
    print(etree.tostring(html).decode("utf-8"))
    # 获取属性的值
    re = html.xpath("//li/a/@href")
    print(re)

get_attr_content()
```
输出：
```
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN" "http://www.w3.org/TR/REC-html40/loose.dtd">
<html><body><div>&#13;
    <ul>&#13;
        <li class="item-0"><a href="link1.html">first item</a></li>&#13;
        <li class="item-1"><a href="link2.html">second item</a></li>&#13;
        <li class="item-inactive"><a href="link3.html">third item</a></li>&#13;
        <li class="item-1"><a href="link4.html">fourth item</a></li>&#13;
        <li class="item-0"><a href="link5.html">lxml</a>&#13;
    </li></ul>&#13;
</div></body></html>
['link1.html', 'link2.html', 'link3.html', 'link4.html', 'link5.html']
```
##### 多值属性匹配
使用contains()函数。
```
def get_multi_attr_content():
    text = """
    <li class="li li-first"><a href="link1.html">first item</a></li>
    """
    html = etree.HTML(text)
    re = html.xpath("//li[@class='li']/a/text()")
    print(re)

    re = html.xpath("//li[contains(@class,'li')]/a/text()")
    print(re)

get_multi_attr_content()
```

##### 多属性匹配
```
def get_multi_attr_match():
    text = """
    <li class="li li-first" name = "item"><a href="link1.html">first item</a></li>
    """
    html = etree.HTML(text)
    re = html.xpath("//li[contains(@class,'li') and @name='item']/a/text()")
    print(re)

get_multi_attr_match()

---
输出：
['first item']
```
【程序解析】：and 是xpath中的运算符。常见运算符如下表：
XPath中的运算符	|描述	|实例	|返回值
|:-:|:-:|:-:|:-:|
or|或	|age=19 or age=20	|如果age等于19或者等于20则返回true反正返回false
|and	|与	|age>19 and age<21|如果age等于20则返回true，否则返回false
mod	|取余	|5 mod 2	|1

【更多关于运算符，可参考】：[http://www.w3school.com.cn/xpath/xpath_operators.asp](http://www.w3school.com.cn/xpath/xpath_operators.asp)


##### 

```
def select_node_by_order():
    html = etree.parse("./test.html", etree.HTMLParser())
    # 选取第一个节点
    re = html.xpath("//li[1]/a/text()")
    print(re)
    # 选取最后一个节点
    re = html.xpath("//li[last()]/a/text()")
    print(re)
    # 选取前两个节点
    re = html.xpath("//li[position()<3]/a/text()")
    print(re)
    # 选取倒数第三个节点
    re = html.xpath("//li[last()-2]/a/text()")
    print(re)
select_node_by_order()

---
输出：
['first item']
['lxml']
['first item', 'second item']
['third item']
```

##### 节点轴选择
```
def select_node_by_axies():
    text = """
    <div>
        <ul>
            <li class="item-0" name="first-G"><a href="link1.html"><span>first item</span></a></li>
            <li class="item-1"><a href="link2.html">second item</a></li>
            <li class="item-inactive"><a href="link3.html">third item</a></li>
            <li class="item-1"><a href="link4.html">fourth item</a></li>
            <li class="item-0"><a href="link5.html">lxml</a>
        </ul>
    </div>
    """
    html = etree.HTML(text)
    # 选中指定元素的所有祖先
    re = html.xpath("//li[1]/ancestor::*")
    print(re)

    # 选中祖先的所有div
    re = html.xpath("//li[1]/ancestor::div")
    print(re)

    # 获取所有的属性值
    re = html.xpath("//li[1]/attribute::*")
    print(re)

    #寻找直接子节点中满足条件的元素;
    re = html.xpath("//li[1]/child::a[@href='link1.html']")
    print(re)

    # 寻找所有后代的元素
    re = html.xpath("//li[1]/descendant::*")
    print(re)

    # 寻找所有后代的元素中的有sapn标签的元素
    re = html.xpath("//li[1]/descendant::span")
    print(re)

    # 获取该节点后面的兄弟节点以及这些兄弟节点的子孙节点;
    re = html.xpath("//li[1]/following::*")
    print(re)

    # 获取该节点后面的兄弟节点以及这些兄弟节点的子孙节点中的第一个节点;
    re = html.xpath("//li[1]/following::*[1]")
    print(re)

    # 获取获取该节点后面的所有兄弟节点
    re = html.xpath("li[1]/following-sibling::*")
    print(re)

select_node_by_axies()
```
输出：
```
[<Element html at 0x3453cd8>, <Element body at 0x3481288>, <Element div at 0x3481260>, <Element ul at 0x3481238>]
[<Element div at 0x3481260>]
['item-0', 'first-G']
[<Element a at 0x3481260>]
[<Element a at 0x3481260>, <Element span at 0x3481288>]
[<Element span at 0x3481288>]
[<Element li at 0x3481238>, <Element a at 0x34812b0>, <Element li at 0x3481210>, <Element a at 0x347cf08>, <Element li at 0x347cd00>, <Element a at 0x347cd28>, <Element li at 0x347cd50>, <Element a at 0x347cb20>]
[<Element li at 0x3481238>]
[]

```






