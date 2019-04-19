1. 介绍一下Dex文件
2. SDK如何优化体积
3. 从点击桌面应用图标，到最终启动App，中间发生了什么事？
4. 大体说清一个应用程序安装到手机上时发生了什么？
5. apk文件的组成
6. 一个图片在app中调用R.id后是如何找到的

# 介绍一下Dex文件

DEX 文件是一种专为 Android 设计的字节码格式，经过优化，使用的内存很少。编译工具链（例如 Jack）将 Java 源代码编译为 DEX 字节码，使其可在 Android 平台上运行。

Dalvik VM和ART VM是Google设计的用于Android平台的虚拟机，区别于传统的Java虚拟机直接加载Class字节码文件的方式，Dalvik虚拟机只支持加载dex格式文件，Dex文件是通过对编译生成的.class文件进行翻译、重构、解释、压缩等处理，生成dex文件。也就是说，Dex 文件格式是专为Dalvik设计的一种压缩格式。

aar文件当中的代码是classes格式。

# SDK如何优化体积

基础库，如网络，图片加载等，可以考虑封装通用的接口供外部实现，SDK只采用provide的形式。

# 从点击桌面应用图标，到最终启动App，中间发生了什么事？

『六个角色，四个进程，五个步骤』

整个流程涉及的主要角色有：

Instrumentation: 监控应用与系统相关的交互行为。
AMS：组件管理调度中心，什么都不干，但是什么都管。
ActivityStarter：Activity启动的控制器，处理Intent与Flag对Activity启动的影响，具体说来有：1 寻找符合启动条件的Activity，如果有多个，让用户选择；2 校验启动参数的合法性；3 返回int参数，代表Activity是否启动成功。
ActivityStackSupervisior：这个类的作用你从它的名字就可以看出来，它用来管理任务栈。
ActivityStack：用来管理任务栈里的Activity。
ActivityThread：最终干活的人，Activity、Service、BroadcastReceiver的启动、切换、调度等各种操作都在这个类里完成。

整个流程主要涉及四个进程：

调用者进程，如果是在桌面启动应用就是Launcher应用进程。
ActivityManagerService等待所在的System Server进程，该进程主要运行着系统服务组件。
Zygote进程，该进程主要用来fork新进程。
新启动的应用进程，该进程就是用来承载应用运行的进程了，它也是应用的主线程（新创建的进程就是主线程），处理组件生命周期、界面绘制等相关事情。

有了以上的理解，整个流程可以概括如下：

1、点击桌面应用图标，Launcher进程将启动Activity的请求以Binder的方式发送给了AMS。
2、AMS接收到启动请求后，交付ActivityStarter处理Intent和Flag等信息，然后再交给ActivityStackSupervisior/ActivityStack 处理Activity进栈相关流程。同时以Socket方式请求Zygote进程fork新进程。
3、Zygote接收到新进程创建请求后fork出新进程。
4、在新进程里创建ActivityThread对象，新创建的进程就是应用的主线程，在主线程里开启Looper消息循环，开始处理创建Activity。
5、ActivityThread利用ClassLoader去加载Activity、创建Activity实例，并回调Activity的onCreate()方法，这样便完成了Activity的启动。

# 大体说清一个应用程序安装到手机上时发生了什么？

1. 复制APK到/data/app目录下，解压并扫描安装包。
2. 资源管理器解析APK里的资源文件, 解析AndroidManifest文件，并在/data/data/目录下创建对应的应用数据目录。
3. 然后对dex文件进行优化，并保存在dalvik-cache目录下。
4. 将AndroidManifest文件解析出的四大组件信息注册到PackageManagerService中。
5. 安装完成后，发送广播。

# apk文件的组成

dex：最终生成的Dalvik字节码。
res：存放资源文件的目录。
asserts：额外建立的资源文件夹。
lib：如果存在的话，存放的是ndk编出来的so库。
META-INF：存放签名信息

# 一个图片在app中调用R.id后是如何找到的

在编译的时候，AAPT会扫描你所定义的所有资源,然后给它们指定不同的资源ID。资源ID 是一个32bit的数字，格式是PPTTNNNN ， PP代表资源所属的包(package) ,TT代表资源的类型(type)，NNNN代表这个类型下面的资源的名称。 对于应用程序的资源来说，PP的取值是0×7f。

AAPT在每一次编译的时候不会去保存上一次生成的资源ID标示，每当/res目录发生变化的时候，AAPT可能会去重新给资源指定ID号，然后重新生成一个R.java文件。因此，在做开发的时候，你不应该在程序中将资源ID持久化保存到文件或者数据库。而资源ID在每一次编译后都有可能变化。
一旦资源被编译成二进制文件的时候，AAPT会生成R.java 文件和“resources.arsc”文件，“R.java”用于代码的编译，而”resources.arsc”则包含了全部的资源名称、资源ID和资源的内容（对于单独文件类型的资源，这个内容代表的是这个文件在其.apk 文件中的路径信息）。这样就把运行环境中的资源ID 和具体的资源对应起来了。

在调试的时候，你可以使用“ aapt dump resources <apk的路径>”来看到对resources.arsc文件的详细描述信息。

