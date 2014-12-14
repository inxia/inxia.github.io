---
layout: post
title: "Git零基础—个人学习过程"
categories:
- technology
tags:
- git


---


##学习前
1	**git语言就是DOS中运行的命令**，Mac自带终端，Windows需要下载msysgit,下载地址：<http://code.google.com/p/msysgit/downloads/list>

2	首先，我们需要明白git的工作区域，分为`工作区`、`暂存区`、`本地仓库`和`远程仓库`

3	git的工作原理`工作区`add→`暂存区`commit→`本地仓库`push→`远程仓库`

4	暂存区和本地仓库都是二进制的方式存在于本地电脑中的
##git网站操作	
1	注册账号
[sign up](https://github.com/join?return_to=%2Fjoin)
 有账号直接登录 [login in](https://github.com/login)

2	**New repository**创建一个远程库
![](http://ww3.sinaimg.cn/mw690/ae1f5766jw1ekb7wdau1qj20lt0f075w.jpg)
RT 1 所示 库的名称可以任意，只要不与你其他库名重复就可以，如果勾选创建文件，那么你的库里就自带一个README.md 文件

##本地操作——终端中操作(以Mac为例)
###1	配置ssh获取key

*	**cd ~/.ssh**检测本地有没有key,如果成功进入，就把.ssh文件夹删除

*	 **cd ~/.ssh**   **rm -rf **进入.ssh文件夹删除所有文件
*	**ssh-keygen -t rsa -C "i.yx@foxmail.com"**邮箱地址改为你自己的登录邮箱
*	连续四下回车，就会生成你的key 如图2所示
*	![](http://ww1.sinaimg.cn/mw690/ae1f5766jw1ekb7xbiz55j20g40emgmy.jpg)
*	在Finder中前往**~/.ssh**文件夹，用记事本等程序打开**id_rsa.pub**文件，复制里面所有内容
*	进入github网站首页，点击右上角的**Settings**进入设置
*	左侧选项栏里点击**SSH keys**
*	选择**Add SSH key**添加一个SSH key
*	Title随便填，就是一个标记的意思
*	把刚才复制的key**粘贴到Key里**，并Add key 保存
###2	工作区

*	**cd Desktop**进入桌面，此处是你创建工作区的位置
*	**mkdir example**创建一个工作区文件夹
*	**cd example**进入工作区
*	**touch README.txt**在工作区里创建一个"README.txt"文档
*	**echo Hello World! > 1.txt**在文档里输入HEllo World!
*	**git init**初始化

###3	缓存区

*	**git add README.txt**//添加该文件到缓存区

*	Linux机制，没有返回消息就是好消息

###4	本地仓库

*	**git commit -m"first commit"**将缓存区文件提交到本地仓库(first commit 是注释，可以随意更改)
*	此操作会返回一些数据,可以不用理会

###5	连接远程仓库

*	**git remote add origin https://github.com/用户名/库名.git**
*	例如我的：git remote add origin https://github.com/ixiao/Notepad.git

###6	发送远程仓库
*	**git push -u origin master**发送到远程仓库的master(默认)目录下

##附录Git常用命令
{

*	git status查看状态，会告诉你当前可以如何操作
*	git add .添加所有文件到缓存区，注意点前有空格
*	git clone git@github.com:用户名/库名.git克隆某人的某库库到当前位置
*	git clone <address：复制代码库到本地；
*	git add <file ...：添加文件到代码库中；
*	git rm <file ...：删除代码库的文件；
*	git add -A 批量操作
*	git commit -m <message：提交更改，在修改了文件以后，使用这个命令提交修改。
*	git pull：从远程同步代码库到本地。
*	git branch：查看当前分支。带*是当前分支。
*	git branch <branch-name：新建一个分支。
*	git branch -d <branch-name：删除一个分支。
*	git checkout <branch-name：切换到指定分支。
*	git log：查看提交记录（即历史的 commit 记录）。
*	git status：当前修改的状态，是否修改了还没提交，或者那些文件未使用。
*	git reset <log：恢复到历史版本。

}


##本人遇到的问题，解决方法总结
```
fatal: unable to access 'https://github.com/ixiao/Notepad.git/: The requested URL returned error: 403`
此时我们更换http协议为shh协议连接

$ git remote rm origin 删除之前的添加信息 (配置文件在 ~/testproject/.git/config)
 
$ git remote add origin git@github.com:用户名/仓库名.git 使用ssh协议连接，添加远程仓库 (此条命令由github提供) 

$ git push -u origin master 再尝试推送到Github test仓库主枝，一般默认为 maste

```
##[Git Pages安装和jekyll安装方法](http://beiyuu.com/github-pages/)