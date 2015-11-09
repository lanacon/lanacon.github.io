---
layout: post
title: CoreData 技术
category: 技术
tag: 技术
description: CoreData 技术分享.
---

CoreData 实际上是对 sqlite 的封装.

例子说明:现在有一个选择课程的功能,课程有方向.

比如说: 前端 是一个方向,后台是一个方向.前端的课程有css,js,ios,android.后台的方向有java,c#,php.

表格如下:

| 前台         | 后台          |
| ------------| ------------- |
| css         | java          |
| javascript  | php           |
| iOS         | c#            |

现在我们对前台/后台进行增删改查.

	(1)向前台的课程中添加一个 android  ,想后台的里面添加一个 python.

	(2)查询出前台的所有课程,和后台的所有课程.
	
	(3) 修改 后台课程中的c#  为 C#
	
	(4) 删除前台中的 css 课程.
	
	

分析上面的例子 ,发现 ,前台可以有多个课程,但是一个课程只能属于一个方向(不考虑特殊情况.),那么他们是一对多的关系.


1. 创建空工程.
2. 创建一个单例,CoreData 的增删改查.

3. 