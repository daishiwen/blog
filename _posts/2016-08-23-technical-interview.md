---
layout: post
category: 技术
tags: [Java, Android, 面试]
title: 技术面试（Android方向）
header: no
show: true
---

一转眼三个月过去了，没有产出一篇博客的三个月。大抵是忙在Joy-Library上了...

作为一个Android工程师面试之前应该掌握哪些技能呢？或者说我们应该具备哪些技能才能称为一个优秀的工程师呢？

### 专业技能

- **Java**

	毋庸多说，Java作为基本功，必须达到熟练甚至精通的程度。

	1. 基本语法

		> static、final、volatile关键字  
		> for、foreach、while、do-while循环  
		> 接口、抽象类、内部类、静态内部类  
		>

	2. 集合

		> List、Map、Set  
		> ArrayList、LinkedList、Hashtable、HashMap、ConcurrentHashMap、HashSet的实现原理  
		>

	3. 多线程

		> Thread和Runnable的区别和联系  
		> 多次start一个线程会怎样  
		> 线程有哪些状态  
		> 线程池的实现原理是怎样的  
		> 造成死锁的原因及排查方法  
		> synchronized和ReentrantLock的区别  
		> synchronized锁普通方法和锁静态方法的区别

	4. IO

		> 阻塞IO、非阻塞IO、多路复用IO、异步IO这四种IO模型  
		> 阻塞/非阻塞的区别  
		> NIO的原理、NIO属于哪种IO模型、NIO的三大组成

	5. Java虚拟机

		> Java虚拟机的内存布局  
		> GC算法及几种垃圾收集器  
		> Java内存模型  
		> happens-before规则  
		> volatile关键字使用规则

	6. 设计模式

		> 23种设计模式  
		> 你的项目中用到了哪些设计模式，如何使用  
		> 知道常用设计模式的优缺点

	7. 数据结构和算法分析

		> 数组、链表  
		> 栈和队列  
		> 树：AVL树、红黑树  
		> [排序算法](http://blog.csdn.net/qy1387/article/details/7752973)（插入排序、希尔排序、快速排序、冒泡排序、选择排序、堆排序、归并排序、基数排序）  
		> [二分查找算法](./BinarySearch-Sample)

- **Android**

	1. Activity => A => B，完整的生命周期

		> A: onCreate onStart onResume onPause  
		> B: onCreate onStart onResume  
		> A: onSaveInstanceState onStop

	2. 从B返回A

		> B: onPause  
		> A: onRestart onStart onResume  
		> B: onStop onDestroy

	3. Activity启动过程  <small>[(参考)](http://www.jianshu.com/p/6037f6fda285)</small>
	4. Activity、Window、View之间的联系  <small>[(参考)](http://www.jianshu.com/p/687010ccad66)</small>

		> Activity像一个工匠（控制单元），Window像窗户（承载模型），View像窗花（显示视图）  
		> ActivityThread.performLaunchActivity() => Activity.attach() => new PhoneWindow()  
		> Activity.setContentView() => Window.setContentView() => generate mDecor ***(new DecorView)*** => generate mContentParent ***(findViewById)***

	5. View绘制流程  <small>[(参考一)](http://www.jianshu.com/p/3299c3de0b7d)</small>  <small>[(参考二)](http://www.jianshu.com/p/836bfdc36407)</small>  <small>[(参考三)](http://www.jianshu.com/p/3e064c045f0f)</small>

		> ViewRootImpl.performTraversals() => performMeasure()、performLayout()、performDraw()  
		> 测量(Measure)  
		> 布局(Layout)  
		> 绘制(Draw)

	6. View.requestLayout()、invalidate()、postInvalidate()  <small>[(参考)](http://www.jianshu.com/p/effe9b4333de)</small>

		> ***requestLayout:*** 子View调用这个方法，会标记当前View及父容器，同时逐层向上提交，直到ViewRootImpl处理该事件，ViewRootImpl会调用三大流程，从measure开始，对每一个含有标记位的View及其子View都会进行测量、布局、绘制。  
		> ***invalidate:*** 调用该方法会引起View树重绘，常用于内部调用（如: View.setVisibility）或者需要刷新界面的时候，需要在主线程中调用。  
		> ***postInvalidate:*** 这个方法与invalidate的作用是一样的，都是使View树重绘，但两者的使用条件不同，postInvalidate是在非UI线程中调用，invalidate则是在UI线程中调用。

	7. Touch事件传递流程和分发机制  <small>[(参考一)](http://www.jianshu.com/p/8236278676fe)</small>  <small>[(参考二)](http://www.jianshu.com/p/1378b334ee85)</small>

		> `ViewGroup`  
		> ***dispatchTouchEvent:*** 用来进行事件分发，无论ViewGroup还是View的事件都是从这个方法开始的。  
		> ***onInterceptTouchEvent:*** 在onInterceptTouchEvent中调用，表示是否拦截当前事件，返回true表示拦截，将不会把事件分发给子View。  
		> ***onTouchEvent:*** 在onInterceptTouchEvent中调用，如果返回true表示消费当前事件，false表示不消耗当前事件。  
		> `View`  
		> ***dispatchTouchEvent***  
		> ***onTouchEvent***

	8. IPC机制(Inter-Process Communication)  <small>[(参考一)](http://www.jianshu.com/p/3f6932db9963)</small>  <small>[(参考二)](http://www.jianshu.com/p/b9b15252b3d6)</small>  <small>[(参考三)](http://www.jianshu.com/p/b96713fc4e5e)</small>  <small>[(参考四)](http://www.jianshu.com/p/6e23037d6d20)</small>

		> 序列化与反序列化  
		> AIDL  
		> Binder  
		> Bundle与Messenger

	9.  LruCache的原理及Lru算法
	10. IntentService
	11. Handler、AsyncTask、Thread、HandlerThread、ThreadLocal

		> ***HandlerThread:*** HandlerThread自带Looper使它可以通过消息机制重复使用线程，节省开支。  [示例](./HandlerThread-Sample)

	12. 开源组件（熟练使用+熟悉原理）

		> RxJava、RxAndroid  
		> Retrofit、OkHttp、Volley  
		> Fresco、Glide、Picasso  
		> Dagger2  
		> EventBus3  
		>
		> Okio（了解）  
		> LeakCanary（会用）

	13. 架构

		> MVC  
		> MVP  
		> MVVM  
		> Clean Architecture

	14. 开源项目

		> [android-architecture](https://github.com/googlesamples/android-architecture)  
		> [android-boilerplate](https://github.com/hitherejoe/Android-Boilerplate)  
		> [Android-CleanArchitecture](https://github.com/android10/Android-CleanArchitecture)  
		> [GithubClient](https://github.com/frogermcs/GithubClient)

	15. Android Studio、Gradle相关
	16. Git相关

持续更新...
