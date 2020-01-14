![一张图解释：requests在python爬虫应用中的地位](https://upload-images.jianshu.io/upload_images/17476267-32fcb2459d2f9062.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#####Requests库概述
【介绍💬】：使用requests可以模拟浏览器的请求。（requests模块本质上是封装了的urllib3，所以比urllib更实用，所以关于urllib，大致了解就可以了，用来写爬虫，代码会很多...）

安装requests
```python
 pip install requests
```
######🐾Requests库的突出优势
【详解💬】：由于requests模块本质上是封装了的urllib3，所以我们不需要手动为 URL 添加查询字串，也不需要对 POST 数据进行表单编码。Keep-alive 和 HTTP 连接池的 功能是 100% 自动化的。
######为什么使用Requests库

>- requests在python2 和python3中通用，方法完全一样；
>- **工作中爬虫基本都使用requests**；
>- requests能够自动帮助我们解压(gzip压缩的等)网页内容，简单易用 Requests支持HTTP连接保持和连接池，支持使用cookie保持会话，支持文件上传，支 持自动确定响应内容的编码，支持国际化的 URL 和 POST 数据自动编码；

**模拟浏览器发送post请求用法**：
```python
response = requests.post(url, data = data, headers=headers)
```
>-  Requests库使用非常广泛，Twitter、Spotify、Microsoft、Amazon、以及若联邦政府机构都在内部使用。 

---

######🐾 Requests库的功能特性
- Keep-Alive & 连接池 
- 国际化域名和 URL
-  带持久 Cookie 的会话
-  浏览器式的 SSL 认证 
- 自动内容解码
-  基本/摘要式的身份认证 
- 优雅的 key/value Cookie
-  自动解压
-  Unicode 响应体
-  HTTP(S) 代理支持
- 文件分块上传 
- 连接超时 
- 分块请求 （等）

---

 ##### cookie和session 
本质上都是基于键值对的字符串。


 ######Cookies 
【简介💬】：Cookies 指某些网站为了辨别用户身份、 进行会话跟踪而存储在用户本地终端上的数据。
那么Cookies在哪儿呢？举个栗子：

![cookies来源 Boss直聘](https://upload-images.jianshu.io/upload_images/17476267-d6a903385210fd6d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 ###### 两者区别:
 - cookie数据存放在客户的浏览器上，session数据放在服务器上 ；
- cookie不是很安全，别人可以分析存放在本地的cookie并进行cookie欺骗（通过用户的cookies获取相关信息）
- session会在一定时间内保存在服务器上。当访问增多，会比较占用服务器的性能；
 - 单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie。

######【优缺点💬】：
- 带上cookie、session的**好处**：很多网站必须登录之后(或者获取某种权限之后)才能能够请求到相关数据 
- 带上cookie、session的**弊端**：一套cookie和session往往和一个用户对应.请求太快，请求次数太多， 容易被服务器识别为爬虫。从而是账号收到损害；

######【使用建议💬】：
- 不需要cookie的时候尽量不去使用cookie 
- 为了获取登录之后的页面，我们必须发送带有cookies的请求，此时为了确保账号安全应该尽量降低数据采集速度。

【小案例💬】：运行小程序，获取网页的cookie
```
# 获取cookie
def opt_cookie():
    browser = webdriver.Chrome()
    browser.get("https://www.baidu.com")
    # 获取所有cookie
    print(browser.get_cookies())
    # 添加cookie
    browser.add_cookie({"name":"username", "value":"Zurich"})
    print(browser.get_cookies())
    # 删除 cookie
    browser.delete_all_cookies()
    print(browser.get_cookies())
opt_cookie()
```
---

##### 🐾爬虫的简单应用

【举个实例】：实现 “在线翻译”
【详解】：主要是借用了[http://fy.iciba.com/](http://fy.iciba.com/)
的在线翻译接口，然后使用较简洁的程序来实现“在线翻译”功能。
![网站首页](https://upload-images.jianshu.io/upload_images/17476267-a7640819bb485191.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

【程序如下】：
```
class JinShan(object):
    def __init__(self,word):
        self.url = 'http://fy.iciba.com/ajax.php?a=fy'
        self.headers = {
            "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_3) AppleWebKit/537. 36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36"
        }
        self.post_data = {"f": "auto", "t": "auto", "w": word}

    def request_post(self):
        response = requests.post(url=self.url, headers=self.headers,data=self.post_data)
        # print(response.content.decode())
        return response.content.decode()

    def translates(self,data):
        dict_data = json.loads(data)
        if dict_data['status'] == 1:
            print(dict_data['content']['out'])
        if dict_data['status'] == 0:
            print(dict_data['content']['word_mean'])

    def run(self):
        data = self.request_post()
        print('译文：')
        self.translates(data)

if __name__ == "__main__":
    print('输入您要翻译的内容：')
    word = input()
    jinshan = JinShan(word)
    jinshan.run()
```
运行结果：
- 汉译英
```
输入您要翻译的内容：
使用requests可以模拟浏览器的请求。
译文：
Use requests to simulate browser requests.
```
- 英译汉
```
输入您要翻译的内容：
Use requests to simulate browser requests.
译文：
使用请求模拟浏览器请求。
```
---

【🌰再举个实例🌰】：爬取百度贴吧
```
import requests
import sys

class BaiduTieBa:
    # 构建请求地址
    def __init__(self,name,pn):
        self.name = name
        self.url = "http://tieba.baidu.com/f?kw={}&ie=utf-8&tab=main".format(name)
        self.headers = {
            "User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.142 Safari/537.36"
        }

        self.url_list = [self.url + str(n*50) for n in range(pn)]
        print(self.url_list)

    def get_data(self,url):
        """
        请求数据
        :param url:
        :return:
        """
        r = requests.get(url, headers = self.headers)
        return r.content

    def save_data(self,data,num):
        # 保存数据
        file_name = self.name + "_" + str(num) + ".html"

        with open(file_name,'wb') as f:
            f.write(data)

    def run(self):
        for url in self.url_list:
            data = self.get_data(url)
            num = self.url_list.index(url)
            self.save_data(data,num)

if __name__ == "__main__":
    name = sys.argv[1]
    pn = int(sys.argv[2])
    baidu = BaiduTieBa(name,pn)
    baidu.run()
```
运行：
```
文件路径...>baidutieba>python baidu_tieba.py 清华大学 3
```
【程序解读】：关于请求头，来源于百度首页，见下图。
```
self.headers = {
    "User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.142 Safari/537.36"
}
```
![User-Agent](https://upload-images.jianshu.io/upload_images/17476267-a5e0900794439f9d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

【🐾运行结果】：


![（已经被识别是爬虫🕸🕷，所以加载出来的图片都是一样的😂😂）](https://upload-images.jianshu.io/upload_images/17476267-6b8ea2e73da057c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---
