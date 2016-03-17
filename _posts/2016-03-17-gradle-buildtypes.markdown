---
layout:	post
title:	"Android Studio区别编译Debug|Release版本"
date:	2016-03-17 11:47:23 +0800
description: "Android Studio区别编译Debug|Release版本"
categories:	build
tags:	["gradle","android studio","debug","release"]
---

## Android Studio区别编译Debug|Release版本

之前用C++，系统有个预设宏，可以直接用来判断Debug,Release版本，最近在弄个Android项目，用了AS发现，Gradle还是蛮方便的，只要做如下设置

	{% highlight python %}
	 buildTypes {
	        release {
	            buildConfigField "boolean", "IsAlpha", "false"
	            minifyEnabled true
	            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
	        }
	        debug {
	            buildConfigField "boolean", "IsAlpha", "true"
	        }
	}
	{% endhighlight %}



在指定目录下会生成以下文件 build\generated\source\buildConfig\debug\com\example\BuildConfig.java (com\example 为你的应用包名）

	{% highlight java %}
	public final class BuildConfig {
	  public static final boolean DEBUG = Boolean.parseBoolean("true");
	  public static final String APPLICATION_ID = "com.example";
	  public static final String BUILD_TYPE = "debug";
	  public static final String FLAVOR = "";
	  public static final int VERSION_CODE = 100;
	  public static final String VERSION_NAME = "";
	  // Fields from build type: debug
	  public static final boolean IsAlpha = true;
	}
	{% endhighlight %}


设置成功之后，IsAlpha会根据你不同的编译目标，生成不同的值。你就可以在你的代码里调用

	{% highlight java %}
	if(BuildConfig.IsAlpha) { 
		//...
	}
	{% endhighlight %}

进行区别初始化，编译APP啦

[参考资料](http://www.cnblogs.com/tt_mc/p/4277180.html "详见参考资料")
