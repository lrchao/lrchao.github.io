---
layout: post
title: Espresso介绍
tags:
- Espresson
categories: Android
description: Espresso介绍
---

# 一、Espresso使用

## 1. 介绍

### 支持API

![](http://img.blog.csdn.net/20151230141011712)

### 准备

#### 一，模拟器中的开发者选项，禁用以下设置

- Window animation scale（窗口动画缩放）
- Transition animation scale（过渡动画缩放）
- Animator duration scale（动画程序时长缩放）

![](http://img.blog.csdn.net/20151230141125723)

#### 二，下载Espresso
在app/build.gradle中

~~~ gradle
androidTestCompile 'com.android.support.test.espresso:espresso-core:2.2.1'
androidTestCompile 'com.android.support.test:runner:0.4.1'
~~~

#### 三，Set the instrumentation runner

在android.defaultConfig:中添加

~~~ java
testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
~~~

**Example build.gradle file**

~~~ java
apply plugin: 'com.android.application'

android {
    compileSdkVersion 22
    buildToolsVersion "22"

    defaultConfig {
        applicationId "com.my.awesome.app"
        minSdkVersion 10
        targetSdkVersion 22.0.1
        versionCode 1
        versionName "1.0"

        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
}

dependencies {
    // App's dependencies, including test
    compile 'com.android.support:support-annotations:22.2.0'

    // Testing-only dependencies
    androidTestCompile 'com.android.support.test:runner:0.4.1'
    androidTestCompile 'com.android.support.test.espresso:espresso-core:2.2.1'
}
~~~

# 二，编写First Test

~~~ java
@RunWith(AndroidJUnit4.class)
@LargeTest
public class HelloWorldEspressoTest {

    @Rule
    public ActivityTestRule<MainActivity> mActivityRule = new ActivityTestRule(MainActivity.class);

    @Test
    public void listGoesOverTheFold() {
        onView(withText("Hello world!")).check(matches(isDisplayed()));
    }
}
~~~

### 设置android studio 运行 test

 - Open Run menu -> Edit Configurations
 - Add a new Android Tests configuration
 - Choose a module
 - Add a specific instrumentation runner:

> android.support.test.runner.AndroidJUnitRunner

# 三，方法介绍

### 1.查找View
Finding a view with onView

~~~ java
onView(withId(R.id.my_view))
~~~

~~~ java
onView(allOf(withId(R.id.my_view), withText("Hello!")))
~~~

~~~ java
onView(allOf(withId(R.id.my_view), not(withText("Unwanted"))))
~~~

### 2，执行action

Performing an action on a view

#### 1.点击view:

~~~ java
onView(...).perform(click());
~~~

#### 2.执行多个操作

输入"hello", 并点击click()，如下：

~~~ java
onView(...).perform(typeText("Hello"), click());
~~~

如果view没有显示出来需要滑动出来调用，如下：

~~~ java
onView(...).perform(scrollTo(), click());
~~~

#### 更多action，见[ViewActions ](https://android.googlesource.com/platform/frameworks/testing/+/android-support-test/espresso/core/src/main/java/android/support/test/espresso/action/ViewActions.java)

### 3，检查是否满足断言

Checking if a view fulfills an assertion
用到
### ViewMatcher
check()作用在onView()返回的对象上   

检查一个view,是否有文本"Hello!", e.g.:

~~~ java
onView(...).check(matches(withText("Hello!")));
~~~

~~~ java
// Don't use assertions like withText inside onView.
onView(allOf(withId(...),withText("Hello!"))).check(matches(isDisplayed()));
~~~

#### matches使用

> 1.matches(HintMatcher.withHint(hintText))  可以只定义Matcher   
> 2.matches(isDisplayed()  已经显示    
> 3.matches(not(isDisplayed())   没有显示    
> 4.matches(isCompletelyDisplayed()  完全显示  
> 5.matches(withText(TEXT_ITEM_30_SELECTED))  有该文本   
> 6.matches(isChecked())  已经check状态   

#### check使用

> 1.check(doesNotExist() 不存在

### 4，用onData处理列表view

Using onData with AdapterView controls (ListView, GridView, …)   
使用自定义的AdapterView时，如果改写，特别的getItem() API,可能会有问题，此时或许重构你的application代码 或者 实现[AdapterViewProtocol](https://android.googlesource.com/platform/frameworks/testing/+/android-support-test/espresso/core/src/main/java/android/support/test/espresso/action/AdapterViewProtocol.java)

**e.g.**     
有一个Spinner,当item选中后，将其值显示显示在TextView上

1.点击Spinner打开选项列表

~~~ java
onView(withId(R.id.spinner_simple)).perform(click());
~~~

2.点击在item "Americano"

~~~ java
onData(allOf(is(instanceOf(String.class)), is("Americano"))).perform(click());
~~~

3.验证TextView显示的是否为点击的item

~~~ java
onView(withId(R.id.spinnertext_simple))
  .check(matches(withText(containsString("Americano"))));
~~~

### 5，注解

#### @Rule
  声明加载的Activity test

~~~ java
@Rule
public ActivityTestRule<MainActivity> mActivityRule = new ActivityTestRule<>(
            MainActivity.class);
~~~

声明加载Intents test

~~~ java
@Rule
public IntentsTestRule<DialerActivity> mActivityRule = new IntentsTestRule<>(
            DialerActivity.class);
~~~

#### @Before
执行test之前，先执行的

~~~ java
@Before
public void initValidStrings() {
    // Produce a string with valid ending.
    mStringWithValidEnding = "Random " + MainActivity.VALID_ENDING;

    // Get one of the available coffee preparations.
    mValidStringToBeTyped = MainActivity.COFFEE_PREPARATIONS.get(0);
}
~~~

### 6，检查内容

1. EditText输入内容，点击按钮提交，改变TextView，检查TextView是否显示为预期的内容，在同一个Activity.
2. 跳转Activity，在目标Activity中检查携带的文本是否正确
3. 自定义matcher
4. 检查是否已经显示isDisplayed()
5. 检查列表的某一项是否存在doesNotExist()
6. 检查列表是否完全显示isCompletelyDisplayed()
7. 检查CheckBox是否为为选中状态
8. 检查调用Intent的ActivityResult，例如拍照后检查(com.example.android.testing.espresso.intents.AdvancedSample
com.example.android.testing.espresso.BasicSample)
9. 检查WebView里的元素执行

# 三，编译中遇到的问题

#### 问题1

> Warning:Conflict with dependency 'com.android.support:support-annotations'. Resolved versions for app (22.2.1) and test app (23.0.1) differ. See http://g.co/androidstudio/app-test-app-conflict for details.


解决:  
参考[http://stackoverflow.com/questions/33317555/conflict-with-dependency-com-android-supportsupport-annotations-resolved-ver]()
或者   将com.android.support:的为test app的版本号

#### 问题2

> Error:(3) Error retrieving parent for item: No resource found that matches the given name 'android:TextAppearance.Material.Widget.Button.Inverse'.
Error:(18) Error retrieving parent for item: No resource found that matches the given name 'android:Widget.Material.Button.Colored'.

解决:   
[http://stackoverflow.com/questions/32075498/error-retrieving-parent-for-item-no-resource-found-that-matches-the-given-name]()
编译的sdk版本要和依赖lib的版本号一致


# 参考

- [官方网址](https://google.github.io/android-testing-support-library/docs/espresso/index.html)
- [Demo](https://github.com/googlesamples/android-testing)


