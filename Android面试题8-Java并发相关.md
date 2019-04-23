1. Java单例的实现
2. Java线程的六种状态
3. synchronized关键字
4. volatile关键字
5. 线程池的理解
6. Java的等待/通知模型
7. Copy-On-Write容器

# Java单例

* 懒汉模式
```java
public class Instance {

    private synchronized volatile Instance mInstance;

    private Instance() {}

    public static Instance getInstance() {
        if(mInstance == null) {
            synchronized(Instance.class) {
                if(mInstance == null) {
                    mInstance = new Instance();
                }
            }
        }
        return mInstance;
    }
}
```

* 静态内部类模式
```java
public class Instance {
    private Instance() {}

    private static class Holder {
        private static final Instance INSTANCE = new Instance();
    }

    public static Instance getInstance() {
        return Holder.INSTANCE;
    }
}
```

# Java线程的六种状态

分别是 NEW RUNNABLE BLOCKED WAITING TIMED_WAITING TERMINATED

* 初始(NEW)：新创建了一个线程对象，但还没有调用start()方法。
* 可运行(RUNNABLE)：Java线程中将就绪（ready）和运行中（running）两种状态笼统的称为“运行”。线程对象创建后，其他线程(比如main线程）调用了该对象的start()方法。该状态的线程位于可运行线程池中，等待被线程调度选中，获取CPU的使用权，此时处于就绪状态（ready）。就绪状态的线程在获得CPU时间片后变为运行中状态（running）。
* 阻塞(BLOCKED)：表示线程阻塞于锁。
* 等待(WAITING)：进入该状态的线程需要等待其他线程做出一些特定动作（通知或中断）。
* 超时等待(TIMED_WAITING)：该状态不同于WAITING，它可以在指定的时间后自行返回。
* 终止(TERMINATED)：表示该线程已经执行完毕。

# synchronized关键字

* synchronized提供了锁机制中的可见性和互斥性。Java中每一个对象都可以作为锁。利用synchronized加锁，有三种表现形式。
* 对于普通同步方法，锁是当前实例对象。
* 对于静态同步方法，锁是当前类的Class对象
* 对于同步方法块，锁是synchronized括号里面配置的对象。
* synchronized关键字提供了隐式获取释放锁的便捷性，但是也缺少一些灵活性。java中的Lock接口及相关实现类可以更灵活的操作锁的获取和释放。

# volatile关键字

* volatile关键字是轻量级synchronized，在多线程并发编程中保证了共享变量的可见性。可见性就是指当一个线程修改一个共享变量，
另外一个线程能够读到这个修改的值。恰当使用可以比synchronized的使用和执行成本更低，因为他不会引起线程上下文的切换和调度。

* 申明一个变量为volatile，那这个volatile变量单个读/写操作，与一个普通变量的读/写操作使用同一个锁来同步，他们的执行效果是一样的。
也就是说，对一个volatile变量的读，总能看到任意线程对这个volatile变量最后的写入。同时，单个volatile变量的读写操作是具有原子性的，
即使是64位的long型和double型，只要申明为volatile变量，那么对他进行读写就是原子性的。

* 重排序指的是编译器和处理器为了优化程序性能而对指令序列进行重新排序的一种手段。java内存模型(JMM)会涉及volatile变量的重排序的时候，
采取了一些策略，来保证volatile变量的这种特性。

# 线程池的理解

线程池的构造器ThreadPoolExecutor有以下几个重要参数：

**corePoolSize**：线程池核心线程数，默认状态下，核心线程会在线程中一直存活。
**maximumPoolSize**: 线程池能容纳的最大线程数，当活动线程达到该数量后，后续任务就会阻塞。
**keepAliveTime**: 非核心线程闲置时的超时时长,超过这个时长，非核心线程就会被回收。如果allowCoreThreadTimeOut属性设置为true，keepAliveTime同样会用于核心线程。
**workQueue**: 线程池中的阻塞队列
**threadFactory**: 线程工厂

ThreadPoolExecutor执行任务时遵循的规则：
1. 如果线程池中的线程数量未达到核心线程的数量，那么会直接启动一个核心线程。
2. 如果线程池中的线程数量已经达到核心线程的数量，那么任务会被插入到任务队列里排队等待执行。
3. 如果无法将任务插入到任务队列里，通常是任务队列已满，如果线程池中的线程数量没有达到最大值，那么会立即启动一个非核心线程进行处理。
4. 如果线程数量已经达到了线程池规定的最大值，那么就拒绝执行此任务。

AysncTask的线程池配置：
1. 核心线程数量等于CPU核心数+1
2. 最大线程数量等于CPU核心数*2+1
3. 核心线程无超时机制，非核心线程闲置超时时长为1s
4. 任务队列的容量是128

# Java的等待/通知模型

Java的等待通知机制，是指一个线程A调用了对象O的wait方法进入等待状态,而另一个线程B调用了对象O的notify()或notifyAll()方法,线程A收到通知后从对象O的wait()方法返回，进而执行后续操作。

等待方遵循的原则：
1. 获取对象的锁
2. 如果条件不满足，就调用对象的wait()方法，被通知后仍然要检察条件
3. 条件满足的话执行对应的逻辑

通知方遵循的原则：
1. 获取对象的锁
2. 改变条件
3. 通知所有等待在对象上的线程

机制：
1. 等待线程首先获取到锁对象，然后调用wait()方法，从而放弃了锁并进入了对象的等待队列WaitQueue中。
2. 由于等待线程释放了锁，所以通知线程就拿到了锁对象。
3. 通知线程调用了notify()方法，将等待线程从等待队列WaitQueue移到了同步队列SynchronizedQueue中，此时等待队列WaitQueue变为阻塞阶段。
4. 通知线程释放锁之后，等待线程再次获取到锁对象，并从wait()方法中返回

# Copy-On-Write容器

CopyOnWrite容器即写时复制的容器。通俗的理解是当我们往一个容器添加元素的时候，不直接往当前容器添加，而是先将当前容器进行Copy，复制出一个新的容器，然后新的容器里添加元素，添加完元素之后，再将原容器的引用指向新的容器。这样做的好处是我们可以对CopyOnWrite容器进行并发的读，而不需要加锁，因为当前容器不会添加任何元素。所以CopyOnWrite容器也是一种读写分离的思想，读和写不同的容器。

适用于读多写少的并发场景, 存在内存占用问题和数据一致性问题

内存占用问题。因为CopyOnWrite的写时复制机制，所以在进行写操作的时候，内存里会同时驻扎两个对象的内存
数据一致性问题。CopyOnWrite容器只能保证数据的最终一致性，不能保证数据的实时一致性。所以如果你希望写入的的数据，马上能读到，请不要使用CopyOnWrite容器。
