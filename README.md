# Android常见面试题汇总

一份适合国内Android开发者的面试题合集, 整理成问答的方式。
你可以把它看成一份考纲，适合于已经了解具体知识点想要参考如何表述，或者曾经了解但是时间久远有些遗忘了的情况。

如果你想更加详细的了解某些题目的原理，可以跳转到下面这个工程：

[JsonChao/Awesome-Android-Interview](https://github.com/JsonChao/Awesome-Android-Interview)

南瓜镇楼！

![](./img/南瓜01.jpg)


## 提纲

本工程收集整理了Android面试中常见的面试题，包括以下几个大类

* 技术相关
    * Android相关
        * Android基础题
        * Android进阶题
        * Android中的开源库
        * gradle相关

    * Java语言
        * Java基础题
        * Java并发相关题
        * JVM Java虚拟机相关题

    * 计算机基础
        * 网络相关基础题
        * 操作系统相关

    * 数据结构与算法

* 非技术相关
    * 职业规划相关
    * 薪资福利相关
    * 其它


其中算法题目部分，可以跳转到下面这个˙地址：

([田野光的算法学习笔记](https://github.com/wangyeming/AlgorithmNote))

## 目录

### Android相关

* [Android基础题-四大组件](/Android/Android-四大组件.md)

1.	Activity和Fragment的生命周期
2.	Activity横竖屏切换的生命周期
3.	对话框出现时Activity的生命周期
4.  通知栏下滑时Activity的生命周期
5.  Activity如何保存状态
6.  Activity的启动模式
7.  跨应用启动的Activity，位于哪个栈中？
8.  Service生命周期 
9.  IntentService的作用
10. LocalBroadcast原理
11. bundle的数据结构，如何存储，既然有了Intent.putExtra，为什么还要用bundle？
12. Serializable和Parcelable区别
13. 不同应用可以存在于同一进程吗？

* [Android基础题-View](/Android/Android-View.md)

1. MeasureSpec的理解
2. View的工作流程
3. 如何自定义View
4. View事件分发机制
5. View的滑动冲突处理

* [Android基础题-动画](/Android/Android-动画.md)

1. Android中动画的种类
2. 来源库Lottie原理

* [Android基础题-Window](/Android/Android-Window.md)

1. Window的作用
2. Window的创建，更新和删除
3. Activity的Window创建过程

* [Android基础题-线程进程类](/Android/Android-线程与进程.md)

1.	线程和进程的区别
2.	Android中线程间通信
3.	handler机制, Looper里面消息队列如何实现
4.	在Java中，如何保证多线程持有同一变量互相不影响
5.	Android中进程间通信
6.  以AIDL为例，说明客户端和服务端建立远程通信的步骤。

* [Android基础题--WebView](/Android/Android-WebView.md)

1. Android和WebView的JS如何通信

* [Android进阶题-项目工程](/Android/Android-项目工程.md)

1. 介绍一下Dex文件
2. SDK如何优化体积
3. 从点击桌面应用图标，到最终启动App，中间发生了什么事？
4. 大体说清一个应用程序安装到手机上时发生了什么？
5. apk文件的组成
6. 一个图片在app中调用R.id后是如何找到的

* [Android进阶题-性能优化](/Android/Android-性能优化.md)

1. ANR的现象，原因和分析解决方案
2. 内存泄露的原因和分析解决方案
3. Bitmap占用内存的大小
4. 如何优化Bitmap的加载？
5. 如何定位和优化滑动卡顿？

* [Android中的开源库](/Android/Android-开源库.md)

1. React Natvie如何封装Android的方法和自定义View？
2. LeakCanary的原理
3. Lottie的原理

### Java语言

* [Java基础问题](/Java/Java语言-Java基础问题.md)

1. Java中的强引用，软引用，弱引用，虚引用
2. Java中对象的生命周期
3. Java的注解

* [Java并发相关](/Java/Java语言-Java并发相关.md)

1. Java单例的实现
2. Java线程的六种状态
3. synchronized关键字
4. volatile关键字
5. 线程池的理解
6. Java的等待/通知模型
7. Copy-On-Write容器
8. 重入锁以及锁的公平性

* [JVM相关](/Java/Java语言-JVM相关.md)

1. 介绍一下Class文件
2. Java虚拟机运行时的数据区
3. 双亲委派模型

* [HashMap相关](/Java/Java语言-HashMap相关.md)

1. 什么时候会使用HashMap？它有什么特点？
2. HashMap的工作原理
3. get和put的原理, equals()和hashCode()的都有什么作用？
4. hash的实现及原因？
5. 如果HashMap的大小超过了负载因子(load factor)定义的容量，怎么办？

## 计算机基础

* [网络基础知识](/common-basic/网络基础知识.md)

1. GET和POST的区别
2. Cookie和Session的区别
3. Http和Https的区别
4. TCP和UDP的区别
5. TCP建立连接和断开连接的过程
6. TCP/IP协议的含义和分层
7. 浏览器输入URL后发生了什么





