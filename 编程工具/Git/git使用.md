# 1.Git概述

Git是一个免费的、开源的分布式版本控制系统，可以快速高效地处理从小型到大型的各种项目。Git易于学习，占地面积小，性能极快。它具有廉价的本地库，方便的暂存区域和多个工作流分支等特性。其性能优于Subversion、CVS、Perforce和ClearCase等版本控制工具



## **1.1** 何为版本控制

版本控制是一种记录文件内容变化，以便将来查阅特定版本修订情况的系统。

版本控制其实最重要的是可以记录文件修改历史记录，从而让用户能够查看历史版本，

方便版本切换。



## **1.2** 为什么需要版本控制

个人开发过渡到团队协作。



➢ 分布式版本控制工具

Git、Mercurial、Bazaar、Darcs……

像 Git 这种分布式版本控制工具，客户端提取的不是最新版本的文件快照，而是把代码

仓库完整地镜像下来（本地库）。这样任何一处协同工作用的文件发生故障，事后都可以用

其他客户端的本地仓库进行恢复。因为每个客户端的每一次文件提取操作，实际上都是一次

对整个文件仓库的完整备份。

分布式的版本控制系统出现之后,解决了集中式版本控制系统的缺陷:



## 1.3 Git工作机制

![1626157608040](C:\Users\86185\AppData\Roaming\Typora\typora-user-images\1626157608040.png)

工作区、暂存区的文件还可以撤销操作，本地库不行



# 2.**Git** 安装

官网地址： https://git-scm.com/

查看 GNU 协议，可以直接点击下一步。

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps10.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps11.png)  



 

选择 Git 安装位置，要求是非中文并且没有空格的目录，然后下一步。

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps12.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps13.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps14.png)  

Git 选项配置，推荐默认设置，然后下一步。

Git 安装目录名，不用修改，直接点击下一步。

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps15.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps16.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps17.png)  

Git 的默认编辑器，建议使用默认的 Vim 编辑器，然后点击下一步。

默认分支名设置，选择让 Git 决定，分支名默认为 master，下一步。

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps18.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps19.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps20.png)  

修改 Git 的环境变量，选第一个，不修改环境变量，只在 Git Bash 里使用 Git。

选择后台客户端连接协议，选默认值 OpenSSL，然后下一步。

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps21.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps22.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps23.png)  

配置 Git 文件的行末换行符，Windows 使用 CRLF，Linux 使用 LF，选择第一个自动

转换，然后继续下一步。

选择 Git 终端类型，选择默认的 Git Bash 终端，然后继续下一步。

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps24.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps25.png)  

选择 Git pull 合并的模式，选择默认，然后下一步。

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps26.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps27.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps28.png)  

选择 Git 的凭据管理器，选择默认的跨平台的凭据管理器，然后下一步。

其他配置，选择默认设置，然后下一步。

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps29.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps30.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps31.png)  

实验室功能，技术还不成熟，有已知的 bug，不要勾选，然后点击右下角的 Install

按钮，开始安装 Git。

点击 Finsh 按钮，Git 安装成功！

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps32.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps33.png)  



右键任意位置，在右键菜单里选择 Git Bash Here 即可打开 Git Bash 命令行终端。

在 Git Bash 终端里输入 git --version 查看 git 版本，如图所示，说明 Git 安装成功。

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps34.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps35.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps36.png)  



# 3.Git常用命令

| 命令名称                             | 作用                 |
| ------------------------------------ | -------------------- |
| git config --global user.name 用户名 | 设置用户签名         |
| git config --global user.email 邮箱  | 设置用户签名         |
| git init                             | 初始化本地库         |
| git status                           | 查看本地库状态       |
| git add 文件名（rm：删除）           | 添加到暂存区         |
| git commit -m "日志信息" 文件名      | 提交到本地库         |
| git reflog（ log）                   | 查看历史记录（详细） |
| git reset --hard 版本号              | 版本穿梭             |



# 4.**Git** 分支操作

## **4.1** 什么是分支

在版本控制过程中，同时推进多个任务，为每个任务，我们就可以创建每个任务的单独

分支。使用分支意味着程序员可以把自己的工作从开发主线上分离开来，开发自己分支的时

候，不会影响主线分支的运行。对于初学者而言，分支可以简单理解为副本，一个分支就是

一个单独的副本。（分支底层其实也是指针的引用）



![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps66.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps67.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps68.png)  

## **4.2** 分支的好处

同时并行推进多个功能开发，提高开发效率。

各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。失败

的分支删除重新开始即可。

## **4.3** 分支的操作

### **4.3.1** 查看分支

**git** **branch** **-v**



### **4.3.2** 创建分支

**git** **branch** 分支名



### **4.3.3** 修改分支

--在 maste分支上做修改

$ **vim** **hello.txt**

--添加暂存区

$ **git** **add** **hello.txt**

--提交本地库

$ **git** **commit** **-m** **"my** **forth** **commit"** **hello.txt**

--查看分支

$ **git** **branch** **-v**



### **4.3.4** 切换分支

**git** **checkout** 分支名



### **4.3.5** 合并分支

**git** **merge** 分支名

案例实操 在 **master** 分支上合并 **hot-fix** 分支

Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 **(master**)

$ **git** **merge** **hot-fix**

(hot-fix 的内容更新到master)



### **4.3.6** 产生冲突

冲突产生的表现：后面状态为 MERGING

Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (master|MERGING)

冲突产生的原因：

合并分支时，**两个分支在同一个文件的同一个位置有两套完全不同的修改**。Git 无法替

我们决定使用哪一个。必须人为决定新代码内容。



### **4.3.7** 解决冲突

1）编辑有冲突的文件，删除特殊符号，决定要使用的内容

特殊符号：<<<<<<< HEAD 当前分支的代码 ======= 合并过来的代码 >>>>>>> hot-fix

2）添加到暂存区

Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (master|MERGING)

$ **git** **add** **hello.txt**

3）执行提交（注意：此时使用 git commit 命令时不能带文件名）

Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (master|MERGING)

$ **git** **commit** **-m** **"merge** **hot-fix"**

[master 69ff88d] merge hot-fix

--发现后面 MERGING消失，变为正常



# 5.[**GitHub**](https://github.com/)[ ](https://github.com/)操作

GitHub 网址[：](https://github.com/)https://github.com/！

## **5.1** 创建远程仓库

## **5.2** 远程仓库操作

- git remote -v：查看当前所有远程地址别名
- git remote add 别名 远程地址：起别名
- git push 别名（远程仓库） 分支：推送本地分支上的内容到远程仓库
- git clone 远程地址：将远程仓库的内容克隆到本地
- git pull 远程库地址别名 远程分支名：将远程仓库对于分支最新内容拉下来后与当前本地分支直接合并



# 6.团队协作





# 7.**IDEA** 集成 **Git**

## **7.1** 配置 **Git** 忽略文件

**1**）**Eclipse** 特定文件

**2**）**IDEA** 特定文件

**3**）**Maven** 工程的 **target** 目录

问题 **1:**为什么要忽略他们？

答：与项目的实际功能无关，不参与服务器上部署运行。把它们忽略掉能够屏蔽 IDE 工具之间的差异。

问题 **2**：怎么忽略？

1）创建忽略规则文件 xxxx.ignore（前缀名随便起，建议是 git.ignore）

这个文件的存放位置原则上在哪里都可以，为了便于让~/.gitconfig 文件引用，建议也放在用户家目录下

## **7.2** 定位 **Git** 程序

## **7.3** 初始化本地库

选择要创建 Git 本地仓库的工程。



![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps161.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps162.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps163.png)  

## **7.4** 添加到暂存区

右键点击项目选择 Git -> Add 将项目添加到暂存区。

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps164.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps165.png)  

## **7.5** 提交到本地库

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps166.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps167.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps168.png)  

## **7.6** 切换版本

在 IDEA 的左下角，点击 Version Control，然后点击 Log 查看版本右键选择要切换的版本，然后在菜单里点击 Checkout Revision。

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps169.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps170.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps171.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps172.png)  

## **7.7** 创建分支

选择 Git，在 Repository 里面，点击 Branches 按钮。在弹出的 Git Branches 框里，点击 New Branch 按钮。

填写分支名称，创建 hot-fix 分支。然后再 IDEA 的右下角看到 hot-fix，说明分支创建成功，并且当前已经切换成 hot-fix 分支



## **7.8** 切换分支

在 IDEA 窗口的右下角，切换到 master 分支。

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps173.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps174.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps175.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps176.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps177.png)  



然后在 IDEA 窗口的右下角看到了 master，说明 master 分支切换成功。

## **7.9** 合并分支

在 IDEA 窗口的右下角，将 hot-fix 分支合并到当前 master 分支。如果代码没有冲突，分支直接合并成功，分支合并成功以后，代码自动提交，无需手动提交本地库。

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps178.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps179.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps180.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps181.png)  

**7.10** 解决冲突

如图所示，如果 master 分支和 hot-fix 分支都修改了代码，在合并分支的时候就会发生冲突。

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps182.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps183.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps184.png)  

我们现在站在 master 分支上合并 hot-fix 分支，就会发生代码冲突。点击 Conflicts 框里的 Merge 按钮，进行手动合并代码。

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps185.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps186.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps187.png)  

手动合并完代码以后，点击右下角的 Apply 按钮。代码冲突解决，自动提交本地库。

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps188.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps189.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps190.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps191.png)  

# 8.**IDEA** 集成 **GitHub**

## **8.1** 设置 **GitHub** 账号

如果出现 401 等情况连接不上的，是因为网络原因，可以使用以下方式连接：

然后去 GitHub 账户上设置 token。

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps192.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps193.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps194.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps195.png)  

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps196.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps197.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps198.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps199.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps200.png)  

点击生成 token。复制红框中的字符串到 idea 中。点击登录。

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps201.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps202.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps203.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps204.png)  



## 8.2 分享工程到 **GitHub**

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps205.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps206.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps207.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps208.png)  

来到 GitHub 中发现已经帮我们创建好了 gitTest 的远程仓库。



## **8.3** **push** 推送本地库到远程库

右键点击项目，可以将当前分支的内容 push 到 GitHub 的远程仓库中。

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps209.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps210.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps211.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps212.png)  

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps213.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps214.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps215.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps216.png)  



## 注意

push 是将本地库代码推送到远程库，如果本地库代码跟远程库代码版本不一致，push 的操作是会被拒绝的。也就是说，要想 push 成功，一定要保证本地库的版本要比远程库的版本高！因此一个成熟的程序员在动手改本地代码之前，一定会先检查下远程库跟本地代码的区别！如果本地的代码版本已经落后，**切记要先 pull 拉取一下远程库的代码**，将本地代码更新到最新以后，然后再修改，提交，推送！



## **8.4** **pull** 拉取远程库到本地库

右键点击项目，可以将远程仓库的内容 pull 到本地仓库。

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps217.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps218.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps219.png)  

注意：pull 是拉取远端仓库代码到本地，如果远程库代码和本地库代码不一致，会自动合并，如果自动合并失败，还会涉及到手动解决冲突的问题。

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps220.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps221.png)  



## **8.5** **clone** 克隆远程库到本地

为 clone 下来的项目创建一个工程，然后点击 Next。

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps222.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps223.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps224.png)  

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps225.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps226.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps227.png)  



# 第 **9** 章 国内代码托管中心**-**码云

**9.1** 简介

众所周知，GitHub 服务器在国外，使用 GitHub 作为项目托管网站，如果网速不好的话，

严重影响使用体验，甚至会出现登录不上的情况。针对这个情况，大家也可以使用国内的项

目托管网站-码云。

66

更多 Java –大数据 –前端 –python 人工智能资料下载，可访问百度：尚硅谷官网

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps228.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps229.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps230.png)  



 

尚硅谷技术课程系列之 GIT

————————[———————](https://link.jianshu.com/?t=http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001373962845513aefd77a99f4145f0a2c7a7ca057e7570000)——————————————

码云是开源中国推出的基于 [Git](https://link.jianshu.com/?t=http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001373962845513aefd77a99f4145f0a2c7a7ca057e7570000)[ ](https://link.jianshu.com/?t=http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001373962845513aefd77a99f4145f0a2c7a7ca057e7570000)[的代码](https://link.jianshu.com/?t=http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001373962845513aefd77a99f4145f0a2c7a7ca057e7570000)托管服务中心，网址是 https://gitee.com/ ，使用

方式跟 GitHub 一样，而且它还是一个中文网站，如果你英文不是很好它是最好的选择。

**9.2** [码云帐号注册和登录](https://gitee.com/)

进入码云官网地[址：](https://gitee.com/)<https://gitee.com/>[，](https://gitee.com/)点击注册 Gitee

输入个人信息，进行注册即可。

帐号注册成功以后，直接登录。

67

更多 Java –大数据 –前端 –python 人工智能资料下载，可访问百度：尚硅谷官网

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps231.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps232.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps233.png)  



 

尚硅谷技术课程系列之 GIT

—————————————————————————————

登录以后，就可以看到码云官网首页了。

**9.3** 码云创建远程库

点击首页右上角的加号，选择下面的新建仓库

68

更多 Java –大数据 –前端 –python 人工智能资料下载，可访问百度：尚硅谷官网

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps234.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps235.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps236.png)  



 

尚硅谷技术课程系列之 GIT

—————————————————————————————

填写仓库名称，路径和选择是否开源（共开库或私有库）

最后根据需求选择分支模型，然后点击创建按钮。

69

更多 Java –大数据 –前端 –python 人工智能资料下载，可访问百度：尚硅谷官网

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps237.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps238.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps239.png)  



 

尚硅谷技术课程系列之 GIT

—————————————————————————————

远程库创建好以后，就可以看到 HTTPS 和 SSH 的链接。

**9.4** **IDEA** 集成码云

**9.4.1** **IDEA** 安装码云插件

Idea 默认不带码云插件，我们第一步要安装 Gitee 插件。

如图所示，在 Idea 插件商店搜索 Gitee，然后点击右侧的 Install 按钮。

70

更多 Java –大数据 –前端 –python 人工智能资料下载，可访问百度：尚硅谷官网

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps240.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps241.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps242.png)  



 

尚硅谷技术课程系列之 GIT

—————————————————————————————

Idea 链接码云和链接 GitHub 几乎一样，安装成功后，重启 Idea。

Idea 重启以后在 Version Control 设置里面看到 Gitee，说明码云插件安装成功。

71

更多 Java –大数据 –前端 –python 人工智能资料下载，可访问百度：尚硅谷官网

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps243.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps244.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps245.png)  



 

尚硅谷技术课程系列之 GIT

—————————————————————————————

然后在码云插件里面添加码云帐号，我们就可以用 Idea 连接码云了。

72

更多 Java –大数据 –前端 –python 人工智能资料下载，可访问百度：尚硅谷官网

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps246.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps247.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps248.png)  



 

尚硅谷技术课程系列之 GIT

—————————————————————————————

**9.4.2** **IDEA** 连接码云

Idea 连接码云和连接 GitHub 几乎一样，首先在 Idea 里面创建一个工程，初始化 git 工

程，然后将代码添加到暂存区，提交到本地库，这些步骤上面已经讲过，此处不再赘述。

➢ 将本地代码 **push** 到码云远程库

自定义远程库链接。

给远程库链接定义个 name，然后再 URL 里面填入码云远程库的 HTTPS 链接即可。码

73

更多 Java –大数据 –前端 –python 人工智能资料下载，可访问百度：尚硅谷官网

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps249.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps250.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps251.png)  



 

尚硅谷技术课程系列之 GIT

—————————————————————————————

云服务器在国内，用 HTTPS 链接即可，没必要用 SSH 免密链接。

然后选择定义好的远程链接，点击 Push 即可。

看到提示就说明 Push 远程库成功。

74

更多 Java –大数据 –前端 –python 人工智能资料下载，可访问百度：尚硅谷官网

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps252.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps253.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps254.png)  



 

尚硅谷技术课程系列之 GIT

—————————————————————————————

去码云远程库查看代码。

只要码云远程库链接定义好以后，对码云远程库进行 pull 和 clone 的操作和 Github 一

致，此处不再赘述。

**9.5** 码云复制 **GitHub** 项目

码云提供了直接复制 GitHub 项目的功能，方便我们做项目的迁移和下载。

具体操作如下：

75

更多 Java –大数据 –前端 –python 人工智能资料下载，可访问百度：尚硅谷官网

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps255.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps256.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps257.png)  



 

尚硅谷技术课程系列之 GIT

—————————————————————————————

将 GitHub 的远程库 HTTPS 链接复制过来，点击创建按钮即可。

如果 GitHub 项目更新了以后，在码云项目端可以手动重新同步，进行更新！

76

更多 Java –大数据 –前端 –python 人工智能资料下载，可访问百度：尚硅谷官网

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps258.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps259.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps260.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps261.png)  



 

尚硅谷技术课程系列之 GIT

—————————————————————————————

77

更多 Java –大数据 –前端 –python 人工智能资料下载，可访问百度：尚硅谷官网

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps262.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps263.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps264.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps265.png)  



 

尚硅谷技术课程系列之 GIT

—————————————————————————————

# 第 **10** 章 自建代码托管平台**-GitLab**

**10.1** **GitLab** 简介

[G](https://baike.baidu.com/item/wiki)itLab 是由 GitLabInc.[开发，](https://baike.baidu.com/item/Git)使用 [MIT](https://baike.baidu.com/item/MIT%E8%AE%B8%E5%8F%AF%E8%AF%81)[ ](https://baike.baidu.com/item/MIT%E8%AE%B8%E5%8F%AF%E8%AF%81)[许可证的](https://baike.baidu.com/item/MIT%E8%AE%B8%E5%8F%AF%E8%AF%81)[基于网络的](https://baike.baidu.com/item/%E7%BD%91%E7%BB%9C)[ ](https://baike.baidu.com/item/%E7%BD%91%E7%BB%9C)[Git](https://baike.baidu.com/item/Git)[ ](https://baike.baidu.com/item/Git)[仓库](https://baike.baidu.com/item/Git)[管](https://baike.baidu.com/item/%E4%BB%93%E5%BA%93)理工具，且具有

[wiki](https://baike.baidu.com/item/wiki)[ ](https://baike.baidu.com/item/wiki)[和](https://baike.baidu.com/item/wiki)[ ](https://baike.baidu.com/item/wiki)issue 跟踪功能。使用 [Git](https://baike.baidu.com/item/Git)[ ](https://baike.baidu.com/item/Git)[作](https://baike.baidu.com/item/Git)为代码管理工具，并在此基础上搭建起来[的](https://baike.baidu.com/item/Ruby%E8%AF%AD%E8%A8%80)[ ](https://baike.baidu.com/item/Ruby%E8%AF%AD%E8%A8%80)[web](https://baike.baidu.com/item/Ruby%E8%AF%AD%E8%A8%80)[ ](https://baike.baidu.com/item/Ruby%E8%AF%AD%E8%A8%80)[服务。](https://baike.baidu.com/item/Ruby%E8%AF%AD%E8%A8%80)

GitLab 由乌克兰[程序员](https://baike.baidu.com/item/Go%E8%AF%AD%E8%A8%80)[ ](https://baike.baidu.com/item/Go%E8%AF%AD%E8%A8%80)[Dm](https://baike.baidu.com/item/Go%E8%AF%AD%E8%A8%80)itriyZaporozhets 和 ValerySizov 开发，它使用 [Ruby](https://baike.baidu.com/item/Ruby%E8%AF%AD%E8%A8%80)[ ](https://baike.baidu.com/item/Ruby%E8%AF%AD%E8%A8%80)[语言写](https://baike.baidu.com/item/Ruby%E8%AF%AD%E8%A8%80)

成。后来，一些部分用 [Go](https://baike.baidu.com/item/Go%E8%AF%AD%E8%A8%80)[ ](https://baike.baidu.com/item/Go%E8%AF%AD%E8%A8%80)[语言重](https://baike.baidu.com/item/Go%E8%AF%AD%E8%A8%80)写。截止 2018 年 5 月，该公司约有 290 名团队成员，以

及 2000 多名开源贡献者。GitLab 被 IBM，Sony，JülichResearchCenter，NASA，Alibaba，

Invincea，O’ReillyMedia，Leibniz-Rechenzentrum(LRZ)，CERN，SpaceX 等组织使用。

**10.2** **GitLab** 官网地址

官网地址[：](https://about.gitlab.com/installation/)[https://about.gitlab.com/](https://about.gitlab.com/installation/)

安装说[明：](https://about.gitlab.com/installation/)<https://about.gitlab.com/installation/>

**10.3** **GitLab** 安装

**10.3.1** 服务器准备

准备一个系统为 CentOS7 以上版本的服务器，要求内存 4G，磁盘 50G。

关闭防火墙，并且配置好主机名和 IP，保证服务器可以上网。

此教程使用虚拟机：主机名：gitlab-server IP 地址：192.168.6.200

**10.3.2** 安装包准备

Yum 在线安装 gitlab- ce 时，需要下载几百 M 的安装文件，非常耗时，所以最好提前把

所需 RPM 包下载到本地，然后使用离线 rpm 的方式安装。

下载地址：

https://packages.gitlab.com/gitlab/gitlab-

ce/packages/el/7/gitlab-ce-13.10.2-ce.0.el7.x86_64.rpm

注：资料里提供了此 rpm 包，直接将此包上传到服务器/opt/module 目录下即可。

**10.3.3** 编写安装脚本

安装 gitlab 步骤比较繁琐，因此我们可以参考官网编写 gitlab 的安装脚本。

[root@gitlab-server module]# vim gitlab-install.sh

sudo rpm -ivh /opt/module/gitlab-ce-13.10.2-ce.0.el7.x86_64.rpm

sudo yum install -y curl policycoreutils-python openssh-server cronie

78

更多 Java –大数据 –前端 –python 人工智能资料下载，可访问百度：尚硅谷官网

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps266.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps267.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps268.png)  



 

尚硅谷技术课程系列之 GIT

—————————————————————————————

sudo lokkit -s http -s ssh

sudo yum install -y postfix

sudo service postfix start

sudo chkconfig postfix on

curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-

ce/script.rpm.sh | sudo bash

sudo EXTERNAL_URL="http://gitlab.example.com" yum -y install gitlab-

ce

给脚本增加执行权限

[root@gitlab-server module]# chmod +x gitlab-install.sh

[root@gitlab-server module]# ll

总用量 403104

-rw-r--r--. 1 root root 412774002 4月 7 15:47 gitlab-ce-13.10.2-

ce.0.el7.x86_64.rpm

-rwxr-xr-x. 1 root root

416 4月 7 15:49 gitlab-install.sh

然后执行该脚本，开始安装 gitlab-ce。注意一定要保证服务器可以上网。

[root@gitlab-server module]# ./gitlab-install.sh

警告：/opt/module/gitlab-ce-13.10.2-ce.0.el7.x86_64.rpm: 头 V4

RSA/SHA1 Signature, 密钥 ID f27eab47: NOKEY

准备中...

\#################################

[100%]

正在升级/安装...

1:gitlab-ce-13.10.2-ce.0.el7

\################################# [100%]

。 。 。 。 。 。

**10.3.4** 初始化 **GitLab** 服务

执行以下命令初始化 GitLab 服务，过程大概需要几分钟，耐心等待…

[root@gitlab-server module]# gitlab-ctl reconfigure

。 。 。 。 。 。

Running handlers:

Running handlers complete

Chef Client finished, 425/608 resources updated in 03 minutes 08

seconds

gitlab Reconfigured!

**10.3.5** 启动 **GitLab** 服务

执行以下命令启动 GitLab 服务，如需停止，执行 gitlab-ctl stop

[root@gitlab-server module]# gitlab-ctl start

ok: run: alertmanager: (pid 6812) 134s

ok: run: gitaly: (pid 6740) 135s

ok: run: gitlab-monitor: (pid 6765) 135s

79

更多 Java –大数据 –前端 –python 人工智能资料下载，可访问百度：尚硅谷官网

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps269.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps270.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps271.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps272.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps273.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps274.png)  



 

尚硅谷技术课程系列之 GIT

—————————————————————————————

ok: run: gitlab-workhorse: (pid 6722) 136s

ok: run: logrotate: (pid 5994) 197s

ok: run: nginx: (pid 5930) 203s

ok: run: node-exporter: (pid 6234) 185s

ok: run: postgres-exporter: (pid 6834) 133s

ok: run: postgresql: (pid 5456) 257s

ok: run: prometheus: (pid 6777) 134s

ok: run: redis: (pid 5327) 263s

ok: run: redis-exporter: (pid 6391) 173s

ok: run: sidekiq: (pid 5797) 215s

ok: run: unicorn: (pid 5728) 221s

**10.3.6** 使用浏览器访问 **GitLab**

使用主机名或者 IP 地址即可访问 GitLab 服务。需要提前配一下 windows 的 hosts 文件。

80

更多 Java –大数据 –前端 –python 人工智能资料下载，可访问百度：尚硅谷官网

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps275.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps276.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps277.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps278.png)  



 

尚硅谷技术课程系列之 GIT

—————————————————————————————

首次登陆之前，需要修改下 GitLab 提供的 root 账户的密码，要求 8 位以上，包含大小

写子母和特殊符号。因此我们修改密码为 Atguigu.123456

然后使用修改后的密码登录 GitLab。

GitLab 登录成功。

81

更多 Java –大数据 –前端 –python 人工智能资料下载，可访问百度：尚硅谷官网

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps279.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps280.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps281.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps282.png)  



 

尚硅谷技术课程系列之 GIT

—————————————————————————————

**10.3.7** **GitLab** 创建远程库

**10.3.8** **IDEA** 集成 **GitLab**

➢

1）安装 GitLab 插件

82

更多 Java –大数据 –前端 –python 人工智能资料下载，可访问百度：尚硅谷官网

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps283.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps284.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps285.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps286.png)  



 

尚硅谷技术课程系列之 GIT

—————————————————————————————

➢

2）设置 GitLab 插件

83

更多 Java –大数据 –前端 –python 人工智能资料下载，可访问百度：尚硅谷官网

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps287.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps288.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps289.png)  



 

尚硅谷技术课程系列之 GIT

—————————————————————————————

➢

3）push 本地代码到 GitLab 远程库

84

更多 Java –大数据 –前端 –python 人工智能资料下载，可访问百度：尚硅谷官网

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps290.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps291.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps292.png)  



 

尚硅谷技术课程系列之 GIT

—————————————————————————————

自定义远程连接

85

更多 Java –大数据 –前端 –python 人工智能资料下载，可访问百度：尚硅谷官网

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps293.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps294.png)  



 

尚硅谷技术课程系列之 GIT

—————————————————————————————

注意：gitlab 网页[上复制过来的连接是](http://gitlab-server/root/git-test.git)[：](http://gitlab.example.com/root/git-test.git)[http://](http://gitlab.example.com/root/git-test.git)[gitlab.example.com](http://gitlab.example.com/root/git-test.git)[/root/git-test.git](http://gitlab.example.com/root/git-test.git)[，](http://gitlab.example.com/root/git-test.git)

需要手动修改为[：](http://gitlab-server/root/git-test.git)[http:/](http://gitlab-server/root/git-test.git)[/gitlab-server](http://gitlab-server/root/git-test.git)[/root/git-test.git](http://gitlab-server/root/git-test.git)

选择 gitlab 远程连接，进行 push。

86

更多 Java –大数据 –前端 –python 人工智能资料下载，可访问百度：尚硅谷官网

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps295.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps296.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps297.png)  



 

尚硅谷技术课程系列之 GIT

—————————————————————————————

首次向连接 gitlab，需要登录帐号和密码，用 root 帐号和我们修改的密码登录即可。

代码 Push 成功。

87

更多 Java –大数据 –前端 –python 人工智能资料下载，可访问百度：尚硅谷官网

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps298.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps299.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps300.png)  



 

尚硅谷技术课程系列之 GIT

—————————————————————————————

只要 GitLab 的远程库连接定义好以后，对 GitLab 远程库进行 pull 和 clone 的操作和

Github 和码云一致，此处不再赘述。

88

更多 Java –大数据 –前端 –python 人工智能资料下载，可访问百度：尚硅谷官网

![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps301.png)![img](file:///C:\Users\86185\AppData\Local\Temp\ksohtml12000\wps302.png)  

