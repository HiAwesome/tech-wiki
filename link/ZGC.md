# [Java ZGC](https://wiki.openjdk.java.net/display/zgc/Main)

#### [Java 17 Command](https://docs.oracle.com/en/java/javase/17/docs/specs/man/java.html)

#### [Welcome to the ZGC Project!](https://wiki.openjdk.org/pages/viewpage.action?pageId=35454984)

The Z Garbage Collector, also known as ZGC, is a garbage collector optimized for low latency and very large heaps. It has been designed with the following goals in mind:

* Handle multi-terabyte heaps
* GC pause times not exceeding 10ms
* **No more than 15% application throughput reduction compared to using G1**

At a glance, ZGC is a concurrent, currently single-generation, region-based, incrementally compacting collector. Stop-The-World phases are limited to root scanning, meaning GC pause times do not increase with the heap- or live-set size.

Setting Parallel/Concurrent GC Threads

ZGC uses both -XX:ParallelGCThreads=<threads> and -XX:ConcGCThreads=<threads> to determine how many worker threads to use during different GC phases. If they are not set, ZGC will try to select an appropriate number. However, please note that the optimal number of threads to use heavily depends on the characteristics of the workload you're running, which means that you almost always want to explicitly specify these to get optimal throughput and latency. We hope to be able to remove this recommendation at some point in the future, when ZGC's heuristics for this becomes good enough, but for now it's recommended that you try different settings and pick the best one.

ParallelGCThreads sets the level of parallelism used during pauses and hence directly affects the pause times. Generally speaking, the more threads the better, as long as you don't over provision the machine (i.e. use more threads than cores/hw-threads) or the application root set is so small that is can easily be handled by just a few threads.

The following GC phases are affected by ParallelGCThreads:

* Pause Mark Start - Number of threads used for marking roots.
* Pause Mark End - Number of threads used for weak root processing (StringTable, JNI Weak Handles, etc.).
* Pause Relocate Start - Number of threads used for relocating roots.

* ConcGCThreads sets the level of parallelism used during concurrent phases. The number of threads to use during these phases is a balance between allowing the GC to make progress and not stealing too much CPU time from the application. Generally speaking, if there are unused CPUs/cores in the system, always allow concurrent threads to use them. If the application is already using all CPUs/cores, then the machine is essentially already over-provisioned and you have to allow for a throughput reduction by either letting concurrent GC threads steal/compete for CPU time, or by actively reducing the application CPU footprint.

NOTE! In general, if low latency (i.e. low application response time) is important to you, then never over-provision your system. Ideally, your system should never have more than 70% CPU utilization.

The following GC phases are affected by ConcGCThreads:

* Concurrent Mark - Number of threads used for concurrent marking.
* Concurrent Reference Processing - Number of threads used for concurrent reference processing (i.e. handling Soft/Weak/Final/PhantomReference objects).
* Concurrent Relocate - Number of threads used for concurrent relocation.

Example:

When running SPECjbb2015, on a two socket Intel Xeon E5-2690 machine, which a total of 2 x 8 = 16 cores (with hyper-threading, 2 x 16 = 32 HW-threads) using a 128G heap, the following options typically results in optimal throughput and latency:

-XX:+UseZGC -Xms128G -Xmx128G -XX:+UseLargePages -XX:ParallelGCThreads=20 -XX:ConcGCThreads=4

#### [Inside Java: GC](https://inside.java/tag/gc)

#### ZGC: The Next Generation Low-Latency Garbage Collector

* [video](https://www.youtube.com/watch?v=OcfvBoyTvA8)
* [cloud](http://cr.openjdk.java.net/~pliden/slides/ZGC-OracleDevLive-2020.pdf)
* [local](../pdf/ZGC-OracleDevLive-2020.pdf)

### [perliden: Oracle ZGC Team Leader Blog](https://malloc.se/)

#### [perliden: ZGC | What's new in JDK 17](https://malloc.se/blog/zgc-jdk17)

#### [Getting 'allocation stall' when enabling ZGC](https://stackoverflow.com/a/61923235)

#### [high cpu usage on openjdk zgc](https://stackoverflow.com/q/64815418)

#### [Embracing JVM unified logging (JEP-158 / JEP-271)](https://blog.arkey.fr/2020/07/28/embracing-jvm-unified-logging-jep-158-jep-271/)

#### [只有 FullGC 才会 STW 吗?](https://www.zhihu.com/question/371699670/answer/1348382472)

#### [zgc Allocation Stall 问题](https://www.cnblogs.com/lizo/p/14270686.html) 

#### [ZGC: Allocation Stall issue](https://answers.ycrash.io/question/zgc-allocation-stall-issue?q=446)

#### [TencentKona-11: ZGC使用手册](https://github.com/Tencent/TencentKona-11/wiki/ZGC%E4%BD%BF%E7%94%A8%E6%89%8B%E5%86%8C)

#### Tencent Kona JDK11 无暂停内存管理 ZGC 生产实践

* [cloud](https://cloud.tencent.com/developer/article/1836895)
* [local](../html/Tencent%20Kona%20JDK11%20无暂停内存管理.html)

#### [云原生时代的 Java 应用优化实践](https://cloud.tencent.com/developer/article/1949451)

#### [58同城 HBase 平台 ZGC 应用实践](https://heapdump.cn/article/3706373)

#### [新一代垃圾回收器 ZGC 的探索与实践](https://tech.meituan.com/2020/08/06/new-zgc-practice-in-meituan.html)

#### 日志分析

* 查看分配速率:  grep 'gc,alloc'
* 查看分配暂停:  grep 'Allocation Stall'
* 查看分配暂停统计:  grep 'Critical: Allocation Stall'
* 查看 STW 时间:  grep -E '(Pause Mark Start|Pause Mark End|Pause Relocate Start)'
* 查看系统 load:  grep 'gc,load'
* 查看 heap 信息:  grep 'gc,heap' LOG_FILE | grep -E '(Mark Start|Used)'

#### 新一代垃圾回收器ZGC设计与实现

* [带你读《新一代垃圾回收器ZGC设计与实现》之一：垃圾回收器概述](https://developer.aliyun.com/article/726110)
* [带你读《新一代垃圾回收器ZGC设计与实现》之二：ZGC内存管理](https://developer.aliyun.com/article/726120)
* [带你读《新一代垃圾回收器ZGC设计与实现》之三：ZGC线程](https://developer.aliyun.com/article/726124)

#### [Java garbage collection: The 10-release evolution from JDK 8 to JDK 18](https://blogs.oracle.com/javamagazine/post/java-garbage-collectors-evolution)
