### 实现注册、登录、查询功能
- （1）注册
- （2）登录
- （3）退出登录
首先附上另一个解法：
- [Python面向过程实现的用户注册、登录、查询功能](https://www.jianshu.com/p/2991a1029ff0)
##### 面向对象与面向过程的区别

- `面向过程`就是分析出解决问题所需要的步骤，然后用函数把这些步骤一步一步实现，使用的时候一个一个依次调用就可以了；
- `面向对象`是把构成问题事务分解成各个对象，建立对象的目的不是为了完成一个步骤，而是为了描叙某个事物在整个解决问题的步骤中的行为。
![面向对象](https://upload-images.jianshu.io/upload_images/17476267-37e1c1ed1c099207.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 二者的优点与缺点

###### 面向过程 
>  - 优点：性能比面向对象高，因为类调用时需要实例化，开销比较大，比较消耗资源;

比如单片机、嵌入式开发、Linux/Unix等一般采用面向过程开发，性能是最重要的因素。
 
>  - 缺点：没有面向对象易维护、易复用、易扩展


###### 面向对象 


 >-  优点：易维护、易复用、易扩展，

由于面向对象有封装、继承、多态性的特性，可以设计出低耦合的系统，使系统更加灵活、更加易于维护。

>-  缺点：性能比面向过程低

- [关于面向对象（这里是一篇比较不错的文章）](https://www.jianshu.com/p/f5659006c082)
---
附上代码：
```
class User:
    def __init__(self,username,pwd):
        self.username = username
        self.pwd = pwd

    def __str__(self):
        return 'username:{},pwd:{}'.format(self.username,self.pwd)

class DB:
    """
    单例模式
    """
    instance = None

    def __new__(cls, *args, **kwargs):
        if DB.instance is None:
            DB.instance = super().__new__(cls)
        return DB.instance

    def __init__(self):
        self.__pool = []

    def save_user(self,user):
        """
        存储数据
        :param user:
        :return:
        """
        self.__pool.append(user)

    def find_user_by_username(self,username):
        """
        查找数据
        :param username:
        :return:
        """
        for user in self.__pool:
            if username == user.username:
                return user
        return None

    def get_all(self):
        return self.__pool[:]
DB1 = DB()
# DB2 = DB()
print(id(DB1))

# 面向过程 ===》 模块划分
class View:
    def show_info(self,info):
        opt = input(info)
        return opt

    def register_info(self):
        """
        读取用户信息
        :return:
        """
        username = input("请输入您的账号：\n")
        pwd =  input("请输入您的密码：\n")
        # self.judge(username,pwd)

    # def judge(self,username,pwd):
        if len(username) <6 or len(username) > 9:
            return print("此账号不合法，长度区间应该在6至9位！")
            return None

        if len(pwd) < 6 or len(pwd) > 9:
            return print("您的密码不合法！")
            return None

        return User(username,pwd)

    def show_user_list(self,users):
        """
        展示用户信息
        :param users:
        :return:
        """
        for user in users:
            print(user)


class Service:
    def __init__(self):
        self.DB1 = DB()

    def register(self,user):
        # View.judge(user.username,user.pwd)
        exist_user = self.DB1.find_user_by_username(user.username)
        if exist_user is not None:
            return print('该账号已经存在！\n')
        self.DB1.save_user(user)
        print('用户注册成功！\n')

    def login(self,user):
        """
        用户注册
        :param user:
        :return:
        """
        # View.judge(user.username, user.pwd)
        exist_user = self.DB1.find_user_by_username(user.username)
        if exist_user is None:
            print("用户账号或密码错误\n")
            return None

        if exist_user.pwd != user.pwd:
            print('用户账号或密码错误\n')
            return None
        return exist_user

    def list(self):
        return self.DB1.get_all()


class App:
    def __init__(self):
        self.v = View()
        self.s = Service()
        self.cur_user = None

    def start(self):
        while True:
            View_opt = self.v.show_info("请选择要进行的操作： \n  (1)注册\n  (2)登录\n  (3)退出\n")
            # v.show_info("请选择要进行的操作： \n (1)查看当前用户信息\n (2)查看用户列表\n(3)退出\n")
            if View_opt == "1":
                user = self.v.register_info()
                self.s.register(user)

            if View_opt == "2":
                user = self.v.register_info()
                # 登录
                login_user = self.s.login(user)
                if login_user is not None:
                    self.cur_user = login_user
                    self.show_home()
            if View_opt == "3":
                print("退出登录~")
                break

    def show_home(self):
        while True:
            user_opt = self.v.show_info("请选择要进行的操作： \n (1)查看当前用户信息\n (2)查看用户列表\n(3)退出\n")
            if user_opt == "1":
                print(self.cur_user)

            if user_opt == "2":
                # 查看用户列表
                users = self.s.list()
                self.v.show_user_list(users)


            if user_opt == "3":
                break

app = App()
app.start()
```

执行结果：

```
请选择要进行的操作： 
  (1)注册
  (2)登录
  (3)退出
1
请输入您的账号：
Zurich
请输入您的密码：
123456
用户注册成功！

请选择要进行的操作： 
  (1)注册
  (2)登录
  (3)退出
1
请输入您的账号：
Alzacar
请输入您的密码：
234567
用户注册成功！

请选择要进行的操作： 
  (1)注册
  (2)登录
  (3)退出
2
请输入您的账号：
Zurich
请输入您的密码：
123456
请选择要进行的操作： 
 (1)查看当前用户信息
 (2)查看用户列表
(3)退出
1
username:Zurich,pwd:123456
请选择要进行的操作： 
 (1)查看当前用户信息
 (2)查看用户列表
(3)退出
2
username:Zurich,pwd:123456
username:Alzacar,pwd:234567
请选择要进行的操作： 
 (1)查看当前用户信息
 (2)查看用户列表
(3)退出
3
请选择要进行的操作： 
  (1)注册
  (2)登录
  (3)退出
3
退出登录~
```
`注释：`此代码块在登录时，若输入的密码长度不符合（6至9位）会报错`@-@`
