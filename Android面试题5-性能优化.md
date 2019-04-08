1. ANR的现象，原因和分析解决方案
2. 内存泄露的原因和分析解决方案

# ANR的现象，原因和分析解决方案

1. ANR全称：Application Not Responding，也就是应用程序无响应。如果App在前台，系统会提示一个对话框给用户，让用户有机会强制杀死App进程。
2. Android系统中，ActivityManagerService(简称AMS)和WindowManagerService(简称WMS)会检测App的响应时间，如果App在特定时间无法相应屏幕触摸或键盘输入时间，或者特定事件没有处理完毕，就会出现ANR。
3. 触发ANR的情况：
	App在前台的话，5秒内无法响应屏幕触摸事件或键盘输入事件，或者广播事件。
	App在后台的话，广播处理事件的时间过长也会导致ANR。
4. 导致ANR可能的原因
	App在主线程执行耗时的IO操作
	App在主线程进行复杂计算
	App在主线程等待其它进程的同步binder回调时，其它进程返回时长很久
	App主线程等待其它线程的synchronized锁
5. 调试方法
	分析ANR的Log，信息保存在：/data/anr/traces.txt
	应用开启严格模式StrictMode
	开发者选项里开始后台ANR对话框
	利用Traceview分析耗时	
