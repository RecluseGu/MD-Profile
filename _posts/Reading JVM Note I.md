---
title: 深入理解Java JVM 读书笔记（一）Java内存区域
date: 2016-05-09 20:29:15
categories:
- 术业专攻
tags:
- Java
- JVM
- 内存区域
- OOM
---
> 最近开始阅读周志明编写的《深入理解Java虚拟机》，发现有些了解却了解的不是很清晰的内容，尤其是有很多不了解的知识，于是在博客中记录学习过程中的点点滴滴。  
<!-- more -->

晚上在无聊的毛中特课上，实验室的小伙伴向我抱怨C++的内存管理很繁琐，对于C、C++的开发人员，他们自行对内存进行管理，负责每一个对象的从出生到死亡的过程；而对于Java程序猿来说，内存管理由JVM负责，帮助Java程序猿省下一大笔的时间，然而当Java内存泄漏发生时(OOM肆意横飞)，就需要我们了解JVM是怎样使用内存，进一步排查、解决问题。

Java虚拟机在运行时将内存划分为若干数据区域，以下是《Java虚拟机规范（Java SE7版）》规定中的几个运行时数据区域，如图所示:
![JVM运行时数据区](http://ww4.sinaimg.cn/large/b36cd9dbgw1f3pfx6z50kj20dw0axdhc.jpg)

## 程序计数器
* 程序计数器（Program Counter Register）是一块比较小的内存空间，可以看做是当前线程所执行的字节码的行号指示器；
* 在Java虚拟机中的多线程是通过线程轮流切换并分配处理器执行时间的方式来实现，在任何时刻，一个处理器（对于多核处理器而言为其中的一个内核）只会执行一条线程中的指令，保证线程切换后恢复到正确的执行位置，因此**每条线程都需要有一个独立的程序计数器**，各个线程之间计数器互不影响，独立存储，则称该类内存区域为**“线程私有”**的内存；
* 如果线程正在执行一个Java方法，计数器记录的是正在执行的字节码指令的地址，如果执行的是Native方法，则计数器的值为空。 

> 注意：Nativa方法（本地方法/原生方法）：简单来说Native方法就是一个Java调用非Java的接口，该方法的实现由非Java语言实现，比如C、C++等。具体参考[百度百科](http://baike.baidu.com/link?url=bnSuD--t5CJsyxCCgHRtHjCQANbPRRBv8UYOAN8IdjV3EP8eSrDceZBwFuLn80ABdVswkUGgmzsE2ceA_EYUnCSP0x9Wl9-QbBUTHJFLpFS)。

## Java虚拟机栈
* Java虚拟机栈描述的是Java方法执行的内存模型：每个方法在执行的同时会创建一个栈帧用于存储局部变量表、操作数栈、动态链接、方法出口等。每一个方法从调用到执行完成的过程，对应一个栈帧在虚拟机栈中入栈道出栈的过程；
* Java虚拟机栈是线程私有的，其生命周期与线程相同；
* Java虚拟机栈存在两种异常：
1. StackOverflowError线程请求的栈深度大于虚拟机所允许的深度；
2. OutOfMemoryError：当虚拟机栈可以动态扩展，而扩展式无法申请到足够内存时会抛出该异常。

## 本地方法栈
* 本地方法栈与虚拟机栈作用类似，虚拟机栈为虚拟机执行Java方法（字节码）服务，本地方法栈则为虚拟机提供Native方法服务；
* 本地方法栈会抛出StackOverflowError 和 OutOfMemoryError异常。

## Java堆
* Java堆得目的就是存放对象实例，几乎所有对象实例都在Java堆中分配内存。Java虚拟机规范中描述为：**所有的对象实例以及数组都要在堆上分配**；
* Java堆是线程共享的，在虚拟机启动时创建；
* Java堆可以分为新生代和老年代；再可以细分为Eden空间、From Survivor空间、To Survivor空间等（详细内容后面提及）；
* 当Java堆中没有内存完成实例分配，并且堆没法扩展时，会抛出OutOfMemoryError异常。

## 方法区
* 方法区用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据；
* 方法区是线程共享的内存区域；
* 当方法区没法满足内存分配需求是，将抛出OutOfMemoryError异常。

>注意：在Java虚拟机规范中，对方法区的限制比较宽松，除了和Java堆一样不需要连续的内存和可以选择固定大小或者可扩展外，还可以选择不实现垃圾回收；因为在方法区中内存回收的目标是针对常量池的回收和类型的卸载，回收的效果略差。

## 运行时常量池
* 运行时常量池是方法区的一部分，Class文件中除了类的版本、字段、方法、接口等描述信息外，还有一个常量池，用于存放编译期生成的各种字面量和符号引用，该部分保存在运行时常量池中；
* 动态性：Java语言并没有要求常量必须在编译时产生，即并非是预置入Class文件中常量池的内容常能进入方法去运行时常量池，运行期间也可能将新的常量放入池中，例如String类的intern方法；
* 当运行时常量池无法申请到内存时会抛出OutOfMemoryError异常。

## 直接内存
* 直接内存并不是虚拟机运行时数据区的一部分，也不是虚拟机规范中的内存区域，但他会导致OOM的出现。
* 随JDK1.4中加入的NIO（New Input/Output）类被引入，目的是避免在Java堆和Native堆中来回复制数据带来的性能损耗；
* 全局共享
* 直接内存分配内存时不会受到Java堆得影响，但是会受到计算机总内存大小以及处理器寻址空间的限制。
