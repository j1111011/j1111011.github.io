---
layout:	post
title:	"gitlab maven repositories"
date:	2017-04-21 19:20:47 +0800
description: ""
categories:	
tags:	[gitlab,maven,repositories]
---

## 提交共享库到Maven仓库

### Step 1.

使用git clone命令把新建的 repo clone到本地目录。

	$git clone http://[host]/[group/[repo].git mvnrepo
	
	

### Step 2.

#### 发布aar文件

在项目工程的 build.gradle 文件中添加：

	apply plugin: 'maven'
	// ... ...
	ext {
		PUBLISH_GROUP_ID = 'com.example'
		PUBLISH_ARTIFACT_ID = 'testmvn'
		PUBLISH_VERSION = android.defaultConfig.versionName
	}
	
	uploadArchives {
		repositories.mavenDeployer {
			def deployPath = file(getProperty('mvn.deployPath'))
			repository(url: "file://${deployPath.absolutePath}")
			pom.project {
				groupId project.PUBLISH_GROUP_ID
				artifactId project.PUBLISH_ARTIFACT_ID
				version project.PUBLISH_VERSION
			}
		}
	}

其中 aar.deployPath 在 可在 gradle.properties

文件中指定:

	mvn.deployPath=D:\\mvnrepo

路径为第一步clone repo选择的目录。

发布aar文件前，只需要在工程目录下执行命令：

	gradlew uploadArchives


#### 发布jar文件

通过以下命令将本地的jar包安装到此目录下：

	mvn install:install-file -DgroupId=com.example -DartifactId=testmvn -Dversion=1.0.0 -Dfile=${jarfile} -Dpackaging=jar -DgeneratePom=true -DlocalRepositoryPath=d:\mvnrepo -DcreateChecksum=true


### Step 3.

进入e:\repo 通过git add/commit/push 将生成的maven文件提交到服务器，即可完成发布。

## 在Android Studio中引用库文件



在工程的 build.grade 文件中添加：

	allprojects {
	    repositories {
	        jcenter()
	        maven { url 'http://[host]/[group/[repo]/raw/[branches]/repository/' }
	    }
	}


项目app的build.gradle添加.
 
	dependencies {
          compile 'com.example:testmvn:1.0.0'
  	}

如果是AAR包，需要附加@aar

	dependencies {
          compile 'com.example:testmvn:1.0.0@aar'
  	}