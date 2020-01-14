### 一、Git 安装
在 Windows 上安装 Git 也有几种安装方法。 官方版本可以在 Git 官方网站下载。
**官网**： [http://git-scm.com/download/win](http://git-scm.com/download/win)

**安装完成之后呢，右击鼠标会有以下两个选项**：
![image](https://upload-images.jianshu.io/upload_images/17476267-27e14ae45f1b251d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**GUI为用户界面模式，Bash为命令行模式。**

GitHub for Windows安装指南 ：
该安装程序包含图形化和命令行版本的 Git。 它也能支持 Powershell，提供了稳定的凭证缓存和健全的换行设置。 
网址为 [http://windows.github.com](http://windows.github.com/)。

其他安装操作还有不少，如需要请自行百度吧。

---

###二、Git 工作原理
#### 2.1 Git是什么？

Git是一种分布式版本控制系统。

【通俗理解吧】：
举一个栗子，boss让你写一个**策划案**，你先完成了一稿，之后又有了一些新的想法，但是并不确定新的想法是否能得到boss的认可，于是你保存了一个**初稿**，之后在初稿的基础上另存了一个文件，做了部分修改完成了一个**修改稿**。OK，这时你的策划案就有了两个版本——**初稿和修改稿**。如果boss对**修改稿**不满意，你可以很轻易的把**初稿**拿出来交差。

**【为什么用Git ？】**
- 我们可以为**每一次变更提交版本更新并且备注更新的内容；**
- 我们可以在**项目的各个历史版本之间自如切换；**
- 我们可以**一目了然的比较出两个版本之间的差异；**
- 我们可以**从当前的修改中撤销一些操作；**
- 我们可以自如的**创建分支、合并分支；**
- 我们可以和**多人协作开发；**
- 我们可以采取**自由多样的开发模式。**

【总之】：它很好的**解决了文件变更过程存储这一个问题。**

#### 2.2 工作流程图
![image - 图片来源网络](https://upload-images.jianshu.io/upload_images/17476267-c36703148cdcb21b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---
## 三、Git 基础用法

由于git是分布式管理工具，需要输入**用户名和邮箱**以作为标识，因此，在命令行输入下列的命令：
![Image](https://upload-images.jianshu.io/upload_images/17476267-c0d952250c277f12.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**PS：**注意git config  --**global**参数，有了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然你也可以对某个仓库指定的不同的用户名和邮箱，可以根据个人情况设置。

---
#### 3.1 创建版本库
【解释】：版本库就是我们所说的“仓库”，英文名repository，你可以理解为一个目录，这个目录里面的所有文件都可以被Git管理，文件的修改，删除Git都能跟踪，**以便任何时刻都可以追踪历史，或者在将来某个时刻还可以将文件”还原”。**
```
命令解析：

cd：进入某个目录

mkdir：创建一个文件

pwd：显示当前的目录路径
```
这几条命令就不做演示了...

---
### 3.2 添加文件到版本库


`git init`: 将这个目录变为git可以管理的仓库

在/f/gitBase1/test1文件路径下有一个999.txt文件，现在要将`test1`变为Git可以管理的仓库。
运行Git init后，文件中多了些东西（.git文件）：

![2.png](https://upload-images.jianshu.io/upload_images/17476267-c466caed5e33f133.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

使用下列的命令，将创建的文件添加到暂存区，然后提交到仓库：
![3.png](https://upload-images.jianshu.io/upload_images/17476267-e76e2b880d007890.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
命令解析：

git add：将文件提交到暂存区

git commit -m：将暂存区文件提交到仓库（单引号内为注释）
```

---
#### 3.3 检查是否有未提交的文件

git status：检查当前文件状态；

通过上述命令，检查该版本库是否有文件未提交：
![Image](https://upload-images.jianshu.io/upload_images/17476267-98c4d940ffdb3c8e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---
#### 3.4 检查文件是否被修改

命令解析：

`git diff：`**查看文件修改的内容**

首先，对原999.txt文件做一点修改。
通过上述命令，检查该版本库是否有文件未提交：
![Image](https://upload-images.jianshu.io/upload_images/17476267-38feed1a81f2968d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---
#### 3.5查看历史变更记录
```
命令解析：

git log：获得历史修改记录

git log --pretty=oneline：
使记录只显示主要的内容，一行显示
```
再一次修改`999.txt文件`中的内容，然后保存提交：
![Image](https://upload-images.jianshu.io/upload_images/17476267-69b69cea06df1107.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

通过上述命令`git log`，查看历史修改记录：
![Image](https://upload-images.jianshu.io/upload_images/17476267-137ea392b6627e2d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
【备注】：当然，若觉得这样看起来比较费事，可以使用命令`git log --pretty=oneline：`，获得精简版本的日志记录。

---
#### 3. 6 版本回退

【命令解析】：
- `cat`：查看文件当前内容;

- `git reset --hard HEAD^`：回退到上一个版本;

- `git reflog：`获取历史版本号;

- `git reset --hard 版本号：`回退到该版本号对应的版本;

PS：如果要回退到上上个版本，可以使用git reset --hard HEAD^^命令，但是这样稍显麻烦，如果回退到100个版本之前，只需要执行这个命令即可：git reset --hard HEAD~100；
![Image](https://upload-images.jianshu.io/upload_images/17476267-842f028778559472.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---
#### 3.7 将本地项目推送到github仓库

关于这个，首先要将Git 关联到GitHub上，可参考我的这篇简书：
[【Git 运用】本地上传项目至GitHub远程仓库](https://www.jianshu.com/p/44290e1e0456)


### 命令汇总


![1.png](https://upload-images.jianshu.io/upload_images/17476267-bb1e01c362d4edf1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![2.png](https://upload-images.jianshu.io/upload_images/17476267-4530780d9550a87e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
