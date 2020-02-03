# 关于Django的视图系统
【定义】：一个视图函数（或类），简称为视图，是一个简单的python函数或类，它接受web请求并且返回web响应。

**响应**：可以是一张网页的html内容，一个重定向，一个404错误，一个hxml 文档，或一张图片。

无论视图本身包含什么逻辑，都要返回响应，代码写在那里都无所谓，只要它在你当前项目目录下面。为了将代码放在某处，大家预定成俗将视图放在项目project或应用程序app目录中的名为views.py的文件中。

- (1)、每个视图函数，都使用HttpRequest对象作为第一个参数，并且通常称之为request。

- (2)、每个视图函数，都会返回一个HttpResponse对象，其中包含生成的响应。

Django使用请求和响应对象来通过系统传递状态。

当浏览器向服务端请求一个页面是，Django创建一个HttpRequest对象，该对象包含关于请求的元数据，然后，Django加载相应的视图，将这个HttpRequest对象作为第一个参数传递给视图函数，每个视图函数负责返回一个HttpResponse对象。

**常见的render，redirect，HttpResponse对象返回的都是一个HttpResponse对象。**

---
##### 视图编码分类
- FBV（function based view）：就是将业务逻辑写在一个函数中
- CBV（class  based view）: 就是以类的方式写；

---

##### 给视图加装饰器

（1）FBV加装饰器
常用的一种方式：在函数上方加装饰器  @wrapper_name；

（2）CBV加装饰器

【必须导入】：
```
from django.utils.decorators import method_decorator
```
【方式】：
　2.1、直接在方法上加：　　
```
@method_decorator(wapper_name)
def  get(self,request):pass
```
　　2.2、给自定义的dispatch方法加：
```
@method_decorator(wrapper_name) 
def  dispatch(self,request,*args,**kwargs)</pre>
```
　　2.3、类上加装饰器
```
@method_decorator(wapper_name, name='post')    #给方法post加装饰器
@method_decorator(wapper_name, name='get')
class AddClass(View)

#【注】：其实也可以直接给View中的dispatch方法加装饰器，这样同样可以实现给自己类下的所有方法加装饰器(自己未定义dispatch方法时)
@method_decorator(wapper_name, name='dispatch')
class AddClass(View)
```
【提升】：
为视图加装饰器或者保护csrf_token
```
rom django.views.decorators.csrf import csrf_exempt,csrf_protect
```
---

#### Requset对象  VS  Respondse对象

##### request对象
当一个页面被请求时，Django会形成一个包含被请求信息的对象；
Django会将这个对象自动传递给响应的视图函数，一般视图函数预定俗称的使用request参数承接这个对象。

请求相关的常用值
|请求常用值|作用|
|:-:|:-:|
path_info  | 返回用户访问的url，不包含域名，仅path不带参数     
get_full_path()   |   返回完整路径，不含域名   
method     |   请求中使用的HTTP方法的字符串表示，全大写表示
GET          |    包含所有HTTP  GET参数的类字典对象
POST     | 　  包含所有HTTP  POST参数的类字典对象
body       |      请求体，byte类型的request.POST的数据就是从body里面提取到的
FILES     |      上传文件，{}    注意form标签加**enctype="multipart/form-data"**属性
HTTP_REFERER |   **跳转到上次访问的页面，用户在未登录前要访问的页面**，登录后跳转到该页面， redirect(request.Meta.get('HTTP_REFERER'，'')

---
####  属性
django将请求报文中的请求行、头部信息、内容主体封装成 HttpRequest 类中的属性。

|请求（属性）|作用|
|:-:|:-:|
HttpRequest.scheme|表示请求方案的字符串
HttpRequest.body |一个字符串，代表请求报文的主体
HttpRequest.POST | 要处理表单数据的时候（推荐使用），一个类似于字典的对象，如果请求中包含表单数据，则将这些数据封装成 QueryDict 对象。
HttpRequest.GET| 一个类似于字典的对象，包含 HTTP GET 的所有参数。
HttpRequest.path |  一个字符串，表示请求的路径组件（不含域名）。
HttpRequest.method| 一个字符串，表示请求使用的HTTP 方法。
HttpRequest.encoding| 一个字符串，表示提交的数据的编码方式（默认为 **utf-8**）
HttpRequest.COOKIES| 一个标准的Python 字典，包含所有的cookie。键和值都为字符串。
HttpRequest.FILES| 一个类似于字典的对象，包含所有的上传文件信息。
HttpRequest.META| 一个标准的Python 字典，包含所有的HTTP 首部。具体的头部信息取决于客户端和服务器
HttpRequest.session|　一个既可读又可写的类似于字典的对象，表示当前的会话。只有当Django 启用会话的支持时才可用。


---

【拎出来详谈】HttpRequest.META：是 一个标准的Python 字典，包含所有的HTTP 首部。**具体的头部信息取决于客户端和服务器**。

|头部信息示例|具体意义|
|:-:|:-:|
 CONTENT_LENGTH | 请求的正文的长度（是一个字符串）。
CONTENT_TYPE|  请求的正文的MIME 类型。
HTTP_ACCEPT  |响应可接收的Content-Type。
HTTP_ACCEPT_ENCODING  |响应可接收的编码。
 HTTP_ACCEPT_LANGUAGE | 响应可接收的语言。
 HTTP_HOST |  客服端发送的HTTP Host 头部。
 HTTP_REFERER  | Referring 页面。
 HTTP_USER_AGENT | 客户端的user-agent 字符串。
   QUERY_STRING  | 单个字符串形式的查询字符串（未解析过的形式）。
 REMOTE_ADDR |  客户端的IP 地址。
REMOTE_HOST   |客户端的主机名。
REMOTE_USER   |服务器认证后的用户。
  REQUEST_METHOD  | 一个字符串，例如"GET" 或"POST"。
 SERVER_NAME | 服务器的主机名。
  SERVER_PORT |  服务器的端口（是一个字符串）。

---
|请求方法|具体作用|
|:-:|:-:|
HttpRequest.get_host()|根据从HTTP_X_FORWARDED_HOST（如果打开 USE_X_FORWARDED_HOST，默认为False）和 HTTP_HOST 头部信息返回请求的原始主机。
HttpRequest.get_full_path()|返回 path
HttpRequest.get_signed_cookie(key, default=RAISE_ERROR, salt='', max_age=None)|　返回签名过的Cookie 对应的值，如果签名不再合法则返回django.core.signing.BadSignature。
HttpRequest.is_secure()|如果请求时是安全的，则返回True；即请求通是过 HTTPS 发起的。
HttpRequest.is_ajax()|请求是通过XMLHttpRequest 发起的，则返回True，方法是检查 HTTP_X_REQUESTED_WITH 相应的首部是否是字符串'XMLHttpRequest'。

---

##### Respondse对象
我们写的每个视图都需要实例化，填充和返回一个HttpResponse。HttpResponse类位于django.http模块中。具体属性见下表

|属性|作用|
|:-:|:-:|
HttpResponse.content：|响应内容
HttpResponse.charset：|响应内容的编码
HttpResponse.status_code：|响应的状态码

---

####  render（）
结合一个给定的模板和一个给定的上下文字典，并返回一个渲染后的 HttpResponse 对象。


|参数|作用|
|:-:|:-:|
request |用于生成响应的请求对象。
template_name|要使用的模板的完整名称，可选的参数
context|添加到模板上下文的一个字典。默认是一个空字典。如果字典中的某个值是可调用的，视图将在渲染模板之前调用它。
content_type|生成的文档要使用的MIME类型。默认为 DEFAULT_CONTENT_TYPE 设置的值。默认为'text/html'
status|响应的状态码。默认为200。
useing| 用于加载模板的模板引擎的名称。
【简单的实例】
```
from django.http import HttpResponse
from django.template import loader

def my_view(request):
    # 视图代码写在这里
    t = loader.get_template('myapp/index.html')
    c = {'foo': 'bar'}
    return HttpResponse(t.render(c, request))
```
---
####  redirect（）
【参数】：
- 一个模型：将调用模型的get_absolute_url() 函数
- 一个视图：可以带有参数，将使用urlresolvers.reverse 来反向解析名称
- 一个绝对的或相对的**URL**，将原封不动的作为重定向的位置。

默认情况下，**redirect() 返回一个临时重定向**；传递**permanent=True** 可以返回一个永久的重定向。










