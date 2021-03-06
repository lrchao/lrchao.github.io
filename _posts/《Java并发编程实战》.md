---
layout: post
title: 《Java并发编程实战》
tags:
- 多线程
categories: Book
description: 《Java并发编程实战》
---

## 一、简介

### 1.1 并发简史

原因：

- 资源利用率
- 公平性
- 便利性

### 1.2 线程的优势

#### 1.2.1 发挥多处理器的强大功能

#### 1.2.2 建模的简单行

#### 1.2.3 异步事件的简化处理

### 1.2.4 响应更灵敏的用户界面

### 1.3 线程带来的风险

#### 1.3.1 安全性问题

#### 1.3.2 活跃性问题

#### 1.3.3 性能问题

### 1.4 线程无处不在

# 第一部分 基础知识

## 二，线程安全性

多个线程访问同一个可变的状态变量时，出现问题修复方式：

1. 不在线程之间共享该状态变量
2. 将状态变量修改为不可变的变量
3. 在访问状态变量时使用同步

### 2.1 什么是线程安全

当多个线程访问某个类时，不管运行时环境采用**何种调度方式**或者这些线程将**如何交替执行**，
并且在主调用代码中**不需要任何额外的同步或者协同**，这个类都能表现出正确的行为，
那么就称这个类是线程安全的。

> 在线程安全类中封装了必要的同步机制，因此客户端无须进一步采取同步措施

**无状态对象一定是线程安全的**

无状态：既不包含任何域，也不包含任何对其他类中域的引用。

### 2.2 原子性

#### 2.2.1 竞态条件

>当某个计算的正确性取决于多个线程的交替执行时序时，那么就会发生竞态条件。

最常见的竞态条件类型就是

- “先检查后执行”操作
- "读取－修改－写入"

#### 2.2.2 示例：延迟初始化中的竞态条件

#### 2.2.3 复合操作

### 2.3 加锁机制

> 要保持状态的一致性，就需要在单个原子操作中更新所有相关的状态变量。

#### 2.3.1 内置锁

每个Java对象都可以用做一个实现同步的锁，这些锁被称为内置锁。

#### 2.3.2 重入

如果某个线程试图获得一个已经由它自己持有的锁，那么这个请求就会成功。   
**“重入”意味着获取锁的操作的粒度是“线程”，而不是“调用”**

如果内置锁不是可重入的，那么下面这段代码将发生死锁

~~~ java
public class Widget {
	public synchronized void doSomething() {
		...
	}
}

public class LogginWidget extends Widget {
	public synchronized void doSomething() {
		System.out.println(toString() + ": calling doSomething");
		super.doSomething();
	}
}
~~~

### 2.4 用锁来保护状态

- 对于可能被多个线程同时访问的可变状态变量，在访问它时都需要持有同一个锁，在这种情况下，我们称状态变量时由这个锁保护的。
- 每个共享的和可变的变量都应该只由一个锁来保护，从而使维护人员知道是哪一个锁。例如：```Vector```
- 对于每个包含多个变量的不变性条件，其中涉及的所有变量都需要由同一个锁来保护。

### 2.5 活跃性与性能

- 通常，在简单性与性能之间存在着互相制约因素。当实现某个同步策略时，一定不要盲目地为了性能而牺牲简单性（这可能会破坏安全性）
- 当执行时间较长的计算或者可能无法快速完成的操作时（例如，网络I/O或控制台I/O），一定不要持有锁。

## 三，对象的共享

### 3.1 可见性

**只要有数据在多个线程之间共享，就使用正确的同步**

#### 3.1.1 失效数据

getter and setter 没有使用同步

#### 3.1.2 非原子的64位操作

非volatile类型的64位数值变量（double和long），JVM允许将64位的读操作或写操作分解为两个32位的操作。在多线程程序中使用共享且可变的long和double等数据类型变量也是不安全的，除非是用关键字```volatile```来声明它们，或者用锁保护起来。

#### 3.1.3 加锁与可见性

加锁的含义不仅仅局限于互斥行为，还包括**内存可见性**。为了确保所有线程都能看到共享变量的最新值，**所有执行读操作或者写操作的线程都必须在同一个锁上同步**。

#### 3.1.4 Volatile变量

含义：   

- volatile变量不会被缓存在寄存器或者对其他处理器不可见的地方，因此在读取volatile类型的变量时总会返回最新写入的值。    
- 写入volatile变量相当于退出同步代码块，而读取volatile变量相当于进入同步代码块

用法：

- 确保它们自身状态的可见性
- 确保它们所引用对象的状态的可见性
- 标示一些重要的程序生命周期事件的发生（例如初始化或关闭）

检查某个状态标记已判断是否退出循坏。   

**加锁机制既可以确保可见性又可以确保原子性，而volatile变量只能确保可见性**

当且仅当满足以下所有条件时，才应该是用volatile变量：

- 对变量的写入操作不依赖变量的当前值，或者你能确保只有单个线程更新变量的值
- 该变量不会与其它状态变量一起纳入不变形条件中
- 在访问变量时不需要加锁

### 3.2 发布与逸出

不要在构造过程中使用this引用逸出。

常见错误：

- 在构造函数中启动一个线程

### 3.3 线程封闭

#### 3.3.1 Ad-hoc线程封闭

Ad-hoc线程封闭是指，维护线程封闭性的职责完全由程序实现来承担。

#### 3.3.2 栈封闭

栈封闭是线程的一种特例，在栈封闭中，只能通过局部变量才能访问对象。

#### 3.3.3 ThreadLocal类

ThreadLocal提供了get和set等访问接口或方法，这些方法为每个使用该变量的线程都存有一份独立的副本。    

用法：

- ThreadLocal对象通常用于防止对可变的单实例变量（Singleton）或全局变量进行共享。
- 当某个频繁执行的操作需要一个临时对象，例如一个缓冲区，而同时又希望避免在每次执行时都重新分配该临时对象。

ThreadLocal变量类似于全局变量，它能降低代码的可重用行，并在类之间引入隐含的耦合性，因此在使用时要格外小心。

### 3.4 不变性

不可变对象一定是线程安全的

#### 3.4.1 Final域

#### 3.4.2 示例：使用Volatile类型来发布不可变对象

使用初始化后内部变量不可改变的对象和volatile保证线程安全

### 3.5 安全发布

#### 3.5.1 不正确的发布：正确的对象被破坏

#### 3.5.2 不可变对象与初始化安全性

满足不可变性的所有需求：   
状态不可修改，所有域都是final类型，以及正确的构建过程

#### 3.5.3 安全发布的常用模式

要安全地发布一个对象，对象的引用以及对象的状态必须同时对其它线程可见。一个正确构造的对象可以通过以下方式来安全地发布：

- 在静态初始化函数中初始化一个对象引用
- 将对象的引用保存到volatile类型的域或者AtmoicReferance对象中
- 将对象的引用保存到某个正确构造对象的final类型域中
- 将对象的引用保存到一个由锁保护的域中

#### 3.5.4 事实不可变对象

如果对象从技术上来看是可变的，单其状态发布后不会再改变，那么把这种对象称为“事实不可变对象”

#### 3.5.5 可变对象

对象的发布需求取决于它的可变性：

- 不可变对象可以通过任意机制来发布
- 事实不可变独享必须通过安全方式来发布
- 可变对象必须通过安全方式来发布，并且必须是线程安全或者由某个锁保护起来

#### 3.5.6 安全地共享对象

在并发程序中使用和共享对象时，可以使用一些实用的策略：

- **线程封闭**。线程封闭的对象只能由一个线程拥有，对象被封闭在该线程中，并且只能由这个线程修改。
- **只读共享**。在没有额外同步的情况下，共享的只读对象可以由多个线程并发访问，但任何线程都不能修改它。共享的只读对象包括不可变对象和事实不可变对象。
- **线程安全共享**。线程安全的对象在其内部实现同步，因此多个线程可以通过对象的公有接口来进行访问而不需要进一步的同步。
- **保护对象**。被保护的对象只能通过持有特定的锁来访问。保护对象包括封装在其它线程安全对象中的对象，以及已发布的并且由某个特定锁保护的对象。

## 四，对象的组合

### 4.1 设计线程安全的类

在设计线程安全类的过程中，需要饱含以下三个基本要素：

- 找出构成对象状态的所有变量
- 找出约束状态变量的不变性条件
- 建立状态对象的并发访问管理策略

#### 4.1.1 收集同步需求

#### 4.1.2 依赖状态的操作

#### 4.1.3 状态的所有权

### 4.2 实例封闭

将数据封装在对象内部，可以将数据的访问限制在对象的方法上，从而更容易确保线程在访问时总能持有正确的锁。

#### 4.2.1 Java监视器模式

### 4.3 线程安全性的委托

#### 4.3.2 独立的状态变量

CopyOnWriteArrayList是一个线程安全的列表，特别适用于管理监听器列表

#### 4.3.3 当委托失效时

### 4.4 在现有的线程安全类中添加功能

#### 4.4.2 组合

当为鲜有的类添加一个原子操作时，有一种更好的方法：组合。

~~~ java
@ThreadSafe
public class ImprovedList<T> implements List<T>{

	private final List<T> list;
	
	public ImprovedList(List<T> list) {
		this.list = list;
	}
	
	public synchronized boolean putIfAbsent(T x) {
		boolean contains = list.contains(x);
		if (contains) 
			list.add(x);
		return !contains;
	}
	
	...
}
~~~

## 五，基础构件模块

### 5.1 同步容器类

#### 5.1.2 迭代器和ConcurrentModificationException

如果不希望在迭代期间对容器加锁，那么一种替代方法就是“克隆”容器。

#### 5.1.3 隐藏迭代器

~~~ java
public class HiddenIterator {

	@GuardedBy("this")
	private final Set<Integer> set = new HashSet<Integer>();
	
	public synchronized void add(Integer i) {
		set.add(i);
	}
	public void addTenThings() {
		Random r = new Random();
		for (int i = 0 ; i < set.size(); i++) {
			add(r.nextInt());
		}
		System.out.println("DEBUG: added ten elements to " + set);
	}
}
~~~

addTenThings()方法可能会抛出ConcurrentModificationException。因为```toString```对容器进行迭代。    

容器的```hashCode```和```equals```等发放也会渐渐的执行迭代操作。

### 5.2 并发容器

#### 5.2.1 ConcurrentHashMap 

特点：

- 采用分段锁
- 不需要在迭代过程中对容器加锁
- 弱一致性(size和isEmpty返回的结果在计算时可能已经过期了)

只有当应用程序需要加锁Map以进行独占访问时，才应该放弃使用ConcurrentHashMap.

#### 5.2.2 CopyOnWriteArrayList

仅当迭代操作远远多余修改操作时，才应该使用“写入时复杂”容器

### 5.3 阻塞队列和生产者 － 消费者模式

#### 5.3.3 双端队列与工作密取

- Deque -> ArrayDeque
- BlockingDeque -> LinkedBlockingDeque

工作密取非常适用于既是消费者也是生产者的问题

### 5.4 阻塞方法与中断方法

对于库代码来说，有两种选择：

- 传递InterruptedException
- 恢复中断 

~~~ java
Thread.currentThread().interrupt();
~~~

### 5.5 同步工具类

#### 5.5.1 闭锁

CountDownLatch   
一次性对象，一旦进入终止状态，就不能被重置。   
用于等待事件

~~~ java
public class TestHarness {

	public long timeTasks(int nThreads, final Runnable task) throws InterruptedException {
		
		final CountDownLatch startGate = new CountDownLatch(1);
		final CountDownLatch endGate = new CountDownLatch(nThreads);
		
		for (int i = 0; i < nThreads; i++) {
			Thread t = new Thread(){
				@Override
				public void run() {
					try {
						startGate.await();	
						try {
							task.run();
						} finally {
							endGate.countDown();
						}
					} catch (InterruptedException ignored) {
						
					}
					
				}
			};
			t.start();
		}
		
		long start = System.nanoTime();
		startGate.countDown();
		endGate.await();
		long end = System.nanoTime();
		return end - start;
	}
}
~~~

#### 5.5.2 FutureTask

#### 5.5.3 信号量

计数信号量用来控制同时访问某个特定资源的操作数量，或者同时执行某个特定操作的数量。   

Semaphore

#### 5.5.4 栅栏

用于等待其它线程     

- CyclicBarrier     
- Exchanger

# 第二部分 结构化并发应用程序

## 六，任务执行

6.2 Executor框架

#### 6.2.3 线程池

- newFixedThreadPool
- newCachedThreadPool
- newSingleThreadExecutor
- newScheduledThreadPool

#### 6.2.5 线程池

