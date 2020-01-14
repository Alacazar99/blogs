###📖  关于 Ajax
【简介💬】：Ajax，即异步的 JavaScript XML。（全称为 Asynchronous JavaScript and XML）。Ajax不是一门编程语言，而是利用 JavaScipt 在保证页面不被刷新、页面链接不改变的情况下与服务器交换数据并更新部分网页的技术。
【换句话说💬】：有了 Ajax ，可以在页面没有被全部刷新的情况下，更新其内容。在这个过程中，页面实际上是在后台与服务器进行了数据交互。

【举个栗子💬】：比如说你在网购的时候，浏览某猫网页时，滑动滚轮，会发现下面刷新了商品图片和相关信息，这就是Ajax 动态加载。实际上这个网页并没有刷新（对，网页链接没有更新，没有重新加载这个网页，但是更新了内容。）这个就是通过 Ajax 获取新数据’并呈现的过程

####📖 Ajax 基本原理
发送 Ajax 请求到网页更新的这个过程，可以简单分为3个步骤：
- （1）发送请求；
- （2）解析内容；
- （3）渲染网页；
####📖 Ajax 分析方法

- （1）查找请求

![以本人的简书首页为例](https://upload-images.jianshu.io/upload_images/17476267-5a6eac754bd10922.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Ajax 特殊的请求类型 ： `xhr` 。在Type中，我们可以找到类型为xhr的请求，然后点击，可以看到如下所示的Ajax的请求头的详细内容。

![Ajax-请求头](https://upload-images.jianshu.io/upload_images/17476267-f9bd04a44b6d8bd8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
【总结💬】：我们看见的网页内容的真实数据可能并不是最原始的页面返回的，而是后来执行 JavaScript后，再次向后台发送了 Ajax 请求，浏览器得到数据后再进一步渲染出来的。

- （2）过滤请求

利用 Chrome 开发者 具的筛选功能筛选出所有的Ajax 请求。找到筛选栏，点击`XHR`，如下图所示：

![过滤请求](https://upload-images.jianshu.io/upload_images/17476267-563aebef431bd4bc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####📖 Ajax 数据分析
以简书“发现”的界面为例，每次点击阅读更多之后，就会发出了一个请求，实现Ajax动态加载更多的文章。具体实现见下图：
![Ajax动态加载](https://upload-images.jianshu.io/upload_images/17476267-8bb62065c42a20ad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
然后打开请求，我们可以直观看出以下内容：
- Request Method: POST
- Status Code: 200 
- Remote Address: 111.62.69.240:443
- Referrer Policy: no-referrer-when-downgrade

![请求](https://upload-images.jianshu.io/upload_images/17476267-c537eb170d912eb3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
我们可以借此查看请求的参数有哪些？具体的功能等等。
【重要发现💬】：如果我们想要获取新的文章内容，只要在发送请求时，改变page这个参数的值即可。
####📖  Ajax应用实例：
关于Ajax动态加载在数据爬取中的应用，可以参考一下：
- [【python】爬虫：案例--多线程下载百度贴吧图片](https://www.jianshu.com/p/ad225af44e6b)
- [【python】爬虫：动态网页爬取](https://www.jianshu.com/p/041903e269dc)


























