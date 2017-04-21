---
layout:	post
title:	"porting cvm to android"
date:	2017-04-13 19:52:12 +0800
description: ""
categories:	porting
tags:	['j2me','jamvm','cvm','phoneme']
---
## 开始，是一件痛苦的事情 
最近有个项目，在Android下面有用到J2ME，由于我就找到一些开源的虚拟机，将这些开源的JavaVM移植到Android下面。
 
这是一个比较痛苦的过程，J2ME是一个过时很久的技术（我个人以为的）。

## J2ME 运行

在开源社区找了一段时间，最终选择JAMVM这款虚拟机来移植，也许是因为之前有过他来支撑在linux embed下的OSGI框架，交叉编译进行的还算顺序。改了几个小地方。就直接可以放到Android/Shell下面去运行了。运行j2se的test.jar很顺序。

	$jamvm -jar test.jar
	true
	true
	false
	true
	true
	test completed.

	
又找到一些功能机时代玩过的小游戏，尝试跑起来，但是JAMVM直接跳抛出异常，退出啦。

	$jamvm -jar games001.jar
	Couldn't find Main-Class attribute in /data/local/tmp/games001.jar Manifest.
	
查看games001.jar得知，他需要Jamvm支持MIDP，于是又去Google上找，后来找到MIDpath，这个项目很多是sh脚本，还有一些编译好的JSR开发包。它本身就支持与Jamvm协作，设置好脚本之后，把一些native包也编译好，放到classpath里面，可以运行games001.jar包，但是在显示上有问题，直接搞得Android Serial 死掉了。

因为找不到合适的解决方案。转而又找到phoneme,编译搞死人，还要自己改一些东西。更要命的是没有说明方案。JSR编译全靠自己去猜。文档非常少，没有Readme，也没有Install编译说明性的文档。有时只能自己从Makefile文件里寻找蛛丝马迹.

PhoneMe编译还算顺序。但是依赖的JSR没有找到说明文档。只能摸着，通过边改边GDOSiteW10查看subsystem.mk得知，编译JSR需要启用JUMP，而编译JUMP又依赖QT3的Embed。N年前的“老古董”。折腾人。又有一条漫长而曲折的路要走。