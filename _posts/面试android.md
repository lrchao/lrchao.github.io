---
layout: post
title: android面试题
tags:
- android
- 面试
categories: Android
description: android面试题
---


### 一，Java基础

#### 1.列举java的集合和继承关系

~~~ java
Collection
    |- List
    |   |- LinkedList
    |   |- ArrarList
    |   |- Vector
    |   |- Stack
    |- Set
    
Map
 |- HashTable
 |- HashMap
 |- WeakHashMap
~~~    
  
#### 2.哪些情况下的对象会被垃圾回收机制处理掉

Java 垃圾回收机制最基本的做法是分代回收。内存中的区域被划分成不同的世代，对象根据其存活的时间被保存在对应世代的区域中。一般的实现是划分成3个世代：年轻、年老和永久。

#### 3.进程和线程的区别

内存：<br>
线程：共享内存 <br>
进程：独立内存

进程和线程的主要差别在于它们是不同的操作系统资源管理方式。进程有独立的地址空间，一个进程崩溃后，在保护模式下不会对其它进程产生影响，而线程只是一个进程中的不同执行路径。

#### 4.java中==和equals和hashCode的区别

如果两个对象根据equals()方法比较是相等的，那么调用这两个对象中任意一个对象的hashCode方法都必须产生同样的整数结果。<br>
如果两个对象根据equals()方法比较是不相等的，那么调用这两个对象中任意一个对象的hashCode方法，则不一定要产生相同的整数结果

覆盖equals时总要覆盖hashCode <br>
  一个很常见的错误根源在于没有覆盖hashCode方法。在每个覆盖了equals方法的类中，也必须覆盖hashCode方法。如果不这样做的话，就会违反Object.hashCode的通用约定，从而导致该类无法结合所有基于散列的集合一起正常运作，这样的集合包括HashMap、HashSet和Hashtable。 
  
#### 5.常见的排序算法时间复杂度

排序法  | 平均时间 | 最差情形 | 稳定度 | 额外空间 | 备注
-------|---------|--------| -------|---------|------
冒泡    | O(n^2) | O(n^2)  | 稳定    | O(1)   | n小时较好
交换    | O(n^2) | O(n^2)  | 不稳定  | O(1)   | n小时较好
选择    | O(n^2) | O(n^2)  | 不稳定  | O(1)   | n小时较好
插入    | O(n^2) | O(n^2)  | 稳定    | O(1)   | 大部分已排序时较好
基数    | O(logR^b) | O(n^2)  | 稳定    | O(n)   | B是真数(0-9),R是基数(个十百)
Shell   | O(nlogn) | O(n^s) 1<s<2  | 不稳定    | O(1)   | s是所选分组
快速    | O(nlogn) | O(n^2)  | 不稳定    | O(nlogn)   | n大时较好
归并    | O(nlogn) | O(nlogn)  | 稳定    | O(1)   | n大时较好
堆    | O(nlogn) | O(nlogn)  | 不稳定    | O(1)   | n大时较好

#### 6.HashMap的实现原理

1. HashMap概述：    HashMap是基于哈希表的Map接口的非同步实现。此实现提供所有可选的映射操作，并允许使用null值和null键。此类不保证映射的顺序，特别是它不保证该顺序恒久不变。
2. HashMap的数据结构： 在java编程语言中，最基本的结构就是两种，一个是数组，另外一个是模拟指针（引用），所有的数据结构都可以用这两个基本结构来构造的，HashMap也不例外。HashMap实际上是一个“链表散列”的数据结构，即数组和链表的结合体。

#### 7.int-char-long各占多少字节数

名称    | 位数   | 字节数 
-------|-------|--------
byte   | 8     | 1
short  | 16    | 2
int    | 32    | 4
long   | 64    | 8
float  | 32    | 4
double | 64    | 8
char   | 16    | 2

#### 8.java多态

面向对象的三大特性：封装、继承、多态。从一定角度来看，封装和继承几乎都是为多态而准备的。这是我们最后一个概念，也是最重要的知识点。

多态的定义：指允许不同类的对象对同一消息做出响应。即同一消息可以根据发送对象的不同而采用多种不同的行为方式。（发送消息就是函数调用）

实现多态的技术称为：动态绑定（dynamic binding），是指在执行期间判断所引用对象的实 际类型，根据其实际的类型调用其相应的方法。

多态的作用：消除类型之间的耦合关系。

现实中，关于多态的例子不胜枚举。比方说按下 F1 键这个动作，如果当前在 Flash 界面下弹出的就是 AS 3 的帮助文档；如果当前在 Word 下弹出的就是 Word 帮助；在 Windows 下弹出的就是 Windows 帮助和支持。同一个事件发生在不同的对象上会产生不同的结果。 下面是多态存在的三个必要条件，要求大家做梦时都能背出来！

多态存在的三个必要条件 一、要有继承； 二、要有重写； 三、父类引用指向子类对象。

 多态的好处：

1.可替换性（substitutability）。多态对已存在代码具有可替换性。例如，多态对圆Circle类工作，对其他任何圆形几何体，如圆环，也同样工作。

2.可扩充性（extensibility）。多态对代码具有可扩充性。增加新的子类不影响已存在类的多态性、继承性，以及其他特性的运行和操作。实际上新加子类更容易获得多态功能。例如，在实现了圆锥、半圆锥以及半球体的多态基础上，很容易增添球体类的多态性。

3.接口性（interface-ability）。多态是超类通过方法签名，向子类提供了一个共同接口，由子类来完善或者覆盖它而实现的。如图8.3 所示。图中超类Shape规定了两个实现多态的接口方法，computeArea()以及computeVolume()。子类，如Circle和Sphere为了实现多态，完善或者覆盖这两个接口方法。

4.灵活性（flexibility）。它在应用中体现了灵活多样的操作，提高了使用效率。

5.简化性（simplicity）。多态简化对应用软件的代码编写和修改过程，尤其在处理大量对象的运算和操作时，这个特点尤为突出和重要。

Java中多态的实现方式：接口实现，继承父类进行方法重写，同一个类中进行方法重载。

#### 9.抽象类接口区别

1. 默认的方法实现 抽象类可以有默认的方法实现完全是抽象的。接口根本不存在方法的实现

2. 实现 :子类使用extends关键字来继承抽象类。如果子类不是抽象类的话，它需要提供抽象类中所有声明的方法的实现。
子类使用关键字implements来实现接口。它需要提供接口中所有声明的方法的实现

3. 构造器:
抽象类可以有构造器 接口不能有构造器

4. 访问修饰符:
抽象方法可以有public、protected和default这些修饰符 接口方法默认修饰符是public。你不可以使用其它修饰符。

5. 多继承:
抽象类在java语言中所表示的是一种继承关系，一个子类只能存在一个父类，但是可以存在多个接口。

#### 10.什么是Serialization?如何实现它？

串行化是将对象转换为字节流的过程，以便将对象存储到内存中，以便在保留对象原始状态和数据时可以重新创建对象。在Java中有两种方法，一种是实现可序列化或可分配的。然而，在Android中，可串行化不应该在Android中使用。Parcelable被创建为更高效的可序列化的，并且执行了大约10倍的序列化，因为序列化是一个缓慢的过程，并且倾向于创建大量的临时对象，这可能会导致垃圾收集更多的发生。

#### 11.Java中final、finally、finalize的区别

final用于声明属性，方法和类，分别表示属性不可交变，方法不可覆盖，类不可继承。<br>
finally是异常处理语句结构的一部分，表示总是执行。<br>
finalize是Object类的一个方法，在垃圾收集器执行的时候会调用被回收对象的此方法，供垃圾收集时的其他资源回收，例如关闭文件等。


#### 12.垃圾回收机制

所有对象在堆上分配区域管理的JVM。只要一个对象被引用,JVM认为它活着。一旦一个对象不再被引用,因此无法达到应用程序代码中,垃圾收集器清除它,回收未使用的内存

#### 13.Arrays  ArrayLists 区别

Array（[]）：最高效；但是其容量固定且无法动态改变；<br>
ArrayList：  容量可动态增长；但牺牲效率；

#### 14.HashSet vs TreeSet.

HashSet:

 - 不能保证元素的排列顺序，顺序有可能发生变化
 - 不是同步的
 - 集合元素可以是null,但只能放入一个null

 TreeSet
 
 可以排序

#### 15.Java中的访问修饰符

Java修饰符类型(public,protected,private,friendly) <br>

修饰符     | 当前类 |  同包 | 子类 | 其他包
----------|-------|-------|----|------
public    | Yes   | Yes   | Yes| Yes
protected | Yes   | Yes   | Yes| No
default   | Yes   | Yes   | No | No
private   | Yes   |  No   | No | No

#### 16.java对象的强引用，软引用，弱引用和虚引用

 - 强引用   
	垃圾回收器绝不会回收它(当内存空 间不足，Java虚拟机宁愿抛出OutOfMemoryError错误，使程序异常终止，也不会靠随意回收具有强引用的对象来解决内存不足问题)

 - 软引用   
 	如果内存空间足够，垃圾回收器就不会回收它，如果内存空间不足了，就会回收这些对象的内存。只要垃圾回收器没有回收它，该对象就可以被程序使用。软引用可用来实现内存敏感的高速缓存。
 	
 - 弱引用   
   弱引用与软引用的区别在于：只具有弱引用的对象拥有更短暂的生命周期。在垃圾回收器线程扫描它 所管辖的内存区域的过程中，一旦发现了只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的内存。不过，由于垃圾回收器是一个优先级很低的线程， 因此不一定会很快发现那些只具有弱引用的对象。 
   
 - 虚引用  
   在任何时候都可能被垃圾回收
   
#### 17.依赖注入是什么？用过哪些库？是否使用过？

创建被调用者的工作不再由调用者来完毕，因此称为控制反转;创建被调用者 实例的工作通常由Spring容器来完毕，然后注入调用者，因此也称为依赖注入。

Dagger,Dagger2

#### 18. synchronized是什么意思？

synchronized可以保证方法或者代码块在运行时，同一时刻只有一个方法可以进入到临界区，同时它还可以保证共享变量的内存可见性

#### 19. String不可变是什么意思？

1. 满足 String Pool (String intern pool) 字符串保留池的需要    
2. Java中String对象的哈希码被频繁地使用, 比如在hashMap 等容器中。
3. String被许多的Java类(库)用来当做参数,例如 网络连接地址URL,文件路径path,还有反射机制所需要的String参数等, 假若String不是固定不变的,将会引起各种安全隐患。

#### 20. transient关键字什么意思？

Java的serialization提供了一种持久化对象实例的机制。当持久化对象时，可能有一个特殊的对象数据成员，我们不想   
用serialization机制来保存它。为了在一个特定对象的一个域上关闭serialization，可以在这个域前加上关键字transient。   
transient是Java语言的关键字，用来表示一个域不是该对象串行化的一部分。当一个对象被串行化的时候，transient型变量的值不包括在串行化的表示中，然而非transient型的变量是被包括进去的。  

#### 21. volatile关键字什么意思？

用volatile修饰的变量，线程在每次使用变量的时候，都会读取变量修改后的最的值。volatile很容易被误用，用来进行原子性操作。

#### 22. 什么是finalize()？

垃圾回收器准备释放内存的时候，会先调用finalize()。

 - 对象不一定会被回收。
 - 垃圾回收不是析构函数。
 - 垃圾回收只与内存有关。
 - 垃圾回收和finalize()都是靠不住的，只要JVM还没有快到耗尽内存的地步，它是不会浪费时间进行垃圾回收的。

#### 23. 实例化和初始化的区别？

一个开辟空间，一个赋值

#### 24. static块什么时间执行？

在java中，类的static块会在类被加载的时候执行且仅会被执行一次。但是有一个问题需要说明，那就是java的类加载机制是在jvm启动的时候java的核心类(rt.jar中全部类)会被全部载入，而用户定义的类仅在被使用的时候才被加载或者由我们主动加载(使用Class.forName(String className)方法)。

#### 25. 解释泛型？

泛型（Generic type 或者 generics）是对 Java 语言的类型系统的一种扩展，以支持创建可以按类型进行参数化的类。可以把类型参数看作是使用参数化类型时指定的类型的一个占位符，就像方法的形式参数是运行时传递的值的占位符一样。

#### 26.StringBuffer与StringBuilder的区别?

StringBuffer 字符串变量（线程安全）   
StringBuilder 字符串变量（非线程安全）

#### 27.什么是ThreadPoolExecutor？

为什么使用它：   
a. 支持在队列中添加、取消、排序任务
b. 复用已经创建的线程

#### 28.什么是反射？

java反射机制，是在运行状态中，对于任何一个类，都能够访问这个类的所有属性和方法，同时任何一个对象也都能够调用它的任意一个方法和属性，这个功能称为java语言的反射机制

#### 29.What is String.intern()? When and why should it be used?

返回字符串对象的规范化表示形式。

一个初始时为空的字符串池，它由类 String 私有地维护。

当调用 intern 方法时，如果池已经包含一个等于此 String 对象的字符串（该对象由 equals(Object) 方法确定），则返回池中的字符串。否则，将此 String 对象添加到池中，并且返回此 String 对象的引用。

它遵循对于任何两个字符串 s 和 t，当且仅当 s.equals(t) 为 true 时，s.intern() == t.intern() 才为 true。

所有字面值字符串和字符串赋值常量表达式都是内部的。

返回：

一个字符串，内容与此字符串相同，但它保证来自字符串池中。

~~~ java
String str1 = "a";
String str2 = "b";
String str3 = "ab";
String str4 = str1 + str2;
String str5 = new String("ab");
 
System.out.println(str5.equals(str3));
System.out.println(str5 == str3);
System.out.println(str5.intern() == str3);
System.out.println(str5.intern() == str4);
~~~

#### 30.What is Java NIO?

Java NIO非堵塞技术实际是采取Reactor模式，或者说是Observer模式为我们监察I/O端口，如果有内容进来，会自动通知我们，这样，我们就不必开启多个线程死等，从外界看，实现了流畅的I/O读写，不堵塞了。

### 二，Android相关

#### 1.内存泄漏的场景

 - 引用没释放造成的内存泄露
 	- 注册没取消造成的内存泄露
 	- 集合中对象没清理造成的内存泄露
 	- static
 	- 线程（内部类的使用）
 - 资源对象没关闭造成的内存泄露(资源性对象比如（Cursor，File文件等)
 - 一些不良代码成内存压力
 	- Bitmap没调用recycle()
 	- 构造Adapter时，没有使用缓存的 convertView

#### 2.Activity的生命周期

#### 3.Android程序的所有组件

#### 4.Service vs IntentService

#### 5.Parcelable和Serializable的区别

Parcelable的性能比Serializable好，在内存开销方面较小，所以在内存间数据传输时推荐使用Parcelable，如activity间传输数据，而Serializable可将数据持久化方便保存，所以在需要保存或网络传输数据时选择Serializable，因为android不同版本Parcelable可能不同，所以不推荐使用Parcelable进行数据持久化

#### 6.四种launchMode

 - standard   
 	不管有没有已存在的实例，都生成新的实例
 - singleTop   
 	如果发现有对应的Activity实例正位于栈顶，则重复利用，不再生成新的实例
 - singleTask   
	如果发现有对应的Activity实例，则使此Activity实例之上的其他Activity实例统统出栈，	使此Activity实例成为栈顶对象，显示到幕前。
 - singleInstance
   singleInstance 新建一个Task，且在该Task中只有它的唯一一个实例。 (只有一个Task会有，且该Task中只有它)。“singleInstance”是其所在栈的唯一activity，它会每次都被重用。

 


## 参考

- [https://github.com/MindorksOpenSource/android-interview-questions]()


