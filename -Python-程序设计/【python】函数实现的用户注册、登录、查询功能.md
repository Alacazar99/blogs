使用字典表示用户对象，例如：[{‘name’:'zhangsan','pwd':'123456'，hasLogin:false}],将字典放入list中来表示数据库，请完成用户 注册、登录功能用户信息。

- （1）注册
- （2）登录
- （3）退出登录

尽可能的使用多个功能简单的函数来实现此过程：

```py
# @Time    : 19-5-25
# @Author  : Zurich.alcazar
# @Email   : 1178824808@qq.com
# @Software: PyCharm

users = [] 
logined_user = None # 定义一个全局变量
def read_username():
    '''
    获取用户名
    :return:
    '''
    username = input('请输入您的用户名(长度为6-9):\n')

    #验证长度
    if len(username) < 6 or len(username) > 9:
        print('您的用户名长度不合法!')
        return read_username()
    return username

    # 验证用户名是否有重复
    name_list = [user.get('username') for user in users]
    if username in name_list:
        print("该用户名已存在!")
        return read_username()

def user_pwd():
    '''
    获取用户密码
    :return:
    '''
    pwd = input('请输入您的密码:\n')
    if len(pwd) < 6 or len(pwd) > 9:
        print('您的密码长度不合法!')
        return user_pwd()
    return pwd

def regiter():
    '''
    用户注册
    :return:
    '''
    username = read_username()
    pwd = user_pwd()
    user = {}
    user['username'] = username
    user['pwd'] = pwd
    users.append(user)
    print('恭喜您注册成功~!')
    print(users) #检验用户注册

def find_user(username):
    '''
    根据用户名查找用户
    :return:
    '''
    for user in users:
        if user['username'] == username:
            return user
    return None

def read_login_username():
    '''
    读取用户名
    :return:
    '''
    username = input('请输入您的用户名:\n')
    user = find_user(username)
    if user is None:
        print('用户不存在!')
        return read_login_username()
    return username

def login():
    """
    用户登录
    :return:
    """
    username = read_login_username()
    pwd = input('请输入您的密码:\n')
    user = find_user(username)
    if user['pwd'] != pwd:
        print('密码输入错误!')

    # 登录成功
    global logined_user
    logined_user = user

    print("登录成功")
    login_success()

def print_user():
    """
    打印当前用户信息
    :return:
    """
    global logined_user
    for k,v in logined_user.items():
        print(k ,":" ,v)

def login_success():
    while True:
        opt = input("请选择以下操作：\n (1)查看当前用户信息\n (2)查看用户列表\n (3)退出登录\n")
        if opt == "1":
            print_user()

        if opt == "2":
            print(users)

        if opt == "3":
            return

def menu():
    '''
    显示界面
    :return:
    '''
    while True:
        opt = input('请输入用户名:\n (1)注册 \n (2)登录\n (3)退出\n')

        if opt == '1':
            regiter()
        if opt == '2':
            login()
        if opt == '3':
            print('您已退出登录~!')
            break
menu()
```
内存分析：
![内存分析](https://upload-images.jianshu.io/upload_images/17476267-7d963ffcd4acc685.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

执行结果：

---------------------------
请选择要执行的操作:
 (1)注册 
 (2)登录
 (3)退出
1
请输入您的用户名(长度为6-9):
Zurich
请输入您的密码:
123456
恭喜您注册成功~!
[{'pwd': '123456', 'username': 'Zurich'}]
请选择要执行的操作:
 (1)注册 
 (2)登录
 (3)退出
1
请输入您的用户名(长度为6-9):
Alzacar
请输入您的密码:
1234567
恭喜您注册成功~!
[{'pwd': '123456', 'username': 'Zurich'}, {'pwd': '1234567', 'username': 'Alzacar'}]
请选择要执行的操作:
 (1)注册 
 (2)登录
 (3)退出
2
请输入您的用户名:
Zurich
请输入您的密码:
1123
密码输入错误!
请选择以下操作：
 (1)查看当前用户信息
 (2)查看用户列表
 (3)退出登录
1
pwd : 123456
username : Zurich
请选择以下操作：
 (1)查看当前用户信息
 (2)查看用户列表
 (3)退出登录
2
[{'pwd': '123456', 'username': 'Zurich'}, {'pwd': '1234567', 'username': 'Alzacar'}]
请选择以下操作：
 (1)查看当前用户信息
 (2)查看用户列表
 (3)退出登录
3
请选择要执行的操作:
 (1)注册 
 (2)登录
 (3)退出
3
您已退出登录~!
---
