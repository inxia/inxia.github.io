---
layout: post
title: 使用GDataXML解析XML文档
date: 2015-1-9 13:23:23
categories:
- technology
tags:
- iOS

---

在IOS平台上进行XML文档的解析有很多种方法，在SDK里面有自带的解析方法，但是大多情况下都倾向于用第三方的库，原因是解析效率更高、使用上更方便，关于IOS平台各种解析XML库的优缺点分析，可以看下这篇文章：http://www.raywenderlich.com/553/how-to-chose-the-best-xml-parser-for-your-iphone-project

这里主要介绍一下由Google提供的一种在IOS平台上进行XML解析的开源库GDataXML.

1.	到<http://code.google.com/p/gdata-objectivec-client/source/browse/trunk/Source/XMLSupport/>下载源码，下载下来后进入文件夹找到XMLSupport文件夹，将里面的GDataXMLNode.h和GDataXMLNode.m文件拖拽到项目中新建的文件夹即可（我这里是建的GDataXML文件夹），注意要选中复制文件到项目中而不是只是引用，如图：![](http://my.csdn.net/uploads/201208/15/1345000844_9371.png)

2.	接下来再进入Build Settings，在搜索框中搜索```Header Search Path```然后双击并点击+按钮添加/usr/include/libxml2,如图：![](http://my.csdn.net/uploads/201208/15/1345000883_6686.png)

3.	接下来再搜索框中搜索```Other linker flags```，同样的方式添加-lxml2，如图:	![](http://my.csdn.net/uploads/201208/15/1345000911_2930.png)


到这里，添加和配置的工作就完成了（是有点麻烦），接下来就看如何使用了：

首先在工程中新建一个xml文件，作为我们要解析的对象，新建方法是在工程中新建一个Empty的文件，命名为users.xml，然后添加内容：


	[html] view plaincopy
	<?xml version="1.0" encoding="utf-8"?>  
	<Users>  
    <User id="001"]]>  
        <name>Ryan</name>  
        <age>24</age>  
    </User>  
    <User id="002"]]>  
        <name>Tang</name>  
        <age>23</age>  
    </User>  
	</Users>  


接下来就可以开始解析了，在需要解析的文件中引入头文件：#import "GDataXMLNode.h"

我是新建的一个Empty工程，所以直接在AppDelegate.m中使用，代码如下：


	[cpp] view plaincopy
	- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions  
	{  
    self.window = [[[UIWindowalloc] initWithFrame:[[UIScreen mainScreen] bounds]] autorelease];  
    // Override point for customization after application launch.  
    self.window.backgroundColor = [UIColorwhiteColor];  
    [self.windowmakeKeyAndVisible];  
      
	//获取工程目录的xml文件  
    NSString *filePath = [[NSBundle mainBundle] pathForResource:@"users" ofType:@"xml"];  
    NSData *xmlData = [[NSData alloc] initWithContentsOfFile:filePath];  
      
	   //使用NSData对象初始化  
    GDataXMLDocument *doc = [[GDataXMLDocument alloc] initWithData:xmlData  options:0error:nil];  
      
      //获取根节点（Users）  
    GDataXMLElement *rootElement = [doc rootElement];  
      
    //获取根节点下的节点（User）  
    NSArray *users = [rootElement elementsForName:@"User"];  
      
    for (GDataXMLElement *user in users) {  
        //User节点的id属性  
        NSString *userId = [[user attributeForName:@"id"] stringValue];  
        NSLog(@"User id is:%@",userId);  
      获取name节点的值  
        GDataXMLElement *nameElement = [[user elementsForName:@"name"] objectAtIndex:0];  
        NSString *name = [nameElement stringValue];  
        NSLog(@"User name is:%@",name);          
      获取age节点的值  
        GDataXMLElement *ageElement = [[user elementsForName:@"age"] objectAtIndex:0];  
	NSString *age = [ageElement stringValue];  
	NSLog(@"User age is:%@",age);  
	NSLog(@"-------------------");  
    }             
    returnYES;  
	}  


编译执行在控制台输出结果如下：![](http://my.csdn.net/uploads/201208/15/1345000995_6872.png)



##GDataXml  相同标签的多个属性

GDataXml  相同标签的多个属性，好多文档都没有介绍获取属性的方法，让我找的好苦啊，必须分享！！！
首先向Frameworks文件中添加libxml2.dylib这个库，而后再Croups & Files 侧边栏中双击我们的工程图标，找到 build 修改两个属性如下：
在Search Paths中 找到Header Search Paths  将其对应的值修改为：/usr/includebxml2
在Linking中找到 Other Linker Flags 对应的值改为：-lxml2

	GDataXMLDocument *doc=[[GDataXMLDocument alloc]initWithXMLString:resp*****eBody opti*****:2 error:nil];
    if (doc!=nil) {
	GDataXMLElement *root=[doc rootElement ];
	NSLog(@"--------root's children:--------\n%@", root);

//取出根节点的所有孩子节点
//取出某一个具体节点(body节点)


	[returnInfo setObject:[[[root elementsForName:@"db:uid"] objectAtIndex:0] stringValue] forKey:@"snsUserUid"];

	[returnInfo setObject:[[[root elementsForName:@"title"]objectAtIndex:0]stringValue] forKey:@"snsNickName"];  

	[returnInfo setObject:[[[root elementsForName:@"db:location"]objectAtIndex:0]stringValue] forKey:@"snsProvince"]; 

	[returnInfo setObject:[[[[root elementsForName:@"link"] objectAtIndex:2]attributeForName:@"href"] stringValue] forKey:@"snsProfileImageUrl"];

	[returnInfo setObject:@"4" forKey:@"snsLandEntrance"];
	NSLog(@"%@",[[[root elementsForName:@"link"] objectAtIndex:2]attributes]);

	NSLog(@"%@",[[[root elementsForName:@"db:location"]objectAtIndex:0]stringValue]);

	NSLog(@"returnInforeturnInforeturnInforeturnInforeturnInfo%@",returnInfo);



附上xml源文件：

	<?xml version="1.0" encoding="UTF-8"?><entry xmlns="http://www.w3.org/2005/Atom" xmlns:db="http://www.douban.com/xmlns/" xmlns:gd="http://schemas.google.com/g/2005" xmlns:openSearch="http://a9.com/-/spec/opensearchrss/1.0/" xmlns:opensearch="http://a9.com/-/spec/opensearchrss/1.0/"><id>http://api.douban.com/people/63522291</id><title>wangjianlewo</title><link href="http://api.douban.com/people/63522291" rel="self"/><link href="http://www.douban.com/people/63522291/" rel="alternate"/><link href="http://img3.douban.com/icon/user_normal.jpg" rel="icon"/><content></content><db:attribute name="n_mails">0</db:attribute><db:attribute name="n_notificati*****">0</db:attribute><db:location id="beijing">北京</db:location><db:signature></db:signature><db:uid>63522291</db:uid><uri>http://api.douban.com/people/63522291</uri></entry><?xmlversion="1.0" encoding="UTF-8"?><entry xmlns="http://www.w3.org/2005/Atom" xmlns:db="http://www.douban.com/xmlns/" xmlns:gd="http://schemas.google.com/g/2005" xmlns:openSearch="http://a9.com/-/spec/opensearchrss/1.0/" xmlns:opensearch="http://a9.com/-/spec/opensearchrss/1.0/"><id>http://api.douban.com/people/63522291</id><title>wangjianlewo</title><link href="http://api.douban.com/people/63522291" rel="self"/><link href="http://www.douban.com/people/63522291/" rel="alternate"/><link href="http://img3.douban.com/icon/user_normal.jpg" rel="icon"/><content></content><db:attribute name="n_mails">0</db:attribute><db:attribute name="n_notificati*****">0</db:attribute><db:location id="beijing">北京</db:location><db:signature></db:signature><db:uid>63522291</db:uid><uri>http://api.douban.com/people/63522291</uri></entry>
