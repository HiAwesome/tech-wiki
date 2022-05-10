# Gradle

#### [What is detachedConfiguration? I have a lots of them for each subproject and resolving them takes 95% of build time](https://discuss.gradle.org/t/what-is-detachedconfiguration-i-have-a-lots-of-them-for-each-subproject-and-resolving-them-takes-95-of-build-time/31595)

can’t explain what exactly a detached configuration is, but I remember similar problems from when we were using the Spring dependency management plugin. I think it has to do with how the Spring plugin “misuses” configurations to achieve it’s goal.

**移除奇怪的 plugin 解决这个问题。**

#### IDEA show: The JavaExec.main property has been deprecated.

* [Lot's of deprecation warnings when running against Gradle 7.1](https://github.com/JetBrains/gradle-intellij-plugin/issues/700)
* [youtrack: gradle: The JavaExec.main property has been deprecated](https://youtrack.jetbrains.com/issue/IDEA-279280)

## Gradle Major Version

发布版本越来越快。

* [20210410: 7.0](https://github.com/gradle/gradle/releases/tag/v7.0.0)
* [20191109: 6.0](https://github.com/gradle/gradle/releases/tag/v6.0.0)
* [20181126: 5.0](https://github.com/gradle/gradle/releases/tag/v5.0.0)
* [20170614: 4.0](https://github.com/gradle/gradle/releases/tag/v4.0.0)
* [20160919: 3.0](https://github.com/gradle/gradle/releases/tag/v3.0.0)
* [20140620: 2.0](https://github.com/gradle/gradle/releases/tag/REL_2.0)
* [20120612: 1.0](https://github.com/gradle/gradle/releases/tag/REL_1.0)
* [20080422: 0.1](https://github.com/gradle/gradle/releases/tag/REL-0.1)

## [实战 Gradle](https://book.douban.com/subject/26609447/)

> * 你所需要的是一个可编程的工具，它能够让你以可执行和有序的任务来表达自动化需求。
> * 要理解Ant的构建脚本, 你需要先了解一些命名规范。一个构建脚本由三个基本元素组成:一个project、多个target和可用的task。
>   * 使用XML作为构建逻辑的定义语言相比于其他更简明的定义语言,会导致构建脚本过于臃肿和啰嗦。
>   * 复杂的构建逻辑会导致又长又难以维护的构建脚本。当尝试用标记语言去定义类似if-then/if-then-else的逻辑语句时,它完全就成了一种负担。
> * Maven 中的依赖管理不仅限于外部库。你也可以将其他 Maven 项目定义为依赖，这种需求出现的原因是软件分解成了多个模块，每个模块都是完成某项功能的组件。
> * 和Ant一样,也要知道 Maven 的缺点:
>   * Maven 推荐一个默认的结构和生命周期,常常会太过限制,也许不适合你的项目需求。
>   * 为 Maven 写定制的扩展过于累赘。你需要去学习Mojos( Maven 的内部扩展API)，如何提供一个插件描述符(又是XML),以及相关的特殊注解,以便提供扩展实现所需要的数据。
> * Gradle 是基于JVM构建工具的新一代版本。它从现有的构建工具如Ant和 Maven 中学到了很多东西,并且把它们的最优思想提升到更高层次。遵循基于约定的构建方式, Gradle 可以用一种声明式的方式为你的问题领域建模,它使用一种强大的且具有表达性的基于Groovy的领域特定语言(DSL),而不是XML。因为Grade是基于JVM的,它允许你使用自己最喜欢的Java或者Groovy语言来编写定制逻辑。
> * Gradle 的座右铭："让不可能成为可能,让可能变得简单,让简单变得优雅"。Make the impossible possible, make the possible easy, and make the easy elegant.
> * 不要改变一个正在运行的系统, 你说呢? 你的团队已经花费大量的时间来建立项目构建代码基础设施。Gradle 并不强迫你完全迁移所有的构建逻辑。它和其他构 建工具如 Ant 和 Maven 有非常好的集成, 这是 Gradle 优先级列表中的最高优先级。
> * 难道你不讨厌给不同的项目安装新的运行时环境? Gradle包装器是救星!它允许你在任何需要运行构建的机器上从一个指定的仓库下载和安装一个 Gradle 运行时的新拷贝。这个过程是在第一次构建执行时自动触发的。包装器对于给一个发布团队分享你的构建或者在持续集成服务器上运行构建是非常有用的。
> * 如果想要深入了解持续交付和所有相关方面的内容,我推荐 Jez Humble 和 David Farley 所著的 Continuous Delivery: Reliable Software Releases through Build, Test, and Deployment Automation(Addison Wesley, 2010)
> * 每个 Gradle 构建都是以一个脚本开始的。 Gradle构建脚本默认的名字是 build.gradle。当在 shell 中执行 gradle 命令时, Gradle 会去寻找名字是 bui1d.gradle 的文件。如果找不到, 就会显示一个帮助信息。
> * 任务通常只会执行一次, 无论它们是在命令行中指定的还是作为另一个任务的依赖。
> * 任务名字缩写: Grade 最有用的特性之一就是能够以驼峰式的缩写在命令行上运行任务。（简单任务使用，复杂任务不适合）
> * 在执行时排除一个任务：有时候你想要在构建运行时排除一个指定任务。 Gradle 提供了一个命令行选项-x来实现。
> * gradle 命令允许你同时定义一个或者多个选项。假设你想要将日级别改变到 INFO, 则可以使用 -i 选项,或者如果想要打印出在执行期间发生错误时的堆栈踪迹信息, 则可以使用 -s 选项。比如: gradle grouptherapy-is 或者 gradle grouptherapy-i-s .
> * Gradle守护进程:
>   * 当 Gradle 成为日常工作的一部分时,你会发现需要重复地运行构建。如果你在一个Web应用上工作,就更是如此。当你改变一个类时,就需要重新构建,然后部署到服务器,刷新页面看浏览器上的变化。许多开发人员喜欢测试驱动的开发方式为了持续地获得关于代码质量的反馈,他们会不断地运行单元测试以更早地发现代码缺陷。不管哪种方式,你都会发现效率很重要。每次初始化一个构建时, JVM都要启动一次, Gradle 的依赖要载入到类加载器中,还要建立项目对象模型。这个过程需要花上好几秒钟。 Gradle守护进程是这个问题的救星!
>   * 守护进程以后台进程方式运行 Gradle。一旦启动, gradle命令就会在后续的构建中重用之前创建的守护进程,以避免启动时造成的开销。在命令行中启动 Gradle 守护进程很简单: 在运行gradle命令时加上 --daemon 选项。你可能会注意到在启动守护进程时也花费点点额外的时间。
>   * 后续触发的 gradle 命令都会重用宇护进程。 守护进程只会被创建一次, 即便你在后续的命令行中加了-daemon选项。守护进程会在3小时空闲时间之后自动过期。任何时候你都可以选择在执行构建时不使用守护进程, 只需要添加命令行选项- -no-daemon即可。要手动停止守护进程, 可以执行 gradle --stop 命令。
> * 你也许注意到某些任务被标记为 UP-TO-DATE 消息。这意味着这个任务被跳过了。 Gradle 的增量式构建支持自动鉴别不需要被运行的任务。特别是在大型的企业级项目中,这个特性是节省时间的好帮手。
> * 改造遗留项目: 很少有企业级软件项目是在一个干浄的环境下开始的。和一个遗留系统集成, 迁移已有项目的技术機, 或者坚持内部标准或者限制, 实在是太常见了。构建工具必须足够灵活, 可以通过改变默认配置来适应来自外部的限制。
> * 定制一个构建的关键就是了解潜在的属性和DSL元素。
> * 针对这个问题, Gradle提供了一个非常方便和实用的解决方案: Gradle包装器。 它是 Gradle的核心特性,能够让机器在没有安装 Gradle运行时的情况下运行 Grade 构建。它也让构建脚本运行在一个指定的 Gradle版本上。它是通过自动从中心仓库下载 Gradle运行时,解压和使用来实现的。最终的目标是创造一个独立于系统、系统配置和 Gradle版本的可靠和可重复的构建。
> * 什么时候使用包装器: 使用包装器被认为是最佳实践, 对每一个 Gradle 项目都是必需的。由包装器支持的 Gradle 脚本非常适合作为自动化发布过程的一部分,比如持续集成和交付。
> * task依赖的执行顺序: 理解 Gradle并不能保证task依赖的执行顺序是很重要的。 dependson方法只是定义了所依赖的task需要先执行。 Gradle 的思想是声明在一个给定的task执行之前什么该被执行, 而没有定义它该如何执行。 如果你之前使用过像Ant这种命令式地定义依赖的构建工具的话,那么 Gradle 的这个概念就有点难以理解了。在本章的后续部分你将看到, 在 Gradle中, 执行顺序是由task的输入/输出规范自动确定的。 这种构建设计有很多好处。一方面, 你不需要知道整个task依赖链上的关系是否发生改变,这样可以提高代码的可维护性和避免潜在的破坏。另一方面,因为构建没有严格的执行顺序,也就是支持task的并行执行,这样可以极大地节约构建执行时间。
> * Gradle构建生命周期阶段: 无论什么时候执行 Gradle 构建, 都会运行三个不同的生命周期阶段: 初始化、配置和执行。
> * Gradle 通过比较两个构建 task 的 inputs 和 outputs 来决定 task 是否是最新的。自从最后一个task执行以来,如果 inputs 和 outputs 没有发生变化, 则认为task是最新的。因此,只有当 inputs和 outputs不同时, taskオ运行; 否则将跳过。
> * builds目录被视为 Gradle项目的指定路径。由于没有编写任何单元测试所以跳过了编译和执行task。
> * 不要害怕使用生命周期钩子。它们不是 Gradle API 的秘密后门。相反, 它们是特意提供的,因为 Gradle 无法预测企业级构建的需求。
> * 通过监听器挂接到构建生命周期只需两个简单的步骤。首先,通过在构建脚本中编写一个类来实现特定的监听器接口。其次,注册监听器实现。
> * 针对版本冲突 Gradle默认的解决策略是获取最新的版本一一也就是说, 如果依赖图中包含了同一个类库的两个版本, 那么它会自动选择最新的。比如 xml-api 类库, Gradle会选择 1.3.03 而不是 1.0.b2, 通过一个箭头 \(->\) 来指示。
> * Gradle 可以让你完全控制传递性依赖, 因此你可以決定排除所有的传递性依赖或者有选择性地排除某些依赖。 在实践中,基于 API 或者框架的一个特定版本来构建自己的一些功是很常见的。
> * 动态版本声明: 动态版本声明有一个特定的语法。如果你想使用最新版本的依赖,则必须使用占位符 latest.integration。 或者, 声明版本属性, 通过使用一个加号 \(+\) 标定它来动态改变。
> * 什么时候使用动态版本? 简单来说最好是少用或者不用。可靠的和可重复的构建是最重要的。选择最新版本的类库可能会导致构建失败。更糟糕的是, 在不知情的情况下,你可能引入了不兼容的类库版本和副作用,这样很难找到原因并且只在应用程序的运行时发生。因此, 声明确切版本的类库应该成为惯例。（个人补充：个人的 SideProject 用起来应该比较合适）
> * Repositoryhandler接口提供了两个方法来允许你定义预配置的 Maven 仓库。 mavencentral() 方法用来将 Maven Central 引用添加到一系列仓库中, mavenlocal() 方法用来在文件系统中关联一个本地 Maven 仓库。
> * Gradle缓存的真正能力在于其元数据。它让 Gradle 实现了额外的优化, 使得构建更智能、更快捷、更可靠。
> * Gradle 通过比较本地和远程的校验和来检测仓库中的工件是否发生变化。 没有发生变化的工件不会再次下载, 并且会重用本地缓存中的。 想象一下仓库中的工件发生了变化, 但校验和仍是相同的。 如果仓库管理员用相同版本的工件取代了原来的, 就会发生这种情况。 最终, 构建将使用一个过时版本的工件。 Gradle 依赖管理器试图通过考虑额外的信息来消除这种情况。例如,可以通过比较 HTTP 头参数 contentLength 的值或者最后的修改日期来确保工件的唯一性。这是 Gradle 的实现相比于其他的依赖管理器如 Ivy 的优势所在。
> * 在多项目构建中是通过 settings 文件来声明子项目的。settings 文件声明了所需的配置来实例化项目的层次结构。 在默认情况下, 这个文件被命名为 settings.gradle, 并且和根项目的 build.gradle文件放在一起。 若想使每个子项目都成为构建的一部分, 则可以调用带有项目路径参数的 include 方法。
> * 并不是所有的自动化测试都一样。它们通常在使用范围、实现难度和执行时间上存在不同。我们将自动化测试分为三种类型: 单元测试、集成测试和功能测试
>   * 单元測试与产品代码实现一起作为一个 task 来执行, 目的是测试代码的最小单元。
>   * 集成測试用来測试一个完整的组件或子系统。你想要确保多个类之间的交互是否按预期运行。集成测试的一个典型场景就是验证产品代码和数据库之间的交互。
>   * 功能测试通常用于测试应用程序的端到端功能,包括从用户的角度与所有外部系统的交互。当我们谈论用户的角度时,通常指的是用户界面。
> * 在项目中如何配置并行执行测试依赖于目标硬件和测试类型(CPU 或 IO 限制)。试着设置数量值以找到平衡点。 为了获得更多的关于如何找到最佳平衡点的信息, 建议你阅读 Venkat Subramaniam 所写的 Programming Concurency on the JVM (The Pragmatic Programmers, 2011)。
> * 请注意 apply 方法调用时传入的 from 属性,它的值可以是任何类型的 URL, 比如 HTTP 地址 http:/myscripts.com/shared/cloudbeesgradle。以基于HTTP(S) 的方式暴露脚本插件是一个组织的部门间共享的较好办法。
> * 测试定制的 task: Gradle 的 API 提供的测试工具允许你在真实的工作环境中测试定制的task和插件。它的思想是提供一个假的 Gradle Project实例, 它暴露了和你在构建脚本中使用的 Pro]ect 对象一样的方法和属性。该 Project 实例是通过 org.gradle.testfixtures.Projectbuilder 类的 build 方法提供的, 而且可以被任何测试类使用。
> * 插件能力vs约定：作为一个插件的开发者, 你常常会徘徊于插件提供的能力和约定两条线上方面, 你可能想要加强另一个项目的功能;比如, 通过task。另一方面, 你想要引入约定帮助用户做出有意义的决定;比如, 标准化的项目布局。如果约定强制和武断地对使用该插件的项目做出了结构上的要求, 那么通过创建两个不同的插件, 将基本功能和约定分离就是一种合理的做法:一个基础插件包含了所提供的能力, 而另一个插件则应用该基础插件并根据约定预配置这些能力。这种方法被Java插件所使用, Java插件继承自Java基础插件。

## 实战升级

### 用到的链接

* [Exclusions declared in a dependency's pom should work as they do in Maven](https://github.com/gradle/gradle/issues/1473)
* [dependency management + spring boot + solr + log4j + junit = error?](https://github.com/spring-gradle-plugins/dependency-management-plugin/issues/59)
* [Gradle 依赖排除](https://www.zhyea.com/2018/02/08/gradle-exclude-dependencies.html)

### 目前未解决的问题

```text
Processing of ********log4j-1.2.16.pom failed: maven-antrun-plugin scope for junit:junit:jar must be one of [compile, runtime, system] but is 'test'. in log4j:log4j:1.2.16
```

log4j:log4j:1.2.16 版本有问题，做全局强制指定版本未成功。
