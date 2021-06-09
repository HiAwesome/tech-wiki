# 与 Java 有关的链接

#### [Check If a String Is Numeric in Java](https://www.baeldung.com/java-check-string-number)

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

## Maven

### [IntelliJ IDEA: Maven dependencies](https://www.jetbrains.com/help/idea/work-with-maven-dependencies.html)

### [Maven Deploy to Nexus](https://www.baeldung.com/maven-deploy-nexus)

### Optional Dependency in Maven

* [Maven: Optional Dependencies and Dependency Exclusions](https://maven.apache.org/guides/introduction/introduction-to-optional-and-excludes-dependencies.html)
* [Baeldung: Optional Dependency in Maven](https://www.baeldung.com/maven-optional-dependency)

### [Understanding Apache Maven - The Series](https://cguntur.me/2020/05/20/understanding-apache-maven-the-series/)

### 使用 surefire 插件时设定快速失败

* [最小可用方案: mvn test | grep -w 'Running\|Tests'](https://stackoverflow.com/a/33739500)
* [Is there a way to “fail fast” for junit with the maven surefire plugin?](https://stackoverflow.com/questions/1923857/is-there-a-way-to-fail-fast-for-junit-with-the-maven-surefire-plugin/32640471)
* [Allow "fail fast" or stop running on first failure](https://issues.apache.org/jira/browse/SUREFIRE-580)
* [Skipping Tests After First Failure](https://maven.apache.org/surefire/maven-surefire-plugin/examples/skip-after-failure.html)
* [maven-surefire-plugin: skipAfterFailureCount](http://maven.apache.org/surefire/maven-surefire-plugin/test-mojo.html#skipAfterFailureCount)

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
