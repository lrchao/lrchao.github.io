---
layout: post
title: Jenkins使用
tags:
- Jenkins
categories: Tools
description: Jenkins配置 使用
---

# 一，与GitLab使用

 1. 下载并安装插件“GitLab Plugin”和“Git plugin”    
  
	![](http://ww3.sinaimg.cn/mw690/5ecfffbajw1f6e9kjmun5j20nm0dlwff.jpg)

2. 在“系统管理”的“系统设置”中的“Gitlab”添加” Gitlab host URL”和“API Token”和勾选”Ignore SSL Certificate Errors” 

	![](http://ww4.sinaimg.cn/mw690/5ecfffbajw1f6e9kiqa76j20nm0e5t9t.jpg)

3. 新建项目，源码管理，勾选”Git”, “Credentials”，ssh设置“id_rsa”文件的全部内容 

	![](http://ww1.sinaimg.cn/mw690/5ecfffbajw1f6e9khtu59j20nm0fuq4j.jpg)
 
	![](http://ww3.sinaimg.cn/mw690/5ecfffbajw1f6e9kgvapcj20nm0f2jtv.jpg)

# 二，相关问题

### 问题一

~~~ java
What went wrong: 
Execution failed for task ‘:app:processDebugResources’. 
A problem occurred starting process ‘command ‘/usr/local/android-sdk-linux/build-tools/23.0.2/aapt”
~~~

__原因__  
缺少32库

__解决__

~~~ java
sudo apt-get install -y libc6-i386 lib32stdc++6 lib32gcc1 lib32ncurses5 lib32z1  
~~~

### 问题二

~~~ java
FATAL: command execution failed 
java.io.IOException: Cannot run program “gradle” (in directory “/home/zhaoliu/.jenkins/workspace/test”): error=2, 没有那个文件或目录 
at java.lang.ProcessBuilder.start(ProcessBuilder.java:1047) 
at hudson.ProcLocalProc.(Proc.java:244)athudson.ProcLocalProc.(Proc.java:216) 
at hudson.LauncherLocalLauncher.launch(Launcher.java:816)athudson.LauncherProcStarter.start(Launcher.java:382) 
at hudson.LauncherProcStarter.join(Launcher.java:389)athudson.plugins.gradle.Gradle.performTask(Gradle.java:262)athudson.plugins.gradle.Gradle.perform(Gradle.java:116)athudson.tasks.BuildStepMonitor1.perform(BuildStepMonitor.java:20) 
at hudson.model.AbstractBuildAbstractBuildExecution.perform(AbstractBuild.java:785)athudson.model.BuildBuildExecution.build(Build.java:205) 
at hudson.model.BuildBuildExecution.doRun(Build.java:162)athudson.model.AbstractBuildAbstractBuildExecution.run(AbstractBuild.java:537) 
at hudson.model.Run.execute(Run.java:1741) 
at hudson.model.FreeStyleBuild.run(FreeStyleBuild.java:43) 
at hudson.model.ResourceController.execute(ResourceController.java:98) 
at hudson.model.Executor.run(Executor.java:410) 
Caused by: java.io.IOException: error=2, 没有那个文件或目录 
at java.lang.UNIXProcess.forkAndExec(Native Method) 
at java.lang.UNIXProcess.(UNIXProcess.java:186) 
at java.lang.ProcessImpl.start(ProcessImpl.java:130) 
at java.lang.ProcessBuilder.start(ProcessBuilder.java:1028) 
… 16 more
~~~

__原因__   
即使配置了gradle_home它也找不到

__解决__   
[Manage Jenkins]-〉[Configure System]配置本地gradle 路径，然后再 在项目配置里边 Gradle Version 选择你刚刚配置的那个gradle name

### 问题三

~~~ java
Failed to notify ProjectEvaluationListener.afterEvaluate(), but primary configuration failure takes precedence. 
java.lang.RuntimeException: SDK location not found. Define location with sdk.dir in the local.properties file or with an ANDROID_HOME environment variable. 
at com.android.build.gradle.internal.SdkHandler.getAndCheckSdkFolder(SdkHandler.java:140) 
at com.android.build.gradle.internal.SdkHandler.getSdkLoader(SdkHandler.java:150) 
at com.android.build.gradle.internal.SdkHandler.initTarget(SdkHandler.java:118) 
at com.android.build.gradle.BasePlugin.ensureTargetSetup(BasePlugin.java:674) 
at com.android.build.gradle.BasePlugin.createAndroidTasks(BasePlugin.java:611) 
at com.android.build.gradle.BasePlugin101.call(BasePlugin.java:566) 
at com.android.build.gradle.BasePlugin101.call(BasePlugin.java:563) 
at com.android.builder.profile.ThreadRecorder1.record(ThreadRecorder.java:55)atcom.android.builder.profile.ThreadRecorder1.record(ThreadRecorder.java:47) 
at com.android.build.gradle.BasePlugin10.execute(BasePlugin.java:562)atcom.android.build.gradle.BasePlugin10.execute(BasePlugin.java:559) 
at org.gradle.internal.event.BroadcastDispatchActionInvocationHandler.dispatch(BroadcastDispatch.java:93)atorg.gradle.internal.event.BroadcastDispatchActionInvocationHandler.dispatch(BroadcastDispatch.java:82) 
at org.gradle.internal.event.AbstractBroadcastDispatch.dispatch(AbstractBroadcastDispatch.java:44) 
at org.gradle.internal.event.BroadcastDispatch.dispatch(BroadcastDispatch.java:79) 
at org.gradle.internal.event.BroadcastDispatch.dispatch(BroadcastDispatch.java:30) 
at
~~~

__原因__   
没有找到ANDROID_HOME的环境变量

__解决__   

1. Go to Jenkins > Manage Jenkins > Configure System 
2. Check “Environment variables” 
3. add name: ANDROID_HOME, value -> your android sdk dir 
4. click “add” 
5. SCROLL DOWN CLICK SAVE

# 三，Gradle配置

Build Task填：

~~~ java
clean check assembleRelease
~~~

因为：
> build = check + assemble   
assemble = assembleRelease + assembleDebug

# 四，jenkins配置

1. jenkins升级，下载jenkins.war，替换

	![](http://ww1.sinaimg.cn/mw690/5ecfffbajw1f6e9kkqt11j20nm0dd771.jpg)
 
 

# 参考
[Jenkins官网](https://jenkins.io/index.html)
