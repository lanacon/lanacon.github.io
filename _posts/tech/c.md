---
layout: post
title: C 语言基础
category: 技术
tags: C语言
description: C语言
---

## C语言学习

mac 下编译C 语言方法:

打开: 终端,进入到文件所在的文件夹,然后编译, ***cc - c hello.c*** 会在当前文件中生成.o 的文件,
该文件为编译生成的中间文件,
然后 可以输入 ***cc hello.o***  进行链接, 链接成功后生成a.out  文件.
文件中目录如下:
![文件目录](http://ww4.sinaimg.cn/large/7f5ba233gw1exnkwppevkj20bo07uglt.jpg)


如果想执行a.out  则可以  ***./a.out***


如果是OC 的.m 文件,和C 语言一样.

* touch OC_First.m

  ![内容](http://ww2.sinaimg.cn/large/7f5ba233gw1exnl8vlbfqj20gw094gme.jpg)

* 在这一步如果头文件没有引入会报错:

	![报错文件](http://ww3.sinaimg.cn/large/7f5ba233gw1exnlabua2qj210m066ad9.jpg)

	这说明 NSLog 方法没有找到 ,根据提示,需要包含 提示的头文件.    	<Foundation/NSObjCRuntime.h>
	根据提示,把头文件包含后,在编译:
	报错:
	
	![](http://ww2.sinaimg.cn/large/7f5ba233gw1exnlf2n4uvj20zw066gp0.jpg)

	解决办法 :  把  Foundataion 的整个框架  引进来.
	
	cc -OC_Fist.o -framework Foundation

* 运行  生成  的  ./a.out 文件,success.





最后总结 流程:


* 编写 .c  或者  XXX.m  文件,
* 编译  cc -c XXX.m  生成 XXX.o文件 
* 链接 cc XXX.o  生成 a.out.
* 运行 ./a.out



如果现在 有两个文件, 一个 是 OC_First.m  一个是 OC_Second.m 如果第一个 文件中调用了第二个文件里面的方法,
那么需要第一个文件里面引用第二个文件的头文件.
新建一个 OC_Second.h 文件.
第一个文件中包含OC_Second.h 文件.

然后,分别编译 两个文件  分别生成  OC_First.o  和  OC_Second.o  

然后链接这两个文件   : cc OC_First.o OC_Second.o -framework Foundation

两个文件中得代码:

![文件内容](http://ww4.sinaimg.cn/large/7f5ba233gw1exnm6vo3qpj212c0d0q6m.jpg)

运行的结果 :

![结果](http://ww3.sinaimg.cn/large/7f5ba233gw1exnm7zddbej20qo01y0u2.jpg)



链接两个文件的方法:

* 编写  a.m , b.m , b.h 三个文件
* a.m  中引入  b.h 头文件
* 分别链接 两个.m 文件 cc -c a.m ,cc -c b.m 生成  a.o , b.o

* 	cc OC_First.o OC_Second.o -framework Foundation

* 最后执行生成的a.out 文件.

