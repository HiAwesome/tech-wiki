## Java异步编程实战

 **翟陆续**


### 1.2 异步编程场景

* 使用Future确实可以获取异步任务的执行结果，但是获取其结果还是会阻塞调用线程的，并没有实现完全异步化处理，所以在JDK8中提供了CompletableFuture来弥补其缺点。CompletableFuture类允许以非阻塞方式和基于通知的方式处理结果，其通过设置回调函数方式，让主线程彻底解放出来，实现了实际意义上的异步处理。


### 2.2 显式使用线程池实现异步编程

* 由上述代码可知，调用线程池队列的drainTo方法把队列中的任务移除到taskList里，如果发现线程池队列还不为空（比如DelayQueue或者其他类型的队列drainTo可能移除元素失败），则循环移除里面的元素，最后返回移除的任务列表。


### 3.2 JDK中的FutureTask

* 总结：当我们创建一个FutureTask时，其任务状态初始化为NEW，当我们把任务提交到线程或者线程池后，会有一个线程来执行该FutureTask任务，具体是调用其run方法来执行任务。在任务执行过程中，我们可以在其他线程调用FutureTask的get()方法来等待获取结果，如果当前任务还在执行，则调用get的线程会被阻塞然后放入FutureTask内的阻塞链表队列；多个线程可以同时调用get方法，这些线程可能都会被阻塞并放到阻塞链表队列中。当任务执行完毕后会把结果或者异常信息设置到outcome变量，然后会移除和唤醒FutureTask内阻塞链表队列中的线程节点，进而这些由于调用FutureTask的get方法而被阻塞的线程就会被激活。
3.2.6 FutureTask的局限性
FutureTask虽然提供了用来检查任务是否执行完成、等待任务执行结果、获取任务执行结果的方法，但是这些特色并不足以让我们写出简洁的并发代码，比如它并不能清楚地表达多个FutureTask之间的关系。另外，为了从Future获取结果，我们必须调用get()方法，而该方法还是会在任务执行完毕前阻塞调用线程，这明显不是我们想要的。
我们真正想要的是：
● 可以将两个或者多个异步计算结合在一起变成一个，这包含两个或者多个异步计算是相互独立的情况，也包含第二个异步计算依赖第一个异步计算结果的情况。
● 对反应式编程的支持，也就是当任务计算完成后能进行通知，并且可以以计算结果作为一个行为动作的参数进行下一步计算，而不是仅仅提供调用线程以阻塞的方式获取计算结果。
● 可以通过编程的方式手动设置（代码的方式）Future的结果；FutureTask不能实现让用户通过函数来设置其计算结果，而是在其任务内部来进行设置。
● 可以等多个Future对应的计算结果都出来后做一些事情。
为了克服FutureTask的局限性，以及满足我们对异步编程的需要，JDK8中提供了CompletableFuture。


### 3.3 JDK中的CompletableFuture

* 一个CompletableFuture任务可能有一些依赖其计算结果的行为方法，这些行为方法被收集到一个无锁基于CAS操作来链接起来的链表组成的栈中；当Completable-Future的计算任务完成后，会自动弹出栈中的行为方法并执行。需要注意的是，由于是栈结构，在同一个CompletableFuture对象上行为注册的顺序与行为执行的顺序是相反的。

* 如上所述，当我们使用CompletableFuture实现异步编程时，大多数时候是不需要显式创建线程池，并投递任务到线程池内的。我们只需要简单地调用CompletableFuture的runAsync或者supplyAsync等方法把异步任务作为参数即可，其内部会使用ForkJoinPool线程池来进行异步执行的支持，这大大简化了我们异步编程的负担，实现了声明式编程（告诉程序我要执行异步任务，但是具体怎么实现我不需要管），当然如果你想使用自己的线程池来执行任务，也是可以非常方便地进行设置的。

* CompletionStage节点可以使用3种模式来执行：默认执行、默认异步执行（使用async后缀的方法）和用户自定义的线程执行器执行（通过传递一个Executor方式）。


### 3.4 JDK8 Stream & CompletableFuture

* 注意：具体这10个rpc请求是否全部并发运行取决于CompletableFuture内线程池内线程的个数，如果你的机器是单核的或者线程池内线程个数为1，那么这10个任务还是会顺序执行的。


### 4.2 如何在Spring中使用异步执行

* 由上可知基于@Async注解实现异步执行的方式时，大大简化了我们异步编程的运算负担，我们不必再显式地创建线程池并把任务手动提交到线程池内，只要直接在需要异步执行的方法上添加@Async注解即可。当然，当我们需要使用自己的线程池来异步执行标注@Async的方法时，还是需要显式创建线程池的，但这时并不需要显式提交任务到线程池。


### 5.1 反应式编程概述

* 在Reactive编程中，运算符是我们装配线中类比的工作站。每个运算符都会向发布者添加行为，并将上一步的发布者包装到新实例中。因此链接整个链，使得数据源自第一个发布者并沿着链向下移动，由每个链点进行转换。最终，订阅者订阅该流，然后激活完成该过程。需要注意，在订阅者订阅发布者之前没有任何事情发生。


### 5.2 Reactive Streams规范

* Reactive Streams的主要目标是管理跨异步边界的流数据交换—考虑将元素从一个线程传递到另一个线程或线程池进行处理，同时确保接收方不会强制缓冲任意数量的数据。换句话说，回压是该模型的组成部分，以便允许在线程之间协调的队列有界。另外如果回压信号是同步的（参见Reactive Manifesto），异步处理的好处就将被否定，因此需要注意强制Reactive Streams实现的所有方面的完全非阻塞和异步行为。


### 6.1 Servlet概述

* 为了保证容器的其他组件不受负面影响，高端的Application Server可能会限制Thread对象的创建。常见的比较经典的Servlet容器实现有Tomcat和Jetty。


### 6.3 Servlet 3.1 提供的非阻塞IO能力

* 在Servlet3.1规范中提供了非阻塞IO处理方式：Web容器中的非阻塞请求处理有助于增加Web容器可同时处理请求的连接数量。Servlet容器的非阻塞IO允许开发人员在数据可用时读取数据或在数据可写时写数据。非阻塞IO对在Servlet和Filter中的异步请求处理有效，否则，当调用ServletInputStream.setReadListener或Servlet OutputStream.setWriteListener方法时将抛出IllegalStateException。基于内核的能力，Servlet3.1允许我们在ServletInputStream上通过函数setReadListener注册一个监听器，该监听器在发现内核有数据时才会进行回调处理函数。

* 需要注意的是，Servlet3.1不仅增加了可以非阻塞读取请求体的ReadListener，还增加了可以避免阻塞写的WriteListener接口，在ServletOutputStream上可以通过set-WriteListener进行设置。当一个WriteListener注册到ServletOutputStream后，当可以写数据时onWritePossible()方法将被容器首次调用，这里我们不再展开讨论。


### 7.3 WebFlux服务器

* 那么WebFlux是如何做到平滑地切换不同服务器的呢？在WebFlux中HttpHandler有一个简单的规范，只有一个方法来处理请求和响应


### 7.5 WebFlux对性能的影响

* 反应式和非阻塞编程通常不会使应用程序运行得更快，虽然在某些情况下它们可以（例如使用WebClient并行执行远程调用）做到更快。相反以非阻塞的方式来执行，需要做更多的额外工作，并且可能会增加处理所需的时间。
反应式和非阻塞的关键好处是能够使用少量固定数量的线程和更少的内存实现系统可伸缩性。这使得应用程序在负载下更具弹性，因为它们以更可预测的方式扩展。但是为了得到这些好处，需要付出一些代价（比如不可预测的网络IO）。


### 7.6 WebFlux的编程模型

* RouterFunction相当于@RequestMapping注解本身，两者的主要区别在于，路由器功能不仅提供数据，还提供行为。


### 7.8 WebFlux的适用场景

* 既然Spring 5中推出了WebFlux，那么我们做项目时到底选择使用Spring MVC还是WebFlux？
这是一个自然会想到的问题，但却是不合理的。因为两者的存在并不是矛盾的，利用两者可扩大我们开发时可用选项的范围。两者的设计是为了保持连续性和一致性，它们可以并排使用，每一方的反馈都有利于双方。


### 8.1 异步、基于事件驱动的网络编程框架—Netty

* Netty之所以说是异步非阻塞网络框架，是因为通过NioSocketChannel的write系列方法向连接里面写入数据时是非阻塞的，是可以马上返回的（即使调用写入的线程是我们的业务线程）。这是Netty通过在ChannelPipeline中判断调用NioSocketChannel的write的调用线程是不是其对应的NioEventLoop中的线程来实现的

* 其实出现粘包和半包的原因是TCP层不知道上层业务的包的概念，它只是简单地传递流，所以需要上层应用层协议来识别读取的数据是不是一个完整的包。


### 8.3 高性能线程间消息传递库—Disruptor

* 计算机系统中为了解决主内存与CPU运行速度的差距，在CPU与主内存之间添加了一级或多级高速缓冲存储器（Cache），这个Cache一般是集成到CPU内部的，所以也叫CPU Cache


### 8.4 异步、分布式、基于消息驱动的框架—Akka

* Actor模型不仅仅被认为是一种高效的解决方案，它已经在世界上一些要求最苛刻的应用中得到了验证。为了突出Actor模型所解决的问题，本节首先讨论传统编程模型与现代多线程和多CPU的硬件架构之间的不匹配：
● 对面向对象中封装（encapsulation）特性的挑战
● 对共享内存在现代计算机架构上的误解
● 对调用堆栈的误解

* 另外，锁只能在单JVM内（本地锁）很好地工作。当涉及跨多台机协调时，只能使用分布式锁。但是分布式锁的效率比本地锁低几个数量级，这是因为分布式锁协议需要跨多台机在网络上进行多次往返通信，其造成的最大影响就是延迟。

*  在现在多核CPU的硬件架构中，多线程之间不再有真正的共享内存，CPU核心之间显式传递数据块（Cache行）将和网络中不同计算机之间传递数据一样。CPU核心之间通信和网络通信的共同点比许多人意识中的要多。现在跨CPU或联网计算机传递消息已成为一种规范。
● 除了通过使用volatile修饰共享的变量或使用原子数据结构保证共享变量内存可见性之外，一个更严格和原则上的方法是将状态保持在并发实体本地，并通过消息显式在并发实体之间传播数据或事件。

* Actor模型最重要的特点是其系统中的Actor之间是相互隔离的，并且Actor之间并不会存在共享内存（共享资源），每个Actor中可以持有一个自己私有状态变量，这个变量是其他Actor改变不了的。

* Actor的状态是本地的而不是共享的，更改和数据通过消息进行传递，这与现代系统中内存的实际工作方式相对应。在许多情况下，这意味着仅传递包含了消息数据的Cache行，而将本地状态和数据保留在原始CPU核中。相同的模型可以精确地映射到远程通信，在远程通信中，状态将保留在机器的主内存中，更改和数据则作为数据包在网络上传播。

* 如果父Actor停止了，则其所有子Actor也将递归停止。这项服务被称为监督（supervisor），它是Akka的核心。


### 9.1 Go语言概述

* 不要通过共享内存来通信，而要通过通信来共享内存。


### 9.2 Go语言的线程模型

* Go线程模型属于多对多线程模型

