---
layout:	post
title:	"Build mips openssl"
date:	2016-04-12 14:15:01 +0800
description: ""
categories:	
tags:	[cross-platform,mips,openssl]
---

这2天折腾编译路由器一个应用程序，程序代码依赖了OpenSSL。尝试编译它，折腾好久，最终找到一个靠谱的方法。

下载源码后，使用

	#./Configure linux-mips32 --cross-compile-prefix=mipsel-buildroot-linux-uclibc- --prefix=/opt/buildroot-gcc463/usr/mipsel-buildroot-linux-uclibc/sysroot/usr/

然后

	#make
	#make install


编译起来还是蛮方便的

推荐一个博客,里面有编译一些开源库的方法[mips交叉编译开源库](http://blog.csdn.net/zjg555543/article/details/7520936)
