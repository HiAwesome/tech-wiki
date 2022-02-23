# 与 Java 有关的链接

#### [https://www.baeldung.com/java-thread-stop](https://www.baeldung.com/java-thread-stop)

#### [java.lang.NoClassDefFoundError: sun/misc/BASE64Encoder](https://stackoverflow.com/a/29692535)

old: ` new sun.misc.BASE64Encoder().encode `

new: ` new org.apache.commons.codec.binary.Base64.encodeBase64String `

#### [Java equivalent to JavaScript's encodeURIComponent that produces identical output?](https://stackoverflow.com/a/611117)

### [Guide To CompletableFuture](https://www.baeldung.com/java-completablefuture)

### [Correct way to check Java version from BASH script](https://stackoverflow.com/a/7335524)

```shell
if type -p java; then
    echo found java executable in PATH
    _java=java
elif [[ -n "$JAVA_HOME" ]] && [[ -x "$JAVA_HOME/bin/java" ]];  then
    echo found java executable in JAVA_HOME     
    _java="$JAVA_HOME/bin/java"
else
    echo "no java"
fi

if [[ "$_java" ]]; then
    version=$("$_java" -version 2>&1 | awk -F '"' '/version/ {print $2}')
    echo version "$version"
    if [[ "$version" > "1.5" ]]; then
        echo version is more than 1.5
    else         
        echo version is less than 1.5
    fi
fi
```

### [What Are The Most Important Skills As A Java Software Engineer?](https://arnoldgalovics.com/java-software-engineer-skills/)

* Problem solving
* Attitude
* Ability to learn new things
* Diligence
* Communication
* Team worker

### Migration Java17

* [java.lang.ExceptionInInitializerError with Java-16 | j.l.ClassFormatError accessible: module java.base does not "opens java.lang" to unnamed module](https://stackoverflow.com/a/67006749)
* [How to solve InaccessibleObjectException ("Unable to make {member} accessible: module {A} does not 'opens {package}' to {B}") on Java 9?](https://stackoverflow.com/a/41265267)
* [Get declared fields of java.lang.reflect.Fields in jdk12](https://stackoverflow.com/a/56043252)
* [Presto: Fix incompatible reflection for modifying static final fields since JDK 12](https://github.com/prestodb/presto/pull/15240)
* [Moqi: Java17ReflectTool](https://github.com/moqimoqidea/moqi-tool-java/blob/master/src/main/java/com/moqi/in20220109/Java17ReflectTool.java)
* [Programmer's Guide to Text Blocks](https://docs.oracle.com/en/java/javase/15/text-blocks/index.html)
* [JEP 396: Strongly Encapsulate JDK Internals by Default](http://openjdk.java.net/jeps/396)

#### [Information about _JAVA_OPTIONS](https://stackoverflow.com/questions/17781405/information-about-java-options) 

* [What is the difference between JDK_JAVA_OPTIONS and JAVA_TOOL_OPTIONS when using Java 11?](https://stackoverflow.com/a/52989562)

#### [Java9 中: jdwp 更新](https://www.oracle.com/java/technologies/javase/9-notes.html)

JDWP 套接字连接器默认只接受本地连接 

JDWP 套接字连接器已更改为仅在代理命令行上未指定 ip 地址或主机名时绑定到 localhost。星号 (*) 的主机名可用于实现旧行为，即将 JDWP 套接字连接器绑定到所有可用接口；这不安全，不推荐。

#### [Tomcat: Developing](https://cwiki.apache.org/confluence/display/TOMCAT/Developing)

* -Xdebug -Xrunjdwp:transport=dt_socket,address=8000,server=y,suspend=n
*  catalina jpda start

#### [spock: Cannot create mock due to module java.base does not "opens java.lang.invoke"](https://github.com/spockframework/spock/issues/1406)

使用 Spock 测试框架与 Java17 的进度目前在此 issue 中进行讨论。

#### [cglib: Java 16 and 17 compatibility](https://github.com/cglib/cglib/issues/191)

cglib 最后一次更新时间是 20190812，Java17 不允许使用 `--illegal-access=permit` 这个命令，因此截至目前 20220106，使用 cglib 作为反射工具的库都会碰到问题。

#### [Multi-Release JAR Files with Maven](https://www.baeldung.com/maven-multi-release-jars)

### [Oracle JDK Migration Guide](https://docs.oracle.com/en/java/javase/17/migrate/getting-started.html)

#### [Strong Encapsulation in the JDK](https://docs.oracle.com/en/java/javase/17/migrate/migrating-jdk-8-later-jdk-releases.html#GUID-7BB28E4D-99B3-4078-BDC4-FC24180CE82B)

* --add-exports: If you have an older tool or library that needs to use an internal API that has been strongly encapsulated, then use the --add-exports runtime option. You can also use --add-exports at compile time to access the internal APIs.
* --add-opens: If you have an older tool or library that needs to access non-public fields and methods of java.* APIs by reflection, then use the --add-opens option.

#### [Run jdeps on Your Code](https://docs.oracle.com/en/java/javase/17/migrate/preparing-migration.html#GUID-BA521187-60FD-4CA5-B998-58A1D05587BC)

Run the jdeps tool on your application to see what packages and classes your applications and libraries depend on. If you use internal APIs, then jdeps may suggest replacements to help you to update your code.

If you use Maven, there’s a [jdeps](https://maven.apache.org/plugins/maven-jdeps-plugin/usage.html) plugin available.

#### [Changes to GC Log Output](https://docs.oracle.com/en/java/javase/17/migrate/migrating-jdk-8-later-jdk-releases.html#GUID-055EA9F4-835E-463F-B9E1-9081B3D9E55D)

Garbage collection (GC) logging uses the JVM unified logging framework, and there are some differences between the new and the old logs. Any GC log parsers that you’re working with will probably need to change.

You may also need to update your JVM logging options. All GC-related logging should use the gc tag (for example, —Xlog:gc), usually in combination with other tags. The —XX:+PrintGCDetails and -XX:+PrintGC options have been deprecated.

See [Enable Logging with the JVM Unified Logging Framework](https://docs.oracle.com/pls/topic/lookup?ctx=javase15&id=unified_logging) in the Java Development Kit Tool Specifications and [JEP 271: Unified GC Logging](https://docs.oracle.com/en/java/javase/17/migrate/migrating-jdk-8-later-jdk-releases.html#:~:text=JEP%20271%3A%20Unified%20GC%20Logging).

### [Migrating a Spring Boot application to Java 17 – the hard way](https://blog.codecentric.de/en/2021/12/migrating-spring-boot-java-17/)

### [Different Ways to Capture Java Heap Dumps](https://www.baeldung.com/java-heap-dump-capture)

* jmap: `jmap -dump:live,format=b,file=/tmp/dump.hprof 12587`
* jcmp: `jcmd 12587 GC.heap_dump /tmp/dump.hprof`
* JVisualVM: `Heap Dump” option`
* Capture a Heap Dump Automatically: `java -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=<file-or-dir-path>`
* JMX: `This MBean provides a dumpHeap method`
* JConsole: `we can navigate to the MBeans tab and find the HotSpotDiagnostic under com.sun.management`
* Programmatic Way: `we simply need to get an instance of a HotSpotDiagnosticMXBean, and call its dumpHeap method`

Explain it use [IntelliJ IDEA: Open an external profiling report](https://www.jetbrains.com/help/idea/open-an-external-profiling-report.html).

#### [Capturing a Java Thread Dump](https://www.baeldung.com/java-thread-dump)

* jstack: `jstack 17264 > /tmp/threaddump.txt`
* jmc: `Start Flight Recording`
* jvisualvm: `Thread Dump`
* jcmd: `jcmd 17264 Thread.print`
* jconsole: `open jconsole and connect to a running Java process`
* kill signal: `kill -3 17264`

Explain it use [IntelliJ IDEA: Analyze external stack traces](https://www.jetbrains.com/help/idea/analyzing-external-stacktraces.html).

#### [Delete a Directory Recursively in Java](https://www.baeldung.com/java-delete-directory)

#### [How can I create a Java 8 LocalDate from a long Epoch time in Milliseconds?](https://stackoverflow.com/a/35187046)

```text
LocalDateTime date = LocalDateTime.ofInstant(Instant.ofEpochMilli(longValue), ZoneId.systemDefault());
```

#### [AspectJ](https://www.baeldung.com/aspectj)

* [Introduction to Spring AOP](https://www.baeldung.com/spring-aop)
* [Introduction to Pointcut Expressions in Spring](https://www.baeldung.com/spring-aop-pointcut-tutorial)
* [Unable to configure Spring AOP with Gradle](https://stackoverflow.com/a/48485017)

#### [Spark Java Info from Oracle](https://blogs.oracle.com/javamagazine/java-spark-web-microservices)

#### [Apache HttpClient timeout](https://stackoverflow.com/a/63790234/7379661)

> Difference between the three timeouts in Apache HttpClient :
>
> `connectTimeout` max time to establish a connection with remote host/server.
>
> `connectionRequestTimeout` time to wait for getting a connection from the connection manager/pool. (HttpClient maintains a connection pool to manage the connections. Similar to database connection pool)
>
> `socketTimeout` max time gap between two consecutive data packets while transferring data from server to client.

#### [Concurrency and parallelism in Java](https://medium.com/@peterlee2068/concurrency-and-parallelism-in-java-f625bc9b0ca4)

#### [How the JVM Locates, Loads, and Runs Libraries](https://blogs.oracle.com/javamagazine/java-jvm-class-loaders)

#### [Escape Analysis in the HotSpot JIT Compiler](https://blogs.oracle.com/javamagazine/java-escape-analysis-optimization)

#### [Project Lombok: Clean, Concise Code](https://blogs.oracle.com/javamagazine/java-lombok-annotation)

#### [For Faster Java Collections, Make Them Lazy](https://blogs.oracle.com/javamagazine/java-lazy-collections-arraylist-hashmap)

#### [Annotations: An Inside Look: How annotations work, how best to use them, and how to write your own](https://blogs.oracle.com/javamagazine/java-annotations-inside-look)

## [Baeldung Java](https://www.baeldung.com/category/java/)

### [Groovy 3.0 Release Notes](https://groovy-lang.org/releasenotes/groovy-3.0.html)

较为重要的升级理由：

> * lambda expressions, e.g. stream.map(e → e + 1)
> * method references and constructor references

#### [Introduction to the Java 8 Date/Time API](https://www.baeldung.com/java-8-date-time-intro)

```java
// 使用 TimeUnit 完成毫秒到秒的转换
long millis = System.currentTimeMillis();
System.out.println("millis = " + millis);
long seconds = TimeUnit.MILLISECONDS.toSeconds(millis);
System.out.println("seconds = " + seconds);
```

#### [How Numeric literal with underscore works in java and why it was added as part of jdk 1.7](https://stackoverflow.com/a/19806674/7379661)

This is used to group the digits in your numeric (say for example for credit card etc)

From [Oracle Website](http://docs.oracle.com/javase/7/docs/technotes/guides/language/underscores-literals.html):

>In Java SE 7 and later, any number of underscore characters (_) can appear anywhere between digits in a numerical literal. This feature enables you, for example, to separate groups of digits in numeric literals, which can improve the readability of your code.
>
>For instance, if your code contains numbers with many digits, you can use an underscore character to separate digits in groups of three, similar to how you would use a punctuation mark like a comma, or a space, as a separator.

To conclude, it's just for a sake of readability.

```java
String hexString = Long.toHexString(1099595516468L);
System.out.println("hexString = " + hexString);

long defineUseHexFormat = 0x010005000a34L;
System.out.println("defineUseHexFormat = " + defineUseHexFormat);
```

#### [Java - 生成随机字符串](https://www.baeldung.com/java-random-string)

```java
int length = 10;
boolean useLetters = true;
boolean useNumbers = false;
String generatedString = RandomStringUtils.random(length, useLetters, useNumbers);

System.out.println(generatedString);
```

#### [Lombok Builder with Default Value](https://www.baeldung.com/lombok-builder-default-value)

#### [Lombok @Builder with Inheritance](https://www.baeldung.com/lombok-builder-inheritance)

#### [Check If a String Is Numeric in Java](https://www.baeldung.com/java-check-string-number)

```text
Benchmark                                     Mode  Cnt      Score     Error  Units
Benchmarking.usingCoreJava                    avgt   20  10162.872 ± 798.387  ns/op
Benchmarking.usingNumberUtils_isCreatable     avgt   20   1703.243 ± 108.244  ns/op
Benchmarking.usingNumberUtils_isParsable      avgt   20   1589.915 ± 203.052  ns/op
Benchmarking.usingRegularExpressions          avgt   20   7168.761 ± 344.597  ns/op
Benchmarking.usingStringUtils_isNumeric       avgt   20   1071.753 ±   8.657  ns/op
Benchmarking.usingStringUtils_isNumericSpace  avgt   20   1157.722 ±  24.139  ns/op
```

### [What is the difference between introspection and reflection?](https://stackoverflow.com/a/25199156)

#### [内省 (计算机科学)](https://zh.wikipedia.org/wiki/%E5%86%85%E7%9C%81_(%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6))

Java 中最简单的类型自省的例子是 `instanceof` 算符。例如：

```java
if (obj instanceof Person) {
    Person p = (Person)obj;
    p.walk();
}
```

`java.lang.Class` 类是更高级自省的基础。例如：

```java
System.out.println(obj.getClass().getName());
```

### [Guava - CaseFormat Class](https://www.tutorialspoint.com/guava/guava_caseformat.htm)

### [What is the simplest way to convert a Java string from all caps (words separated by underscores) to CamelCase (no word separators)?](https://stackoverflow.com/a/16667311/7379661)

### [Java Warning “Unchecked Cast”](https://www.baeldung.com/java-warning-unchecked-cast)

## 著名的库

### [Quick Guide to MapStruct](https://www.baeldung.com/mapstruct)

### [Google Guava User Guide](https://github.com/google/guava/wiki)

### [Apache Commons](http://commons.apache.org/)

#### [BeanUtils 1.9.4 User Guide](http://commons.apache.org/proper/commons-beanutils/javadocs/v1.9.4/apidocs/org/apache/commons/beanutils/package-summary.html#package_description)

##### [Bean映射工具之Apache BeanUtils VS Spring BeanUtils](https://pjmike.github.io/2018/11/03/Bean%E6%98%A0%E5%B0%84%E5%B7%A5%E5%85%B7%E4%B9%8BApache-BeanUtils-VS-Spring-BeanUtils)

【强制】避免用 Apache Beanutils 进行属性的 copy。说明：Apache BeanUtils 性能较差，可以使用其他方案比如 Spring BeanUtils, Cglib BeanCopier，注意均是浅拷贝。    ——[Java开发手册（嵩山版）](https://github.com/alibaba/p3c/blob/master/Java%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C%EF%BC%88%E5%B5%A9%E5%B1%B1%E7%89%88%EF%BC%89.pdf)

### [对象拷贝之Apache BeanUtils、Spring的BeanUtils、Mapstruct、BeanCopier、PropertieyUtils对比（深拷贝）](https://blog.csdn.net/ZYC88888/article/details/109681423)

### [常见 Bean 映射工具分析评测及 Orika 介绍](https://www.jianshu.com/p/40e0e64797b9)

```text
Benchmark                     Mode  Samples   Score  Score error  Units  
o.s.MyBenchmark.apache        avgt      100  25.246        0.535  us/op  
o.s.MyBenchmark.beanCopier    avgt      100   0.004        0.000  us/op  
o.s.MyBenchmark.byHand        avgt      100   0.004        0.000  us/op  
o.s.MyBenchmark.dozer         avgt      100   5.855        0.260  us/op  
o.s.MyBenchmark.orika         avgt      100   0.353        0.017  us/op  
o.s.MyBenchmark.spring        avgt      100   0.627        0.020  us/op  
```

统计报告中 Units 单位为微秒/次，由 Score 项可以看出，基于 ASM 的 cglib BeanCopier 拷贝速度基本和手写 get/set 方法的速度无异，其次的就是基于 javassist 的 Orika 了，Orika 的速度是 spring BeanUtils 的两倍，Dozer 的 20 倍，Apache BeanUtils 的 120 倍。

结论：默认使用 [Spring BeanUtils](https://github.com/spring-projects/spring-framework/blob/main/spring-beans/src/main/java/org/springframework/beans/BeanUtils.java), 可以增加依赖时使用 [Mapstruct](https://github.com/mapstruct/mapstruct).

## Java 枚举是自动可序列化的

* [Java Enums Are Inherently Serializable](https://www.infoworld.com/article/2072870/java-enums-are-inherently-serializable.html)
* [Is custom enum Serializable too?](https://stackoverflow.com/questions/15521309/is-custom-enum-serializable-too)

## Java libs and third party libs

* [Converting Iterable to Collection in Java](https://www.baeldung.com/java-iterable-to-collection)


## Postman

* [Dynamic variables](https://learning.postman.com/docs/writing-scripts/script-references/variables-list/), 随机变量列表。

## [Why use @PostConstruct?](https://stackoverflow.com/questions/3406555/why-use-postconstruct)

## JDK Mission Control

macOS 指定 JDK Mission Control 使用 JDK 版本, 打开 JMC 包内容的 Info.plist 将其中

```xml
<array>
    <string>-keyring</string>
    <string>~/.eclipse_keyring</string>
</array>
```

更新为

```xml
<array>
    <string>-keyring</string>
    <string>~/.eclipse_keyring</string>
    <string>-vm</string>
    <string>/Library/Java/JavaVirtualMachines/jdk1.8.0_261.jdk/Contents/Home/jre/</string>
</array>
```

即新增后 vm 参数，具体的值放在 array 最后一个参数上，以 jre 结尾。
 
## VisualVM

* [Mac OS 指定 VisualVM 使用 JDK 版本](https://github.com/oracle/visualvm/issues/13#issuecomment-280287797) create ~/Library/Application Support/VisualVM/2.0.4/etc/visualvm.conf file. Its content should look like this: visualvm_jdkhome=visualvm_jdkhome=/Library/Java/JavaVirtualMachines/jdk-11.0.8.jdk/Contents/Home 

## Java 正则表达式

* [replaceAll的妙用：正则表达式的捕获与反向引用](https://blog.csdn.net/kingzhsh/article/details/98247650)

## Java 中的 Native 关键字作用？

* [native keyword in java](https://www.geeksforgeeks.org/native-keyword-java/)

Native关键字的主要目标是：

* 改善系统性能。
* 实现机器级别或者内存级别的通信。
* 使用已经存在的遗留的非 Java 代码。

关于关键字 Native 的重点：

* 对于 Native 方法，一般使用 (C, C++) 等旧的语言来实现，Java 不负责提供实现。因此，Native 方法声明应以分号结尾。
* Native 方法不能被 abstract 修饰。
* Native 方法不能被声明为 static，因为不能保证旧语言 (C, C++) 遵循 IEEE 754 标准。
* Native 关键字的主要优点是性能提高，但是 Native 关键字的主要缺点是它破坏了 Java 的平台独立性。

那些类下有大量的 Native 方法：

* System
* Object
* Thread

Native 方法充分体现出"语言设计者实现出来的机制总是比开发者自己做的效率更高，因为他们可以不受语言本身的限制。 ——《函数式编程思维》"

## 为什么在 Java 中 System.arraycopy 方法是 Native 的?

* [Why is System.arraycopy native in Java?](https://stackoverflow.com/questions/2772152/why-is-system-arraycopy-native-in-java)
    * 在本机代码中，可以使用单个 memcpy/memmove，这与n个不同的复制操作memmove相反。性能差异很大。

## 为什么说 Java 中的 Date 类设计的比较失败？

* [What's wrong with Java Date & Time API?](https://stackoverflow.com/questions/1969442/whats-wrong-with-java-date-time-api)
    * 年数基于 1990，月数基于 0。
    * Date 被设计为可变的，结果是任何时候您想要返回一个日期（例如，作为实例结构），都需要返回该日期的副本而不是日期对象本身（否则，人们可以更改您的结构）。
    * Calendar 用来修复 Date 的问题，依然是可变的。
    * Date代表DateTime，但为了顺应SQL领域的需求，还有另一个子类java.sql.Date，代表了一天（尽管没有与之关联的时区）。
    * 没有TimeZone与关联的Date，因此范围（例如“全天”）通常表示为午夜至午夜（通常在任意时区）。
    * 它们具有双重性质。它们既代表时间戳又代表日历日期。事实证明，在对日期进行计算时这是有问题的。

* [Java Dates - What's the correct class to use?](https://stackoverflow.com/questions/8511430/java-dates-whats-the-correct-class-to-use)
    * Before Java 8: [Joda Time](https://github.com/JodaOrg/joda-time)
    * In or After Java 8: [Java Time](https://docs.oracle.com/javase/9/docs/api/java/time/package-summary.html)

## Java 中 DateFormat 是线程不安全的会导致什么问题？

* [“Java DateFormat is not threadsafe” what does this leads to?](https://stackoverflow.com/questions/4021151/java-dateformat-is-not-threadsafe-what-does-this-leads-to)
    * 如果您同时解析两个日期，则一个调用可能被另一个数据污染。
    * 在多线程环境中安全使用DateFormats的另一种方法是使用 ThreadLocal 变量来保存 DateFormat 对象，这意味着每个线程将拥有自己的副本，而无需等待其他线程释放它。
    * 如果您使用的是Java 8，则可以使用 [DateTimeFormatter](https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html)。

* [Why is Java's SimpleDateFormat not thread-safe?](https://stackoverflow.com/questions/6840803/why-is-javas-simpledateformat-not-thread-safe)
    * SimpleDateFormat 将中间结果存储在实例字段中。因此，如果两个线程使用一个实例，则它们可能会使彼此的结果混乱。
    * 您还可以使用线程安全的 [joda-time DateTimeFormat](https://www.joda.org/joda-time/apidocs/index.html)。
    * ThreadLocal + SimpleDateFormat = SimpleDateFormatThreadSafe
    
## 如何为 Java 编写符合规范的注释？

有如下两份资料以供参考：
    
* [How to Write Doc Comments for the Javadoc Tool](https://www.oracle.com/technetwork/articles/java/index-137868.html)
* [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html#s7-javadoc) 

## 为何标注 Java Thread 下部分方法为过时的？

* 官方文档：[Java Thread Primitive Deprecation](https://docs.oracle.com/javase/8/docs/technotes/guides/concurrency/threadPrimitiveDeprecation.html), 解释了为什么将如下几个方法标注为过时：
    * Thread.stop
    * Thread.suspend
    * Thread.resume
    * Thread.destroy
    * Runtime.runFinalizersOnExit
