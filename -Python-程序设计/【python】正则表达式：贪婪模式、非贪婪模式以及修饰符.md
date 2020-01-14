###### 什么是正则表达式
【简介】：正则表达式（Regluar Expressions）又称规则表达式，**用来判断文本的格式**。用正则表达式可以指定想要匹配的字符串规则，然后通过这个规则来**匹配、查找、替换或切割**那些符合指定规则的文本。

【本质】正则表达式本质上是一个**小巧的、高度专用的编程语言**。 许多程序设计语言都支持通过正则表达式进行字符串操作。

主要功能如下：
- 【匹配验证】：判断给定的字符串是否符合正则表达式所指定的过滤规则，从而可以判断某个字符串的内容是否符合特定的规则（如email地址、手机号码等）；

【注意点】：当正则表达式用于匹配验证时，通常需要在正则表达式字符串的首部和尾部加上^和$，以匹配整个待验证的字符串。

- *查找与替换*： 判断给定字符串中是否包含满足正则表达式所指定的匹配规则的子串，如查找一段文本中的所包含的IP地址。另外，还可以对查找到的子串进行内容替换。

- *字符串分割与子串截取*：基于子串查找功能还可以以符合正则表达式所指定的匹配规则的字符串作为分隔符对给定的字符串进行分割


**【知识点补充】：正则表达式的语法是由正则表达式引擎决定的（目前主流的正则引擎分为3类：` DFA、传统型NFA 和 POSIX NFA）`，不同编程语言或应用程序所使用的引擎可能不同，它们对正则表达式的语法支持会有差别。**


---

![常用的一些正则表达式-汇总](https://upload-images.jianshu.io/upload_images/17476267-b3171001875d9f2a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
【程序】
```
import re
tel_reg = "^1[^0-9]{10}$"
r = re.match(tel_reg,'18222203932')
if r:
    print(r.group())
    print(r.span())
else :
    print("电话号码不合法")

输出：
电话号码不合法
```
###### match()

从字符串的起始位置匹配正则表达式。如果匹配成功，就返回匹配结果，否则返回None

- match:从字符串的开头开始匹配，如果一开头不匹配，就会导致失败。
- match一般用来判断文本的格式是否合法。

>- ^ 以..开始，$ 代表..结束
>- ^ 用在[]中，代表排除。
>- [^a-z] 表示不能是小写的a-z
>- 量词：表示规定文本出现的次数。例如：{10} 出现10次。{1，}代表出现1次以上。 {1,10} 出现1-10次。

---
【实例】
```
mail_reg = '\w+@\w+\.(com|org|net)'
tel_reg = "^1\d{10}$"
r = re.match(mail_reg,'98098323@qq.org')
if r:
    print(r.group(0))
    print(r.group(1))
    print(r.span())
else :
    print("邮箱格式不合法")

---
输出：
98098323@qq.org
org
(0, 15)
```
【解释】() 可以对正则表达式进行分组，group(0)或者group()返回的是整个正则表达式的内容。group(n) 返回的是第n个组中的内容，n从1开始。

#####关于字符串的一些用法

|语法|用法|等价|
|:-:|:-:|:-:|
|\d|匹配任意一个数字字符| [0-9]
|\D|匹配任意一个数字字符| [0-9]
|\s|匹配任意一个空白字符串|[t\n\d\f\h]
|\S|匹配任意一个非空白字符串|[^\t\n\d\f\h]|
|\w|匹配任意一个`alphanumeric character`  |[a-z,A-Z,0-9]|
|\W|匹配任意一个`non-alphanumeric`|[a-z,A-Z,0-9]|

---
##### 关于贪婪模式和非贪婪模式
一句话解释：
- 贪婪模式，默认情况下，会尽可能多的匹配符合正则的文本。
- 非贪婪模式，会尽可能多的给下一个匹配正则留出文本。

下面来看两张图：
![贪婪模式匹配过程分析](https://upload-images.jianshu.io/upload_images/17476267-d7a022c6ad324d87.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![ 非贪婪模式匹配过程分析](https://upload-images.jianshu.io/upload_images/17476267-a4f4513e6265f263.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

举个栗子：
```
content = "hello 12324 world, this is a Demo regex"
reg = "^He.*?(\d+).*regex"
r = re.match(reg,content,re.I)
if r:
    print(r.group())
    print(r.group(1))
else :
    print("没有匹配")


输出：
hello 12324 world, this is a Demo regex
12324
```
【总结】贪婪模式与非贪婪模式影响的是被量词修饰的子表达式的匹配行为，贪婪模式在整个表达式匹配成功的前提下，尽可能多的匹配；非贪婪模式在整个表达式匹配成功的前提下，尽可能少的匹配。

【补充】非贪婪模式只**被部分NFA引擎**所支持。从匹配效率上来看，能达到相同匹配结果时，**贪婪模式的匹配效率通常会比较高**，因为它回溯过程会比较少。


---
###### 关于修饰符
正则表达式可以包含一些可选标志修饰符来控制匹配的模式。修饰符被指定为一个可选的标志。**多个标志可以通过`按位 OR(|)` 来指定**。详细介绍见下表所示
|修饰符|描述|
|:-:|:-:|
|re.I |忽略大小写|
|re.S|使.可以匹配包括换行符在内的所有字符|
|re.L|本地化识别<sup>(lacale-aware)</sup> 匹配|
|re.M|多行匹配，影响 ^ 和 $|
|re.U|根据Unicode字符集解析字符|
|re.X|通过给与更灵活的格式以便将正则表达式写的更便于理解|

【示例】
```
reg = 'w{3}\..+\.com\?.*'
reg = "^www\.baidu\.com\?key=dasda$"
content = "www.baidu.com?key=dasda"
r = re.match(reg,content)
print(r)

# search():扫描整个字符串，返回第一个匹配结果
content = "aaa121aad323ddd"
reg = '\d+'
r = re.search(reg,content)
r
print(r.group())

输出：
<re.Match object; span=(0, 23), match='www.baidu.com?key=dasda'>
121

# findall() 可以返回所有匹配的字符串
r = re.findall(reg,content)
print(r)

输出：
['121', '323']
```
看了那么多的讲解...
下面我们来用正则表达式处理一下网易云音乐的一部分源代码，并筛选出歌曲名称。
源码（部分）如下：
```
html = '''
<ol>
<li onmouseover="this.className='z-hvr'" onmouseout="this.className=''" class="">
<span class="no no-top">1</span>
<a href="/song?id=1374483648" class="nm s-fc0 f-thide" title="Beautiful People (feat. Khalid)">Beautiful People (feat. Khalid)</a>
<div class="oper">
</div>
【中间部分太多...部分省略】
</li>
<li onmouseover="this.className='z-hvr'" onmouseout="this.className=''" class="">
<span class="no">10</span>
<a href="/song?id=1361615183" class="nm s-fc0 f-thide" title="你在哪里">你在哪里</a>
<div class="oper">
<a href="#" class="s-bg s-bg-11" title="播放" hidefocus="true" data-res-type="18" data-res-id="1361615183" data-res-action="play" data-res-from="31" data-res-data="19723756"></a>
</li>
</ol>
'''
```
处理过程：
```
reg = "<li.*?<a href.*?>(.*?)</a>"
r = re.findall(reg,html,re.S)
if r:
    for song in r:
       print("【",song,"】")

# sub()用来替换符合格式的内容
content = '12fdf322fdsfjj323njjdsjkf'
reg = '\d+'
r = re.sub(reg,'',content)
print(r)
```
输出歌曲名称：
```
【 Beautiful People (feat. Khalid) 】
【 Heartbeat (BTS WORLD OST) 】
【 I Fell In Love With The Devil (Radio Edit) 】
【 你好 】
【 晚安 】
【 平胸女子 】
【 憾 】
【 Room to Fall 】
【 キスだけで 】
【 你在哪里 】
```
---

###### 【知识补充】
- 时间匹配：compile()

- sub()：用来替换符合格式的内容

【示例】
```
content = 'Zurich19982321426.Alacazar'
reg = '\d+'
r = re.sub(reg,'',content)
print(r)

c1 = "2019-6-29 14:10"
c2 = "2016-9-13 17:40"
p = re.compile("\d{2}:\d{2}")
print(re.sub(p,'',c1))
print(re.sub(p,'',c2))
```
输出：
```
Zurich.Alacazar
2019-6-29 
2016-9-13 
```
---
【参考】关于正则表达式的文章：
https://www.cnblogs.com/yyds/p/6913550.html
[https://blog.csdn.net/luanpeng825485697/article/details/78386400](https://blog.csdn.net/luanpeng825485697/article/details/78386400)

![正则表达式汇总（来源网络）感谢感谢，这张表总结的很全了~](https://upload-images.jianshu.io/upload_images/17476267-6f1ef5f97d4ed78e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
