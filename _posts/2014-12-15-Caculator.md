---
layout: post
title: Storyboard制作简易计算器
date: 2014-12-15 12:23:23
categories:
- technology


---


* 简易计算器实现思路	     在控制器中声明四个属性和一个输出口	1. BOOL isUserInputNumber;判断当前是否是输入数字状态	2. double numberA;输入的第一个数	3. double numberB;输入的第二个数	4. NSString * operateType;操作类型(+、-、*、/)
	5. IBOutlet UILabel displayNumber;界面的文本标签*	按数字键完成的操作：	1. 	首先判断是不是连续按数字键（isUserInputNumber=YES）：	2.	如果是，则分别取得界面文本标签中的数字和数字按钮上的数字，然后组合起来赋值给界面文本标签。	3.	如果不是，则界面文本标签显示数字按钮上的数字。
	4. isUserInputNumber标识置成YES。*	按加减乘除键完成的操作：	* 首先保存输入的第一个数，
	* 然后保存操作类型(+、-、*、/)。
	* 最后isUserInputNumber标识置成NO。*	按等号键完成的操作：	* 声明保存结果的变量result。	* 如果按等号前按了数字键（isUserInputNumber=YES），保存输入的第二个数。
	* 然后条件分支判断操作类型(+、-、*、/)，条件分支中处理相同，如下：根据操作类型加减乘除进行计算，保存结果。
	* 最后isUserInputNumber标识置成NO。*	按clear键完成的操作：	*	界面中文本标签中的数字置成0。	*	isUserInputNumber标识置成NO。