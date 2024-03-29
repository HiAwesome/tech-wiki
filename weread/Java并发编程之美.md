## Java并发编程之美

 **翟陆续 薛宾田**


### 1.4 等待线程执行终止的join方法

* 线程A调用线程B的join方法后会被阻塞，当其他线程调用了线程A的interrupt（）方法中断了线程A时，线程A会抛出InterruptedException异常而返回。


### 1.9 线程死锁

* 环路等待条件：指在发生死锁时，必然存在一个线程—资源的环形链，即线程集合{T0, T1, T2, …, Tn}中的T0正在等待一个T1占用的资源，T1正在等待T2占用的资源，……Tn正在等待已被T0占用的资源。


### 1.11 ThreadLocal

* 总结：InheritableThreadLocal类通过重写代码（2）和（3）让本地变量保存到了具体线程的inheritableThreadLocals变量里面，那么线程在通过InheritableThreadLocal类实例的set或者get方法设置变量时，就会创建当前线程的inheritableThreadLocals变量。当父线程创建子线程时，构造函数会把父线程中inheritableThreadLocals变量里面的本地变量复制一份保存到子线程的inheritableThreadLocals变量里面。


### 2.3 Java中的线程安全问题

* 如果不使用同步措施，由于递增操作是获取—计算—保存三步操作，因此可能导致计数不准确


### 2.4 Java中共享变量的内存可见性问题

* 线程A这次又需要修改X的值，获取时一级缓存命中，并且X=1，到这里问题就出现了，明明线程B已经把X的值修改为了2，为何线程A获取的还是1呢？这就是共享变量的内存不可见问题，也就是线程B写入的值对线程A不可见。


### 2.5 Java中的synchronized关键字

* 拿到内部锁的线程会在正常退出同步代码块或者抛出异常后或者在同步块内调用了该内置锁资源的wait系列方法时释放该内置锁。

* 由于Java中的线程是与操作系统的原生线程一一对应的，所以当阻塞一个线程时，需要从用户态切换到内核态执行阻塞操作，这是很耗时的操作，而synchronized的使用就会导致上下文切换。


### 2.6 Java中的volatile关键字

* 当一个变量被声明为volatile时，线程在写入变量时不会把值缓存在寄存器或者其他地方，而是会把值刷新回主内存。当其他线程读取该共享变量时，会从主内存重新获取最新值，而不是使用当前线程的工作内存中的值。

* 那么一般在什么时候才使用volatile关键字呢？● 写入变量值不依赖变量的当前值时。因为如果依赖当前值，将是获取—计算—写入三步操作，这三步操作不是原子性的，而volatile不保证原子性。● 读写变量值时没有加锁。因为加锁本身已经保证了内存可见性，这时候不需要把变量声明为volatile的。


### 2.8 Java中的CAS操作

* ABA问题的产生是因为变量的状态值产生了环形转换，就是变量的值可以从A到B，然后再从B到A。如果变量的值只能朝着一个方向转换，比如A到B, B到C，不构成环形，就不会存在问题。JDK中的AtomicStampedReference类给每个变量的状态值都配备了一个时间戳，从而避免了ABA问题的产生。


### 2.11 伪共享

* 伪共享的产生是因为多个变量被放入了一个缓存行中，并且多个线程同时去写入缓存行中不同的变量。

* 所以在单个线程下顺序修改一个缓存行中的多个变量，会充分利用程序运行的局部性原则，从而加速了程序的运行。而在多线程下并发修改一个缓存行中的多个变量时就会竞争缓存行，从而降低程序运行性能。

* JDK 8提供了一个sun.misc.Contended注解，用来解决伪共享问题。


### 2.12 锁的概述

* 公平锁表示线程获取锁的顺序是按照线程请求锁的时间早晚来决定的，也就是最早请求锁的线程将最早获取到锁。

* 在没有公平性需求的前提下尽量使用非公平锁，因为公平锁会带来性能开销。

* 可重入锁的原理是在锁内部维护一个线程标示，用来标示该锁目前被哪个线程占用，然后关联一个计数器。一开始计数器值为0，说明该锁没有被任何线程占用。当一个线程获取了该锁时，计数器的值会变成1，这时其他线程再来获取该锁时会发现锁的所有者不是自己而被阻塞挂起。


### 4.2 JDK 8新增的原子操作类LongAdder

* JDK 8中新增的LongAdder原子性操作类，该类通过内部cells数组分担了高并发下多线程同时对一个原子变量进行更新时的竞争量，让多个线程可以同时对cells数组里面的元素进行并行的更新操作。另外，数组元素Cell使用@sun.misc.Contended注解进行修饰，这避免了cells数组内多个原子变量被放入同一个缓存行，也就是避免了伪共享，这对性能也是一个提升。


### 5.2 主要方法源码解析

* 迭代器的弱一致性是怎么回事，所谓弱一致性是指返回迭代器后，其他线程对list的增删改对迭代器是不可见的


### 第6章 Java并发包中锁原理剖析

* 使用诊断工具可以观察线程被阻塞的原因，诊断工具是通过调用getBlocker（Thread）方法来获取blocker对象的，所以JDK推荐我们使用带有blocker参数的park方法，并且blocker被设置为this，这样当在打印线程堆栈排查问题时就能知道是哪个类被阻塞了。


### 6.2 抽象同步队列AQS概述

* 其实这里的Lock对象等价于synchronized加上共享变量，调用lock.lock（）方法就相当于进入了synchronized块（获取了共享变量的内置锁），调用lock.unLock（）方法就相当于退出synchronized块。调用条件变量的await（）方法就相当于调用共享变量的wait（）方法，调用条件变量的signal方法就相当于调用共享变量的notify（）方法。调用条件变量的signalAll（）方法就相当于调用共享变量的notifyAll（）方法。


### 6.3 独占锁ReentrantLock的原理

* 下面使用ReentrantLock来实现一个简单的线程安全的list。

* 假设Thread1获取锁后调用了对应的锁创建的条件变量1，那么Thread1就会释放获取到的锁，然后当前线程就会被转换为Node节点插入条件变量1的条件队列。


### 6.4 读写锁ReentrantReadWriteLock的原理

* 如果当前要获取读锁的线程已经持有了写锁，则也可以获取读锁。但是需要注意，当一个线程先获取了写锁，然后获取了读锁处理事情完毕后，要记得把读锁和写锁都释放掉，不能只释放写锁。

* 上节介绍了如何使用ReentrantLock实现线程安全的list，但是由于ReentrantLock是独占锁，所以在读多写少的情况下性能很差。下面使用ReentrantReadWriteLock来改造它，代码如下。


### 6.5 JDK 8中新增的StampedLock锁探究

* 下面通过JDK 8里面提供的一个管理二维点的例子来理解以上介绍的概念。

* 另外，这里的x, y没有被声明为volatie的，会不会存在内存不可见性问题呢？答案是不会，因为加锁的语义保证了内存的可见性。

* StampedLock提供的读写锁与ReentrantReadWriteLock类似，只是前者提供的是不可重入锁。但是前者通过提供乐观读锁在多线程多读的情况下提供了更好的性能，这是因为获取乐观读锁时不需要进行CAS操作设置锁的状态，而只是简单地测试状态。


### 7.1 ConcurrentLinkedQueue原理探究

* 可见，offer操作中的关键步骤是代码（5），通过原子CAS操作来控制某时只有一个线程可以追加元素到队列末尾。进行CAS竞争失败的线程会通过循环一次次尝试进行CAS操作，直到CAS成功才会返回，也就是通过使用无限循环不断进行CAS尝试方式来替代阻塞算法挂起调用线程。相比阻塞算法，这是使用CPU资源换取阻塞所带来的开销。

* poll方法在移除一个元素时，只是简单地使用CAS操作把当前节点的item值设置为null，然后通过重新设置头节点将该元素从队列里面移除，被移除的节点就成了孤立节点，这个节点会在垃圾回收时被回收掉。另外，如果在执行分支中发现头节点被修改了，要跳到外层循环重新获取新的头节点。

* 入队、出队都是操作使用volatile修饰的tail、head节点，要保证在多线程下出入队线程安全，只需要保证这两个Node操作的可见性和原子性即可。


### 7.2 LinkedBlockingQueue原理探究

* 代码（1）通过fullyLock获取双重锁，获取后，其他线程进行入队或者出队操作时就会被阻塞挂起。

* 由于进行出队、入队操作时的count是加了锁的，所以结果相比ConcurrentLinkedQueue的size方法比较准确。这里考虑为何在ConcurrentLinkedQueue中需要遍历链表来获取size而不使用一个原子变量呢？这是因为使用原子变量保存队列元素个数需要保证入队、出队操作和原子变量操作是原子性操作，而ConcurrentLinkedQueue使用的是CAS无锁算法，所以无法做到这样。


### 7.3 ArrayBlockingQueue原理探究

* ArrayBlockingQueue通过使用全局独占锁实现了同时只能有一个线程进行入队或者出队操作，这个锁的粒度比较大，有点类似于在方法上添加synchronized的意思。其中offer和poll操作通过简单的加锁进行入队、出队操作，而put、take操作则使用条件变量实现了，如果队列满则等待，如果队列空则等待，然后分别在出队和入队操作中发送信号激活等待线程实现同步。另外，相比LinkedBlockingQueue, ArrayBlockingQueue的size操作的结果是精确的，因为计算前加了全局锁。


### 7.4 PriorityBlockingQueue原理探究

* 下面我们通过一个案例来体会PriorityBlockingQueue的使用方法。在这个案例中，会把具有优先级的任务放入队列，然后从队列里面逐个获取优先级最高的任务来执行。


### 7.5 DelayQueue原理探究

* 下面我们通过一个简单的案例来加深对DelayQueue的理解


### 8.2 类图介绍

* keeyAliveTime：存活时间。如果当前线程池中的线程数量比核心线程数量多，并且是闲置状态，则这些闲置的线程能存活的最大时间。


### 8.4 总结

* 线程池巧妙地使用一个Integer类型的原子变量来记录线程池状态和线程池中的线程个数。通过线程池状态来控制任务的执行，每个Worker线程可以处理多个任务。线程池通过线程的复用减少了线程创建和销毁的开销。


### 第10章 Java并发包中线程同步器原理剖析

* 在日常开发中经常会遇到需要在主线程中开启多个线程去并行执行任务，并且主线程需要等待所有子线程执行完毕后再进行汇总的场景。在CountDownLatch出现之前一般都使用线程的join（）方法来实现这一点，但是join方法不够灵活，不能够满足不同场景的需要，所以JDK开发组提供了CountDownLatch这个类，我们前面介绍的例子使用CountDownLatch会更优雅。

* 这里总结下CountDownLatch与join方法的区别。一个区别是，调用一个子线程的join（）方法后，该线程会一直被阻塞直到子线程运行完毕，而CountDownLatch则使用计数器来允许子线程运行完毕或者在运行中递减计数，也就是CountDownLatch可以在子线程运行的任何时候让await方法返回而不一定必须等到线程结束。另外，使用线程池来管理线程时一般都是直接添加Runable到线程池，这时候就没有办法再调用线程的join方法了，就是说countDownLatch相比join方法让我们对线程同步有更灵活的控制。


### 10.2 回环屏障CyclicBarrier原理探究

* 为了满足计数器可以重置的需要，JDK开发组提供了CyclicBarrier类，并且CyclicBarrier类的功能并不限于CountDownLatch的功能。


### 10.3 信号量Semaphore原理探究

* 可见公平性还是靠hasQueuedPredecessors这个函数来保证的。


### 10.4 总结

* 而Semaphore采用了信号量递增的策略，一开始并不需要关心同步的线程个数，等调用aquire方法时再指定需要同步的个数，并且提供了获取信号量的公平性策略。


### 第11章 并发编程实践

* 本节结合logback中异步日志的实现介绍了并发组件ArrayBlockingQueue的使用


### 11.2 Tomcat的NioEndPoint中ConcurrentLinkedQueue的使用

* 通过分析Tomcat中NioEndPoint的实现源码介绍了并发组件ConcurrentLinkedQueue的使用


### 11.3 并发组件ConcurrentHashMap使用注意事项

* put（K key, V value）方法判断如果key已经存在，则使用value覆盖原来的值并返回原来的值，如果不存在则把value放入并返回null。而putIfAbsent（K key, V value）方法则是如果key已经存在则直接返回原来对应的值并不使用value覆盖，如果key不存在则放入value并返回null


### 11.4 SimpleDateFormat是线程不安全的

* 第三种方式：使用ThreadLocal，这样每个线程只需要使用一个SimpleDateFormat实例，这相比第一种方式大大节省了对象的创建销毁开销，并且不需要使多个线程同步。


### 11.6 对需要复用但是会被下游修改的参数要进行深复制

* 本节通过一个简单的消息发送例子说明了需要复用但是会被下游修改的参数要进行深复制，否则会导致出现错误的结果；另外引用类型作为集合元素时，如果使用这个集合作为另外一个集合的构造函数参数，会导致两个集合里面的同一个位置的元素指向的是同一个引用，这会导致对引用的修改在两个集合中都可见，所以这时候需要对引用元素进行深复制。


### 11.9 线程池使用FutureTask时需要注意的事情

* 本节通过案例介绍了在线程池中使用FutureTask时，当拒绝策略为DiscardPolicy和DiscardOldestPolicy时，在被拒绝的任务的FutureTask对象上调用get（）方法会导致调用线程一直阻塞，所以在日常开发中尽量使用带超时参数的get方法以避免线程一直阻塞。

