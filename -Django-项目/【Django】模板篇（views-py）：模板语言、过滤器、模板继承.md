### 模板文件
---
templates文件夹下面的文件都叫模板文件。模板文件可以将View视图中需要在前端HTML页面中展示的数据，通过模板引擎的语法规则，展示出来。

使用模板的优势：
能够将业务逻辑的Python代码和页面设计的HTML代码分离，使代码更干净整洁更容易维护，使Python程序员和HTML/CSS程序员分开协作，提高生产的效率，且将HTML代码分离出来，还能使其能够复用；

Django处理模板分为两个阶段：
- 1.加载：根据给定的路径找到模板文件，编译后放在内存中。
- 2.渲染：使用上下文数据对模板插值并返回生成的字符串。

##### 模板语言
---
模板语言分为以下四类：

- （1）变量  {{  }}

- （2）标签  { % 代码块 % }
- （3）过滤器
- （4）注释  {# 代码或html #}

下面将就这几点进行详细介绍。
##### 渲染变量
---
用于渲染变量：
```Django
{{  }}
```
关于变量的【bug 预防】
- 如果变量不存在， 模版系统将插入’’ (空字符串)；
- 变量名必须由字母、数字、下划线（不能以下划线开头）和点组成；
- 当模版引擎遇到一个变量，将计算这个变量，然后将结果输出；
- 在模板中调用方法时不能传递参数；
- 当模版引擎遇到点(".")，会按照下列顺序查询：
    - 字典查询，例如：foo[“bar”]；
    - 属性或方法查询，例如：foo.bar；
    - 数字索引查询，例如：foo[bar]；
    
##### 过虑器
---
语法如下:
```Django
 {{val | filter_name: 参数}}
```
使用管道符号`|`来应用过滤器，用于进行计算、转换操作，可以使用在**变量、标签**中。
如果过滤器需要参数，则使用冒号 `:` 传递参数。

部分过滤器参数举例：
```
{{ bio | truncatewords:"30" }}显示前30个字
{{ "abcd"|capfirst }}：|第一个字母大写
{{ "abcd"|center:"50" }}： 输出指定长度的字符串，并把值对中
{{ "123spam456spam789"|cut:"spam" }}：查找删除指定字符串
{{ value|date:"F j, Y" }}：|格式化日期
{{ value|default_if_none:"(N/A)" }}： |值是None，使用指定值
{{ list|first }} ：返回列表第一个元素
{{ list|join:", " }} ：用指定分隔符连接列表
{{ list|length }} ：返回列表个数
{% if 列表|length_is:"3" %} ：列表个数是否指定数值
{{ object|pprint }}： 显示一个对象的值
{{ 列表|random }}： 返回列表的随机一项
{{ string|removetags:"br p div" }}： 删除字符串中指定html标记
{{ string|rjust:"50" }}： 把字符串在指定宽度中对右，其它用空格填充
{{ 列表|slice:":2" }}： 切片
{{ string|slugify }}： 字符串中留下减号和下划线，其它符号删除，空格用减号替换
{{ 3|stringformat:"02i" }}： 字符串格式，使用Python的字符串格式语法
{{ "E<A>A</A>B<C>C</C>D"|striptags }}： 剥去[X]HTML语法标记
```

##### 渲染标签：
---
```Django
{%  %} 
```
【标签的作用】： 
- 在输出中创建文本；
-  控制循环或逻辑；
 - 加载外部信息到模板中供以后的变量使用；

##### for标签语法
---
```
{%for item in 列表%}
循环逻辑；
{{forloop.counter}}表示当前是第几次循环，从1开始；
{%empty%}
列表为空或不存在时执行此逻辑；
{%endfor%}
```
##### if标签语法

```
{%if ...%}
逻辑1
{%elif ...%}
逻辑2
{%else%}
逻辑3
{%endif%}
```
include：加载模板并以标签内的参数渲染。
```
{ %include "foo/bar.html" % }
```
url：反向解析
```
{ % url 'name' p1 p2 %}
```
srf_token：这个标签用于跨站请求伪造保护
```
{ % csrf_token %}
```
【BUG预防】：模板中不要随意添加空格，尤其是变量和标签。
【补充】关于深度查询：一般利用点来完成.
```django
 {# 想要列表l的第二个元素: l.1 #} 
<p>{{ l.1 }}</p>
{# 获取字典info中name那个key对应的value #}
 <p>{{ info.name }}</p>
{# 获取对象alex的name属性值 #}
<p>{{ alex.name }}</p>
{# 获取person_list列表中第2个对象的age属性值 #} 
<p>{{ person_list.1.age }}</p>
```
##### 模板继承
---
- 模板继承可以减少页面内容的重复定义，实现页面内容的重用-
- 典型应用：网站的头部、尾部是一样的，这些内容可以定义在父模板中，子模板不需要重复定义；
- block标签：在父模板中预留区域，在子模板中填充；
- extends继承：继承，写在模板文件的第一行；
- 定义父模板，例如：base.html；


##### 继承模板
---
```
{% extends 这里填写被继承的HTML页面 %}
{% block 这里填写继承的block %}  {% endblock %}
{% include 这里填写需要引入的HTML子页面 %}
```
- 如果你在模版中使用 `{% extends %}` 标签，它必须是模版中的第一个标签。
- 在base模版中设置越多的 `{% block %}` 标签越好。子模版不必定义全部父模版中的blocks;

##### 子模板中使用block填充预留区域(base.html)
---
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>
        {% block title %}
        {% endblock %} - Microblog
    </title>
</head>
<body>
    <div>
        MicroBlog
        <a href="{{ url_for('index')  }}">Home</a>
        {% if current_user.is_anonymous %}
            <a href="{{ url_for('login')  }}">Login</a>
        {% else %}
            <a href="{{ url_for('logout') }} ">logout</a>
            <a href="{{ url_for('user', username=current_user.username) }}">Profile</a>
        {% endif %}
    </div>

    <hr>
    {% with messages = get_flashed_messages() %}
        {% if messages %}
            <ul>
                {% for message in messages %}
                    <li>{{ message }}</li>
                {% endfor %}
            </ul>
        {% endif %}
    {% endwith %}
    {% block content %}
    {% endblock %}
</body>
</html>
```

【BUG预防】：
-
- 如果在模版中使用extends标签，它必须是模版中的第一个标签
- 不能在一个模版中定义多个相同名字的block标签
- 子模版不必定义全部父模版中的blocks，如果子模版没有定义block，则使用了父模版中的默认值
- 为了更好的可读性，可以给endblock标签一个名字

