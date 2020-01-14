### 什么是异常？
- 异常即是一个事件，该事件会在程序执行过程中发生，影响了程序的正常执行。
- 一般情况下，在Python无法正常处理程序时就会发生一个异常。
-  异常是Python对象，表示一个错误。
-  当Python脚本发生异常时我们需要捕获处理它，否则程序会终止执行。
# `部分异常归纳：`
![python标准异常](https://upload-images.jianshu.io/upload_images/17476267-a09d70815593f1d0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
-----------
### 异常处理
捕捉异常可以使用try/except语句。 
try的工作原理
> 当开始一个`try`语句后，python就在当前程序的上下文中作标记，这样当异常出现时就可以回到这里，try子句先执行，接下来会发生什么依赖于执行时是否出现异常。 
如果当`try`后的语句执行时发生异常，python就跳回到try并执行第一个匹配该异常的except子句，异常处理完毕，控制流就通过整个try语句（除非在处理异常时又引发新的异常）。 
如果在try后的语句里发生了异常，却没有匹配的except子句，异常将被递交到上层的try，或者到程序的最上层（这样将结束程序，并打印缺省的出错信息）。 
如果在try子句执行时没有发生异常，python将执行else语句后的语句（如果有else的话），然后控制流通过整个try语句。
```
opt = None
try:
    opt = int(input("请输入一个整数：\n"))
except:
    print("您输入的不是整数@-@")
print(opt)
```
执行：
```
请输入一个整数：
g
您输入的不是整数@-@
None
```
------------------------------
##### 关于异常类型
- 语句块
```
try:
    能发生的异常代码
except:
    异常处理代码
except Type：
    处理指定类型异常代码
except Type as data：
    获取异常信息
except （Type1 , Type2 , Type3）：
    同时处理三种异常，如果想获取异常数据，可使用 as 语句
```

```
try:
    pass
except 异常类型(1):
    pass
except 异常类型(2):
    pass
except (异常类型(3) , 异常类型(4) , 异常类型(5)):
    pass
except Exception as e :
    print(e)
```
>-  定义这种方式捕捉异常时，一定要先写具体的异常，再写具有普遍性的异常；
>-  所有的异常，继承自Exception
```
try:
except:
    发生异常执行
else:
    没有发生异常时,执行
finally:
    有没有异常都会执行
可以只有try...finally 语句
```
--------------------------------------
举个例子：
```
try:
    num = int(input("请输入一个整数：\n"))
    retult = 8 / num
    print(result)
except ValueError:
    print("请您输入整数@~@")
except ZeroDivisionError:
    print("除数不能为Zero")
except Exception as e:
    print("未知错误%s" % e)
else:
    print("程序正常")
finally:
    print("执行完毕~")
```
- 异常的传递： 异常发生后，会传递给方法（函数）的调用者A
如果A也有捕捉到该异常，则按捕捉机制处理。如果A没有捕捉到异常，则会层层向上传递。
最终会传递到python解释器，此处就简单的终止程序。
```
请输入一个整数：
9
未知错误name 'result' is not defined
finally:有没有异常都要执行
```
-------------------------
##### raise
当程序出现错误，python会自动引发异常，也可以通过raise显示地引发异常。一旦执行了raise语句，raise后面的语句将不能执行。
> raise语法格式如下：
raise [Exception [, args [, traceback]]]
#####自定义异常
用户自定义异常

- 通过创建一个新的异常类，程序可以命名它们自己的异常。异常应该是典型的继承自Exception类，通过直接或间接的方式。
- 在try语句块中，用户自定义的异常后执行except块语句，变量 e 是用于创建Networkerror类的实例。
```
class paramIncalidException(Exception):
    def __init__(self,code,msg):
        self.code = code
        self.msg = msg

def login(username,pwd):
    if len(username) < 6:
        raise paramIncalidException(2019,"账号长度在1-6位之间")
    if username != "Zurich":
        raise paramIncalidException(2020,"账号错误")
    print('登录成功~')
#
try:
    login('Zuricf','12345')
    login('Zurick', '12345')
    print('后续操作')
except paramIncalidException as o:
    # print(o.code,o.msg)
    print(o)
    #raise o # 改不了就抛出。
```
输出：
(2019, '账号长度在1-6位之间')
##### 程序发生异常怎么处理？
判断：是否可控
- 不可控的异常（内存溢出等）：忽略，无法解决；
- 可控的异常：处理它
不管是否可控，都要记录下来
```
try:
    pass
except Exception:
    #记录异常
    #处理
    #不能处理，raise 抛出
```
