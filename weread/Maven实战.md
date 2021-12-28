# Maven实战

 **许晓斌**


## 划线部分


### 2.3.2 ~/.m2

* mvn help：system

* 默认情况下，~/.m2目录下除了repository仓库之外就没有其他目录和文件了，不过大多数Maven用户需要复制M2_HOME/conf/settings.xml文件到~/.m2/settings.xml。这是一条最佳实践，我们将在2.7小节详细解释。


### 2.7.1 设置MAVEN_OPTS环境变量

* 通常需要设置MAVEN_OPTS的值为-Xms128m-Xmx512m


### 2.7.2 配置用户范围settings.xml

* 推荐使用用户范围的settings.xml，主要是为了避免无意识地影响到系统中的其他用户。如果有切实的需求，需要统一系统中所有用户的settings.xml配置，当然应该使用全局范围的settings.xml。


### 3.1 编写POM

* POM（Project Object Model，项目对象模型）定义了项目的基本信息，用于描述项目如何构建，声明项目依赖，等等。

* 没有任何实际的Java代码，我们就能够定义一个Maven项目的POM，这体现了Maven的一大优点，它能让项目对象模型最大程度地与实际代码相独立，我们可以称之为解耦，或者正交性。这在很大程度上避免了Java代码和POM代码的相互影响。


### 3.2 编写主代码

* 一般来说，项目中Java类的包都应该基于项目的groupId和artifactId，这样更加清晰，更加符合逻辑，也方便搜索构件或者Java类。


### 3.3 编写测试代码

* 在Maven执行测试（test）之前，它会先自动执行项目主资源处理、主代码编译、测试资源处理、测试代码编译等工作，这是Maven生命周期的一个特性。

* 由于历史原因，Maven的核心插件之一——compiler插件默认只支持编译Java 1.3，因此需要配置该插件使其支持Java 5

* surefire是Maven中负责执行测试的插件，这里它运行测试用例HelloWorldTest，并且输出测试报告，显示一共运行了多少测试，失败了多少，出错了多少，跳过了多少。


### 3.4 打包和运行

* 现在执行mvn clean install，待构建完成之后打开target/目录，可以看到hello-world-1.0-SNAPSHOT.jar和original-hello-world-1.0-SNAPSHOT.jar，前者是带有Main-Class信息的可运行jar，后者是原始的jar


### 5.1 何为Maven坐标

* Maven坐标的元素包括groupId、artifactId、version、packaging、classifier。


### 5.2 坐标详解

* ·classifier：该元素用来帮助定义构建输出的一些附属构件。

* 这里还要强调的一点是，packaging并非一定与构件扩展名对应，比如packaging为maven-plugin的构件扩展名为jar。


### 5.3.1 account-email的POM

* 当依赖范围是test的时候，该依赖只会被加入到测试代码的classpath中。也就是说，对于项目主代码，该依赖是没有任何作用的。


### 5.3.3 account-email的测试代码

* 针对接口测试是一个单元测试的最佳实践。


### 5.3.4 构建account-email

* 使用mvn clean install构建account-email


### 5.4 依赖的配置

* ·type：依赖的类型，对应于项目坐标定义的packaging。大部分情况下，该元素不必声明，其默认值为jar。

* ·optional：标记依赖是否可选


### 5.5 依赖范围

* 依赖范围就是用来控制依赖与这三种classpath（编译classpath、测试classpath、运行classpath）的关系

* ·provided：已提供依赖范围。使用此依赖范围的Maven依赖，对于编译和测试classpath有效，但在运行时无效。典型的例子是servlet-api，编译和测试项目的时候需要该依赖，但在运行项目的时候，由于容器已经提供，就不需要Maven重复地引入一遍。

* ·system：系统依赖范围。该依赖与三种classpath的关系，和provided依赖范围完全一致。但是，使用system范围的依赖时必须通过systemPath元素显式地指定依赖文件的路径。由于此类依赖不是通过Maven仓库解析的，而且往往与本机系统绑定，可能造成构建的不可移植，因此应该谨慎使用。

* ·import（Maven 2.0.9及以上）：导入依赖范围。该依赖范围不会对三种classpath产生实际的影响


### 5.6 传递性依赖

* 有了传递性依赖机制，在使用Spring Framework的时候就不用去考虑它依赖了什么，也不用担心引入多余的依赖。Maven会解析各个直接依赖的POM，将那些必要的间接依赖，以传递性依赖的形式引入到当前的项目中。


### 5.7 依赖调解

* 路径最近者优先。

* 第一声明者优先。


### 5.8 可选依赖

* 为什么要使用可选依赖这一特性呢？可能项目B实现了两个特性，其中的特性一依赖于X，特性二依赖于Y，而且这两个特性是互斥的，用户不可能同时使用两个特性。

* 使用<optional>元素表示mysql-connector-java和postgresql这两个依赖为可选依赖，它们只会对当前项目B产生影响，当其他项目依赖于B的时候，这两个依赖不会被传递。

* 最后，关于可选依赖需要说明的一点是，在理想的情况下，是不应该使用可选依赖的。


### 5.9.1 排除依赖

* 传递性依赖会给项目隐式地引入很多依赖，这极大地简化了项目依赖的管理，但是有些时候这种特性也会带来问题。

* 代码中使用exclusions元素声明排除依赖，exclusions可以包含一个或者多个exclusion子元素，因此可以排除一个或者多个传递性依赖。


### 5.9.2 归类依赖

* 也就是说，可以使用美元符号和大括弧环绕的方式引用Maven属性。


### 5.9.3 优化依赖

* 在这些工作之后，最后得到的那些依赖被称为已解析依赖（Resolved Dependency）。

* 当这些依赖经Maven解析后，就会构成一个依赖树，通过这棵依赖树就能很清楚地看到某个依赖是通过哪条传递路径引入的。

* 使用dependency：list和dependency：tree可以帮助我们详细了解项目中所有依赖的具体信息，在此基础上，还有dependency：analyze工具可以帮助分析当前项目的依赖。

* 　使用但未声明的依赖与声明但未使用的依赖

* 这种依赖意味着潜在的风险，当前项目直接在使用它们，例如有很多相关的Java import声明，而这种依赖是通过直接依赖传递进来的，当升级直接依赖的时候，相关传递性依赖的版本也可能发生变化，这种变化不易察觉，但是有可能导致当前项目出错。

* 因此，显式声明任何项目中直接用到的依赖。

* 由于dependency：analyze只会分析编译主代码和测试代码需要用到的依赖，一些执行测试和运行时需要的依赖它就发现不了。


### 6.2 仓库的布局

* 任何一个构件都有其唯一的坐标，根据这个坐标可以定义其在仓库中的唯一存储路径，这便是Maven的仓库布局方式。

* packaging决定了构件的扩展名


### 6.3.1 本地仓库

* 用户会想要自定义本地仓库目录地址。这时，可以编辑文件~/.m2/settings.xml，设置localRepository元素的值为想要的仓库地址。

* Maven使用Install插件将该文件复制到本地仓库中


### 6.3.3 中央仓库

* 中央仓库包含了这个世界上绝大多数流行的开源Java构件，以及源码、作者信息、SCM、信息、许可证信息等，每个月这里都会接受全世界Java程序员大概1亿次的访问，它对全世界Java开发者的贡献由此可见一斑。由于中央仓库包含了超过2000个开源项目的构件，因此，一般来说，一个简单Maven项目所需要的依赖构件都能从中央仓库下载到。这也解释了为什么Maven能做到“开箱即用”。


### 6.3.4 私服

* 建立私服之后，便可以将这些构件部署到这个内部的仓库中，供内部的Maven项目使用。

* 建立私服是用好Maven十分关键的一步


### 6.4 远程仓库的配置

* 在repositories元素下，可以使用repository子元素声明一个或者多个远程仓库。

* 对于releases和snapshots来说，除了enabled，它们还包含另外两个子元素updatePolicy和checksumPolicy：
￼


### 6.4.1 远程仓库的认证

* Maven使用settings.xml文件中并不显而易见的servers元素及其server子元素配置仓库认证信息。


### 6.4.2 部署至远程仓库

* distributionManagement包含repository和snapshotRepository子元素，前者表示发布版本构件的仓库，后者表示快照版本的仓库。

* 配置正确后，在命令行运行mvn clean deploy，Maven就会将项目构建输出的构件部署到配置对应的远程仓库，如果项目当前的版本是快照版本，则部署到快照版本仓库地址，否则就部署到发布版本仓库地址。


### 6.5 快照版本

* Maven的快照版本机制就是为了解决上述问题。在该例中，小张只需要将模块A的版本设定为2.1-SNAPSHOT，然后发布到私服中，在发布的过程中，Maven会自动为构件打上时间戳。

* 快照版本只应该在组织内部的项目或模块间依赖使用，因为这时，组织对于这些快照版本的依赖具有完全的理解及控制权。项目不应该依赖于任何组织外部的快照版本依赖，由于快照版本的不稳定性，这样的依赖会造成潜在的危险。也就是说，即使项目构建今天是成功的，由于外部的快照版本依赖实际对应的构件随时可能变化，项目的构建就可能由于这些外部的不受控制的因素而失败


### 6.6 从仓库解析依赖的机制

* 最后，用户还可以从命令行加入参数-U，强制检查更新，使用参数后，Maven就会忽略<updatePolicy>的配置。

* 为此，Maven 3不再支持在插件配置中使用LATEST和RELEASE。

* 最后，仓库元数据并不是永远正确的，有时候当用户发现无法解析某些构件，或者解析得到错误构件的时候，就有可能是出现了仓库元数据错误，这时就需要手工地，或者使用工具（如Nexus）对其进行修复。


### 6.7 镜像

* 在这种情况下，任何需要的构件都可以从私服获得，私服就是所有仓库的镜像。

* 需要注意的是，由于镜像仓库完全屏蔽了被镜像仓库，当镜像仓库不稳定或者停止服务的时候，Maven仍将无法访问被镜像仓库，因而将无法下载构件。


### 6.8.1 Sonatype Nexus

* Sonatype Nexus
地址：http://repository.sonatype.org/


### 6.8.2 Jarvana

* Jarvana
地址：http://www.jarvana.com/jarvana/


### 6.8.3 MVNbrowser

* MVNbrowser
地址：http://www.mvnbrowser.com


### 6.8.4 MVNrepository

* MVNrepository
地址：http://mvnrepository.com/


### 7.1 何为生命周期

* Maven的生命周期是抽象的，这意味着生命周期本身不做任何实际的工作，在Maven的设计中，实际的任务（如编译源代码）都交由插件来完成。

* 生命周期抽象了构建的各个步骤，定义了它们的次序，但没有提供具体实现。

* 每个构建步骤都可以绑定一个或者多个插件行为，而且Maven为大多数构建步骤编写并绑定了默认插件。


### 7.2.1 三套生命周期

* Maven拥有三套相互独立的生命周期，它们分别为clean、default和site。

* clean生命周期的目的是清理项目，default生命周期的目的是构建项目，而site生命周期的目的是建立项目站点。

* 每个生命周期包含一些阶段（phase），这些阶段是有顺序的，并且后面的阶段依赖于前面的阶段，用户和Maven最直接的交互方式就是调用这些生命周期阶段。以clean生命周期为例，它包含的阶段有pre-clean、clean和post-clean。当用户调用pre-clean的时候，只有pre-clean阶段得以执行；当用户调用clean的时候，pre-clean和clean阶段会得以顺序执行；当用户调用post-clean的时候，pre-clean、clean和post-clean会得以顺序执行。


### 7.2.3 default生命周期

* default生命周期定义了真正构建时所需要执行的所有步骤，它是所有生命周期中最核心的部分

* ·compile编译项目的主源码。一般来说，是编译src/main/java目录下的Java文件至项目输出的主classpath目录中

* ·test使用单元测试框架运行测试，测试代码不会被打包或部署。

* ·package接受编译好的代码，打包成可发布的格式，如JAR

* install将包安装到Maven本地仓库，供本地其他Maven项目使用。

* deploy将最终的包复制到远程仓库，供其他开发人员和Maven项目使用。



### 7.2.4 site生命周期

* site生命周期的目的是建立和发布项目站点，Maven能够基于POM所包含的信息，自动生成一个友好的站点，方便团队交流和发布项目信息。


### 7.2.5 命令行与生命周期

* ·＄mvn clean：该命令调用clean生命周期的clean阶段。实际执行的阶段为clean生命周期的pre-clean和clean阶段。

* ·＄mvn test：该命令调用default生命周期的test阶段。实际执行的阶段为default生命周期的validate、initialize等，直到test的所有阶段。这也解释了为什么在执行测试的时候，项目的代码能够自动得以编译。

* ·＄mvn clean install：该命令调用clean生命周期的clean阶段和default生命周期的install阶段。实际执行的阶段为clean生命周期的pre-clean、clean阶段，以及default生命周期的从validate至install的所有阶段。该命令结合了两个生命周期，在执行真正的项目构建之前清理项目是一个很好的实践。

* ·＄mvn clean deploy site-deploy：该命令调用clean生命周期的clean阶段、default生命周期的deploy阶段，以及site生命周期的site-deploy阶段。实际执行的阶段为clean生命周期的pre-clean、clean阶段，default生命周期的所有阶段，以及site生命周期的所有阶段。该命令结合了Maven所有三个生命周期，且deploy为default生命周期的最后一个阶段，site-deploy为site生命周期的最后一个阶段。


### 7.3 插件目标

* 对于插件本身，为了能够复用代码，它往往能够完成多个任务。

* maven-dependency-plugin有十多个目标，每个目标对应了一个功能，上述提到的几个功能分别对应的插件目标为dependency：analyze、dependency：tree和dependency：list。这是一种通用的写法，冒号前面是插件前缀，冒号后面是该插件的目标。类似地，还可以写出compiler：compile（这是maven-compiler-plugin的compile目标）和surefire：test（这是maven-surefire-plugin的test目标）


### 7.4 插件绑定

* Maven的生命周期与插件相互绑定，用以完成实际的构建任务。具体而言，是生命周期的阶段与插件的目标相互绑定，以完成某个具体的构建任务。


### 7.4.1 内置绑定

* 相对于clean和site生命周期来说，default生命周期与插件目标的绑定关系就显得复杂一些。

* 表7-3　default生命周期的内置插件绑定关系及具体任务（打包类型：jar）
￼
注意，表7-3只列出了拥有插件绑定关系的阶段，default生命周期还有很多其他阶段，默认它们没有绑定任何插件，因此也没有任何实际行为


### 7.4.2 自定义绑定

* 对于自定义绑定的插件，用户总是应该声明一个非快照版本，这样可以避免由于插件版本变化造成的构建不稳定性。

* 该例中配置了一个id为attach-sources的任务，通过phrase配置，将其绑定到verify生命周期阶段上，再通过goals配置指定要执行的插件目标。

* 有很多插件的目标在编写时已经定义了默认绑定阶段。可以使用maven-help-plugin查看插件详细信息，了解插件目标的默认绑定阶段。

* 当多个插件目标绑定到同一个阶段的时候，这些插件声明的先后顺序决定了目标的执行顺序


### 7.5.1 命令行插件配置

* maven-surefire-plugin提供了一个maven.test.skip参数，当其值为true的时候，就会跳过执行测试。于是，在运行命令的时候，加上如下-D参数就能跳过测试


### 7.5.2 POM中插件全局配置

* 用户可以在声明插件的时候，对此插件进行一个全局的配置。也就是说，所有该基于该插件目标的任务，都会使用这些配置。


### 7.5.3 POM中插件任务配置

* 除了为插件配置全局的参数，用户还可以为某个插件任务配置特定的参数。


### 7.6 获取插件信息

* 由于Maven的插件非常多，而且这其中的大部分没有完善的文档，因此，使用正确的插件并进行正确的配置，其实并不是一件容易的事


### 7.6.1 在线插件信息

* 基本上所有主要的Maven插件都来自Apache和Codehaus。由于Maven本身是属于Apache软件基金会的，因此它有很多官方的插件，每天都有成千上万的Maven用户在使用这些插件，它们具有非常好的稳定性。

* 除了Apache上的官方插件之外，托管于Codehaus上的Mojo项目也提供了大量了Maven插件，详细的列表可以访问：http://mojo.codehaus.org/plugins.html。需要注意的是，这些插件的文档和可靠性相对较差，在使用时，如果遇到问题，往往只能自己去看源代码。

* 虽然并非所有插件都提供了完善的文档，但一些核心插件的文档还是非常丰富的。

* 并不是所有插件目标参数都有表达式，也就是说，一些插件目标参数只能在POM中配置


### 7.6.2 使用maven-help-plugin描述插件

* 除了访问在线的插件文档之外，还可以借助maven-help-plugin来获取插件的详细信息。


### 7.7 从命令行调用插件

* 但Maven还支持直接从命令行调用插件目标。

* 为了达到该目的，Maven引入了目标前缀的概念，help是maven-help-plugin的目标前缀，dependency是maven-dependency-plugin的前缀，有了插件前缀，Maven就能找到对应的artifactId。不过，除了artifactId，Maven还需要得到groupId和version才能精确定位到某个插件。下一节将详细解释这个过程


### 7.8.1 插件仓库

* 值得一提的是，Maven会区别对待依赖的远程仓库与插件的远程仓库

* 不同于repositories及其repository子元素，插件的远程仓库使用pluginRepositories和pluginRepository配置。

* 除了pluginRepositories和pluginRepository标签不同之外，其余所有子元素表达的含义与第6.4节所介绍的依赖远程仓库配置完全一样。


### 7.8.2 插件的默认groupId

* 在POM中配置插件的时候，如果该插件是Maven的官方插件（即如果其groupId为org.apache.maven.plugins），就可以省略groupId配置

* 笔者不推荐使用Maven的这一机制，虽然这么做可以省略一些配置，但这样的配置会让团队中不熟悉Maven的成员感到费解，况且能省略的配置也就仅仅一行而已


### 7.8.3 解析插件版本

* 同样是为了简化插件的配置和使用，在用户没有提供插件版本的情况下，Maven会自动解析插件版本。

* Maven遍历本地仓库和所有远程插件仓库，将该路径下的仓库元数据归并后，就能计算出latest和release的值。latest表示所有仓库中该构件的最新版本，而release表示最新的非快照版本。

* Maven 3调整了解析机制，当插件没有声明版本的时候，不再解析至latest，而是使用release。这样就可以避免由于快照频繁更新而导致的插件行为不稳定。

* 因此，使用插件的时候，应该一直显式地设定版本，这也解释了Maven为什么要在超级POM中为核心插件设定版本


### 7.8.4 解析插件前缀

* Maven在解析插件仓库元数据的时候，会默认使用org.apache.maven.plugins和org.codehaus.mojo两个groupId。

* 当Maven解析到dependency：tree这样的命令后，它首先基于默认的groupId归并所有插件仓库的元数据org/apache/maven/plugins/maven-metadata.xml；其次检查归并后的元数据，找到对应的artifactId为maven-dependency-plugin；然后结合当前元数据的groupId org.apache.maven.plugins；最后使用第7.8.3节描述的方法解析得到version，这时就得到了完整的插件坐标。如果org/apache/maven/plugins/maven-metadata.xml没有记录该插件前缀，则接着检查其他groupId下的元数据，如org/codehaus/mojo/maven-metadata.xml，以及用户自定义的插件组。如果所有元数据中都不包含该前缀，则报错


### 8.1.3 account-persist的测试代码

* 该测试用例遵守了测试接口而不测试实现这一原则。也就是说，测试代码不能引用实现类，由于测试是从接口用户的角度编写的，这样就能保证接口的用户无须知晓接口的实现细节，既保证了代码的解耦，也促进了代码的设计。


### 8.2 聚合

* 对于聚合模块来说，其打包方式packaging的值必须为pom，否则就无法构建。

* POM的name字段是为了给项目提供一个更容易阅读的名字。之后是本书之前都没提到过的元素modules，这是实现聚合的最核心的配置。用户可以通过在一个打包方式为pom的Maven项目中声明任意数量的module元素来实现模块的聚合。

* 为了方便用户构建项目，通常将聚合模块放在项目目录的最顶层，其他模块则作为聚合模块的子目录存在，这样当用户得到源码的时候，第一眼发现的就是聚合模块的POM

* 如果使用平行目录结构，聚合模块的POM也需要做相应的修改，以指向正确的模块目录

* 上述输出中显示的是各模块的名称，而不是artifactId，这也解释了为什么要在POM中配置合理的name字段，其目的是让Maven的构建输出更清晰。输出的最后是一个项目构建的小结报告，包括各个模块构建成功与否、花费的时间，以及整个构建花费的时间、使用的内存等


### 8.3 继承

* 在Maven的世界中，也有类似的机制能让我们抽取出重复的配置，这就是POM的继承


### 8.3.1 account-parent

* 作为父模块的POM，其打包类型也必须为pom

* 这个更新过的POM没有为account-email声明groupId和version，不过这并不代表account-email没有groupId和version。实际上，这个子模块隐式地从父模块继承了这两个元素，这也就消除了一些不必要的配置。


### 8.3.2 可继承的POM元素

* groupId和version是可以被继承的，那么还有哪些POM元素可以被继承呢？以下是一个完整的列表


### 8.3.3 依赖管理

* 到目前为止，我们能够确定这两个子模块都包含那四个依赖，不过我们无法确定将来添加的子模块就一定需要这四个依赖。

* Maven提供的dependencyManagement元素既能让子模块继承到父模块的依赖配置，又能保证子模块依赖使用的灵活性。

* 上述POM中的依赖配置较原来简单了一些，所有的springframework依赖只配置了groupId和artifactId，省去了version，而junit依赖不仅省去了version，还省去了依赖范围scope。

* 使用这种依赖管理机制似乎不能减少太多的POM配置，不过笔者还是强烈推荐采用这种方法。其主要原因在于在父POM中使用dependencyManagement声明依赖能够统一项目范围中依赖的版本，当依赖版本在父POM中声明之后，子模块在使用依赖的时候就无须声明版本，也就不会发生多个子模块使用依赖版本不一致的情况。这可以帮助降低依赖冲突的几率。

* import的依赖范围，推迟到现在介绍是因为该范围的依赖只在dependencyManagement元素下才有效果，使用该范围的依赖通常指向一个POM，作用是将目标POM中的dependencyManagement配置导入并合并到当前POM的dependencyManagement元素中

* 上述代码中依赖的type值为pom，import范围依赖由于其特殊性，一般都是指向打包类型为pom的模块。


### 8.3.4 插件管理

* 当子模块需要生成源码包的时候，只需要如下简单的配置


### 8.4 聚合与继承的关系

* 聚合与继承其实是两个概念，其目的完全是不同的。前者主要是为了方便快速构建项目，后者主要是为了消除重复配置。

* 对于聚合模块来说，它知道有哪些被聚合的模块，但那些被聚合的模块不知道这个聚合模块的存在。
对于继承关系的父POM来说，它不知道有哪些子模块继承于它，但那些子模块都必须知道自己的父POM是什么。
如果非要说这两个特性的共同点，那么可以看到，聚合POM与继承关系中的父POM的packaging都必须是pom，同时，聚合模块与继承关系中的父模块除了POM之外都没有实际的内容

* 在现有的实际项目中，读者往往会发现一个POM既是聚合POM，又是父POM，这么做主要是为了方便。


### 8.5 约定优于配置

* Maven提倡“约定优于配置”（Convention Over Configuration），这是Maven最核心的设计理念之一。

* 也许这时候有读者会问，如果我不想遵守约定该怎么办？这时，请首先问自己三遍，你真的需要这么做吗？如果仅仅是因为喜好，就不要耍个性，个性往往意味着牺牲通用性，意味着增加无谓的复杂度。

* 任何一个Maven项目都隐式地继承自该POM，这有点类似于任何一个Java类都隐式地继承于Object类。因此，大量超级POM的配置都会被所有Maven项目继承，这些配置也就成为了Maven所提倡的约定。

* Maven设定核心插件的原因是防止由于插件版本的变化而造成构建不稳定。


### 8.6 反应堆

* 在一个多模块的Maven项目中，反应堆（Reactor）是指所有模块组成的一个构建结构。对于单模块的项目，反应堆就是该模块本身，但对于多模块项目来说，反应堆就包含了各模块之间继承与依赖的关系，从而能够自动计算出合理的模块构建顺序。


### 8.6.1 反应堆的构建顺序

* 模块间的依赖关系会将反应堆构成一个有向非循环图（Directed Acyclic Graph，DAG）


### 8.6.2 裁剪反应堆

* ·-am，--also-make同时构建所列模块的依赖模块

* ·-amd-also-make-dependents同时构建依赖于所列模块的模块

* ·-pl，--projects<arg>构建指定的模块，模块间用逗号分隔
·-rf-resume-from<arg>从指定的模块回复反应堆

* 在开发过程中，灵活应用上述4个参数，可以帮助我们跳过无须构建的模块，从而加速构建。在项目庞大、模块特别多的时候，这种效果就会异常明显


### 9.3.3 创建Nexus宿主仓库

* Deployment Policy用来配置该仓库的部署策略，选项有只读（禁止部署）、关闭重新部署（同一构件只能部署一次）以及允许重新部署。


### 9.4 Nexus的索引与构件搜索

* ·GAV搜索（GAV Search）允许用户通过设置GroupId、ArtifactId和Version等信息来进行更有针对性的搜索。
·类名搜索（Classname Search）允许用户搜索包含某个Java类的构件。
·校验和搜索（Checksum Search）允许用户直接使用构件的校验和来搜索该构件。


### 9.5 配置Maven从Nexus下载构件

* 所幸Maven还提供了Profile机制，能让用户将仓库配置放到setting.xml中的Profile中

* 可以创建一个匹配任何仓库的镜像，镜像的地址为私服，这样，Maven对任何仓库的构件下载请求都会转到私服中。

* 配置仓库及插件仓库的主要目的是开启对快照版本下载的支持，当Maven需要下载发布版或快照版构件的时候，它首先检查central，看该类型的构件是否支持，得到正面的回答之后，再根据镜像匹配规则转而访问私服仓库地址


### 9.6.1 使用Maven部署构件至Nexus

* Nexus的仓库对于匿名用户是只读的。为了能够部署构件，还需要在settings.xml中配置认证信息


### 9.6.2 手动部署第三方构件至Nexus

* Nexus允许用户一次上传一个主构件和多个附属构件（即Classifier）。最后，单击页面最下方的Upload Artifact（s）按钮将构件上传到仓库中


### 9.7.1 Nexus的访问控制模型

* 这三个用户对应了三个权限级别：
·admin：该用户拥有对Nexus服务的完全控制，默认密码为admin123。
·deployment：该用户能够访问Nexus，浏览仓库内容，搜索，并且上传部署构件，但是无法对Nexus进行任何配置，默认密码为deployment123。
·anonymous：该用户对应了所有未登录的匿名用户，它们可以浏览仓库并进行搜索


### 9.9 其他私服软件

* Nexus的主色调为蓝色，Archiva的主色调为橙色，而Artifactory的主色调为绿色，这或许是各个团队自我风格的一种体现吧。


### 10.1.1 account-captcha的POM

* 需要注意的是，Kaptcha依赖还有一个classifier元素，其值为jdk5，Kaptcha针对Java 1.5和Java 1.4提供了不同的分发包，因此这里使用classifier来区分两个不同的构件。
POM的最后声明了Sonatype Forge这一公共仓库，这是因为Kaptcha并没有上传的中央仓库，我们可以从Sonatype Forge仓库获得该构件。如果有自己的私服，就不需要在POM中声明该仓库了，可以代理Sonatype Forge仓库，或者直接将Kaptcha上传到自己的仓库中。


### 10.2 maven-surefire-plugin简介

* 在默认情况下，maven-surefire-plugin的test目标会自动执行测试源码路径（默认为src/test/java/）下所有符合一组命名模式的测试类。这组模式为：
·**/Test*.java：任何子目录下所有命名以Test开头的Java类。
·**/*Test.java：任何子目录下所有命名以Test结尾的Java类。
·**/*TestCase.java：任何子目录下所有命名以TestCase结尾的Java类。


### 10.3 跳过测试

* 代码清单10-12　配置插件跳过测试运行

* 配置插件跳过测试编译和运行

* 实际上maven-compiler-plugin的testCompile目标和maven-surefire-plugin的test目标都提供了一个参数skip用来跳过测试编译和测试运行，而这个参数对应的命令行表达式为maven.test.skip


### 10.4 动态指定要运行的测试用例

* maven-surefire-plugin提供了一个test参数让Maven用户能够在命令行指定要运行的测试用例。

* 需要注意的是，上述几种从命令行动态指定测试类的方法都应该只是临时使用，如果长时间只运行项目的某几个测试，那么测试就会慢慢失去其本来的意义。


### 10.5 包含与排除测试用例

* 自动运行以Tests结尾的测试类


### 10.6.2 测试覆盖率报告

* Maven通过cobertura-maven-plugin与之集成，用户可以使用简单的命令为Maven项目生成测试覆盖率报告。


### 10.7 运行TestNG测试

* TestNG允许用户使用一个名为testng.xml的文件来配置想要运行的测试集合


### 10.8 重用测试代码

* 打包测试代码

* 上述依赖声明中有一个特殊的元素type，所有测试包构件都使用特殊的test-jar打包类型。需要注意的是，这一类型的依赖同样都使用test依赖范围


### 11.1 持续集成的作用、过程和优势

* 持续部署：有些错误只有在部署后才能被发现，它们往往是具体容器或者环境相关的，自动化部署能够帮助我们尽快发现这类问题。


### 12.4 使用jetty-maven-plugin进行测试

* 可以用单元测试覆盖的代码就不应该依赖于Web页面测试，且不说页面测试更加耗时耗力，这种方式还无法自动化，更别提重复性了。因此Web页面测试应该仅限于页面的层次，例如JSP、CSS、JavaScript的修改，其他代码修改（如数据访问），请编写单元测试

* jetty-maven-plugin能够帮助我们节省时间，它能够周期性地检查项目内容，发现变更后自动更新到内置的Jetty Web容器中


### 12.5 使用Cargo实现自动化部署

* 虽然cargo-maven2-plugin和jetty-maven-plugin的功能看起来很相似，但它们的目的是不同的，jetty-maven-plugin主要用来帮助日常的快速开发和测试，而cargo-maven2-plugin主要服务于自动化部署。例如专门的测试人员只需要一条简单的Maven命令，就可以构建项目并部署到Web容器中，然后进行功能测试


### 13.1 何为版本管理

* 所有自动化测试应当全部通过

* 项目没有配置任何快照版本的依赖

* 项目没有配置任何快照版本的插件

* 项目所包含的代码已经全部提交到版本控制系统中


### 13.2 Maven的版本号定义约定

* <主版本>.<次版本>.<增量版本>-<里程碑版本>


### 13.3 主干、标签与分支

* 使用版本控制工具时我们都会遇到主干（trunk）、标签（tag）和branch（分支）的概念。


### 第14章 灵活的构建

* Maven为了支持构建的灵活性，内置了三大特性，即属性、Profile和资源过滤。


### 14.1 Maven属性

* 内置属性：主要有两个常用内置属性——＄{basedir}表示项目根目录，即包含pom.xml文件的目录；＄{version}表示项目版本。

* ·Java系统属性：所有Java系统属性都可以使用Maven属性引用，例如＄{user.home}指向了用户目录。用户可以使用mvn help：system查看所有的Java系统属性。

* 在一个多模块项目中，模块之间的依赖比较常见，这些模块通常会使用同样的groupId和version。因此这个时候就可以使用POM属性

* 大量的Maven插件用到了Maven属性，这意味着在配置插件的时候同样可以使用Maven属性来方便地自定义插件行为。例如从10.6节我们知道，maven-surefire-plugin运行后默认的测试报告目录为target/surefire-reports，这实际上就是＄{project.build.directory}/surefire-reports，如果查阅该插件的文档，会发现该插件提供了reportsDirectory参数来配置测试报告目录。因此如果想要改变测试报告目录，例如改成target/test-reports


### 14.2 构建环境的差异

* 手动往往就意味着低效和错误，因此需要找到一种方法，使它能够自动地应对构建环境的差异。


### 14.3 资源过滤

* 资源文件的处理其实是maven-resources-plugin做的事情，它默认的行为只是将项目主资源文件复制到主代码编译输出目录中，将测试资源文件复制到测试代码编译输出目录中。不过只要通过一些简单的POM配置，该插件就能够解析资源文件中的Maven属性，即开启资源过滤。

* 主资源目录和测试资源目录都可以超过一个，虽然会破坏Maven的约定，但Maven允许用户声明多个资源目录，并且为每个资源目录提供不同的过滤配置

* 我们将数据库配置的变化部分提取成了Maven属性，在POM的profile中定义了这些属性的值，并且为资源目录开启了属性过滤。最后，只需要在命令行激活profile，Maven就能够在构建项目的时候使用profile中属性值替换数据库配置文件中的属性引用。


### 14.4.1 针对不同环境的profile

* 同样的属性在两个profile中的值是不一样的，dev profile提供了开发环境数据库的配置，而test profile提供的是测试环境数据库的配置。类似地，还可以添加一个基于产品环境数据库配置的profile。由于篇幅原因，在此不再赘述。现在，开发人员可以在使用mvn命令的时候在后面加上-Pdev激活dev profile，而测试人员可以使用-Ptest激活test profile。


### 14.4.2 激活profile

* 可以进一步配置当某系统属性test存在，且值等于x的时候激活profile

* Maven能够根据项目中某个文件存在与否来决定是否激活profile

* maven-help-plugin提供了一个目标帮助用户了解当前激活的profile


### 14.4.3 profile的种类

* 现在不用担心POM外部的profile会对项目产生太大的影响了，事实上这样的profile仅仅能用来影响到项目的仓库和Maven属性。


### 14.6 在profile中激活集成测试

* 从该例中可以看到，profile不仅可以用来应对不同的构建环境以保持构建的可移植性，还可以用来分离构建的一些较耗时或者耗资源的行为，并给予更合适的构建频率


### 第17章 编写Maven插件

* 例如，如果想要配置Maven自动为所有Java文件的头部添加许可证声明，那么可以通过关键字maven plugin license找到maven-license-plugin[3]，这个托管在Googlecode上的项目完全能够满足我的需求。


### 17.2 案例：编写一个用于代码行统计的Maven插件

* Maven插件项目的POM有两个特殊的地方

* 它的packaging必须为maven-plugin，这种特殊的打包类型能控制Maven为其在生命周期阶段绑定插件处理相关的目标，例如在compile阶段，Maven需要为插件项目构建一个特殊插件描述符文件。

* 从上述代码中可以看到一个artifactId为maven-plugin-api的依赖，该依赖中包含了插件开发所必需的类，例如稍后会看到的AbstractMojo


### 17.3 Mojo标注

* Mojo标注每个Mojo都必须使用@Goal标注来注明其目标名称，否则Maven将无法识别该目标

* @requiresProject<true/false>表示该目标是否必须在一个Maven项目中运行，默认为true。大部分插件目标都需要依赖一个项目才能执行，但有一些例外。例如maven-help-plugin的system目标，它用来显示系统属性和环境变量信息，不需要实际项目，因此使用了@requiresProject false标注。另外，maven-archetype-plugin的generate目标也是一个很好的例子。


### 17.4 Mojo参数

* Maven支持种类多样的Mojo参数，包括单值的boolean、int、float、String、Date、File和URL，多值的数组、Collection、Map、Properties等


### 17.5 错误处理和日志

* 如果Maven执行插件目标的时候遇到MojoFailureException，就会显示“BUILDFAILURE”的错误信息，这种异常表示Mojo在运行时发现了预期的错误。例如maven-surefire-plugin运行后若发现有失败的测试就会抛出该异常

* 如果Maven执行插件目标的时候遇到MojoExecutationException，就会显示“BUILD ERROR”的错误信息。这种异常表示Mojo在运行时发现了未预期的错误


### 17.6 测试Maven插件

* Maven社区有一个用来帮助插件集成测试的插件，它就是maven-invoker-plugin。该插件能够用来在一组项目上执行Maven，并检查每个项目的构建是否成功，最后，它还可以执行BeanShell或者Groovy脚本来验证项目构建的输出。


### 18.1.1 Maven Archetype Plugin

* Archetype并不是Maven的核心特性，它也是通过插件来实现的，这一插件就是maven-archetype-plugin（http://maven.apache.org/archetype/maven-archetype-plugin/）。尽管它只是一个插件，但由于其使用范围非常广泛，主要的IDE（如Eclipse、NetBeans和IDEA）在集成Maven的时候都着重集成了archetype特性，以方便用户快速地创建Maven项目。


### 18.2 编写Archetype

* 也许你所在组织的一些项目都使用同样的框架和项目结构，为一个个项目重复同样的配置及同样的目录结构显然是难以让人接受的。更好的做法是创建一个属于自己的Archetype，这个Archetype包含了一些通用的POM配置、目录结构，甚至是Java类及资源文件，然后在创建项目的时候，就可以直接使用该Archetype，并提供一些基本参数，如groupId、artifactId、version，maven-archetype-plugin会处理其他原本需要手工处理的劳动。这样不仅节省了时间，也降低了错误配置发生的概率。

* 默认情况下，maven-archetype-plugin要求用户在使用Archetype生成项目的时候必须提供4个参数：groupId、artifactId、version和package。除此之外，用户在编写Archetype的时候可以要求额外的参数


### 18.3.1 什么是Archetype Catalog

* 当用户以不指定Archetype坐标的方式使用maven-archetype-plugin的时候，会得到一个Archetype列表供选择，这个列表的信息来源于一个名为archetype-catalog.xml的文件


### 18.3.3 生成本地仓库的Archetype Catalog

* maven-archetype-plugin提供了一个名为crawl的目标，用户可以用它来遍历本地Maven仓库的内容并自动生成archetype-catalog.xml文件


## 个人笔记部分


### 7.8.1 插件仓库

* 值得一提的是，Maven会区别对待依赖的远程仓库与插件的远程仓库  （个人笔记: 坑我很久，今天填坑成功。）


### 7.8.2 插件的默认groupId

* 笔者不推荐使用Maven的这一机制，虽然这么做可以省略一些配置，但这样的配置会让团队中不熟悉Maven的成员感到费解，况且能省略的配置也就仅仅一行而已  （个人笔记: 贼他妈赞同。）

