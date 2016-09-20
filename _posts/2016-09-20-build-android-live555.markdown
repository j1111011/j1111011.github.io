---
layout:	post
title:	"build android live555"
date:	2016-09-20 09:49:36 +0800
description: ""
categories:	
tags:	[android,live555,standalone-toolchain]
---

# step 1. generate android standalone-toolchian.

	$$NDK_HOME/build/tools/make-standalone-toolchain.sh --platform=android-5 --install-dir=/tmp/my-android-toolchain [ --arch=x86 ]
	
	$export PATH=$PATH:/tmp/my-android-toolchain/bin

	--arch defalut arm
	

# step 2. generate android-x86 Makefiles.

	$copy config.armlinux config.android-x86
	$vi config.android-x86 
	COMPILE_OPTS =		$(INCLUDES) -fPIE -I. -O2 -DSOCKLEN_T=socklen_t -D_LARGEFILE_SOURCE=1 -D_FILE_OFFSET_BITS=32
	C =			c
	C_COMPILER =		i686-linux-android-gcc
	C_FLAGS =		$(COMPILE_OPTS)
	CPP =			cpp
	CPLUSPLUS_COMPILER =	i686-linux-android-c++
	CPLUSPLUS_FLAGS =	$(COMPILE_OPTS) -Wall -DBSD=1
	OBJ =			o
	LINK =			i686-linux-android-c++ -o
	LINK_OPTS =		-L. -fPIE -pie
	CONSOLE_LINK_OPTS =	$(LINK_OPTS)
	LIBRARY_LINK =		i686-linux-android-ar cr 
	LIBRARY_LINK_OPTS =	
	LIB_SUFFIX =			a
	LIBS_FOR_CONSOLE_APPLICATION =
	LIBS_FOR_GUI_APPLICATION =
	EXE =
	
	$./genMakefiles android-x86

# step 3. compile
	$ make 


