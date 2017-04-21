---
layout:	post
title:	"android smali study on #1"
date:	2017-04-20 14:48:58 +0800
description: ""
categories:	
tags:	[apktool,jarsigner,smali,android,crack]
---
最近在学习Android的反编译，使用JEB或者Dex2Jar 反编译成java代码。这些代码包含大量的goto , break label. 语句，很奇怪，Java语言本身并没有这些语法，但是反编译出来生成的，却有大量的goto语句。除非看懂整个代码的逻辑语义，否则改起来，还是挺吃力的。

于是转念一想，如果我改动并不是很大，只是加一些内容，减一些内容。是不是修改smali，然后重新打包更快呢？

想到就开始做，写了一个小的示例程序。
在DemoA 实现了 test方法，用Toast输出一个字符串。在Smali代码中会生成如下的函数：

	.method public test()V
    .locals 2

    .prologue
    .line 9
    const-string v0, "Toast form test."

    const/4 v1, 0x1

    invoke-static {p0, v0, v1}, Landroid/widget/Toast;->makeText(Landroid/content/Context;Ljava/lang/CharSequence;I)Landroid/widget/Toast;

    move-result-object v0

    invoke-virtual {v0}, Landroid/widget/Toast;->show()V

    .line 10
    return-void
	.end method

在调用的地方，会生成对应代码：

	# virtual methods
	.method public run()V
	    .locals 1
	
	    .prologue
	    .line 18
	    iget-object v0, p0, Lcom/yh/jl/smali1/MainActivity$1;->this$0:Lcom/yh/jl/smali1/MainActivity;
	
	    invoke-virtual {v0}, Lcom/yh/jl/smali1/MainActivity;->test()V
	
	    .line 19
	    return-void
	.end method

在DemoB中定义test()函数，然后会生成代码：

	.method public test2()V
    .locals 2

    .prologue
    .line 12
    const-string v0, "Toast form test2."

    const/4 v1, 0x1

    invoke-static {p0, v0, v1}, Landroid/widget/Toast;->makeText(Landroid/content/Context;Ljava/lang/CharSequence;I)Landroid/widget/Toast;

    move-result-object v0

    invoke-virtual {v0}, Landroid/widget/Toast;->show()V

    .line 13
    return-void
	.end method

将上述代码复制到 DemoA 的Smali代码中 ，在调用test的地方，加上

		invoke-virtual {v0}, Lcom/yh/jl/smali1/MainActivity;->test2()V

将代码重新打包，签名。再安装到手机，运行就可以看到，会输出2个Toast，这也是我们想到的。这个简单的例子说明在smali增加语句是很容易 的，同理，替换，删除功能是不是也简单呢？那可能需要我们自己多研究研究啦。
