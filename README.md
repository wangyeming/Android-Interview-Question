# Android常见面试题汇总

目前总计有67道题

## [Android面试题1-四大组件类](Android面试题1-四大组件类.md)

1.	Activity生命周期
2.	Activity横竖屏切换的生命周期
3.	对话框出现时activity的生命周期
4.  通知栏下滑时activity的生命周期
5.  Activity如何保存状态
6.  activity 启动模式
7.  Fragment生命周期
8.  Service生命周期 
9.  bundle的数据结构，如何存储，既然有了Intent.putExtra，为什么还要用bundle？
10. Serializable和Parcelable区别
11. 不同应用可以存在于同一进程吗？
12. 跨应用启动的Activity，位于哪个栈中？
13. LocalBroadcast原理

## [Android面试题2-View类](Android面试题2-View类.md)

1. MeasureSpec的理解
2. View的工作流程
3. 如何自定义View
4. View事件分发机制
5. View的滑动冲突处理

## [Android面试题3-动画类](Android面试题3-动画类.md)

1. Android中动画的种类
2. 来源库Lottie原理

## [Android面试题4-window类](Android面试题4-window类.md)

1. Window的作用
2. Window的创建，更新和删除
3. Activity的Window创建过程

## [Android面试题5-线程进程类](Android面试题5-线程进程类.md)

1.	线程和进程的区别
2.	线程间通信
3.	handler机制, Looper里面消息队列如何实现
4.	如何保证多线程持有同一变量互相不影响
5.	进程间通信
6.  以AIDL为例，说明客户端和服务端建立远程通信的步骤

## [Android面试题6-项目工程类](Android面试题6-项目工程类.md)

1. 介绍一下Dex文件
2. SDK如何优化体积
3. 从点击桌面应用图标，到最终启动App，中间发生了什么事？
4. 大体说清一个应用程序安装到手机上时发生了什么？
5. apk文件的组成
6. 一个图片在app中调用R.id后是如何找到的

## [Android面试题7-性能优化](Android面试题7-性能优化.md)

1. ANR的现象，原因和分析解决方案
2. 内存泄露的原因和分析解决方案
3. Bitmap占用内存的大小
4. 如何优化Bitmap的加载？

## [Android面试题8-Java并发相关](Android面试题8-Java并发相关.md)

1. Java单例的实现
2. Java线程的六种状态
3. synchronized关键字
4. volatile关键字
5. 线程池的理解
6. Java的等待/通知模型
7. Copy-On-Write容器
8. 重入锁以及锁的公平性


## [Android面试题9-JVM相关](Android面试题9-JVM相关.md)

1. 介绍一下Class文件
2. Java虚拟机运行时的数据区
3. 双亲委派模型

## [Android面试题10-Java基础问题](Android面试题10-Java基础问题.md)

1. Java中的强引用，软引用，弱引用，虚引用
2. Java中对象的生命周期

## [Android面试题11-网络基础知识](Android面试题11-网络基础知识.md)

1. GET和POST的区别
2. Cookie和Session的区别
3. Http和Https的区别
4. TCP和UDP的区别
5. TCP建立连接和断开连接的过程
6. TCP/IP协议的含义和分层
7. 浏览器输入URL后发生了什么

## [Android面试题12-WebView与JS交互](Android面试题12-WebView与JS交互.md)

1. Android和WebView的JS如何通信

## [Android面试题13-HashMap相关](Android面试题13-HashMap相关.md)

1. 什么时候会使用HashMap？他有什么特点？
2. HashMap的工作原理
3. get和put的原理, equals()和hashCode()的都有什么作用？
4. hash的实现及原因？
5. 如果HashMap的大小超过了负载因子(load factor)定义的容量，怎么办？

## [Android面试题14-开源库](Android面试题14-开源库.md)

1. React Natvie如何封装Android的方法和自定义View？
2. LeakCanary的原理