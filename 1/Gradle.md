# Gradle

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
> *
> *
> *
> *
> *

