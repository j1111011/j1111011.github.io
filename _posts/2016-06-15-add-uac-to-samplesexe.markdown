---
layout:	post
title:	"向现有EXE文件中加入 以管理员权限运行（UAC） 权限"
date:	2016-06-15 09:46:40 +0800
description: ""
categories:	
tags:	['uac','requireAdministrator']
---

# 先编写一个 XML ，以这个命名$YOU_PAMGRAM.exe.intermediate.manifest 
往文件写入以下内容

	{% highlight xml %}

	<?xml version='1.0' encoding='UTF-8' standalone='yes'?>
	<assembly xmlns='urn:schemas-microsoft-com:asm.v1' manifestVersion='1.0'>
	  <trustInfo xmlns="urn:schemas-microsoft-com:asm.v3">
	    <security>
	      <requestedPrivileges>
	        <requestedExecutionLevel level='requireAdministrator' uiAccess='false' />
	      </requestedPrivileges>
	    </security>
	  </trustInfo>
	</assembly>

	{% endhighlight %}

# 打开 VS2010 的 命令行

使用MT将以上文件编入程序，该程序就具备以管理员权限运行权限

	mt.exe -manifest $YOU_PAMGRAM.exe.intermediate.manifest -outputresource:$YOU_PAMGRAM.exe;#1


备注：$YOU_PAMGRAM 是你对应的程序名称	