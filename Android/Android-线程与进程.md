# Android基础题-线程与进程

这里线程和进程的概念可以划分到 计算机基础/操作系统相关, 而如何保证多线程持有同一变量互相不影响，可以划分到Java语言/Java并发相关题

## 目录

1.	Android中线程间通信
2.	handler机制, Looper里面消息队列如何实现
3.	在Java中，如何保证多线程持有同一变量互相不影响
4.	Android中进程间通信
5.  以AIDL为例，说明客户端和服务端建立远程通信的步骤。

## 线程间通信

* 主要推荐Handler机制
* 广播
* 共享内存
* 文件/数据库
* 传统的java技术，例如java.io包的管道，Object的信号量等,BlockingQueue阻塞队列等。

## Handler机制

Android的Handler机制是Android中进行线程间通信的最核心方法。Android系统中为Java层和Native层分别设计了对称的几个角色，Looper，Handler，Message，MessageQueue。

Android系统中将通信的消息封装成**Message**对象,并且配备有专门的**Handler**去做事件的处理。而**Message**存放在一个叫**MessageQueue**的消息队列当中被分发处理的，而保证消息队列中消息不断被分发出去，正是**Looper**对象所做的事。

其中主线程默认开始looper，是可以直接使用Handler的。而一般的Java线程中，只有开启了线程Looper,通过调用Looper.prepare()和Looper.loop()开启内部消息队列的循环。

当读到消息时，会分发给对应的Handler去做处理。而MessageQueue本身在没有消息的时候，通过阻塞队列的方式，来实现消息的等待。

在Java层的消息队列代码里，next()方法和enqueueMessage()方法最后都会调用到natvie的pollOnce()和wake()方法，而这两个方法都是通过Linux的epoll模型来实现的。pollOnce() 通过等待被激活，然后从消息队列中获取消息。wake()方法则是激活处于等待状态的消息队列，通知它有消息到达了。

## 主线程的死循环一直运行是不是特别消耗CPU资源呢？

并不是，这里就涉及到Linux pipe/epoll机制，简单说就是在主线程的MessageQueue没有消息时，便阻塞在loop的queue.next()中的nativePollOnce()方法里，此时主线程会释放CPU资源进入休眠状态，直到下个消息到达或者有事务发生，通过往pipe管道写端写入数据来唤醒主线程工作。这里采用的epoll机制，是一种IO多路复用机制，可以同时监控多个描述符，当某个描述符就绪(读或写就绪)，则立刻通知相应程序进行读或写操作，本质是同步I/O，即读写是阻塞的。所以说，主线程大多数时候都是处于休眠状态，并不会消耗大量CPU资源。

## 如何保证多线程持有同一变量互相不影响

在Java中，可以通过ThreadLocal来实现。ThreadLocal来也就是线程局部变量，为每一个使用该变量的线程都提供一个变量值的副本，每一个线程都可以独立地改变自己的副本，而不会和其它线程的副本冲突。从线程的角度看，

在ThreadLocal类中有一个Map，用于存储每一个线程的变量的副本。概括起来说，对于多线程资源共享的问题，同步机制采用了“以时间换空间”的方式，而ThreadLocal采用了“以空间换时间”的方式。前者仅提供一份变量，让不同的线程排队访问，而后者为每一个线程都提供了一份变量，因此可以同时访问而互不影响。

## 进程间通信

最简单的方式是基于Bundle进行数据传递。因为四大组件中的三个Activity,Service,Broadcast都可以传递Bundle格式的数据。
四大组件中，Broadcast和ContentProvider都可以进行进程间通信。
在Android中，最重要的IPC机制也就是Binder，基于Binder封装的Messanger(信使)和AIDL也是常用的可以实现IPC通信的方法。
此外，文件共享，Socket, Linux的命名管道，共享内容，信号量等方式也可以。

AIDL是(Android Interface Definition Language)安卓接口定义语言的简称，我们可以利用它定义客户端与服务使用IPC进行通信时的接口，本质上AIDL是用来快速实现Binder的工具。

Binder是Android系统提供的一种IPC机制。Binder通信采用C/S架构,从进程的角度，客户端和服务端都有用自己独立的进程用户空间，并共享内核空间，跨进程机制便是利用可共享的内存空间来完成底层通信的。

从组件角度，包含Client、Server、ServiceManager以及binder驱动，其中ServiceManager用于管理系统中的各种服务。而客户端和服务端进行通信，可以简单的概括为三步：1.注册服务，服务端注册服务前到ServiceManager中。2. 获取服务，客户端使用某个服务前，先向ServiceManager那里获取到相应的服务。 3. 使用服务，客户端再根据得到的服务信息，建立与服务端所在的服务端进程的通路，从而实现通信。

## 以AIDL为例，说明客户端和服务端建立远程通信的步骤

我们以客户端和远程进程中的服务，通过AIDL来建立IPC通信的步骤为例，说明一下AIDL的用法。

1. 接口中需要用到的对象，需要实现序列化Parcelable接口，并创建aidl文件并申明对象parcelable。接口中用到的回调interface参数，也需要创建对应的aidl文件。
2. 定义AIDL回调接口，其中接口用到的对象和回调接口均引入自aidl文件
3. sync代码，自动生成AIDL接口对应Binder文件，其中Binder文件实现了接口的方法的同时，还实现了几个方法，一个是asBinder，返回当前的Binder对象，一个是onTransact()方法，参数包括int的code值，parcel的data值，parcel的reply值，以及int的flags四个参数，并返回一个boolean值。
服务端通过code确定客户端请求的是什么方法，接着从data中取出方法所需的参数，然后执行目标方法。当方法执行完成后，就向reply中写入返回值。此外，函数的返回值如果返回false表示返回失败，可以利用它来做权限验证，拒绝不合法进程的请求。
4. 实现自动生成的Binder文件中的Stub类，作为Binder在服务端的存根。
5. 申明运行在远程进程的Service，在onBind()方法中返回我们刚才通过Binder文件中的Stub类生成的Binder对象。
6. 绑定服务的时候，在bindService()方法中的ServiceConnection接口中，对于onServiceConnected方法中的IBinder参数，用Stub.asInterface()方法将IBinder转换成本地进程可以调用的接口，也就是实现Stub到Proxy的转换。

此时我们以及可以实现客户端调用服务端的方法，并收到回调。

7. 为Binder设置上死亡代理，也就是服务端如果意外退出，客户端可以收到Binder连接断裂的消息，可以进行恢复等逻辑处理。方法是通过Binder.linkToDeath()方法，传入DeathRecipient对象。