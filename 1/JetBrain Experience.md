# Jetbrain Experience

## [Official documents](https://www.jetbrains.com/help/idea/discover-intellij-idea.html)

### [Write and edit source code: Code reference information](https://www.jetbrains.com/help/idea/viewing-reference-information.html)

关于代码参考信息的基础操作：

* [Quick Definition](https://www.jetbrains.com/help/idea/viewing-reference-information.html#view-definition-symbols): Option + Space. 以弹窗的方式快速显示定义，作用于所有的 Symbol 对象，高频使用。
* [Quick Type Definition](https://www.jetbrains.com/help/idea/viewing-reference-information.html#quick-type-definition), 默认无快捷键，使用 `View | Quick Type Definition` 触发，显示 Symbol 对象的类型定义。经测试：一是 List 这样的集合则会显示为 List<E>；二是必须将光标置于对象之上，例如 `String symbol` 则只有 symbol 可以触发；三是复杂 Type 预览一部分意义不大，不如直接 Command + 光标点击进入类本身方便；所以低频使用。
* [Type Info](https://www.jetbrains.com/help/idea/viewing-reference-information.html#expression-static-data): Control + Shift + P. 显示类型信息，比较方便，高频使用。官方举例为在 `List<String> list = new ArrayList<>(); list.add("1");` 中光标放在第二个 list 之上，触发 Type Info 后会显示如下五条内容：
  * Type: List\<String\>
  * Nullability: non-null
  * Constraints: exactly ArrayList
  * Locality: local object
  * Size: 0
* [Context Info](https://www.jetbrains.com/help/idea/viewing-reference-information.html#productivity-tips): Control + Shift + Q. 官网一句话介绍 `If the current method or class declaration is not visible, you can view it in the tooltip`，经测试用于屏幕无法展示函数名时会显示函数名以及注解，无法展示类名时则展示类名以及注解，比较方便。

### [Analysis: Profiling tools](https://www.jetbrains.com/help/idea/cpu-profiler.html)

### [Analysis: Dependencies analysis: Dependency Structure Matrix](https://www.jetbrains.com/help/idea/dsm-analysis.html)

* [Youtube Video, 20200408: Profiling Tools and IntelliJ IDEA Ultimate](https://www.youtube.com/watch?v=OQcyAtukps4)
* [Async Profiler](https://www.jetbrains.com/help/idea/async-profiler.html)
* [Java Flight Recorder](https://www.jetbrains.com/help/idea/java-flight-recorder.html)
* [Run with a profiler](https://www.jetbrains.com/help/idea/run-with-profiler.html)
  * [如何读懂火焰图？](https://www.ruanyifeng.com/blog/2017/09/flame-graph.html)
  * [《性能之巅》学习笔记之火焰图 其之一](https://zhuanlan.zhihu.com/p/73385693)
  * [性能调优利器：火焰图](https://www.infoq.cn/article/a8kmnxdhbwmzxzsytlga)
* [Read the profiling report](https://www.jetbrains.com/help/idea/read-the-profiling-report.html)
* [Analyze memory snapshots](https://www.jetbrains.com/help/idea/analyze-hprof-memory-snapshots.html)
* [CPU and memory live charts](https://www.jetbrains.com/help/idea/cpu-and-memory-live-charts.html)

### [JVM frameworks: Spring](https://www.jetbrains.com/help/idea/spring-support.html)

* [Tutorial: Create your first Spring application](https://www.jetbrains.com/help/idea/your-first-spring-application.html)
* [Tutorial: Explore Spring support features](https://www.jetbrains.com/help/idea/spring-support-tutorial.html)
* [Spring diagrams](https://www.jetbrains.com/help/idea/spring-diagrams.html)
* [Spring Boot](https://www.jetbrains.com/help/idea/spring-boot.html)

## Debug

### [Debugging Code](https://www.jetbrains.com/help/idea/debugging-code.html)

* [Tutorial: Remote debug 远程 Debug](https://www.jetbrains.com/help/idea/tutorial-remote-debug.html), 本地按照教程使用非 jar 包运行的方式失败，原因是 [Unable to open debugger port through IntelliJ](https://stackoverflow.com/a/40641628), 以 jar 运行则成功。
* [Tutorial: Detect concurrency issues 检测并发问题](https://www.jetbrains.com/help/idea/detect-concurrency-issues.html)

### JDWP

Java 应用可以远程 Debug 借助了 JDWP 协议，即 [Java Debug Wire Protocol](https://docs.oracle.com/javase/8/docs/technotes/guides/troubleshoot/introclientissues005.html), [Java7 jdwp-spec](https://docs.oracle.com/javase/7/docs/technotes/guides/jpda/jdwp-spec.html).

一篇与此有关的文章质量较高，[Java远程调试（Remote Debugging）的那些事](https://www.jianshu.com/p/d168ecdce022).

> * 远程JVM调试怎么工作的
> 
> 一切源于被称作 Agents 的东西。
> 
> 运行着各种编译过的 .class 文件的JVM， 有一种特性，可以允许外部的库（Java或C++写的libraries）在运行时注入到 JVM 中。这些外部的库就称作 Agents, 他们有能力修改运行中 .class 文件的内容。
> 
> 这些 Agents 拥有的这些 JVM 的功能权限， 是在 JVM 内运行的 Java Code 所无法获取的， 他们能用来做一些有趣的事情，比如修改运行中的源码， 性能分析等。 像 JRebel 工具就是用了这些功能达到魔术般的效果。
> 
> 传递一个 Agent Lib 给 JVM, 通过添加 agentlib:libname[=options] 格式的启动参数即可办到。像上面的远程调试我们用的就是 **-agentlib:jdwp=... **来引入 jdwp 这个 Agent 的。
> 
> jdwp 是一个 JVM 特定的 JDWP（Java Debug Wire Protocol） 可选实现，用来定义调试者与运行JVM之间的通讯，它的是通过 JVM 本地库的 jdwp.so 或者 jdwp.dll 支持实现的。
> 
> * 它到底是怎么工作的呢？
> 
> 简单来说， jdwp agent 会建立运行应用的 JVM 和调试者（本地或者远程）之间的桥梁。既然他是一个Agent Library, 它就有能力拦截运行的代码。
> 
> 在 JVM 架构里， debugging 功能在 JVM 本身的内部是找不到的，它是一种抽象到外部工具的方式（也称作调试者 debugger）。这些调试工具或者运行在 JVM 的本地 或者在远程。这是一种解耦，模块化的架构。

## 代理

* JetBrains 挂代理：Preferences -> Appearance & Behavior -> System Settings -> Http Proxy: 选中 Manual proxy configuration -> HTTP，然后输入 Host 和 Port 分别是 127.0.0.1 和 6152。最后测试访问 Google 成功即可。
* [npm 加上代理](https://www.jhipster.tech/configuring-a-corporate-proxy/), npm config set proxy https://localhost:6152 && npm config set https-proxy https://localhost:6152 即可。
* Maven 设定使用代理
  * 在 Maven 的 setting 中设定 proxies 属性，参考 [Configuring a proxy](https://maven.apache.org/guides/mini/guide-proxies.html) 。
  * IDEA 中为单个项目配置代理需要为 Maven -> Importing -> VM options for importer 设定 -DproxySet=true -DproxyHost=127.0.0.1 -DproxyPort=6152
  * 当有项目依赖不需要代理时，在 IDEA 中 Maven -> Importing -> VM options for importer 设定 -DproxySet=false 
    ```xml
    <proxies>
        <!-- 设置 HTTP 代理 -->
        <proxy>
            <id>trojan-proxy</id>
            <active>true</active>
            <protocol>http</protocol>
            <host>127.0.0.1</host>
            <port>6152</port>
        </proxy>
    </proxies>
    ``` 

## Maven

* [Maven. Ignored Files](https://www.jetbrains.com/help/idea/maven-ignored-files.html), 在 Settings/Preferences | Build, Execution, Deployment | Build Tools | Maven | Ignored Files 中去掉打勾即可。
* IDEA Maven 项目报错，鼠标悬浮在 Maven 中显示 Problems: Cannot reconnect. 的解决办法 [Intellij Idea Maven 'cannot reconnect' error](https://stackoverflow.com/a/30615332), 即手动删除整个 ～/.m2/repository 文件夹，然后再次更新您的 Maven 项目。
* 删除 Maven 下载失败的包：搜索电脑上后缀名为 jar.lastUpdated 和 pom.lastUpdated 的文件并且全部删除即可。Windows 下用 Everything ( 语法 *.jar.lastUpdated 和 *.pom.lastUpdated ) ，Mac 下直接 Option + Command + Space ( 语法 jar.lastUpdated 和 pom.lastUpdated ) 。
* Maven 执行 IDEA打包执行 test 的插件：maven-surefire-plugin，具体见 [maven-surefire-plugin](https://maven.apache.org/surefire/maven-surefire-plugin/usage.html) 和 [Maven doesn't execute any unit test](https://stackoverflow.com/questions/16708013/maven-doesnt-execute-any-unit-test) 。打包报警：The parameter forkMode is deprecated since version 2.14. Use forkCount and reuseForks instead.useSystemClassloader setting has no effect when not forking，解决方案：[Maven surefire forkMode pertest deprecated. What is the new settings?
](https://stackoverflow.com/questions/40096382/maven-surefire-forkmode-pertest-deprecated-what-is-the-new-settings/40096619#40096619) ，[Migrating the Deprecated forkMode Parameter to forkCount and reuseForks](http://maven.apache.org/surefire/maven-surefire-plugin/examples/fork-options-and-parallel-execution.html#Migrating_the_Deprecated_forkMode_Parameter_to_forkCount_and_reuseForks) ，我在代码中进行了测试，还是会有报警提示，进一步查看问题是发现快捷键设置有误，出现报警提示是因为走了 Maven 的 Debug 模式。
* 使用 IDEA 打包带有 Scala 部分的 Maven 文件，首先要引入 Build 部分的插件 (1) **scala-maven-plugin** , (2) **maven-assembly-plugin[可选，一般用来打 jar-with-dependencies 的包]** , 然后在执行打包命令的时候，网络上的命令是：mvn clean scala:compile compile package , 但是因为使用了 Maven Helper 插件，所以省略最前面的 mvn , 也就是 **clean scala:compile compile package** 即可。
* Maven 执行 Scala 程序去掉一些 Warning 的常用处理方案：
    * Multiple versions of scala libraries detected!  ----->  在 scala-maven-plugin 的 configuration 中输入以下键值对：<scalaCompatVersion>2.11</scalaCompatVersion>    <scalaVersion>2.11.11</scalaVersion>    目前观测 scalaCompatVersion 是兼容版本， scalaVersion 是精确版本。
    * Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!  -----> 在 pom 文件中加入 <properties><project.build.sourceEncoding>UTF-8</project.build.sourceEncoding></properties>
* [Maven 设定自动下载 Source 和 Doc](https://www.baeldung.com/maven-download-sources-javadoc), 在 IDEA 中 Preference > Build, Execution, Deployment > Build Tools > Maven > importing 对 Sources、Documentation、Annotations 打勾，在 Maven Setting 中加入以下设定：
  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
  
      <profiles>
          <!-- downloadSources profile -->
          <profile>
              <id>downloadSources</id>
              <properties>
                  <downloadSources>true</downloadSources>
                  <downloadJavadocs>true</downloadJavadocs>
              </properties>
          </profile>
  
      </profiles>
  
      <activeProfiles>
          <activeProfile>downloadSources</activeProfile>
      </activeProfiles>
  
  </settings>
  ```
* 无法启动可能是因为使用 Proxifier 配置有问题，此代理只需要下载不到 maven 依赖或者插件的时候用，其他时候关闭。
* 默认 IDEA 全局 maven 依赖使用私服，demo 依赖自带 Bundled (Maven 3)，且 User setting file 和 Local repository 都不覆盖，意味着私服的 repository 需要不同地址，相互隔离。
* Maven 项目总是打包不成功，却能正常启动，请对每个项目进行 mvn clean install 操作。
* [Unable to find javadoc command: The environment variable JAVA_HOME is not correctly set](https://superuser.com/a/1367009), This is because IntelliJ's internal consoles use their own environment. You can set variables for Maven in the settings dialog under `Build, Execution, Deployment` > `Build Tools` > `Maven` > `Runner` > `Environment variables`. Add `JAVA_HOME` with whatever `echo $JAVA_HOME` returns in your usual terminal window as a value and Maven will be able to find the `javadoc` command!

## Tips

* To start a JavaScript debug session, hold Cmd+Shift and click the URL link. —— IntelliJ IDEA 2021.1
* Code format:
  * 选中了代码块按 `Command + Option + L`, 仅格式化选中部分的代码。
  * 未选中任何代码块按 `Command + Option + L`, 格式化这个文件。
  * 按 `Shift + Command + Option + L` 出现格式化菜单。范围可以三选一，分别是 Only VCS changed text, Selected text, Whole file；格式化选项三选多，分别是 Optimize imports, Rearrange code, Code cleanup.
* Completion with tab: This replaces the word at the caret rather than simply inserts it. 选中 IDEA 给出的建议然后使用 `Tab` 而不是 `Enter` 会替换插入符号处的元素而不是简单的插入。
* Collapse: 使用 `Command + -` 与 `Command + =` 收起展开单个方法，使用 `Command + Shift + -` 与 `Command + Shift + =` 收起展开所有方法。对类名之上的注解和导入语句同样适用。
* Expand and shrink the code selection: We moved the caret to the beginning of the `if` statement. Press `Option + 上` two times to select it. A keyword might be a good starting point for choosing the corresponding statement with
  just a few presses. 经测试对 if, for, method, class 都有效果，选择时只需要先将光标放在关键字最前面，然后按两次 `Option + 上` 即可，非常方便。
* IDEA 对 Java 中静态字段或方法的建议需要按 Control + Space 两次：Sometimes, you need to see suggestions for static constants or methods. Press `Control + Space` twice to ge them in the lookup.
* [Docker on Intellij IDEA](https://www.jetbrains.com/help/idea/docker.html), Docker 支持，需要熟悉。
* [Structural search and replace](https://www.jetbrains.com/help/idea/structural-search-and-replace.html), 结构化的搜索和替换。先熟悉基础语法和规则，再找适合场景测试。
* [宏: macros](https://www.jetbrains.com/help/idea/using-macros-in-the-editor.html), 功能存在，官网有简单的例子，目前还未找到特别适合的使用场景。
* Insert Live Template: Command + J, 很好的解决了在 Java 文档注释中无法触发类似于 fixme 这样的模板的问题。
* URL Mapping, shortcut `Shift + Command + \`, 适合在类似 Spring MVC 这样的 Web 项目中根据地址找到相关代码。
* [XPath search](https://www.jetbrains.com/help/idea/xpath-search.html), 搜索项目中 XML Files 符合 XPath 约束的内容，位置在顶级菜单 `Edit -> Find -> Find by XPath`, 默认快捷键 Option + Command + X.
* [Change the highlighting level for a file](https://www.jetbrains.com/help/idea/disabling-and-enabling-inspections.html#change-highlighting-level-for-file), 更改文件的突出显示级别，快捷键 Shift + Option + Command + H, 一般用于设定当前文件近检查语法而忽略低级语音规则（例如单词正确性）。
* [Explore test results](https://www.jetbrains.com/help/idea/viewing-and-exploring-test-results.html), 其中查看最近的测试列表快捷键为 Shift + Command + 分号 
* [File templates](https://www.jetbrains.com/help/idea/using-file-and-code-templates.html), 文件模版功能。主要看顶级菜单 `Tools -> Save File as Template...` 是否实用。语法为 Apache Velocity, 简易规则见 [VTL reference guide](http://velocity.apache.org/engine/2.0/vtl-reference.html).
* 方法交换入参的位置，需要光标放在入参上: Shift + Option + Command + 左/右
* 目前设定版本控制同文件对比仓库版本为 Option + V, 对比单文件差异时比较方便。
* 在顶级菜单 `Help -> Diagnostic Tools` 放置了一批 IntelliJ IDEA 检查自身的诊断工具，例如：
  * [Activity Monitor](https://www.jetbrains.com/help/idea/activity-monitor.html) 
  * Analyze Plugin Startup Performance, 分析插件启动时间
  * Profile Indexing, 诊断索引的问题
  * Profile Slow Startup, 诊断慢启动的问题
* Productivity Guide 中统计了大部分 Tips 及用户调用频率，目前已对呼出此菜单设定快捷键 Option + 右中括号。
* Keymap 的两种搜索方式：默认是用操作名找快捷键，点击 `Find Shortcut` 后则是用快捷键找操作名，结合使用较好。
* [Join Lines](https://www.jetbrains.com/help/idea/working-with-source-code.html#editor_lines_code_blocks): Control + Shift + J, 合并行操作，经测试与 Command + Enter 为互逆操作，比较方便。 
* [Add Carets to Ends of Selected Lines](https://www.jetbrains.com/webstorm/guide/tips/add-carets-at-line-end): Option + Shift + G, 经测试是多行编辑的行尾模式，等于多选行之后按 Command + 右键。
* [IDEA 中全屏工具窗口](https://stackoverflow.com/a/36046318), Mac 默认快捷键是 Shift + Command + ' 最后一个按键是单引号。
* [IDEA 中导航到下一个引用上](https://www.jetbrains.com/help/idea/find-highlight-usages.html), 首先 Command + Shift + f7 使得元素变成高亮状态，然后使用 Command + G, Command + Shift + G 进行跳转, 也可以使用 Ctrl + Command + G 进入所有命中多处编辑模式。
* [IDEA 中文件的多行编辑操作](https://www.jetbrains.com/help/idea/working-with-source-code.html#multiple_cursor)
    * [IntelliJ IDEA way of editing multiple lines](https://stackoverflow.com/a/27862706)。Clone Caret Above / Below 功能: 需要先按一次 option，第二次再按下 option 不释放(两次的间隔时间可以稍微长点，目前 Mac 配置 double option 会激活 Alfred 弹窗), 这个时候使用上下键进行多行光标的插入。
    * Column Selection Mode: 这个功能使得光标可以选择文本编辑器的任何位置，而不是局限于有代码的部分。如果打开了状态栏，会看到显示文本编码的部分显示 Column 字样。Mac 默认快捷键是 Shift + Command + 8, 但是这里有一个问题，当按下 Shift 时数字 8 实际上变成了 \*, 导致无法触发，故而设定快捷键 Option + 8 触发 Column Selection Mode 模式。为了方便使用后序的 Clone Caret Above / Below 功能，也配置了相对应的快捷键，其中 Clone Caret Above 配置了 Option + 9, Clone Caret Below 配置了 Option + 0.
* [IDEA 设置 go 代码显示时中不自动折叠 return/panic/format 语句](https://stackoverflow.com/a/59320983): Go to Settings/Preferences | Editor | General | Code Folding | Go and toggle them on/off as needed.
* [IDEA 显示 Kotlin not configured](https://stackoverflow.com/a/64404454), 在 Gradle 项目中使用命令 rm -rf .idea .gradle gradle
* [IDEA 移除外部包装代码块](https://stackoverflow.com/a/8882692), **Code | Unwrap/Remove...** Command + Shift + fn + Backspace on Mac, Ctrl + Shift + Delete On Windows. 这个快捷键和 `Command + Option + T` Surround 为相反操作，可以轻易对于语句块加上 `try / catch / final` 然后又去掉。
* [IDEA 设置不再生成 Java 方法的 @param 和 @return 注释](https://www.jetbrains.com/help/idea/working-with-code-documentation.html#add-new-comment), Disable automatic comments: In the **Settings/Preferences** dialog, go to **Editor | General | Smart Keys**, and clear the **Insert documentation comment stub** checkbox.
* IDEA 设置保存文件时去掉自动生成的回车： Setting -> Editor -> General, 去掉 Ensure an empty line at the end of a file on Save 的打勾。 
* JetBrains 产品禁用双击 Shift 的 Search Everywhere，参考 [How do I disable the Search Everywhere shortcut?](https://stackoverflow.com/a/48894157) :
    1. 打开 Find Action...（在 Windows 和 Linux 上为“ Ctrl-Shift-A”，在 macOS 上为“ Cmd-Shift-A”）
    2. 键入“ Registry...”，按 Enter
    3. 找到 key 为 ide.suppress.double.click.handler（只需开始键入即可进行速度搜索）并启用它（打对勾）
* IDEA 中临时 SQL 文件运行时需要绑定 Session（运行环境），如果需要去除的话点击右上角的固定 Session 并选择 Detach Session 即可。
* **IDEA 查找 Database 插件的表对象、存储过程对象，调用 Symbols 查询即可，Mac 快捷键为 Option + Command + O**
* IDEA 装上了刷 LeetCode 的插件，定义临时文件地址为 Scratch 文件夹下的 leetcode 文件夹，操作起来非常直接顺手。
* **LeetCode 账号美区为 a 结尾，中区为 b 结尾，密码一致。**
* IDEA 使用 lombok 插件无效，在设置中找到 Setting -> Build, Execution, Deployment -> Compiler -> Annotation Processors，然后打勾 Enable annotation processing。
* git 修改提交者的名称和邮箱：
    * 当前用户名：git config user.name 你的目标用户名;
    * 当前邮箱：git config user.email 你的目标邮箱名;
    * 全局用户名：git config  --global user.name 你的目标用户名；
    * 全局邮箱：git config  --global user.email 你的目标邮箱名;    
* IDEA 安装 lombok 插件用来抑制代码监测中找不到 lombok 的报错。
* 整理 IDEA 的细分使用教程：
    * [Introduction to Version Control Systems in IntelliJ IDEA](https://www.youtube.com/watch?v=MaQnpCaiop0) ：① Git 的 commit message 框右上角的钟表标志可以显示最近的所有 commit message，然后选择一个开始做修改比较方便。② Git 的 commit and push 有快捷键，Mac 默认是 Option + Command + K。
* JetBrains APP 可能在粘贴 SQL 语句的时候出现问题，比如单引号出现了两次导致语法出错，全选重写粘贴一遍即可。
* IDEA 设置复制进来文件 [不自动格式化](https://intellij-support.jetbrains.com/hc/en-us/community/posts/207280009-Paste-without-auto-formatting) ：Settings/Preferences | Editor | General | Smart Keys --> Reformat on Paste 设置为 None 即可。
* 设置了 IDEA 单文件查 History 的快捷键：Control + Command + H。
* 在目前版本的 IDEA（2019.1.2）中，局部变量被重新赋值的话 IDEA 默认为这个变量名显示加上下划线，相当于 Java 中 Final 修饰局部变量名的作用。
* 在 scratches 文件夹下建立分格式的文件夹是良好的工程实践，目前这个文件夹地址在：/Users/moqi/Library/Preferences/IntelliJIdea2019.1/scratches 目录，优势有两点：
    * 这个文件夹各个工程共享，对在一个项目中对某一文件的操作可以快速反应到其他项目中。
    * 建立 txt json sql 等文件夹可以把临时文件中有意义的文件重命名并放在对应文件夹下，方便归类检索。
    * 有关 Scratch 文件的更多介绍请看 [Scratch files](https://www.jetbrains.com/help/idea/scratches.html) 。
* [IntelliJ IDEA中文乱码解决](https://www.jianshu.com/p/fc012cc4a9d9) 。
* IDEA [文件对比文件](https://www.jetbrains.com/help/idea/comparing-files-and-folders.html) ：在一个文件上右键选择 Compare With（这里也可以使用快捷键 Command + D），然后 IDEA 会打开 Finder 选中第二个文件开始比较。也可以比较文件夹，链接有比较详细的说明。最后是打开 Diff 比较器出现两个文本比较框，非常实用，目前设置快捷键是 Control + Shift + Command + D。
* IDEA 提供直接载入 Github 项目功能，需要安装 [Chrome 插件: JetBrains Toolbox Extension](https://chrome.google.com/webstore/detail/jetbrains-toolbox-extensi/offnedcbhjldheanlbojaefbfbllddna) ，然后 Git 项目旁边会有 IDEA 图标，点击即可（前提需要在本机配置好 SSH public key 到 Github 账户，如果出现克隆失败参考 Github 关于 SSH key gen 的说明即可）。
* IDEA 提供 [SSH 保存功能](https://stackoverflow.com/questions/33938824/why-intellij-doesnt-provide-functionality-to-store-ssh-sessions/37074514#37074514) :
    * 您可以在以下设置 ssh 连接 Tools | Deployment | Configuration。
    * 使用加号按钮添加新服务器。输入名称并选择 SFTP 作为类型。
    * 现在您可以输入主机，用户名和密码（或密钥对），**注意这里可以不进行 Test 测试，只要保存了相关信息即可**。
    * 下次你选择 Tools | Start SSH session 应该有你的服务器名称。
* 登录 JetBrains 账号的 IDEA 不能选择导出配置，关闭 Sync 配置即可，然后将配置导入 DataGrip 使用 Mac 10.5 + 的 keymap。
* IDEA 设置不在同一行显示目录名称（方便多文件比较）, Preferences -> Editor -> General -> Editor Tabs：**去掉 Show tabs in one row 的对号或者打勾**，其他 JetBrain 的编辑器应该同样适用。
* **PyCharm 使用 Python Console 输出**，需要在 Run/Debug Configuration 里面给 Run With Python Console 打勾，取消打勾则在默认输出 Console 输出。启用 Python Console 可以和代码中的方法进行交互式运行。
* **使用 IDEA 运行 Spark 程序，日志配置文件无效解决方案**：
    * 在 VM options 中加入  -Dlog4j.debug, 查看是哪个 jar 的配置文件覆盖了项目自身 log4j.properties, 确定被覆盖且 jar 包不可拆分时进行下一步。
    * 在 VM options 中加入 -Dlog4j.configuration=file:/Users/moqi/opt/data/log4j_dir/log4j-error.properties 为项目指定 log4j.properties.
    * 如果是在 Spark 部署环境下则参考 [How to log using log4j to local file system inside a Spark application that runs on YARN?](https://stackoverflow.com/questions/28454080/how-to-log-using-log4j-to-local-file-system-inside-a-spark-application-that-runs)
* 应该把开源项目自身放在一个 IDEA 项目里面，比如 Spark、Hadoop 等, 在此项目中查看源码方便快捷好用。
* [IDEA 的 42 个技巧 On Youtube](https://www.youtube.com/watch?v=eq3KiAH4IBI)
* IDEA [JavaDoc 注释加上 URL](https://stackoverflow.com/questions/1082050/linking-to-an-external-url-in-javadoc) 的语法是：@see \<a href="http://google.com">http://google.com</a\> 在 ScalaDoc 注释加上 URL 的语法是 [[http://scala-lang.org Scala web site]] 
* Mac 上 IDEA 快捷键将 Command + 2 给结构( Structure )，将 Command + 3 给继承关系( Hierarchy )。
* 鼠标悬停在打开类选项卡上面显示文件的绝对路径，按住 Ctrl 鼠标左键点击打开多层目录选择一个打开文件目录。
* 运行web项目没有跑起来首先检查是否把 resources 文件标记为合适的文件类型，如果无法读取 mapper 配置文件，而文件目录在java下，记得配置合适的扫描
* 新装 JRebel 或者项目 使用的时候 需要在 JRebel Panel 块点击启用。
* IDEA通过左边项目栏的 "Show Members" 控制"定位"展示到"类"的程度，还是"类里方法"的程度。
* SVN 提交的时候选择 Optimize imports 和 Cleanup ，Git只选择 Optimize imports 即可。
    * Optimize imports 优化导入包，会自动去掉没有使用的包。
    * Cleanup 清除下版本控制系统，去掉一些版本控制系统的错误信息。
* 打断点的时候，在 Log message to console 下有个 Evaluate and log 选项，一般写 System.err.println() 语句，从颜色上和程序本身代码区分开来，好用。
* 可以给成员变量打断点，断点图标内是一个横线，可以选择此成员变量在下列情况下进入断点：①被访问；②被改变值。
* **项目中定义 JSON 格式的时候，记得所有的 key 都只作为容器，而不具有业务数据意义。比如用户名密码登录，前端给后端传递的 JSON 应该是 {"username":"zhangsan", "password":"123456"} 而不是 {"zhangsan":"123456"}，即使是最复杂的 JSON 也要遵守这个约定，确保可以看到所有参数的实际意义。**
* 配置控制台的编码：
    * 在 Windows 和 Linux 中：在配置文件下 -Dconsole.encoding=UTF-8
    * 在 macOS 中：打开 Info.plist 位于 /Applications/RubyMine.app/Contents，找到标签<key>VMOptions</key>，并修改它如下<key>VMOptions</key>  <string>-Xms16m -Xmx512m -XX:MaxPermSize=120m -Xbootclasspath/p:../lib/boot.jar -ea -Dconsole.encoding=<encoding name> </string>
* 初始使用 IDEA 的时候设定剪贴板保存数和最近文件数：
    * 路径：Setting -> Editor -> General -> Limits：
    * Maximum number of contents to keep in clipboard： 目前设置 10 (原 5)
    * Recent files limit：目前设置 10 (原 50)
* 保存和还原更改：使用IntelliJ IDEA时，您不必担心保存更改的文件：所有更改都会自动保存。您的开发工作流程的任何阶段都可以撤消不需要的更改。任何文件或目录都可以恢复到以前的任何状态。原文：
    * Saving and Reverting Changes：
    * When working with IntelliJ IDEA, you don't need to worry about saving changed files: all changes are auto saved. 
    * Unwanted changes can be undone at any stage of your development workflow. Any file or directory can be reverted to any of the previous states.
* 在测试项目进行创建方法的时候，从 test100 开始，这样每次减一每次都在最上面。
* 在选中代码块进行替换的时候，记得先 Ctrl + R，然后选择 In Selection。
* **项目中使用 lambda 表达式的时候，记得写注释过渡。**
* IDEA 关闭 Java 自动导包：设置 -> search 'auto import' -> 去掉 Add unambiguous imports on the fly 的√ 
* **IDEA debug 的断点不起作用处理方式**：
    * If you are using Maven dependencies, go to Maven Projects -> refresh
    * If that does not work, Try top menu --> Build --> Rebuild Project
    * If that still doesn't work, try top menu --> File --> Invalidate Cache/Restart
    * If that still doesn't work, then $CATALINA_BASE/bin/catalina.sh stop, then start
* 设置 IDEA 打开 markdown 文件默认 show preview only：Settings --> Languages & Frameworks --> Markdown --> Default layout : Preview only
* 善用 IDEA 的 bug 分组，将一块的 bug 同时打开或者关闭，提高效率。
* **IDEA如果报黄线或者红线，即警告或者错误时查看中文示意的处理方法**：
    * 光标放在问题处，Alt + Enter 会出现一般的解决方案，这个时候不要按回车，按 →
    * 出现一堆选项，选择最上面 Edit inspection profile setting
    * 在 Inspections 界面，左边会出现问题的原因，右边的 Description 会出现具体的描述，Ctrl + A 全选借助翻译插件 Ctrl + Shift + Y 得到中文示意。
* Setting -> Editor -> Code Style -> Java -> Code Generation: Final Modifier，下面两个选项分别表示提取为成员变量或者提取为参数变量加 final 修饰。如果常有校验值是否只被赋值一次的需求，可以勾选。
* 在 Mac 或者 Linux 系统运行项目，run 可以，但是 debug 报错，内容是 Required connector 'com.sun.jdi.SharedMemoryListen' not found. Check your JDK installation. 这个时候在设置 Build, Wxecution, Deployment -> Debugger 中勾选 Show alternative source switcher 即可。
* 在 Mac 系统下如何找 IDEA 安装路径（适用于用 Jetbrain Toolbox 管理的情况）：先 Alfred 搜索 idea 进入 Toolbox 目录下 IDEA，然后再"显示包内容"看到 Contents，点进去看到 info.plist, 以文本方式打开这个文件，就可以看到最下面映射的真是目录，例如此条笔记记录时本机的目录是：<string>/Users/moqi/Library/Application Support/JetBrains/Toolbox/apps/IDEA-U/ch-0/182.3911.36/IntelliJ IDEA.app</string>
* IDEA 设定单行注释不从一行的最开头开始，在设置里找对应语言的 Line comment at first column 属性，去掉前面的对勾即可。Java 的话可以在下面的 Add a space at comment start 打对勾，这样注释就从当前行有字的地方开始，并且有一个空格。
* IDEA 如果在 make 或 rebuild 过程中很慢，可以增加此（Setting -> Build, Execution, Deployment -> Compiler -> Build process heap size (Mbytes)）堆内存设置，一般大内存的机器设置 1500 以上都是不要紧的。目前 MacBook 设定为 3000。
* 在 Setting -> Editor -> General -> Appearance 下有两个设置：默认 IntelliJ IDEA 是没有勾选 **Show line numbers** 显示行数的，建议一般这个要勾选上。默认 IntelliJ IDEA 是没有勾选 **Show method separators** 显示方法线的，这种线有助于我们区分开方法，所以也是建议勾选上的。
* 当我们设置了组件窗口的 **Pinned Mode** 属性之后，在切换到其他组件窗口的时候，已设置该属性的窗口不会自动隐藏。也就是说关闭了这个属性，则使用完一个组件窗口的时候就自动隐藏了。
* 我们可以根据选择的代码，查看该段代码的本地历史，这样就省去了查看文件中其他内容的历史了。除了对文件可以查看历史，文件夹也是可以查看各个文件变化的历史。（选中代码中某一段，右键单击 Local History -> **Show History for Selection**）
* **IDEA 出现 " [错误：找不到或无法加载主类](https://stackoverflow.com/questions/10654120/error-could-not-find-or-load-main-class-in-intellij-ide) "**：解决方案：1. 如果原来或者刚才一直可用，突然出现这种情况，特别是对其他语言比如说 Scala 的类出现这种情况，则首先怀疑是文件夹并没有设置为 (Sources Root) 属性，如果在测试包下，则需要设定为 (Test Sources Root) 属性，**目前发现在未操作的情况下 IDEA 可能会将其他语言的包设定为(Unmark as Sources Root) 属性**。 2. 清空 IDEA 缓存: IDEA -> file -> invalidate Cache/restart 
