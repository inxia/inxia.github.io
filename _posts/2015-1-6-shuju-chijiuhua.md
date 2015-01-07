---
layout: post
title: 数据持久化与对象归档
date: 2015-1-6 12:23:23
categories:
-  technology 


---

###获取文件路径步骤
1、使用NSSearchPathForDirectoriesInDomain()函数来查找各种目录

2、从返回的目录数组中获取需要的目录

3、使用NSString的实例方法stringByAppendingPathComponent:组成文件的完整路径

####参考代码：
//获取文件路径

```
- (NSString *)getFilePath
{
    //首先获取应用程序的Documents目录
    NSArray *pathArray = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
    NSString *docPath = [pathArray objectAtIndex:0];
    //然后获取文件完整路径
    NSString *filePath = [docPath stringByAppendingPathComponent:FILENAME];
    NSLog(@"filePath:%@",filePath);
    return filePath;
}
```

NSFileManager&NSFileHandle读取文件步骤：

//1、获取文件管理器（NSFileManager的类方法defaultManager：）

//2、检查文件是否存在（NSFileManager的实例方法fileExistsAtPath:）

//2.1、如果文件存在，打开文件（NSFileHandle的fileHandleForReadingAtPath:类方法，参数是文件路径，需要调用方法获取，返回NSFileHandle的实例）

//2.2、读取有效数据并返回（首先使用NSFileHandle的实例方法availableData:读取到NSData类型的缓冲区中，然后使用NSString的实例方法initWithData: encoding:将缓冲区数据转换成NSString类型）

####参考代码：
//读取文件

```
- (NSString *)readFile
{
    //获取文件管理器
    NSFileManager *fManager = [NSFileManager defaultManager];
    //检查文件是否存在
    if([fManager fileExistsAtPath:[self getFilePath]]){
        //如果文件存在，打开文件
        NSFileHandle *fHandle = [NSFileHandle fileHandleForReadingAtPath:[self filePath]];
        //读取有效数据并返回
        NSData *data = [fHandle availableData];
        NSString *msg = [[NSString alloc] initWithData:data  encoding:NSUTF8StringEncoding];
        return [msg autorelease];
    }
    return @"";
}
```

###保存文件步骤

//1、获取文件管理器（NSFileManager的类方法defaultManager：）

//2、检查文件是否存在（NSFileManager的实例方法fileExistsAtPath: ，参数是文件路径，需要调用方法获取）

//2.1、不存在此文件的话，创建该文件（NSFileManager的createFileAtPath: contents: attributes:实例方法）

//3、写入文件内容，创建一个NSFileHandle，相当于打开（NSFileHandle的fileHandleForWritingAtPath:类方法）

//4、写入内容（获取内容字符串，调用内容字符串的dataUsingEncoding:实例方法转换成NSData类型，调用NSFileHandle的实例方法writeData:写入文件）

//5、写入完成之后关闭文件（调用NSFileHandle的实例方法closeFile:）

###参考代码：

```
- (IBAction)btnSavePressed:(UIButton *)sender
{
    //首先判断该文件是否存在
    NSFileManager *fManager = [NSFileManager defaultManager];
    NSString *filePath = [self getFilePath]; 
    if([fManager fileExistsAtPath:filePath] == NO){
        //不存在此文件的话，创建该文件
        [fManager createFileAtPath:filePath contents:nil attributes:nil];
    }
    //写入文件内容，创建一个NSFileHandle，相当于打开
    NSFileHandle *fHandle = [NSFileHandle fileHandleForWritingAtPath:filePath];
    //写入内容
    NSString *msg = self.txtMsg.text;
    NSData *data = [msg dataUsingEncoding:NSUTF8StringEncoding];
    //[fHandle seekToEndOfFile]; //将文件指针移动到结尾，能实现追加效果
    [fHandle writeData:data];
    //fHandle
    //写入完成之后关闭文件
    [fHandle closeFile];
    
}

```

##对象归档
对象归档实现步骤：

*	要对一个类对象进行归档，需要实现NSCoding协议，协议包含两个方法

*	-(void)encodeWithCoder:(NSCoder *) encoder;

*	-(id)initWithCoder:(NSCoder *) decoder;

####参考代码：

```
@implementation Student
-(void)encodeWithCoder:(NSCoder *)aCoder
{
    [aCoder encodeObject:_name forKey:@"name"];
    [aCoder encodeInt:_age forKey:@"age"];
    [aCoder encodeObject:_gender forKey:@"gender"];
    [aCoder encodeObject:_address forKey:@"address"];
    
}
-(id)initWithCoder:(NSCoder *)aDecoder
{
    if(self = [super init])
    {
        _name = [aDecoder decodeObjectForKey:@"name"];
        _age = [aDecoder decodeIntForKey:@"age"];
        _gender = [aDecoder decodeObjectForKey:@"gender"];
        _address = [aDecoder decodeObjectForKey:@"address"];
    }
    return self;
}
-(id)copyWithZone:(NSZone *)zone
{
    Student * copy = [[[self class] allocWithZone:zone]init];
    if(copy)
    {
        copy.name = [self.name copyWithZone:zone];
        copy.age = self.age;
        copy.gender = [self.gender copyWithZone:zone];
        copy.address = [self.address copyWithZone:zone];
    }
    return copy;
}
@end

```

####//进行归档步骤：

//首先创建要归档的类的对象，并给属性赋值

//实例化一个NSMutableData对象，用来将对象归档其中

//符合NSCoding协议的对象创建归档时，需要使用一个NSKeyedArchiver实例来做这件事情，使用其initForWritingWithMutableData:实例方法创建NSKeyedArchiver实例

//调用NSKeyedArchiver的实例方法encodeObject: forKey:进行归档

//调用NSKeyedArchiver的实例方法finishEncoding结束归档

//调用NSMutableData对象的实例方法writeToFile: atomically: 将归档后对象写入文件

//将对象归档到文件中之后，可以从文件中取消归档重新组成对象
 同样需要借助于一个NSKeyedUnarchiver的实例来完成

####//取消归档步骤：

//首先使用NSFileManager的实例方法fileExistsAtPath:判断文件是否存在

//如果存在，调用NSData的实例方法initWithContentsOfFile: 实例化一个NSData对象，用来将对象解档

// 使用NSKeyedUnarchiver的实例方法initForReadingWithData:创建NSKeyedUnarchiver对象

//使用NSKeyedUnarchiver对象的decodeObjectForKey:实例方法解档出相应对象
