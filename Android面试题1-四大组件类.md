四大组件类问题
1.	Activity生命周期
2.	Activity横竖屏切换的生命周期
3.	对话框出现时activity的生命周期
4.  通知栏下滑时activity的生命周期
5.  Activity如何保存状态
6.  activity 启动模式
7.  Fragment生命周期
8.  Service生命周期 

# 生命周期类
## Activity生命周期

从Activity启动到最终Activity被销毁，总共经历生命周期：

onCreate() -> onStart() -> onResume() -> onPause() -> onStop() -> onDestroy()

onResume()和onPause()决定了Activity是否在前台
onStart()和onStop()决定了Activity是否可见
onStop()之后，activity重新恢复到前台，会调onRestart()方法

## 横竖屏切换对Activity生命周期的影响

不设置Activity的android:configChanges，或设置Activity的android:configChanges="orientation"，或设置Activity的android:configChanges="orientation|keyboardHidden"，切屏会重新调用各个生命周期，切横屏时会执行一次，切竖屏时会执行一次。

配置 android:configChanges="orientation|keyboardHidden|screenSize"，才不会销毁 activity，且只调用 onConfigurationChanged方法。

## 对话框出现时activity的生命周期

没有影响,原理都是windowmanager.addView()来添加的

## 通知栏下滑时activity的生命周期

没有影响

## Activity保存状态

onSaveInstanceState() 和 onRestoreInstanceState()

## Activity 启动模式

task是一组相互关联的activity的集合，它是存在于framework层的一个概念，控制界面的跳转和返回。这个task存在于一个称为back stack的数据结构中，也就是说，framework是以栈的形式管理用户开启的activity。这个栈的基本行为是，当用户在多个activity之间跳转时，执行压栈操作，当用户按返回键时，执行出栈操作。

Activity有四种启动模式，分别为standard，singleTop，singleTask，singleInstance。

**standard** 

在这种模式下启动的activity可以被多次实例化，即在同一个Task中可以存在多个activity的实例,每个实例都会处理一个Intent对象。

**singleTop**

如果一个以singleTop模式启动的activity的实例已经存在于任务桟的桟顶，那么再启动这个Activity时，不会创建新的实例，而是重用位于栈顶的那个实例，并且会调用该实例的onNewIntent()方法将Intent对象传递到这个实例中.如果以singleTop模式启动的activity的一个实例已经存在与任务桟中，但是不在桟顶，那么它的行为和standard模式相同，也会创建多个实例。

**singleTask**

在任务栈中会判断是否存在相同的activity，如果存在，那么会清除该activity之上的其他activity对象显示，如果不存在，则会创建一个新的activity放入栈顶,并且调用他的onNewIntent()方法。

**singleInstance**

总是在新的任务中开启，并且这个新的任务中有且只有这一个实例，也就是说被该实例启动的其他activity会自动运行于另一个任务中。当再次启动该activity的实例时，会重用已存在的任务和实例。并且会调用这个实例的onNewIntent()方法，将Intent实例传递到该实例中。和singleTask相同，同一时刻在系统中只会存在一个这样的Activity实例。

## Fragment生命周期

onAttach() -> onCreate() -> onCreateView() -> onActivityCreated() -> onStart() -> onResume() -> onPause() -> onStop() -> onDestroyView() -> onDestroy() -> onDetach()

## Service生命周期

Service四个手动调用的方法 startService() stopService() bindService() unBindService()
Service五个内部自动调用的方法 onCreate() onStartCommand() onDestroy() onBind() onUnbind()

![](/img/Service生命周期-startService.png)

![](/img/Service生命周期-stopService.png)

![](/img/Service生命周期-bindService.png)

![](/img/Service生命周期-unbindService.png)


startService开启的Service，调用者退出后Service仍然存在； 
bindService开启的Service，调用者退出后，Service随着调用者销毁。