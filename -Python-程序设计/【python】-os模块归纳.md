##### import os

- os.path.isdir(name): 判断name是不是目录，不是目录就返回false

- os.path.isfile(name): 判断name这个文件是否存在，不存在返回false

- os.path.exists(name): 判断是否存在文件或目录name

- os.path.getsize(name): 或得文件大小，如果name是目录返回0L

- os.path.abspath(name): 获得绝对路径

- os.path.isabs(): 判断是否为绝对路径

- os.path.normpath(path): 规范path字符串形式

- os.path.split(name): 分割文件名与目录（事实上，如果你完全使用目录，它也会将最后一个目录作为文件名而分离，同时它不会判断文件或目录是否存在）

- os.path.splitext(): 分离文件名和扩展名

- os.path.join(path,name): 连接目录与文件名或目录

- os.path.basename(path): 返回文件名

 - os.path.dirname(path): 返回文件路径
- os.sep:取代操作系统特定的路径分隔符

- os.name: 指示你正在使用的工作平台。比如对于Windows，它是'nt'，而对于Linux/Unix用户，它是'posix'。

- os.getcwd: 得到当前工作目录，即当前python脚本工作的目录路径。

- os.getenv()和os.putenv: 分别用来读取和设置环境变量

- os.listdir(): 返回指定目录下的所有文件和目录名

- os.remove(file) :删除一个文件

- os.chmod(file): 修改文件权限和时间戳

- os.mkdir(name): 创建目录

- os.rmdir(name): 删除目录

- os.system():运行shell命令

- os.exit():终止当前进程

- os.linesep:给出当前平台的行终止符。例如，`Windows使用'\r\n'，`Linux使用'\n' ;而Mac使用'\r'

- os.path.split(): 返回一个路径的目录名和文件名

- os.path.isfile()和os.path.isdir() 分别检验给出的路径是一个目录还是文件

- os.path.existe(): 检验给出的路径是否真的存在

- os.listdir(dirname): 列出dirname下的目录和文件

- os.getcwd(): 获得当前工作目录

- os.curdir: 返回当前目录（'.'）

- os.chdir(dirname): 改变工作目录到dirname

