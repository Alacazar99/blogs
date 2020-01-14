### 一、GitHub账号（必备）

关于这个，自行创建注册吧...
【快捷连接】：https://github.com/

### 二、对Git工具有一定的了解

关于git 的安装和简单上手，可以看看我这一篇的博客：[Git 基础操作指南（基础、全面）](https://www.jianshu.com/p/72db80019dcf)

---
### 三、创建远程GitHub上存放的地址

在这里，我就举一个实际的例子好了...
前段时间我们java老师让写了一个`【java实现的学生信息管理系统】`，我就把这个小项目放在GitHub上面吧...

【第一步】：在GitHub首页，点击**New repository**新建一个项目存放的仓库，也就是项目地址。如图：
![创建](https://upload-images.jianshu.io/upload_images/17476267-1deb84780f9bd8a5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

【第二步】：创建仓库，下面试一些详细描述，可忽略哦...
- `Repository name`: 仓库名称
- `Description(可选)`: 仓库描述介绍
- `Public, Private` : 仓库权限（公开共享，私有或指定合作者）
- `Initialize this repository with a README`: 添加一个README.md
- `gitignore`: 不需要进行版本管理的仓库类型，对应生成文件.gitignore
- `license`: 证书类型，对应生成文件LICENSE

![创建](https://upload-images.jianshu.io/upload_images/17476267-e41e87329a8bed14.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**然后呢，把你创建的这个仓库地址给保存好了**！很重要，很重要，很重要！...就是这个地址：`git@github.com:Alacazar99/Java_stu.git`


![HTTPS（仓库地址）](https://upload-images.jianshu.io/upload_images/17476267-2fa5eba74ddf12da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 四、调用Git代码，上传项目
【第一步】：首先找到你的项目文件夹，如果不是太大的话，可以拷贝在桌面上，然后右击项目文件夹，选择`Git Bush Here`。

从下图可以看出，我写的学生信息管理系统命名为：`java项目`

然后输入命令：
```
$ git clone git@github.com:Alacazar99/Java_stu.git
```
**【解释一下】：
$ git  clone  后面添加你前面创建的那个仓库地址...**

这条命令：将github上面的仓库克隆到本地的项目文件`java项目`文件中，生成文件夹`Java_stu`
![目录](https://upload-images.jianshu.io/upload_images/17476267-4550c6d68c6c00d7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

【下一步】：`cd Java_stu`: 
下一步，我们将对git可以管理的仓库`Java_stu`进行操作...
![Iamge.png](https://upload-images.jianshu.io/upload_images/17476267-6ebb650d4316c14e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
【有个小细节】：将我们想上传的项目文件完整的放入仓库Java_stu中，然后依次执行：
- `git add .  ` ：把Java_stu文件夹下面的文件都添加到暂存区        (**注：别忘记后面的**`.`)；

- `git commit  -m  "提交信息"  ：`（**注：“提交信息”里面换成你需要，如“first commit”**）
- `git push -u origin master`

![Image.png](https://upload-images.jianshu.io/upload_images/17476267-bef273b6a3a30536.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

操作到这里就结束了，可以看看我上传的小东西，Hahaha...

https://github.com/Alacazar99/Java_stu.git
