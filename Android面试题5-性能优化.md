1. ANR的现象，原因和分析解决方案
2. 内存泄露的原因和分析解决方案

# ANR的现象，原因和分析解决方案

1. ANR全称：Application Not Responding，也就是应用程序无响应。如果App在前台，系统会提示一个对话框给用户，让用户有机会强制杀死App进程。
2. Android系统中，ActivityManagerService(简称AMS)和WindowManagerService(简称WMS)会检测App的响应时间，如果App在特定时间无法响应屏幕触摸或键盘输入事件，或者特定事件没有处理完毕，就会出现ANR。
3. 触发ANR的情况：
	App在前台的话，5秒内无法响应屏幕触摸事件或键盘输入事件，或者广播事件。
	App在后台的话，广播处理事件的时间过长也会导致ANR。

	input事件前后台5s
	Porvider事件前后台10s
	Broadcast前台10s，后台60s
	Service前台20s，后台200s
4. 导致ANR可能的原因
	App在主线程执行耗时的IO操作
	App在主线程进行复杂计算
	App在主线程等待其它进程的同步binder回调时，其它进程返回时长很久
	App主线程等待其它线程的synchronized锁
5. 调试方法
	分析ANR的Log，重要进程的各个线程的信息保存在：/data/anr/traces.txt
	traces文件和CPU使用情况保存在 /data/system/dropbox目录
	应用开启严格模式StrictMode
	开发者选项里开始后台ANR对话框
	利用Traceview分析耗时
6. 发生ANR的Trace信息主线程是空闲状态或者停留的位置是非耗时的原因可能是什么？	可能是抓取trace过去耗时而错过现场
	可能是主线程消息队列堆积了大量消息而最后抓取快照一刻只是瞬时状态，可以是广播的「queued-work-looper」一直在处理sp操作。	

# 内存泄露的原因和分析解决方案

内存泄露是指用动态存储分配函数动态开辟的空间，在使用完毕后未释放，结果导致一直占据该内存单元。直到程序结束。即所谓的内存泄漏。也就是内存空间使用完毕后没有及时回收。Java虚拟机的垃圾回收机制方便了Java开发者的同时，日常开发也不可避免的遇到内存泄露的问题。Java采用引用计数的方式标记和操作对象，Java虚拟机会定期通过对象的引用计数和可达性分析来标记对象是否需要回收，来回收那些无引用的Java对象。简单的例子，如果程序退出后某处还持有某个需要被回收的对象的引用，那么就造成了内存泄露，根据时间的长度和泄露内存的大小，影响程序性能甚至导致OOM。

Android中常见的内存泄露的例子,比如说Activity的context被静态变量，单例等持有导致退出页面后，Activity资源得不到回收释放。

分析内存泄露的方式有以下几种：
1. Debug版本的应用包可以考虑集成内存泄露检测工具,例如Apache开源的LeakCanary
2. 导出和分析hprof(Heap Profilling)文件，可以通过adb shell am dumpyheap命令导出，也可以通过Android Studio自带的Android Profile工具中的内存管理工具导出。导出后的hprof文件可以通过Android Studio浏览和分析具体对象的内存占用和引用信息。