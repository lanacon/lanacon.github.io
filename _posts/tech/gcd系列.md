---
layout: post
title: iOS GCD 技术系列1
category: 技术
tags: iOS
description: iOS GCD 技术系列1
---

多线程编程的缺点: 
	
	 * 数据不一致 数据竞争 (对数据进行加锁.)
	 * 停止等待事件的线程会导致多个线程相互持续等待(死锁,A线程需要B 线程中的资源,B)
	 * 使用太多线程会消耗大量内存.

 多线程的优点:

 	 * 可以保证应用程序的响应性.




1. 什么是GCD?


	
	苹果官方: 异步执行任务的的技术之一.
	
2. GCD 模板
	
    	
    		dispatch_async(queue,^{
    	
		/*
	
		* 长时间处理的任务,例如数据库访问
	
		*/
	
		/*
	
		* 长时间处理结束,主线程使用处理好的结果.
	
		*/
		dispatch_asyn(dispatch_get_main_queue(),^{
	
		/* 
	
		* 只有在主线程才能操作的处理
	
		\* 如更新UI.
	
		*/
		});
	
		});


	在导入GCD 之前,cocoa 框架提供了 NSObject 类的performSelectorInBackground:withObject  实例方法和performSelectorOnMainThread 实例方法等简单的多线程编程技术.如:

	
	
    	//
    	- (void)otherThreadTodoWork
    	{
    		[self preformSelectorInBackground:@selector:(doWork) 		withObject:nil];
    	}
    	
    	/*
    	* 后台线程处理方法
    	*/
    	- (void)doWork
    	{
    		/*
    		* 长时间处理,例如数据库操作.
    		*/
    		
    		/*
    		*处理完成以后,主线程使用处理的结果.
    		*/
    		
    		[self performSelectorOnMainThread:@selector(reloadView) withObject:nil waitUntilDone:NO];
    	}
    	
    	/*
    	* 主线程处理方法
    	*/
    	
    	- (void)reloadView
    	{
    		
    		//只在主线程中执行的处理.
    		
    	}
    	
   ...
   
3. GCD 的种类
	dispatch_queue
	
	*  Serial (单线程),conCurrent(并发执行,多线程);
	
	使用serial 这种类型,每个线程中可以加入多个任务,每个任务是排队执行的,按照加入的先后顺序,任务1 执行完 执行任务2...,在queue 中只有一个 线程 在执行
	
	* conCurrent 是并发执行,把任务加入到线程中,多可线程同时执行.苹果系统内部会根据自己系统当前的资源进行分配线程个数.
	
   
   
   
4. 实例.
