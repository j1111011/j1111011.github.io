---
layout:	post
title:	"porting cvm to android"
date:	2017-04-13 19:52:12 +0800
description: ""
categories:	porting
tags:	['j2me','jamvm','gnuclasspath']
---
# 生成好Android的独立工具链之后，将其路径加入到PATH
# 编译gnuclasspath
    $./configure --host=arm-linux-androideabi --disable-gtk-peer --disable-gconf-peer --with-antlr-jar=/home/jiangliang/antlr-4.6-complete.jar --disable-gjdoc --prefix=/opt/jvm/
--disable-gtk-peer 去掉GTK的依赖
--disable-gconf-peer 去掉GCONF的依赖
--disable-gjdoc 因为没有搞定anltr.jar的问题，禁用gjdoc，禁用anltr依赖
    $ make && make install
# 编译jamvm
    $ ./configure --prefix=/opt/jvm --with-classpath-install-dir=/opt/jvm --host=arm-linux-androideabi --enable-static
    $ make && make install

# 将jamvm部署到android当中。
    $ jamvm -jar test.jar
会提示
java/lang/NoClassDefFoundError: jamvm/java/lang/VMClassLoaderData错误
    
    $ jamvm -version
    java version "1.5.0"
    JamVM version 2.0.0
    Copyright (C) 2003-2014 Robert Lougher <rob@jamvm.org.uk>
    
    This program is free software; you can redistribute it and/or
    modify it under the terms of the GNU General Public License
    as published by the Free Software Foundation; either version 2,
    or (at your option) any later version.
    
    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.
    
    Build information:
    
    Execution Engine: inline-threaded interpreter with stack-caching
    Compiled with: gcc 4.9.x 20150123 (prerelease)
    
    Boot Library Path: /opt/jvm/lib/classpath
    Boot Class Path: /opt/jvm/share/jamvm/classes.zip:/opt/jvm/share/classpath/glibj.zip

classpath 设置是正确的，为什么还不能启动。
最后通过使用

    $jamvm -Xbootclasspath:/opt/jvm/share/jamvm/classes.zip:/opt/jvm/share/classpath/glibj.zip -jar test.jar
    
执行成功。不甚理解。最终，在查看jamvm源码:src/class.c中。

    void setBootClassPath(InitArgs *args) {
        char *path = args->bootpath;
        if(path == NULL)
            path = getCommandLineProperty("sun.boot.class.path");
        if(path == NULL)
            path = getCommandLineProperty("java.boot.class.path");
        if(path == NULL)
            path = getenv("BOOTCLASSPATH");
        if(path == NULL)
            path = classlibBootClassPathOpt(args);
            
看到一些端倪，查看android系统变量时发现：

    $ echo $BOOTCLASSPATH
    /system/framework/core.jar:/system/framework/conscrypt.jar:/system/framework/okhttp.jar:/system/framework/core-junit.jar:/system/framework/bouncycastle.jar:/system/framework/ext.jar:/system/framework/framework.jar:/system/framework/framework2.jar:/system/framework/telephony-common.jar:/system/framework/voip-common.jar:/system/framework/mms-common.jar:/system/framework/android.policy.jar:/system/framework/services.jar:/system/framework/apache-xml.jar:/system/framework/webviewchromium.jar

这样就理解了，上面报的jamvm/java/lang/VMClassLoaderData错误。原来他使用了默认安卓设置的bootclasspath,而没有使用默认设置的bootclasspath.