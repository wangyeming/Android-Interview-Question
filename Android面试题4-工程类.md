1.	dex文件是啥，aar包里面是dex还是class	
2.	sdk如何优化体积
3.	安卓中插件化框架的介绍

# Dex文件

在明白什么是 Dex 文件之前，要先了解一下 JVM，Dalvik 和 ART。JVM 是 JAVA 虚拟机，用来运行 JAVA 字节码程序。Dalvik 是 Google 设计的用于 Android平台的运行时环境，适合移动环境下内存和处理器速度有限的系统。ART 即 Android Runtime，是 Google 为了替换 Dalvik 设计的新 Android 运行时环境，在Android 4.4推出。ART 比 Dalvik 的性能更好。Android 程序一般使用 Java 语言开发，但是 Dalvik 虚拟机并不支持直接执行 JAVA 字节码，所以会对编译生成的 .class 文件进行翻译、重构、解释、压缩等处理，这个处理过程是由 dx 进行处理，处理完成后生成的产物会以 .dex 结尾，称为 Dex 文件。Dex 文件格式是专为 Dalvik 设计的一种压缩格式。所以可以简单的理解为：Dex 文件是很多 .class 文件处理后的产物，最终可以在 Android 运行时环境执行。

aar文件当中的代码是classes格式。