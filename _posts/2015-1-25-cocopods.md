---
layout: post
title: CocoaPods安装和使用教程
date: 2015-1-23 13:23:23
categories:
- technology
tags:
- iOS

---


##CocoaPods是什么？

CocoaPods应该是iOS最常用最有名的类库管理工具了，上述两个烦人的问题，通过cocoaPods，只需要一行命令就可以完全解决，当然 前提是你必须正确设置它。重要的是，绝大部分有名的开源类库，都支持CocoaPods。所以，作为iOS程序员的我们，掌握CocoaPods的使用是 必不可少的基本技能了。

##如何下载和安装CocoaPods？

在安装CocoaPods之前，首先要在本地安装好Ruby环境。至于如何在Mac中安装好Ruby环境，请google一下，本文不再涉及。

假如你在本地已经安装好Ruby环境，那么下载和安装CocoaPods将十分简单，只需要一行命令。在Terminator（也就是终端）中输入以下命令（注意，本文所有命令都是在终端中输入并运行的.

```
sudo gem install cocoapods
```
但是，且慢。如果你在天朝，在终端中敲入这个命令之后，会发现半天没有任何反应。原因无他，因为那堵墙阻挡了cocoapods.org。

为了验证你的Ruby镜像是并且仅是taobao，可以用以下命令查看：

```
gem sources -l
```

只有在终端中出现下面文字才表明你上面的命令是成功的：

```
*** CURRENT SOURCES ***
http://ruby.taobao.org/
```

这时候，你再次在终端中运行：

```
sudo gem install cocoapods
```

等上十几秒钟，CocoaPods就可以在你本地下载并且安装好了，不再需要其他设置。

##如何使用CocoaPods？

好了，安装好CocoPods之后，接下来就是使用它。所幸，使用CocoPods和安装它一样简单，也是通过一两行命令就可以搞定。

###场景1：利用CocoaPods，在项目中导入AFNetworking类库

AFNetworking类库在GitHub地址是：[https://github.com/AFNetworking/AFNetworking](https://github.com/AFNetworking/AFNetworking)

为了确定AFNetworking是否支持CocoaPods，可以用CocoaPods的搜索功能验证一下。在终端中输入：

```
pod search AFNetworking
```
过几秒钟之后，你会在终端中看到关于AFNetworking类库的一些信息。比如：

![](http://ww3.sinaimg.cn/mw690/ae1f5766gw1eolwofn5whj21360n67c7.jpg)

这说明，AFNetworking是支持CocoaPods，所以我们可以利用CocoaPods将AFNetworking导入你的项目中。利用cd命令进入项目路径。

```
cd /Users/xiao/Desktop/Zodiac-constellation-search-iOS
```
利用cd命令进入项目路径后，初始化pod环境。

```
pod init
```

为了巩固上面搜索方法，我们再搜索一下`MMProgressHUD`这个第三方库。

```
pod search MMProgressHUD
```

然后把图中的**pod 'MMProgressHUD', '~> 0.3.0'**添加到项目主目录下的**Podfile**文件里，替换所有原始文件。

![](http://ww4.sinaimg.cn/mw690/ae1f5766gw1eolx9b20gsj216o0bg42v.jpg)

```
pod install
```

![](http://ww1.sinaimg.cn/mw690/ae1f5766gw1eolxs8fhuyj20ss0g2jvj.jpg)

当出现如上图片，那就恭喜你，完成了！

以后需要打开项目记得使用**蓝框勾选的文件**打开哦。
