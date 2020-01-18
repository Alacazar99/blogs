#### 简述配置Java开发运行环境的步骤

- （1） 下载Java安装包（JDK）网址：[https://www.oracle.com/index.html](https://www.oracle.com/index.html)

- （2） 傻瓜式安装，其中注意尽量不要安装在C盘，记住安装的路径（后续步骤中将配置路径）。

- （3） 环境配置，（右击"我的电脑"——"属性"——"高级"——"环境变量"）。

- （4） 配置三个环境变量：`JAVA_HOME , CLASSPATH , PATH` ，具体流程见下图。


![Image](https://upload-images.jianshu.io/upload_images/17476267-52615711c6816c1a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---
#### java文件打包
假设有两个.java文件（main.java和Box.java）如下所示：
![Box.java](https://upload-images.jianshu.io/upload_images/17476267-bbcfa9254c5149da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Main.java](https://upload-images.jianshu.io/upload_images/17476267-f549458b5fdb98da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

文件目录结构描述 : 新建两个文件夹，文件包名main和shape，其中main中包含主类Main.class；shape 中包含类Box.class。

【重点】现在将shape包压缩至“abc.jar”中，打包过程如下：

![错误示例](https://upload-images.jianshu.io/upload_images/17476267-34cc6ec959f454dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![正确.示例](https://upload-images.jianshu.io/upload_images/17476267-fca3c845746a925a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打包过程**cmd**窗口命令如下： 
- 2.1 【将foo文件打包】：`jar cvf abc,jar –C foo/ . `

- 2.2 【配置环境变量】：`set classpath=（文件夹的绝对路径，例如）C：User\...\abc.jar;`

(【注意事项】：末尾要加`;`  下图为没加`;`导致的错误示例) 

运行命令：
```
java main.Main
```

---
#### 将main主类压缩打包

###### 打 jar 包的过程

打包过程cmd窗口命令如下： 
- 1.【将foo_main文件打包】：`Jar –cvfn egf.jar create_main_jar.mf –C foo_main/ . `

- 2.【创建一个清单文件`create_main_jar.mf`】：内部命令如下图所示。(备注，要多留一个空行) 

-  3.【运行】：
```
Java –jar egf.jar 
```
文件内容大致如下：

![mf 文件](https://upload-images.jianshu.io/upload_images/17476267-9d0b5c2ad81ef18c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![运行结果](https://upload-images.jianshu.io/upload_images/17476267-e683619fb07c50d1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



