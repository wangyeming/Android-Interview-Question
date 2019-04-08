1.	Activity生命周期
2.	Activity横竖屏切换的生命周期
3.	对话框出现时activity的生命周期
4.	线程间通信
5.	进程间通信
6.	dex文件是啥，aar包里面是dex还是class
7.	looper里面消息队列如何实现
8.	handler机制
9.	sdk如何优化体积
10.	activity 启动模式，singleTask和singleTop是怎么个原理
11.	安卓中插件化框架的介绍
12.	service的生命周期
13.	线程和进程的区别
14.	进程的内存模型
15.	自定义view的流程，onMeasure, onLayout的作用
16.	如何保证多线程持有同一变量互相不影响


# Activity生命周期

从Activity启动到最终Activity被销毁，总共经历生命周期：

onCreate() -> onStart() -> onResume() -> onPause() -> onStop() -> onDestroy()

onResume()和onPause()决定了Activity是否在前台
onStart()和onStop()决定了Activity是否可见
onStop()之后，activity重新恢复到前台，onRestart()方法

# 横竖屏切换对Activity生命周期的影响

不设置Activity的android:configChanges，或设置Activity的android:configChanges="orientation"，或设置Activity的android:configChanges="orientation|keyboardHidden"，切屏会重新调用各个生命周期，切横屏时会执行一次，切竖屏时会执行一次。

配置 android:configChanges="orientation|keyboardHidden|screenSize"，才不会销毁 activity，且只调用 onConfigurationChanged方法。

# Activity保存状态

onSaveInstanceState() 和 onRestoreInstanceState()

# Fragment生命周期

