【写在前面】在学习python装饰器之前呢，先要对python中的闭包函数有一定的理解，附上一篇关于闭包的文章（写的很棒）：https://blog.csdn.net/weixin_44141532/article/details/87116038

![学Python，就来关注我叭](https://upload-images.jianshu.io/upload_images/17476267-b33048873491757c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

闭包格式：
```
def 外层函数(参数):
    def 内层函数():
        print("内层函数执行", 参数)
    return 内层函数


内层函数的引用 = 外层函数("传入参数")
内层函数的引用()
```
外层函数中的参数，不一定要有，据情况而定，但是一般情况下都会有并在内函数中使用到。

---
好了，下面进入主题，来了解一下到底什么是装饰器，好像关于这个我有在前面的简书中略有提及。`python装饰器本质上就是一个函数，它可以让其他函数在不需要做任何代码变动的前提下增加额外的功能，装饰器的返回值也是一个函数对象（函数的指针）。`
装饰器函数的外部函数传入我们要装饰的函数名字，返回经过修饰后函数的名字；内层函数（闭包）负责修饰被修饰函数。
##### 关于装饰器的几点属性：
- 实质： 是一个函数

- 参数：要装饰的函数名（并非函数调用）

- 返回：是装饰完的函数名（也非函数调用）

- 作用：为已经存在的对象添加额外的功能

- 特点：不需要对对象做任何的代码上的变动


举个栗子来看看函数装饰器
```
def log_wrapper(fun):
    def inner_fun(*args, **kwargs):
        print("用户输入：",args)
        rs = fun(*args,**kwargs)
        print("系统返回结果：",rs)

    return inner_fun

def valid_wrapper(fun):
    def inner_fun(*args,**kwargs):
        print("验证数据有效性")
        username = kwargs.get('username')
        pwd = kwargs.get('pwd')

        if len(username) < 2:
            print("用户名不合法")
            return 'invalid'
        if len(pwd) < 2:
            print("密码长度不合法")
            return 'invalid'

        return 'valid'

    return inner_fun

#带参数的装饰器
def valid_param(length):
    def inner_wrapper(fun):
        def inner_fun(*args,**kwargs):
            username = kwargs.get('username')
            pwd = kwargs.get('pwd')

            if len(username) < length:
                print("用户名长度小于%d" % length)
                return 'invalid'
            if len(pwd) < length:
                print("密码长度长度小于%d" % length)
                return ''

            return 'valid'
        return inner_fun
    return inner_wrapper


# @valid_wrapper
@valid_param(4)
@log_wrapper
def login(username,pwd):
    if username == "zs" and pwd == "123":
        return True
    return False

rs = login(username="zs1234",pwd="123")
print(rs)

----
output:  密码长度长度小于4
```
加了装饰器以后，函数的函数值就是装饰器的返回值


【总结】装饰器最大的作用就是`对于我们已经写好的程序，我们可以抽离出一些雷同的代码组建多个特定功能的装饰器`，这样我们就可以针对不同的需求去使用特定的装饰器，这时因为源码去除了大量泛化的内容而使得源码具有更加清晰的逻辑。

---
##### 下面介绍一下类装饰器
先上代码：
```
# 类装饰器
def valid_user_exist(cls):
    class WrapperClass:
        def __init__(self,username,pwd):
            print(username,pwd)
            self.username = username
            self.pwd = pwd

        def register(self):
            if self.username == "zs":
                return "user exist"

            wrapper = cls(self.username,self.pwd)
            wrapper.register()
            return "register success"

    return WrapperClass

@valid_user_exist
class User:
    def __init__(self,username,pwd):
        self.username = username
        self.pwd = pwd

    def register(self):
        print(self.username + "注册成功~！")

u = User('Zurich','1234')
rs = u.register()
print(rs)
```
前面已经提及了函数装饰器，让函数去装饰其他的函数或者方法，那么同样的，我们也可以使用一个类去装饰其他的类或者方法；
`【一切函数、类...皆对象】`(我喜欢这句话)
输出：
```
Zurich 1234
Zurich注册成功~！
register success
```
要看看内存分析吗
![分析](https://upload-images.jianshu.io/upload_images/17476267-768a7ed52ac31fa6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---
【推荐】：这里看见篇写的非常好的文章：https://www.cnblogs.com/lianyingteng/p/7743876.html


