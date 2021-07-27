# Maven

## [Maven Home](https://maven.apache.org/index.html)

### [Maven Release History](https://maven.apache.org/docs/history.html)

## [Maven 实战](https://book.douban.com/subject/5345682/)

> * mvn help:system 。该命令会打印出所有的 Java 系统属性和环境变量，这些信息对我们日常的编程工作很有帮助。
> * 默认情况下，~/.m2目录下除了repository仓库之外就没有其他目录和文件了，不过大多数Maven用户需要复制M2_HOME/conf/settings.xml文件到~/.m2/settings.xml。推荐使用用户范围的settings.xml，主要是为了避免无意识地影响到系统中的其他用户。如果有切实的需求，需要统一系统中所有用户的settings.xml配置，当然应该使用全局范围的settings.xml。
> * 没有任何实际的Java代码，我们就能够定义一个Maven项目的POM，这体现了Maven的一大优点，它能让项目对象模型最大程度地与实际代码相独立，我们可以称之为解耦，或者正交性。这在很大程度上避免了Java代码和POM代码的相互影响。
> * 在Maven执行测试（test）之前，它会先自动执行项目主资源处理、主代码编译、测试资源处理、测试代码编译等工作，这是Maven生命周期的一个特性。
> * 依赖调解: 路径最近者优先；第一声明者优先。
> * 最后，关于可选依赖需要说明的一点是，在理想的情况下，是不应该使用可选依赖的。前面我们可以看到，使用可选依赖的原因是某一个项目实现了多个特性，在面向对象设计中，有个单一职责性原则，意指一个类应该只有一项职责，而不是糅合太多的功能。这个原则在规划Maven项目的时候也同样适用。
> * 优化依赖：在这些工作之后，最后得到的那些依赖被称为已解析依赖（Resolved Dependency）。当这些依赖经Maven解析后，就会构成一个依赖树，通过这棵依赖树就能很清楚地看到某个依赖是通过哪条传递路径引入的。使用 dependency:list 和 dependency：tree 可以帮助我们详细了解项目中所有依赖的具体信息，在此基础上，还有 dependency:analyze 工具可以帮助分析当前项目的依赖。
>   * 该结果中重要的是两个部分。首先是 Used undeclared dependencies，意指项目中使用到的，但是没有显式声明的依赖，这里是spring-context。这种依赖意味着潜在的风险，当前项目直接在使用它们，例如有很多相关的 Java import 声明，而这种依赖是通过直接依赖传递进来的，当升级直接依赖的时候，相关传递性依赖的版本也可能发生变化，这种变化不易察觉，但是有可能导致当前项目出错。例如由于接口的改变，当前项目中的相关代码无法编译。这种隐藏的、潜在的威胁一旦出现，就往往需要耗费大量的时间来查明真相。因此，显式声明任何项目中直接用到的依赖。
>   * 结果中还有一个重要的部分是Unused declared dependencies，意指项目中未使用的，但显式声明的依赖，这里有spring-core和spring-beans。需要注意的是，对于这样一类依赖，我们不应该简单地直接删除其声明，而是应该仔细分析。由于dependency：analyze只会分析编译主代码和测试代码需要用到的依赖，一些执行测试和运行时需要的依赖它就发现不了。很显然，该例中的spring-core和spring-beans是运行Spring Framework项目必要的类库，因此不应该删除依赖声明。当然，有时候确实能通过该信息找到一些没用的依赖，但一定要小心测试。
> * 从仓库解析依赖的机制：最后，用户还可以从命令行加入参数-U，强制检查更新，使用参数后，Maven就会忽略<updatePolicy>的配置。
> * 何为生命周期：Maven的生命周期是抽象的，这意味着生命周期本身不做任何实际的工作，在Maven的设计中，实际的任务（如编译源代码）都交由插件来完成。生命周期抽象了构建的各个步骤，定义了它们的次序，但没有提供具体实现。每个构建步骤都可以绑定一个或者多个插件行为，而且Maven为大多数构建步骤编写并绑定了默认插件。
>   * Maven拥有三套相互独立的生命周期，它们分别为clean、default和site。clean生命周期的目的是清理项目，default生命周期的目的是构建项目，而site生命周期的目的是建立项目站点。
>   * 每个生命周期包含一些阶段（phase），这些阶段是有顺序的，并且后面的阶段依赖于前面的阶段，用户和Maven最直接的交互方式就是调用这些生命周期阶段。以clean生命周期为例，它包含的阶段有pre-clean、clean和post-clean。当用户调用pre-clean的时候，只有pre-clean阶段得以执行；当用户调用clean的时候，pre-clean和clean阶段会得以顺序执行；当用户调用post-clean的时候，pre-clean、clean和post-clean会得以顺序执行。
> * 命令行与生命周期：
>   * mvn clean：该命令调用clean生命周期的clean阶段。实际执行的阶段为clean生命周期的pre-clean和clean阶段。
>   * mvn test：该命令调用default生命周期的test阶段。实际执行的阶段为default生命周期的validate、initialize等，直到test的所有阶段。这也解释了为什么在执行测试的时候，项目的代码能够自动得以编译。
>   * mvn clean install：该命令调用clean生命周期的clean阶段和default生命周期的install阶段。实际执行的阶段为clean生命周期的pre-clean、clean阶段，以及default生命周期的从validate至install的所有阶段。该命令结合了两个生命周期，在执行真正的项目构建之前清理项目是一个很好的实践。
>   * mvn clean deploy site-deploy：该命令调用clean生命周期的clean阶段、default生命周期的deploy阶段，以及site生命周期的site-deploy阶段。实际执行的阶段为clean生命周期的pre-clean、clean阶段，default生命周期的所有阶段，以及site生命周期的所有阶段。该命令结合了Maven所有三个生命周期，且deploy为default生命周期的最后一个阶段，site-deploy为site生命周期的最后一个阶段。
> * 插件目标：对于插件本身，为了能够复用代码，它往往能够完成多个任务。
>   * maven-dependency-plugin有十多个目标，每个目标对应了一个功能，上述提到的几个功能分别对应的插件目标为dependency：analyze、dependency：tree和dependency：list。这是一种通用的写法，冒号前面是插件前缀，冒号后面是该插件的目标。类似地，还可以写出compiler：compile（这是maven-compiler-plugin的compile目标）和surefire：test（这是maven-surefire-plugin的test目标）。
> * 插件绑定：Maven的生命周期与插件相互绑定，用以完成实际的构建任务。具体而言，是生命周期的阶段与插件的目标相互绑定，以完成某个具体的构建任务。
> * 从命令行调用插件：为了达到该目的，Maven引入了目标前缀的概念，help是maven-help-plugin的目标前缀，dependency是maven-dependency-plugin的前缀，有了插件前缀，Maven就能找到对应的artifactId。不过，除了artifactId，Maven还需要得到groupId和version才能精确定位到某个插件。
> * 插件仓库：值得一提的是，Maven会区别对待依赖的远程仓库与插件的远程仓库。不同于repositories及其repository子元素，插件的远程仓库使用pluginRepositories和pluginRepository配置。
> * 插件的默认groupId：在POM中配置插件的时候，如果该插件是Maven的官方插件（即如果其groupId为org.apache.maven.plugins），就可以省略groupId配置。笔者不推荐使用Maven的这一机制，虽然这么做可以省略一些配置，但这样的配置会让团队中不熟悉Maven的成员感到费解，况且能省略的配置也就仅仅一行而已。
> * 依赖管理：Maven提供的dependencyManagement元素既能让子模块继承到父模块的依赖配置，又能保证子模块依赖使用的灵活性。使用这种依赖管理机制似乎不能减少太多的POM配置，不过笔者还是强烈推荐采用这种方法。其主要原因在于在父POM中使用dependencyManagement声明依赖能够统一项目范围中依赖的版本，当依赖版本在父POM中声明之后，子模块在使用依赖的时候就无须声明版本，也就不会发生多个子模块使用依赖版本不一致的情况。这可以帮助降低依赖冲突的几率。
> * import的依赖范围，推迟到现在介绍是因为该范围的依赖只在dependencyManagement元素下才有效果，使用该范围的依赖通常指向一个POM，作用是将目标POM中的dependencyManagement配置导入并合并到当前POM的dependencyManagement元素中。
> * 聚合与继承的关系：聚合与继承其实是两个概念，其目的完全是不同的。前者主要是为了方便快速构建项目，后者主要是为了消除重复配置。
>   * 对于聚合模块来说，它知道有哪些被聚合的模块，但那些被聚合的模块不知道这个聚合模块的存在。
>   * 对于继承关系的父POM来说，它不知道有哪些子模块继承于它，但那些子模块都必须知道自己的父POM是什么。
>   * 如果非要说这两个特性的共同点，那么可以看到，聚合POM与继承关系中的父POM的packaging都必须是pom，同时，聚合模块与继承关系中的父模块除了POM之外都没有实际的内容。
>   * 在现有的实际项目中，读者往往会发现一个POM既是聚合POM，又是父POM，这么做主要是为了方便。
> * Maven设定核心插件的原因是防止由于插件版本的变化而造成构建不稳定。
> * 反应堆：在一个多模块的Maven项目中，反应堆（Reactor）是指所有模块组成的一个构建结构。对于单模块的项目，反应堆就是该模块本身，但对于多模块项目来说，反应堆就包含了各模块之间继承与依赖的关系，从而能够自动计算出合理的模块构建顺序。模块间的依赖关系会将反应堆构成一个有向非循环图（Directed Acyclic Graph，DAG）。
> * 裁剪反应堆：
>   * -am，--also-make同时构建所列模块的依赖模块
>   * -amd-also-make-dependents同时构建依赖于所列模块的模块
>   * -pl，--projects<arg>构建指定的模块，模块间用逗号分隔
>   * -rf-resume-from<arg>从指定的模块恢复反应堆
>   * 在开发过程中，灵活应用上述4个参数，可以帮助我们跳过无须构建的模块，从而加速构建。在项目庞大、模块特别多的时候，这种效果就会异常明显。
> * 何为版本管理：
>   * 所有自动化测试应当全部通过。毫无疑问，失败的测试代表了需要修复的问题，因此发布版本之前应该确保所有测试都能得以正确执行。
>   * 项目没有配置任何快照版本的依赖。快照版本的依赖意味着不同时间的构建可能会引入不同内容的依赖，这显然不能保证多次构建能够生成同样的结果。
>   * 项目没有配置任何快照版本的插件。快照版本的插件配置可能会在不同时间引入不容内容的Maven插件，从而影响Maven的行为，破坏构建的稳定性。
>   * 项目所包含的代码已经全部提交到版本控制系统中。项目已经发布了，可源代码却不在版本控制系统中，甚至丢失了。这意味着项目丢失了某个时刻的状态，因此这种情况必须避免，版本发布的时候必须确保所有的源代码都已经提交了。
> * Maven为了支持构建的灵活性，内置了三大特性，即属性、Profile和资源过滤。
> * Maven属性：
>   * 内置属性：主要有两个常用内置属性——＄{basedir}表示项目根目录，即包含pom.xml文件的目录；＄{version}表示项目版本。
>   * Java系统属性：所有Java系统属性都可以使用Maven属性引用，例如＄{user.home}指向了用户目录。用户可以使用mvn help：system查看所有的Java系统属性。 
>   * 在一个多模块项目中，模块之间的依赖比较常见，这些模块通常会使用同样的groupId和version。因此这个时候就可以使用POM属性。
>   * 手动往往就意味着低效和错误，因此需要找到一种方法，使它能够自动地应对构建环境的差异。
> * Maven插件：
>   * 如果想要配置Maven自动为所有Java文件的头部添加许可证声明，那么可以通过关键字maven plugin license找到maven-license-plugin[3]，这个托管在Googlecode上的项目完全能够满足我的需求。
> * Maven插件项目的POM有两个特殊的地方:
>   * 它的packaging必须为maven-plugin，这种特殊的打包类型能控制Maven为其在生命周期阶段绑定插件处理相关的目标，例如在compile阶段，Maven需要为插件项目构建一个特殊插件描述符文件。
>   * 一个artifactId为maven-plugin-api的依赖，该依赖中包含了插件开发所必需的类。
> Mojo标注：
>   * 每个Mojo都必须使用@Goal标注来注明其目标名称，否则Maven将无法识别该目标。
>   * @requiresProject<true/false> 表示该目标是否必须在一个Maven项目中运行，默认为true。大部分插件目标都需要依赖一个项目才能执行，但有一些例外。例如maven-help-plugin的system目标，它用来显示系统属性和环境变量信息，不需要实际项目，因此使用了@requiresProject false标注。另外，maven-archetype-plugin的generate目标也是一个很好的例子。
> * Maven支持种类多样的Mojo参数，包括单值的boolean、int、float、String、Date、File和URL，多值的数组、Collection、Map、Properties等。
> * 错误处理和日志：
>   * 如果Maven执行插件目标的时候遇到MojoFailureException，就会显示“BUILDFAILURE”的错误信息，这种异常表示Mojo在运行时发现了预期的错误。例如maven-surefire-plugin运行后若发现有失败的测试就会抛出该异常。
>   * 如果Maven执行插件目标的时候遇到MojoExecutationException，就会显示“BUILD ERROR”的错误信息。这种异常表示Mojo在运行时发现了未预期的错误。
> * 测试Maven插件：Maven社区有一个用来帮助插件集成测试的插件，它就是maven-invoker-plugin。该插件能够用来在一组项目上执行Maven，并检查每个项目的构建是否成功，最后，它还可以执行BeanShell或者Groovy脚本来验证项目构建的输出。
> * Maven Archetype Plugin: Archetype并不是Maven的核心特性，它也是通过插件来实现的，这一插件就是 [maven-archetype-plugin](（http://maven.apache.org/archetype/maven-archetype-plugin/) 。尽管它只是一个插件，但由于其使用范围非常广泛，主要的IDE（如Eclipse、NetBeans和IDEA）在集成Maven的时候都着重集成了archetype特性，以方便用户快速地创建Maven项目。
> * 编写Archetype: 也许你所在组织的一些项目都使用同样的框架和项目结构，为一个个项目重复同样的配置及同样的目录结构显然是难以让人接受的。更好的做法是创建一个属于自己的Archetype，这个Archetype包含了一些通用的POM配置、目录结构，甚至是Java类及资源文件，然后在创建项目的时候，就可以直接使用该Archetype，并提供一些基本参数，如groupId、artifactId、version，maven-archetype-plugin会处理其他原本需要手工处理的劳动。这样不仅节省了时间，也降低了错误配置发生的概率。
> * 默认情况下，maven-archetype-plugin要求用户在使用Archetype生成项目的时候必须提供4个参数：groupId、artifactId、version和package。除此之外，用户在编写Archetype的时候可以要求额外的参数。
> * 什么是Archetype Catalog: 当用户以不指定Archetype坐标的方式使用maven-archetype-plugin的时候，会得到一个Archetype列表供选择，这个列表的信息来源于一个名为archetype-catalog.xml的文件。
> * 生成本地仓库的Archetype Catalog: maven-archetype-plugin提供了一个名为crawl的目标，用户可以用它来遍历本地Maven仓库的内容并自动生成archetype-catalog.xml文件。

### [Profiles on Maven](https://maven.apache.org/guides/introduction/introduction-to-profiles.html)

How can I tell which profiles are in effect during a build?

```shell
# must on pom.xml dir
mvn help:active-profiles
```

show all profile:

```shell
mvn help:all-profiles
```

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

### [Maven Deploy to Nexus](https://www.baeldung.com/maven-deploy-nexus)

### Maven 设定使用代理

* 在 Maven 的 setting 中设定 proxies 属性，参考 [Configuring a proxy](https://maven.apache.org/guides/mini/guide-proxies.html) 。
* IDEA 中为单个项目配置代理需要为 Maven -> Importing -> VM options for importer 设定 -DproxySet=true -DproxyHost=127.0.0.1 -DproxyPort=8888
* 当有项目依赖不需要代理时，在 IDEA 中 Maven -> Importing -> VM options for importer 设定 -DproxySet=false
  ```xml
  <proxies>
      <!-- 设置 HTTP 代理 -->
      <proxy>
          <id>trojan-proxy</id>
          <active>true</active>
          <protocol>http</protocol>
          <host>127.0.0.1</host>
          <port>8888</port>
      </proxy>
  </proxies>
  ``` 

### Maven on Jetbrains

* [IntelliJ IDEA: Maven dependencies](https://www.jetbrains.com/help/idea/work-with-maven-dependencies.html)
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
