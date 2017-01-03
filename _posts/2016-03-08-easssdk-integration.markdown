---
layout:	post
title:	"AssSDK 集成方法及环境配置 "
date:	2016-03-08 13:52:59 +0800
description: ""
categories:	
tags:	[]
---
### 请点击 [下载](http://7xrgjr.com1.z0.glb.clouddn.com/asssdk-release.aar) AAR库

### 选择导入AAR库

从Android Studio以下Menu导入库包
File->New->New Moudle->Import .JAR/.AAR package

如下图所示：

图1：

![图1](http://7xrgjr.com1.z0.glb.clouddn.com/import_arr_1.png)

图2：

![图2](http://7xrgjr.com1.z0.glb.clouddn.com/import_arr_2.png)

选择我们的asssdk-release.aar包，并导入

### 配置环境

在APP的gradle.build

	{% highlight python %}
	dependencies {
	    compile fileTree(include: ['*.jar'], dir: 'libs')
	    testCompile 'junit:junit:4.12'	    
	}
	{% endhighlight %}

之中加入

	{% highlight python %}
	compile 'com.google.code.gson:gson:2.4'
	compile 'com.mcxiaoke.volley:library:1.0.19'
    compile project(':asssdk-release')
	{% endhighlight %}


在入口函数加入集成代码：
	
	{% highlight java %}
	import com.yiba.asssdk.AssSdkSettings;
	public class MainActivity extends Activity {
	
	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
	        super.onCreate(savedInstanceState);
	        setContentView(R.layout.activity_main);
	        AssSdkSettings.setAppName("sdkdemo");
	        AssSdkSettings.setAppVersion("1.0.0");
	        AssSdkSettings.StartService(getApplicationContext());
	    }
	}
	{% endhighlight %}


### 编译生成