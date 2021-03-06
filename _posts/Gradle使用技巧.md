---
layout: post
title: Gradle-使用技巧
tags:
- Gradle
categories: 构建
description: Gradle-使用技巧
---

#### 1，引用project层的变量

project下的 ```build.gradle``` 文件：

~~~ gradle
ext {
	compileSdkVersion = 23
    supportLibraryVersion = '23.4.0'
}
~~~

app下的```build.gradle``` 文件：

~~~ gradle
compileSdkVersion rootProject.ext.compileSdkVersion

compile "com.android.support:support-v4:$rootProject.ext.supportLibraryVersion"
~~~ 

#### 2，library module中的的Manifest,xml使用plachHolder

在Lib module的build.gradle中使用

~~~ gradle 

publishNonDefault true
 productFlavors {

        local {
            manifestPlaceholders = [
                    rong_cloud_key: "6tnym1b2rnzwk731"
            ]
        }

        yingyongbao {
            manifestPlaceholders = [
                    rong_cloud_key: "cpj2xarl2jby8n321"
            ]
        }
    }
~~~

在app的build.gradle中

~~~ gradle 
dependencies {
    compile project(':Rong_Cloud_Android_IMKit_SDK_v2_6_8_dev')
    localCompile project(path: ':Rong_Cloud_Android_IMKit_SDK_v2_6_8_dev', configuration: 'localRelease')
    yingyongbaoCompile project(path: ':Rong_Cloud_Android_IMKit_SDK_v2_6_8_dev', configuration: 'yingyongbaoRelease')
}
~~~




## 示例文件 

~~~ gradle
buildscript {
    //定义一些项目需要的JAR函数库
    LIBS_DIR = "../../../libs"

    //需要从maven中央库得到gradle的android插件
    repositories {
        mavenCentral()
    }

    dependencies {
        classpath 'com.android.tools.build:gradle:0.4.2'
    }
}

//声明项目是一个android构建
apply plugin: 'android'

dependencies {
    //同时用本地maven库查找依赖
    repositories {
        mavenLocal()
    }

    //下面是一些app需要的jar文件
    compile files("${LIBS_DIR}/hiscore/hiscore.jar")
    compile files("${LIBS_DIR}/GoogleAnalytics/libGoogleAnalytics.jar")

    //这是一个我存放在本地maven仓库（使用“aar”格式）的android函数库
    compile ('com.mopub.mobileads:mopub-android-sdk:unknown')
}

//android构建的项目定义
android {
    compileSdkVersion 15
    buildToolsVersion "17.0.0"

    // 使用不同的包名
    productFlavors {
        flavor1 {
            minSdkVersion 14
            packageName='com.youxiachai.androidgradle.playstore'
        }

        flavor1 {
            minSdkVersion 14           packageName='com.youxiachai.androidgradle.amazonappstore'
        }

    }

    //下面的代码路径不是推荐的新项目结构
    //我仍然使用的Eclipse风格结构
    sourceSets {
        main {
            manifest.srcFile 'AndroidManifest.xml'
            java.srcDirs = ['src']

            resources.srcDirs = ['src']
            aidl.srcDirs = ['src']

            renderscript.srcDirs = ['src']

            res.srcDirs = ['res']
            assets.srcDirs = ['assets']
        }

        instrumentTest.setRoot('tests')
    }

    //声明创建一个带签名的发布版本细节
    signingConfigs {
        release {
            storeFile file("../keys/android.keystore")
            storePassword "######"
            keyAlias "######"
            keyPassword "######"           
        }
    }

    //声明此发布构建在签名之前需要运行proguard
    buildTypes {
        release {
            runProguard true
            proguardFile getDefaultProguardFile('proguard-android.txt')
            proguardFile 'proguard.cfg'
            signingConfig signingConfigs.release
        }    
    }    
}

~~~

__从命令行构建app可以运行下面的命令__

~~~ java
gradle assembleDebug    #debug构建
gradle assembleRelease  #release构建
~~~

## 参考

1. 可查看 [Android Plugin DSL Reference](http://google.github.io/android-gradle-dsl/current/) 了解可配置的 
属性以及它们的默认值。 
2. [https://www.gitbook.com/book/chaosleong/gradle-for-android/details]()   
Demo: [https://github.com/LiuRanchao/TestGradle]()
3. 检查输入信息和显示的是否相同
4. [美团Android自动化之旅—适配渠道包](http://tech.meituan.com/mt-apk-adaptation.html)
5. [Library Module的PlaceHolder](http://stackoverflow.com/questions/24860659/multi-flavor-app-based-on-multi-flavor-library-in-android-gradle)
6. [统一管理version Code](https://medium.com/@ivan_m/use-variables-to-manage-gradle-android-dependencies-library-version-numbers-92f0e362a2fc#.catbtxq4c)


