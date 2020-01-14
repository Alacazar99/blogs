##### Django框架
【简介】：Python下有许多款不同的 Web 框架，Django是**重量级的web框架**中最有代表性的框架之一。

下面介绍使用Pycharm图形化界面创建 Django项目 。

#####一、创建Django项目

点击**file   =>  new project**创建新项目。选择Django栏目，输入**项目名称**，这里采用mysite。选择Python解释器版本，点击**create**创建。如图：
![第一步](https://upload-images.jianshu.io/upload_images/17476267-cee6b28230035ac9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 【第二种方式】项目创建过程也可以这样做：
在 **teminal命令窗格** 中通过命令安装Django库：

```
pip install django django-admin 
python manage.py startproject mysite
```
以及开始一个新的项目：
```
python manage.py startproject mysite
```
生成的目录如下图所示：
![第二步](https://upload-images.jianshu.io/upload_images/17476267-c7b8c5144222ee43.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 三、各文件和目录的用途：
上图为Django项目的根目录。它包含了一系列自动生成的目录和文件，具备各自专有的用途。
- 外层的mysite目录与Django无关，只是你项目的容器，可以任意命名；

- **manage.py**：一个命令行工具，用于与Django进行不同方式的交互脚本，非常重要，也是 Django的管理主程序；
-  **内层的mysite/ 目录**是真正的项目文件包裹目录，他的名字是你引用内部文件的包明，例如： mysite.urls。 - mysite/init.py ： 一个定义包的空文件；
-  **mysite/settings.py**： 项目的主配置文件 
- **mysite/urls.py**： 路由文件，所有的任务都是从这里开始分配，相当于Django驱动站点的内容 表格；
 - **mysite.wsgi.py** ： 一个基于WSGI的web服务器进入点，提供底层的网络通信功能（通常不用care...）；

---

#####四、创建APP
　　在每个Django项目中可以包含多个APP，相当于一个大型项目中的**分系统，子模块，功能部件**等等，相互之间比较独立，但也有联系。
APP应用和project项目的区别：
- 一个APP实现某个功能，比如博客，公共档案数据库或者见到的投票系统 ；
- 一个project是配置文件和多个APP的集合，这里APP组合成整个站点 ；
- 一个project可以包含多个APP ；
- 一个APP可以属于多个project；

【所有的APP能够共享项目资源】

在 teminal 中通过命令创建APP：
```
 python manage.py startapp 文件
```
![【第三步】](https://upload-images.jianshu.io/upload_images/17476267-050ca1ff7b97f521.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


---

##### 五、编写路由
　路由都在urls文件里面，它将浏览器输入的url映射到相应的业务处理逻辑。如下所示：
```
from django.urls import path,include

urlpatterns = [
    path('user/', include("user.urls")),
    path("",include("blog.urls"))
]
```

---
##### 六、运行web服务

在 teminal 中通过命令运行web.
```
 python manage.py runserver
```

---

#####七、使用静态文件
前端三大块 *HTML，CSS，JS* 还有各种**插件**在Django项目中存放，我们将这些文件统称为**“静态文件”**，因为 这些文件的**内容基本是固定不变的，不需要动态生成**，所以一般将静态文件放在static目录中（约定俗成的吧...）。
**为了让Django找到这个目录，依然需要对settings进行配置路径**。
例如：

```
// 配置静态文件
STATIC_URL = '/static/'

STATICFILES_DIRS = (
    os.path.join(BASE_DIR,'static'),
)
```

然后在index.html文件中，引入js文件，项目写到现在，大概的项目文件目录如下图所示：
![插入js文件](https://upload-images.jianshu.io/upload_images/17476267-86e7f894d7bff947.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---

##### 八、如何返回动态页面

在网页中，当我们收到用户产生的数据之后，通常可以根据数据，进行处理后返回动态页面。

所以，Django采用自己的模板语言，**根据提供的数据，替换掉HTML中的相应部分。**

【博客-用户注册界面】
在此html代码中，采用Django模板语言进行了替换。
```django
{% extends 'base.html' %}
{% block title %} 微博客-用户注册 {% endblock %}
{% block content %}
    {% load widget_tweaks %}

    <div class="container">
        <div class="row">
            <div class="panel panel-default">
              <div class="panel-heading">
                用户注册
              </div>
              <div class="panel-body">
                  <form id="register_form" action="{% url 'user:register' %}" method="POST">
                      {% csrf_token %}
                      {% for field in form %}
                          <div class="form-group">
                              {{ field.label_tag }}
                              {% render_field field class="form-control" %}
                              {% if field.html_name == 'valid_code' %}
                              <a href="#" id="send_code_to_email">给邮箱发送验证码</a>
                              {% endif %}
                              {{ field.errors }}
                          </div>
                      {% endfor %}
                      <button type="submit" class="btn btn-primary pull-right">提交信息</button>
                  </form>
              </div>
            </div>
        </div>
    </div>
{% endblock %}
```

---

##### 九、数据库连接

（1）首先是注册app（打开mysite/settings.py配置文件，这是整个Django项目的设置中心）：

```
INSTALLED_APPS = [
    'user.apps.UserConfig',
    'blog.apps.BlogConfig',
    # django 自带
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # 第三方app
    'widget_tweaks'
]
```

虽然这一块儿的内容不用深究，但是终究要明白这是什么意思；
【补充】：
```
django.contrib.admin：admin管理后台站点 
django.contrib.auth：身份认证系统 
django.contrib.contenttypes：内容类型框架 
django.contrib.sessions：会话框架 
django.contrib.messages：消息框架 
django.contrib.staticfiles：静态文件管理框架
```
（2）在settings中，配置数据库相关的参数
```
# 数据库连接
DATABASES = {
     'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'vblog',
        'USER': 'root',
        'PASSWORD': '',
        'HOST': '127.0.0.1',
        'PORT': '3306',
    }
}
```
【注意】：确保你在settings文件中提供的数据库用户具有创建数据库表的权限，因为在接下来，我们需要自动创建许多数据表。

（3）编辑 models.py

【解释】：Django通过自定义Python类的形式来定义具体的模型，每个模型的物理存在方式就是一个 Python的类Class，每个模型代表数据库中的一张表，每个类的实例代表数据表中的一行数据，类 中的每个变量代表数据表中的一列字段。

【重点】：**Django通过模型，将Python代码和数据库操作结合起 来，实现对SQL查询语言的封装**。
如下代码，实现了对User类的封装。
```
from django.db import models
from django.utils import timezone
import hashlib

# Create your models here.
class User(models.Model):
    gender_choice = (
        (1, "男"), (0, "女")
    )
    user_name = models.CharField(verbose_name="用户名", unique=True, null= False, max_length=100)
    password = models.CharField(verbose_name="密码", null=False,max_length=200)
    gender = models.CharField(choices=gender_choice, max_length=2)
    email = models.EmailField(null=False, unique=True,max_length=100)
    tel = models.CharField(null=True, unique=True, max_length=20)
    create_time = models.DateTimeField(default=timezone.now)

    class Meta:
        db_table = 'tb_user'
        verbose_name = '用户'
        verbose_name_plural = verbose_name

    def __str__(self):
        return self.user_name

    def save(self, force_insert=False, force_update=False, using=None,
             update_fields=None):
        self.password = hashlib.md5(self.password.encode("utf-8")).hexdigest()
        super().save()
```
Django通过ORM对数据库进行操作，奉行代码优先的理念，将Python程序员和数据库管理员进行分工解耦。

（4）在 teminal 中通过命令创建数据库和数据表
```
 python manage.py makemigrations
 Python manage.py migrate 
```
【总结】：修改模型时的操作分三步：
- 在models.py中修改模型； 
- 运行 python manage.py makemigrations 为改动创建迁移记录； 
- 运行 python manage.py migrate  将操作同步到数据库。


（5）修改views.py中的业务逻辑

看图，如何将数据存入数据库中。
![修改业务逻辑](https://upload-images.jianshu.io/upload_images/17476267-aacc49fc72702c4e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

【最后】：重启Web服务后，刷新页面，之后和用户交互的数据都能保存到数据库中，任何时候都可以 从数据库中读取数据，展示到页面上。
```
python manage.py runserver
```
---
【成果展示】
![大概就做到酱紫](https://upload-images.jianshu.io/upload_images/17476267-054c924fc1916a07.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Django-留言板作品](https://upload-images.jianshu.io/upload_images/17476267-1a0e14fb2612174e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

（留待更新...）






