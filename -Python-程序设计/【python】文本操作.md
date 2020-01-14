##### 1. 字符编码简介
- 1.1. ASCII        `一个字符串占：8 bits   2**8=256`
ASCII(American Standard Code for Information Interchange)，是一种单字节的编码。单字节可以表示256个不同的字符，可以表示所有的英文字符和许多的控制 符号。
- 1.2. Unicode ` 2**16 `
所有语言的字符都用同一种字符集来表示，这就是Unicode。
- 1.3. UTF-8
是一种很别扭的编码，具体表现在他是变长的（变长编码），并且兼容ASCII，ASCII字符使用1字节表示。
- 1.4. 国标码   GBK
-------------------------------------
推荐一篇博客：[此篇博客中关于文本操作超详细（点击查看）](https://www.cnblogs.com/xinchrome/p/5011304.html)
##### 2.    文本操作主要包括：
- 2.1.文件的读写操作
- 2.2.文件的各种系统操作
-  2.3.存储对象

##### 3.基于字符`read & write`
-` ‘w'`是指文件将被写入数据，语句的其它部分很好理解
- 正如你所见，在Python的面向对象机制下，这确实非常简单。需要注意的是，当你再次使用`“w”`方式在文件中写数据，所有原来的内容都会被删除。如果想保留原来的内容，可以使用`“a”`方式在文件中结尾附加数据.

##### 4.二进制方式读写

>文本文件 = 二进制文件
> 文本文件本质上存储时，也是二进制。可以用文本编辑器查看
 而二进制文件无法通过文本编辑器查看

- 4.1.在Windows和Macintosh环境下，有时可能需要以二进制方式读写文件，比如图片和可执行文件。`此时，只要在打开文件的方式参数中增加一个“b”即可`
- 4.2.然而，python没有二进制类型，但可以存储二进制类型的数据，就是用string字符串类型来存储二进制数据
- 4.3.python本身并没有对二进制进行支持,但是提供了一个模块来弥补，就是`struct模块。`
----------------------------------------
###### 5. 各种系统操作

 python中对文件、文件夹（文件操作函数）的操作需要涉及到`os模块`和`shutil模块`。

*各种系统操作列表*：

- 得到当前工作目录，即当前Python脚本工作的目录路径: os.getcwd()

- 返回指定目录下的所有文件和目录名:os.listdir()

- 函数用来删除一个文件:os.remove()

- 删除多个目录：os.removedirs（r“c：\python”）

- 检验给出的路径是否是一个文件：os.path.isfile()

- 检验给出的路径是否是一个目录：os.path.isdir()

- 判断是否是绝对路径：os.path.isabs()

- 检查是否快捷方式os.path.islink ( filename )

- 检验给出的路径是否真地存:os.path.exists()

- 返回一个路径的目录名和文件名:os.path.split() eg os.path.split('/home/swaroop/byte/code/poem.txt') 
- 分离扩展名：os.path.splitext()

- 获取路径名：os.path.dirname()

- 获取文件名：os.path.basename()

- 运行shell命令: os.system()

- 读取和设置环境变量:os.getenv() 与os.putenv()
给出当前平台使用的行终止符:os.linesep Windows使用'\r\n'，Linux使用'\n'而Mac使用'\r'

指示你正在使用的平台：os.name 对于Windows，它是'nt'，而对于Linux/Unix用户，它是'posix'

重命名：os.rename（old， new）

创建多级目录：os.makedirs（r“c：\python\test”）

创建单个目录：os.mkdir（“test”）

获取文件属性：os.stat（file）

修改文件权限与时间戳：os.chmod（file）

终止当前进程：os.exit（）

获取文件大小：os.path.getsize（filename


-------------------------------------
关于open 模式：

`w`： 以写方式打开，
`a` ：以追加模式打开 (从 EOF 开始, 必要时创建新文件)
`r+`：以读写模式打开
`w+`： 以读写模式打开 (参见 w )
`a+`：以读写模式打开 (参见 a )
`rb `：以二进制读模式打开
`wb`： 以二进制写模式打开 (参见 w )
`ab `：以二进制追加模式打开 (参见 a )
`rb+`： 以二进制读写模式打开 (参见 r+ )
`wb+`： 以二进制读写模式打开 (参见 w+ )
`ab+ `：以二进制读写模式打开 (参见 a+ )
![图片来源：菜鸟教程](https://upload-images.jianshu.io/upload_images/17476267-e1b40ee84a4f4317.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

-----------------------------------
目录操作：
`os.mkdir("file") `创建目录
复制文件：
`shutil.copyfile("oldfile","newfile") `oldfile和newfile都只能是文件
`shutil.copy("oldfile","newfile") `oldfile只能是文件夹，newfile可以是文件，也可以是目标目录
复制文件夹：
`shutil.copytree("olddir","newdir") `olddir和newdir都只能是目录，且newdir必须不存在
重命名文件（目录）
`os.rename("oldname","newname") `文件或目录都是使用这条命令
移动文件（目录）
`shutil.move("oldpos","newpos") `
删除文件
`os.remove("file")`
删除目录
`os.rmdir("dir")`只能删除空目录
`shutil.rmtree("dir") `空目录、有内容的目录都可以删
转换目录
`os.chdir("path") `换路径








