---
layout:	post
title:	"set windows system environment"
date:	2016-05-28 11:45:18 +0800
description: ""
categories:	
tags:	["windows","setenv"]
---

# 添加系统变量

Windows 提供了API函数SetEnvironmentVariable，不过这个函数只能修改当前进程的环境变量，而不能修改其他进程和系统的变量。
要修改系统的环境变量，需要修改注册表SYSTEM\CurrentControlSet\Control\Session Manager\Environment搜索下的项,在下面增加一个字项就可以了。

	{% highlight c++ %}
	// system environment
	QSettings settings("HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Session Manager\\Environment",QSettings::NativeFormat);
	settings.setValue("K","V");
	settings.sync();
	// user environment
	QSettings users("HKEY_CURRENT_USER\\Environment",QSettings::NativeFormat);
    users.setValue("K","V");
    users.sync();
	{% endhighlight %}


# 隐藏CMD窗口

由于写了一个控制台程序，需要不显示窗口，搜索到：

	{% highlight c++ %}
	#pragma comment(linker, "/subsystem:\"windows\" /entry:\"mainCRTStartup\"")
	{% endhighlight %}

使用这一句，可以隐藏

# QT的程序增加管理员权限。
在pro文件中增加：

	{% highlight c++ %}
	QMAKE_LFLAGS += /MANIFESTUAC:"level='requireAdministrator'uiAccess='false'"
	{% endhighlight %}
