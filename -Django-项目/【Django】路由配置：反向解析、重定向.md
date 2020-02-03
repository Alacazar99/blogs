## 简单的路由配置

```
from django.conf.urls import url

urlpatterns = [
     url(正则表达式, views视图函数，参数，别名),
]
```
- 正则表达式：一个正则表达式字符串;
- views视图函数：一个可调用对象，通常为一个视图函数或一个指定视图函数路径的字符串;
- 参数：可选的要传递给视图函数的默认参数（字典形式）;
- 别名：一个可选的name参数;

【注意事项】：

（1）：若要从URL 中捕获一个值，只需要在它周围放置一对圆括号。
（2）：不需要添加一个前导的反斜杠，因为每个URL 都有。例如，应该是^articles 而不是 ^/articles。
（3）：每个正则表达式前面的'r' 是可选的但是建议加上。它告诉Python 这个字符串是“原始的” —— 字符串中任何字符都不应该转义
（4）：urlpatterns中的元素按照书写顺序从上往下逐一匹配正则表达式，一旦匹配成功则不再继续



```
from django.contrib import admin from django.urls import path,re_path
from app01 import views
urlpatterns = [    path('admin/', admin.site.urls),

# 路由配置：路径 ----> 视图函数    
re_path(r'^articles/2003/$', views.special_case_2003),    
# ^articles/2003/$ ：正则匹配；匹配以articles/2003/开头、以articles/2003/结尾的路径； 唯一匹配
re_path(r'^articles/([0-9]{4})/$', views.year_archive),    
# ([0-9]{4}) 是一个分组匹配（加了括号）；匹配到路径后，request会传入 year_archive 函数的 第一个参数；
#分组匹配结果会以位置参数传入到year_archive函数的第二个参数， 
#e.g. year_archive(r equest,1999);so year_archive函数需要有两个参数    
# 从上到下执行，所以如果匹配到了2003，会走第一个路径，下面的不再执行    
# 匹配分组之后，视图函数一定要传入相应的位置参数
re_path(r'^articles/([0-9]{4})/([0-9]{2})/$', views.month_archive)

 # 同理， month_archive需要有三个参数

re_path(r'^articles/(?P<y>[0-9]{4})/(?P<m>[0-9]{2})/(?P<c>[0-9]+)/$', views.article _detail)
 
# (?P<名字>)：这是有名分组（就是给每个组取了个名字，用的比较多），有名分组利用的是关键字传参；   
 # 有名分组取的名字一定要和后面函数的形参相同；有名分组传参不依赖于位置顺序 
```
---

##  反向解析
在使用Django 项目时，一个常见的需求是获得URL 的最终形式，以用于嵌入到生成的内容中（视图中和显示给用户的URL等）或者用于处理服务器端的导航（重定向等）。

在需要URL 的地方，对于不同层级，Django 提供不同的工具用于URL 反查：

- 在模板中：使用url 模板标签。(前端网页)
- 在Python 代码中：使用from django.urls import reverse()函数；（写视图函数）

 反向解析的过程：用户通过 /login/ 这个接口 到达urls.py，然后通过 path("login/",views.login,name="log") 到达 views.py（用于视图函数）
```
 # request.method 表示获取 请求方式（GET/POST）

# request.GET 表示获取所有的 GET请求数据；字典的形式        
# <QueryDict: {}>        

 # 反向解析的过程：用户通过 /login/ 这个接口 到达urls.py，然后通过 path("login/",vie ws.login,name="log") 到达 views.py(视图层)     
   
# render(request, "login.html") 方法在渲染 login.html 这个页面的时候，
#会在 action ="{% url 'log' %}" 这一步 找到别名为 "log" 的url，并将 {% url 'log' %} 替换为该url        

# request.PSOT 表示获取所有的 POST请求数据；字典的形式        
# < QueryDict: {'username': ['123'], 'psw': ['123']} >
# 获取请求体（POST）                 
# 每次请求一定要有 返回值     
```
---

##  重定向
在实现逻辑功能时，可能会需要实现重定向的功能。

（1）、通过redirect函数或HttpResponseRedirect函数硬编码的形式
```
from django.shortcuts import redirect,HttpResponseRedirect

return redirect('/app1/alluser/')  # 硬编码形式
return redirect('/app1/finduser/?userid=62')  # 传递参数
--------------------- 
```
（2）、通过URLconf路由命名空间的形式。

```
return redirect('app1_name:alluserpath')
```
（3）、如果在逻辑函数中不做任何处理，可以直接在url中配置。








