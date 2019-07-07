# Android中的开源库

## 目录

1. React Natvie如何封装Android的方法和自定义View？
2. LeakCanary的原理
3. Lottie的原理

## React Natvie如何封装Android的方法和自定义View？

对于Android的方法，React Natvie提供了NativeModule接口，例如我们可以继承它的默认抽象实现ReactContextBaseJavaModule，通过对方法加上@ReactMethod的注解的形式，给RN提供Java接口。

对于自定义View, React Natvie提供了ViewManager接口，例如我们可以继承它的默认抽象实现SimpleViewManager类，提供实现createViewInstance()方法创建并返回自定义View的实例。此外，我们也可以对方法加上@ReactProp注解并指定name，从而让RN开发的时候可以设置自定义View的属性。

## LeakCanary的原理

LeakCanary是由square公司开源的内存泄露检测分析工具，可以用于自动监听和检测Activiy对象内存泄露，也可以手动监听具体的对象的内存泄露。

本质上需要解决两个问题：
1. 如何知道对象是否被回收了？
2. 如果对象没有被回收，如何收集信息，找出谁持有了对象的强引用，导致对象没有被回收。

先说第一个问题，如何知道对象是否被回收

借助于手动GC + ReferenceQueue + WeakReference

基于的原理是创建对象的弱引用时，指定引用队列。当对象的可达性发生变化时，系统GC会讲对象插入到应用队列中。

而第二个问题，LeakCanary借助的是haha这个库，同样是由square开源的 Android 堆分析库，分析hprof文件生成Snapshot对象，用以查询对象的最短引用路径。

我们以debug模式下，LeakCanary自动检测Activity对象为例，具体过程：

1. 在Application中注册一个ActivityLifecycleCallbacks来监听Activity的销毁。
2. 在Activity销毁的回调时，将Activity封装成弱引用对象。并传入引用队列。
3. 首先检查Activity是否回收了。如果没有，在子线程中强制触发系统的gc(垃圾回收), 此时如果Activity对象从强可达状态变为弱可达状态，那么弱引用的Activity对象应该会自动插入到引用队列中。(leakCanary会等待100毫秒，确保activity会插入到引用队列。)
4. 如果Activity对象没有被插入到引用队列当中，也就是Activity对象没有被回收，此时会通过堆分析服务(HeapAnalyzerService),通过分析java堆的信息，生成堆信息文件，并通过haha来找出对象的最短引用链。
5. 最后通过回调DisplayLeakService，来进行前台通知的UI展示等。

## Lottie的原理

lottie由Airbnb推出的移动端动画实现框架。Lottie使用json文件来作为动画数据源, 这个文件作用是把图片中的元素进行来拆分，并且描述每个元素的动画执行路径和执行时间。Lottie的功能就是读取这些数据，然后绘制到屏幕上。

Lottie 提供了一个 LottieAnimationView 给用户使用，而实际 Lottie 的核心是 LottieDrawable，它承载了所有的绘制工作，LottieAnimationView则是对LottieDrawable 的封装，再附加了一些例如 解析 的功能。