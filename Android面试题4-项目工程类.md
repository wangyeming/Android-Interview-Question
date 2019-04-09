1. 介绍一下Dex文件
2. SDK如何优化体积

# 介绍一下Dex文件

Dalvik VM和ART VM是Google设计的用于Android平台的虚拟机，区别于传统的java虚拟机直接加载Class字节码文件的方式，Dalvik虚拟机只支持加载dex格式文件，Dex文件是通过对编译生成的.class文件进行翻译、重构、解释、压缩等处理，生成dex文件。也就是说，Dex 文件格式是专为Dalvik设计的一种压缩格式。

aar文件当中的代码是classes格式。

# SDK如何优化体积

基础库，如网络，图片加载等，可以考虑封装通用的接口供外部实现，SDK只采用provide的形式。

