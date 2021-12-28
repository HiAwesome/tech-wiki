# Spring实战（第5版）

 **克雷格·沃斯**


## 划线部分


### 1.1 什么是Spring

* @Configuration注解会告知Spring这是一个配置类，会为Spring应用上下文提供bean。这个配置类的方法使用@Bean注解进行了标注，表明这些方法所返回的对象会以bean的形式添加到Spring的应用上下文中（默认情况下，这些bean所对应的bean ID与定义它们的方法名称是相同的）。

* 自动配置起源于所谓的自动装配（autowiring）和组件扫描（component scanning）。

* Spring Boot是Spring框架的扩展，提供了很多增强生产效率的方法。最为大家所熟知的增强方法就是自动配置（autoconfiguration），Spring Boot能够基于类路径中的条目、环境变量和其他因素合理猜测需要配置的组件并将它们装配在一起。


### 1.2 初始化Spring应用

* 选择了将应用构建成一个可执行的JAR文件，而不是WAR文件。这可能是你所做出的最奇怪的选择之一，对Web应用来说尤为如此。毕竟，传统的Java Web应用都是打包成WAR文件，JAR只是用来打包库和较为少见的桌面UI应用的。

* Spring Boot starter依赖的特别之处在于它们本身并不包含库代码，而是传递性地拉取其他的库。

* @SpringBootApplication是一个组合注解，它组合了3个其他的注解。•@SpringBootConfiguration：将该类声明为配置类。尽管这个类目前还没有太多的配置，但是后续我们可以按需添加基于Java的Spring框架配置。这个注解实际上是@Configuration注解的特殊形式。•@EnableAutoConfiguration：启用Spring Boot的自动配置。我们随后会介绍自动配置的更多功能。就现在来说，我们只需要知道这个注解会告诉Spring Boot自动配置它认为我们会用到的组件。•@ComponentScan：启用组件扫描。这样我们能够通过像@Component、@Controller、@Service这样的注解声明其他类，Spring会自动发现它们并将它们注册为Spring应用上下文中的组件。


### 1.3 编写Spring应用

* 当探测到变更的时候，DevTools只会重新加载包含项目代码的类加载器，并重启Spring的应用上下文，在这个过程中另外一个类加载器和JVM会原封不动。这个策略非常精细，但是它能减少应用启动的时间。


### 1.4 俯瞰Spring风景线

* SpringMicroservices in Action一书


### 2.4 使用视图控制器

* 采用扩展已有配置类的方式能够避免创建新的配置类，从而减少项目中制件的数量。但是，我倾向于为每种配置（Web、数据、安全等）创建新的配置类，这样能够保持应用的引导配置类尽可能地整洁和简单。


### 2.5 选择视图模板库

* 你可能已经注意到了，在表2.2中，JSP并不需要在构建文件中添加任何特殊的依赖。这是因为Servlet容器本身（默认是Tomcat）会实现JSP，因此不需要额外的依赖。但是，如果你选择使用JSP，会有另外一个问题。事实上，Java Servlet容器包括嵌入式的Tomcat和Jetty容器，通常会在“/WEB-INF”目录下寻找JSP。如果我们将应用构建成一个可执行的JAR文件，就无法满足这种需求了。因此，只有在将应用构建为WAR文件并部署到传统的Servlet容器中时，才能选择JSP方案。如果你想要构建可执行的JAR文件，那么必须选择Thymeleaf、FreeMarker或表2.2中的其他方案。

* 另外一种更简单的方式是使用Spring Boot的DevTools，就像我们在第1章中的做法一样。DevTools提供了很多非常有用的开发期特性，其中有一项功能就是禁用所有模板库的缓存，但是在应用部署的时候DevTools会将自身禁用掉（从而能够重新启用模板缓存）。


### 2.6 小结

* 大多数的请求处理方法最终会返回一个视图的逻辑名称，比如Thymeleaf模板，请求会转发到这样的视图上（同时会带有任意的模型数据）。


### 3.1 使用JDBC读取和写入数据

* Spring定义了一系列的构造型（stereotype）注解，@Repository是其中之一，其他注解还包括@Controller和@Component。为JdbcIngredientRepository添加@Repository注解之后，Spring的组件扫描就会自动发现它，并且会将其初始化为Spring应用上下文中的bean。


### 3.2 使用Spring Data JPA持久化数据

* 你可能会想，我们应该需要编写它们的实现类，包括每个实现类所需的十多个方法。但是，Spring Data JPA带来的好消息是，我们根本就不用编写实现类！当应用启动的时候，Spring Data JPA会在运行期自动生成实现类。这意味着，我们现在就可以使用这些repository了。我们只需要像使用基于JDBC的实现那样将它们注入控制器中就可以了。

* 本质上，Spring Data定义了一组小型的领域特定语言（Domain-Specific Language，DSL），在这里持久化的细节都是通过repository方法的签名来描述的。

* 尽管方法名称约定对于相对简单的查询非常有用，但是，不难想象，对于更为复杂的查询，方法名可能会面临失控的风险。在这种情况下，可以将方法定义为任何你想要的名称，并为其添加@Query注解，从而明确指明方法调用时要执行的查询，如下面的样例所示：


### 4.2 配置Spring Security

* 我们注意到UserRepositoryUserDetailsService上添加了@Service。这是Spring的另外一个构造型（stereotype）注解，它表明这个类要包含到Spring的组件扫描中，所以我们不需要再明确将这个类声明为bean了。Spring将会自动发现它并将其初始化为一个bean。

* 这是因为在默认情况下，所有的请求都需要认证。接下来，我们看一下Web请求是如何被拦截和保护的，这样我们才能解决这个先有鸡还是先有蛋的诡异问题。


### 4.3 保护Web请求

* 这些规则的顺序是很重要的。声明在前面的安全规则比后面声明的规则有更高的优先级。如果我们交换这两个安全规则的顺序，那么所有的请求都会有permitAll()的规则，对“/design”和“/orders”声明的规则就不会生效了。


* CSRF的防护非常重要，并且很容易在表单中实现，所以我们没有理由禁用它。


### 4.4 了解用户是谁

* 尽管这个代码片段充满了安全性相关的代码，但是它与前文所述的其他方法相比有一个优势：它可以在应用程序的任何地方使用，而不仅仅是在控制器的处理器方法中。这使得它非常适合在较低级别的代码中使用。


### 5.1 细粒度的自动配置

* 但是，在开始使用配置属性之前，我们需要理解这些属性是从哪里来的。

* Spring环境会拉取多个属性源，包括：
•JVM系统属性；
•操作系统环境变量；
•命令行参数；
•应用属性配置文件。
它会将这些属性聚合到一个源中，通过这个源可以注入到Spring的bean中。


### 5.2 创建自己的配置属性

* 尽管我们很容易就可以将@Validated、@Min和@Max注解用到OrderController（和其他可以注入OrderProps的地方），但是这样会使OrderController更加混乱。通过配置属性的持有者bean，我们将所有的配置属性收集到了一个地方，这样就能让使用这些属性的bean尽可能保持整洁。

* 为了帮助那些使用你所定义的配置属性的人（有可能就是你本人），为这些属性创建一些元数据是非常好的办法，至少它能消除IDE上那些烦人的黄色警告。


### 5.3 使用profile进行配置

* 相对于这种方式，我更加倾向于采用Spring profile。profile是一种条件化的配置，在运行时，根据哪些profile处于激活状态，可以使用或忽略不同的bean、配置类和配置属性。

* 定义特定profile相关的属性的另外一种方式仅适用于YAML配置。它会将特定profile的属性和非profile的属性都放到application.yml中，它们之间使用3个中划线进行分割，并且使用spring.profiles属性来命名profile。

* 因此，我推荐使用环境变量来设置处于激活状态的profile。

* 如果以可执行JAR文件的形式运行应用，那么我们还可以以命令行参数的形式设置激活的profile

* 在Spring应用中，profile不仅能够用来条件化地设置配置属性，接下来我们看一下如何基于处于激活状态的profile来声明特定的bean。

* 在这种情况下，@Profile注解可以将某些bean设置为仅适用于给定的profile。


### 第6章 创建REST服务

* 随着客户端的可选方案越来越多，许多应用程序采用了一种通用的设计，那就是将用户界面推到更接近客户端的地方，而让服务器公开API，通过这种API，各种客户端都能与后端功能进行交互。


### 6.1 编写RESTful控制器

* 这个新的DesignTacoController是一个REST控制器，是由@RestController注解声明的。

* 与REST最密切相关之处在于，@RestController注解会告诉Spring，控制器中的所有处理器方法的返回值都要直接写入响应体中，而不是将值放到模型中并传递给一个视图以便于进行渲染。

* Spring借助@CrossOrigin注解让CORS的使用更加简单。正如我们所看到的，@CrossOrigin允许来自任何域的客户端消费该API。

* 因为控制器的基础路径是“/design”，所以这个控制器方法处理的是针对“/design/{id}”的GET请求，其中路径的“{id}”部分是占位符。请求中的实际值将会传递给id参数，它通过@PathVariable注解与{id}占位符进行匹配。

* 客户端实际上接收到了一个无法使用的响应，但是状态码却提示一切正常。有一种更好的方式是在响应中使用HTTP 404 (NOT FOUND)状态。

* 现在，tacoById()返回的不是一个Taco对象，而是ResponseEntity<Taco>。如果找到taco，我们就将Taco包装到ResponseEntity中，并且会带有OK的HTTP状态（这也是之前的行为）。如果找不到taco，我们就将会在ResponseEntity中包装一个null，并且会带有NOT FOUND的HTTP状态，从而表明客户端试图抓取的taco并不存在。


* 在onSubmit()方法中，我们调用了HttpClient的post()方法而不是get()方法。这意味着我们不再是从API中抓取数据，而是向API发送数据。具体来讲，我们将一个taco设计（存放到model变量中）借助HTTP POST请求发送至“/design”的API端点上。

* 但是，我们设置了consumes属性。consumes属性用于指定请求输入，而produces用于指定请求输出。在这里，我们使用consumes属性，表明该方法只会处理Content-type与application/json相匹配的请求。

* @RequestBody注解能够确保请求体中的JSON会被绑定到Taco对象上。

* 在POST请求的情况下，201 (CREATED)的HTTP状态更具有描述性。它会告诉客户端，请求不仅成功了，还创建了一个资源。在适当的地方使用@ResponseStatus将最具描述性和最精确的HTTP状态码传递给客户端是一种更好的做法。


* GET请求用来从服务端往客户端传输数据，而PUT请求则是从客户端往服务端发送数据。

* 从这个意义上讲，PUT真正的目的是执行大规模的替换（replacement）操作，而不是更新操作。HTTP PATCH的目的是对资源数据打补丁或局部更新。

* 从语义上讲，PUT意味着“将数据放到这个URL上”，其本质上就是替换已有的数据。

* 这是因为Spring MVC的映射注解，虽然包括了@PatchMapping和@PutMapping，但是它们只能用来指定某个方法能够处理什么类型的请求，这些注解并没有规定该如何处理请求，尽管PATCH在语义上代表局部更新，但是在处理器方法中实际编写代码执行更新的还是我们自己。

* 在这里，我们不是用新发送过来的数据完全替换已有的订单，而是探查传入Order对象的每个字段，并将所有非null的值应用到已有的订单上。这种方式允许客户端只发送要改变的属性就可以，并且对于客户端没有指定的属性，服务器端会保留已有的数据。

* 在这里，我选择捕获该EmptyResultDataAccessException异常，但是什么都没有做。在这里，我的想法是如果你尝试删除一个并不存在的资源，那么它的结果和删除之前存在这个资源是一样的。也就是，最终的效果都是资源不复存在。所以在删除之前资源是否存在并不重要。另外一种办法就是可以让deleteOrder()返回ResponseEntity，在资源不存在的时候将响应体设置为null并将HTTP状态码设置为NOT FOUND。

* deleteOrder()方法唯一需要注意的是它使用了@ResponseStatus注解，以确保响应的HTTP状态码为204（NO CONTENT）。


### 6.2 启用超媒体

* 对API URL进行硬编码和字符串操作会让客户端代码变得很脆弱。

* Spring HATEOAS项目为Spring提供了超链接的支持。它提供了一些类和资源装配器（assembler），在Spring MVC控制器返回资源之前能够为其添加链接。

* 除此之外，TacoResource并没有包含Taco的id属性。这是因为没有必要在API中暴露数据库相关的ID。从API客户端的角度来看，资源的self链接将会作为该资源的标识符。

* 我们现在的taco列表已经完全具备了超链接，不仅是它本身（recents链接），而且所有的taco条目和每个taco中的配料都有了超链接。


### 6.3 启用数据后端服务

* Spring Data REST是Spring Data家族中的另外一个成员，它会为Spring Data创建的repository自动生成REST API。我们只需要将Spring Data REST添加到构建文件中，就能得到一套API，它的操作与我们定义repository接口是一致的。

* 实际上，Spring Data REST还暴露了一个主资源（home resource），这个资源包含了所有端点的链接。我们只需要向API的基础路径发送GET请求就能得到它的结果

* @RestResource注解能够为实体提供任何我们想要的关系名和路径。

* 你可能会发现，很多命令行shell遇到请求中的&符号会出错，所以我们在前面的curl命令中为整个URL使用了引号

* UI代码需要硬编码才能请求带有指定参数的taco列表。当然，这可以正常运行。但是，如果让客户端太多地了解如何构建API请求，就会在一定程度上增加脆弱性。如果客户端能够从链接列表中查找URL就太好了。如果URL能够更简洁，就像前面看到“/design/recent”一样，那就更棒了。

* 需要注意的是，尽管@RepositoryRestController的名称和@RestController非常相似，但是它并没有和@RestController相同的语义。具体来讲，它并不能保证处理器方法返回的值会自动写入响应体中。所以，我们要么为方法添加@ResponseBody注解，要么返回包装响应数据的ResponseEntity。这里我们选择的方案是返回ResponseEntity。


### 6.4 小结

* 为了绕过视图和模型的逻辑并将数据直接写入响应体中，控制器处理方法既可以添加@ResponseBody注解也可以返回ResponseEntity对象。


### 第7章 消费REST服务

* Spring应用除了提供对外API之外，同时要对另外一个应用的API发起请求的场景并不罕见。实际上，在微服务领域，这正变得越来越普遍。因此，花点时间研究一下如何使用Spring与REST API交互是非常值得的。


### 7.2 使用Traverson导航REST API

* 当你既要导航API又要更新或删除资源时，你需要组合使用RestTemplate和Traverson。


### 第8章 发送异步消息

* 异步消息是一个应用程序向另一个应用程序间接发送消息的一种方式，这种间接性能够为进行通信的应用带来更松散的耦合和更大的可伸缩性。


### 8.2 使用RabbitMQ和AMQP

* 我们要使用Spring发送和接收RabbitMQ代理的消息，只需要添加这项依赖就可以了。

* 实际上，@RabbitListener和@JmsListener的运行方式非常相似是一件令人兴奋的事情。这意味着当我们使用RabbitMQ替代Artemis或ActiveMQ的时候，不需要学习全新的编程模型。同样令人兴奋的是，RabbitTemplate和JmsTemplate之间也具有这样的相似性。


### 10.1 反应式编程概览

* 反应式编程不是银弹。


### 10.2 初识Reactor

* 把Reactor的Bismuth版本


### 11.2 定义函数式请求处理器

* 首先，所有基于注解的编程方式都会存在注解该做什么以及注解如何做之间的割裂。注解本身定义了该做什么，而具体如何去做则是在框架代码的其他部分定义的。如果想要进行自定义或扩展，编程模型就会变得很复杂，因为这样的变更需要修改注解之外的代码。除此之外，这种代码的调试也是比较麻烦的，因为我们无法在注解上设置断点。

* 更明显的差异在于，路由是由方法引用处理的。如果RouterFunction背后的行为相对简单和简洁，那么lambda是很不错的选择。在很多场景下，最好将功能抽取到一个单独的方法中（甚至抽取到一个独立类的方法中），以便于保持代码的可读性。


### 11.5 保护反应式Web API

* 如果我们希望拦截基于Servlet技术的Web框架的请求，以确保该请求得到了恰当的授权，那么Servlet Filter是显而易见的方案。


### 第12章 反应式持久化数据

* 此，很重要的一点在于，要让整个数据流变成反应式和非阻塞的，也就是从控制器直到数据库。


### 12.1 理解Spring Data的反应式概况

* 不幸的是，至少目前还不支持关系数据库的反应式处理。希望这种情况能在不久的将来得到解决。￼


### 12.2 使用反应式的Cassandra repository

* 需要说明一点，将Taco Cloud领域类型调整为使用Cassandra，并不是简单地将几个JPA注解替换为Cassandra注解就可以了。我们必须重新考虑如何对数据进行建模。

* 如果我们想要插入很多数据，那么可能需要选择ReactiveCassandraRepository；否则，最好选择ReactiveCrudRepository，因为在不同数据库类型之间它更具可移植性。


### 13.1 思考微服务

* 因为很多的远程调用会累积并降低应用的速度。

* Cloud Native（Manning，2019）。


### 13.4 小结

* 不管采用哪种方案，客户端代码都不需要硬编码它们所消费的服务的地址。


### 14.2 运行配置服务器

* 我们必须要告诉它，它要对外提供的配置属性都位于何处。


### 14.6 在运行时刷新配置属性

* 每种方案都有其优点和缺点。手动刷新能够更精确地控制服务何时更新最新配置，但是它需要向每个微服务实例发送一个HTTP请求。自动更新能够让应用中的每个微服务即时使用最新的配置，但它是由配置仓库的提交自动触发的，对于有些项目来说过于危险。

* 在Config Server客户端应用中添加Actuator之后，我们可以在任意时间发送HTTP POST请求到“/actuator/refresh”，通知它从后端仓库刷新配置属性。

* 注意，响应中包含一个JSON数组，列出了发生变更的属性名。这个数组包含greeting.message属性，还包含config.client.version属性（当前配置对应的Git提交的哈希值）的变化。因为现在的配置基于一个新的Git提交，所以每当后端的配置仓库有变化时，这个值都会跟着变化。

* 这样做的结果就是，在配置属性变更推送到后端的Git仓库之后，所有的Config Server客户端应用能够立即获取最新的配置属性值。


### 14.7 小结

* Config Server客户端能够借助手动或自动刷新得到新的属性，前者通过Actuator端点来实现，后者通过Spring Cloud Bus和Git webhooks来实现。


### 15.2 声明断路器

* 为了实现这一点，我们可以在应用的主配置类上添加@EnableHystrix。

* 后备行为方法的唯一规则是它们要与原始方法具有相同的签名（除了方法名称之外）。

* 唯一的要求就是必须要在后备方法的底部有一个不会失败的方法，该方法不需要使用断路器。

* 默认情况下，如果断路器保护的方法调用超过20次，而且50%以上的调用在10秒的时间内发生失败，那么断路器就会进入打开状态。所有后续的调用都将会由后备方法处理。在5秒之后，断路器进入半开状态，将会再次尝试调用原始的方法。


### 15.4 聚合多个Hystrix流

* 幸运的是，Netflix的另一个项目Turbine提供了将所有微服务的所有Hystrix流聚合到一个Hystrix流中的办法，这样Hystrix dashboard就能对其进行监控了。Spring Cloud Netflix支持以类似于创建其他Spring Cloud服务的方式创建Turbine服务。要创建Turbine服务，我们需要创建一个新的Spring Boot项目并将Turbine starter依赖添加到构建文件中：


### 16.1 Actuator概览

* 在机器领域中，执行机构（Actuator）指的是负责控制和移动装置的组件。


### 16.2 消费Actuator端点

* 所有的应用，不管其外部依赖是什么，至少都会有一个针对文件系统的健康指示器，名为diskSpace。diskSpace健康指示器能够显示文件系统的健康状况（希望它是UP状态），这个状态的值是由还有多少剩余空间决定的。如果可用磁盘空间低于阈值，那么它将会报告DOWN的状态。

* 匹配上的（positive matches，即已通过的条件化配置）、未匹配上的（negative matches，即失败的条件化配置）以及非条件化的类。

* 尽管Actuator端点所提供的信息有助于观察运行中Spring Boot应用的内部状况，但是它们并不适用于人类直接使用。作为REST端点，它们是供其他应用消费的，这里所说的其他应用也可能是UI。考虑到这一点，我们在第17章会看到如何在用户友好的Web应用中展现Actuator信息。现在，我们看一下如何自定义Actuator的端点。


### 16.3 自定义Actuator

* 很重要的一点就是，尽管我只展现了如何使用HTTP与端点交互，但是它们还会暴露为MBean，我们可以使用任意的JMX客户端来进行访问。


### 19.2 构建和部署WAR文件

* 虽然WAR文件作为Java部署的主流方案已经有20多年的历史了，但是它们确实是为将应用程序部署到传统Java应用服务器而设计的。按照我们所选择的平台，现代云部署方案并不需要WAR文件，有些甚至都不支持这种格式。随着我们进入云部署的新时代，JAR文件可能是更好的选择。


### 19.4 在Docker容器中运行Spring Boot

* 在分发云中部署的各种应用时，Docker已经成为事实标准。


### 19.6 小结

* 借助Spotify的Maven Dockerfile插件，能够非常容易地容器化Spring应用。


## 个人笔记部分


### 11.6 小结

* 11.6 小结
•Spring WebFlux提供了一个反应式的Web框架，它的编程模型是与Spring MVC对应的，甚至共享了很多相同的注解。
•Spring 5还提供了函数式编程模型，作为Spring WebFlux的替代方案。
•反应式控制器可以使用WebTestClient来进行测试。
•在客户端，Spring 5提供了WebClient，也就是Spring RestTemplate的反应式等价实现。
•在保护Web应用方面，尽管WebFlux在底层有一些区别，但是Spring Security 5为反应式安全所提供的编程模型与非反应式Spring MVC应用相比并没有特别大的差异。  （个人笔记: web框架，编程模型，客户端，测试，安全）


### 16.1 Actuator概览

* 在机器领域中，执行机构（Actuator）指的是负责控制和移动装置的组件。  （个人笔记: cultrue means, it's a inherited like interface in java.）


### 19.4 在Docker容器中运行Spring Boot

* 在分发云中部署的各种应用时，Docker已经成为事实标准。  （个人笔记: So you should know more about Docker.）


### 6.4 小结

* 为了绕过视图和模型的逻辑并将数据直接写入响应体中，控制器处理方法既可以添加@ResponseBody注解也可以返回ResponseEntity对象。  （个人笔记: Prune backend!）


### 15.4 聚合多个Hystrix流

* 幸运的是，Netflix的另一个项目Turbine提供了将所有微服务的所有Hystrix流聚合到一个Hystrix流中的办法，这样Hystrix dashboard就能对其进行监控了。Spring Cloud Netflix支持以类似于创建其他Spring Cloud服务的方式创建Turbine服务。要创建Turbine服务，我们需要创建一个新的Spring Boot项目并将Turbine starter依赖添加到构建文件中：  （个人笔记: another demand, another open source project, use it, create another springboot application.）


### 6.2 启用超媒体

* 对API URL进行硬编码和字符串操作会让客户端代码变得很脆弱。  （个人笔记: 
不够稳健）

* 除此之外，TacoResource并没有包含Taco的id属性。这是因为没有必要在API中暴露数据库相关的ID。从API客户端的角度来看，资源的self链接将会作为该资源的标识符。  （个人笔记: System design）

* 我们现在的taco列表已经完全具备了超链接，不仅是它本身（recents链接），而且所有的taco条目和每个taco中的配料都有了超链接。  （个人笔记: 解除耦合代价大）


### 19.6 小结

* 借助Spotify的Maven Dockerfile插件，能够非常容易地容器化Spring应用。  （个人笔记: Try it later.）


### 14.7 小结

* Config Server客户端能够借助手动或自动刷新得到新的属性，前者通过Actuator端点来实现，后者通过Spring Cloud Bus和Git webhooks来实现。  （个人笔记: More important on centralized configuration）


### 第6章 创建REST服务

* 随着客户端的可选方案越来越多，许多应用程序采用了一种通用的设计，那就是将用户界面推到更接近客户端的地方，而让服务器公开API，通过这种API，各种客户端都能与后端功能进行交互。  （个人笔记: 
前后端分离）


### 12.1 理解Spring Data的反应式概况

* 不幸的是，至少目前还不支持关系数据库的反应式处理。希望这种情况能在不久的将来得到解决。  （个人笔记: 
Wait for a new jdbc）


### 第12章 反应式持久化数据

* 此，很重要的一点在于，要让整个数据流变成反应式和非阻塞的，也就是从控制器直到数据库。  （个人笔记: Back to end.）


### 14.6 在运行时刷新配置属性

* 每种方案都有其优点和缺点。手动刷新能够更精确地控制服务何时更新最新配置，但是它需要向每个微服务实例发送一个HTTP请求。自动更新能够让应用中的每个微服务即时使用最新的配置，但它是由配置仓库的提交自动触发的，对于有些项目来说过于危险。  （个人笔记: You just need some trade off again.）

* 在Config Server客户端应用中添加Actuator之后，我们可以在任意时间发送HTTP POST请求到“/actuator/refresh”，通知它从后端仓库刷新配置属性。  （个人笔记: Trigger change by http post request every time.）

* 注意，响应中包含一个JSON数组，列出了发生变更的属性名。这个数组包含greeting.message属性，还包含config.client.version属性（当前配置对应的Git提交的哈希值）的变化。因为现在的配置基于一个新的Git提交，所以每当后端的配置仓库有变化时，这个值都会跟着变化。  （个人笔记: It means you get config.client.version every time, another fields is what you actrually change.）

* 这样做的结果就是，在配置属性变更推送到后端的Git仓库之后，所有的Config Server客户端应用能够立即获取最新的配置属性值。  （个人笔记: As soon as possible.）


### 15.2 声明断路器

* 为了实现这一点，我们可以在应用的主配置类上添加@EnableHystrix。  （个人笔记: Every time we want config something on whole application, we will append @Enable*** on @SpringBootApplication. It's on application level.）

* 后备行为方法的唯一规则是它们要与原始方法具有相同的签名（除了方法名称之外）。  （个人笔记: It makes backup behavior method can hold all request from origin method.）

* 唯一的要求就是必须要在后备方法的底部有一个不会失败的方法，该方法不需要使用断路器。  （个人笔记: Recursion failed need resursion hystrix and final normal method.）

* 默认情况下，如果断路器保护的方法调用超过20次，而且50%以上的调用在10秒的时间内发生失败，那么断路器就会进入打开状态。所有后续的调用都将会由后备方法处理。在5秒之后，断路器进入半开状态，将会再次尝试调用原始的方法。  （个人笔记: It means hystrix think origin method will be re healthy now.）


### 5.3 使用profile进行配置

* 定义特定profile相关的属性的另外一种方式仅适用于YAML配置。它会将特定profile的属性和非profile的属性都放到application.yml中，它们之间使用3个中划线进行分割，并且使用spring.profiles属性来命名profile。  （个人笔记: yaml 大法好）


### 8.2 使用RabbitMQ和AMQP

* 我们要使用Spring发送和接收RabbitMQ代理的消息，只需要添加这项依赖就可以了。  （个人笔记: 从更高的层次上来看，他引用新的组件的方式是一种更高级的模版代码。）

* 实际上，@RabbitListener和@JmsListener的运行方式非常相似是一件令人兴奋的事情。这意味着当我们使用RabbitMQ替代Artemis或ActiveMQ的时候，不需要学习全新的编程模型。同样令人兴奋的是，RabbitTemplate和JmsTemplate之间也具有这样的相似性。  （个人笔记: 更高层次的面向接口编程。）


### 13.1 思考微服务

* Cloud Native（Manning，2019）。  （个人笔记: Wow, new book）

* Bundle und unbundled, always that, the war never stop.


### 10.2 初识Reactor

* 把Reactor的Bismuth版本  （个人笔记: 案例中scope：import解决maven单继承问题，当前项目可以存在多个父pom）


### 14.2 运行配置服务器

* 我们必须要告诉它，它要对外提供的配置属性都位于何处。  （个人笔记: 绝对重要的配置属性。）


### 19.2 构建和部署WAR文件

* 虽然WAR文件作为Java部署的主流方案已经有20多年的历史了，但是它们确实是为将应用程序部署到传统Java应用服务器而设计的。按照我们所选择的平台，现代云部署方案并不需要WAR文件，有些甚至都不支持这种格式。随着我们进入云部署的新时代，JAR文件可能是更好的选择。  （个人笔记: Jar is more modern than war.）


### 11.2 定义函数式请求处理器

* 更明显的差异在于，路由是由方法引用处理的。如果RouterFunction背后的行为相对简单和简洁，那么lambda是很不错的选择。在很多场景下，最好将功能抽取到一个单独的方法中（甚至抽取到一个独立类的方法中），以便于保持代码的可读性。  （个人笔记: 工具类结构）


### 16.2 消费Actuator端点

* 所有的应用，不管其外部依赖是什么，至少都会有一个针对文件系统的健康指示器，名为diskSpace。diskSpace健康指示器能够显示文件系统的健康状况（希望它是UP状态），这个状态的值是由还有多少剩余空间决定的。如果可用磁盘空间低于阈值，那么它将会报告DOWN的状态。  （个人笔记: so you better know diskSpace is so important for any application.）

* 匹配上的（positive matches，即已通过的条件化配置）、未匹配上的（negative matches，即失败的条件化配置）以及非条件化的类。  （个人笔记: It's core magic on springboot auto config.）

* 尽管Actuator端点所提供的信息有助于观察运行中Spring Boot应用的内部状况，但是它们并不适用于人类直接使用。作为REST端点，它们是供其他应用消费的，这里所说的其他应用也可能是UI。考虑到这一点，我们在第17章会看到如何在用户友好的Web应用中展现Actuator信息。现在，我们看一下如何自定义Actuator的端点。  （个人笔记: so it's pretty good design: major service expose http and response json, then user could use it by cli or UI tools. Generally cli used by shell or other automatic tools, UI tools used by dashboard to see whole applecation states.）


### 12.2 使用反应式的Cassandra repository

* 需要说明一点，将Taco Cloud领域类型调整为使用Cassandra，并不是简单地将几个JPA注解替换为Cassandra注解就可以了。我们必须重新考虑如何对数据进行建模。  （个人笔记: 
有点oltp vs olap 那味了。）

* 如果我们想要插入很多数据，那么可能需要选择ReactiveCassandraRepository；否则，最好选择ReactiveCrudRepository，因为在不同数据库类型之间它更具可移植性。  （个人笔记: 接口的设计与其见名知义的重要性。）


### 6.1 编写RESTful控制器

* 与REST最密切相关之处在于，@RestController注解会告诉Spring，控制器中的所有处理器方法的返回值都要直接写入响应体中，而不是将值放到模型中并传递给一个视图以便于进行渲染。  （个人笔记: 
意味着不使用传统MVC）

* 客户端实际上接收到了一个无法使用的响应，但是状态码却提示一切正常。有一种更好的方式是在响应中使用HTTP 404 (NOT FOUND)状态。  （个人笔记: 一切按照规范来。）

* 现在，tacoById()返回的不是一个Taco对象，而是ResponseEntity<Taco>。如果找到taco，我们就将Taco包装到ResponseEntity中，并且会带有OK的HTTP状态（这也是之前的行为）。如果找不到taco，我们就将会在ResponseEntity中包装一个null，并且会带有NOT FOUND的HTTP状态，从而表明客户端试图抓取的taco并不存在。  （个人笔记: 
专业．）

* 在onSubmit()方法中，我们调用了HttpClient的post()方法而不是get()方法。这意味着我们不再是从API中抓取数据，而是向API发送数据。具体来讲，我们将一个taco设计（存放到model变量中）借助HTTP POST请求发送至“/design”的API端点上。  （个人笔记: 
角度好．）

* 但是，我们设置了consumes属性。consumes属性用于指定请求输入，而produces用于指定请求输出。在这里，我们使用consumes属性，表明该方法只会处理Content-type与application/json相匹配的请求。  （个人笔记: 
生产者和消费者、）

* 在POST请求的情况下，201 (CREATED)的HTTP状态更具有描述性。它会告诉客户端，请求不仅成功了，还创建了一个资源。在适当的地方使用@ResponseStatus将最具描述性和最精确的HTTP状态码传递给客户端是一种更好的做法。  （个人笔记: 
专业，精确。）

* 为什么会有两种不同的HTTP方法来更新资源？  （个人笔记: 
Good question!）

* 从这个意义上讲，PUT真正的目的是执行大规模的替换（replacement）操作，而不是更新操作。HTTP PATCH的目的是对资源数据打补丁或局部更新。  （个人笔记: 
全局更新V S 部分更新）

* 在这里，我选择捕获该EmptyResultDataAccessException异常，但是什么都没有做。在这里，我的想法是如果你尝试删除一个并不存在的资源，那么它的结果和删除之前存在这个资源是一样的。也就是，最终的效果都是资源不复存在。所以在删除之前资源是否存在并不重要。另外一种办法就是可以让deleteOrder()返回ResponseEntity，在资源不存在的时候将响应体设置为null并将HTTP状态码设置为NOT FOUND。  （个人笔记: 

It depend on what consumer want.）


### 5.2 创建自己的配置属性

* 尽管我们很容易就可以将@Validated、@Min和@Max注解用到OrderController（和其他可以注入OrderProps的地方），但是这样会使OrderController更加混乱。通过配置属性的持有者bean，我们将所有的配置属性收集到了一个地方，这样就能让使用这些属性的bean尽可能保持整洁。  （个人笔记: 规范）

* 为了帮助那些使用你所定义的配置属性的人（有可能就是你本人），为这些属性创建一些元数据是非常好的办法，至少它能消除IDE上那些烦人的黄色警告。  （个人笔记: 哈哈）


### 6.3 启用数据后端服务

* 你可能会发现，很多命令行shell遇到请求中的&符号会出错，所以我们在前面的curl命令中为整个URL使用了引号  （个人笔记: 自动转义）

* UI代码需要硬编码才能请求带有指定参数的taco列表。当然，这可以正常运行。但是，如果让客户端太多地了解如何构建API请求，就会在一定程度上增加脆弱性。如果客户端能够从链接列表中查找URL就太好了。如果URL能够更简洁，就像前面看到“/design/recent”一样，那就更棒了。  （个人笔记: 取舍）

* 需要注意的是，尽管@RepositoryRestController的名称和@RestController非常相似，但是它并没有和@RestController相同的语义。具体来讲，它并不能保证处理器方法返回的值会自动写入响应体中。所以，我们要么为方法添加@ResponseBody注解，要么返回包装响应数据的ResponseEntity。这里我们选择的方案是返回ResponseEntity。  （个人笔记: 不太优雅）


### 16.3 自定义Actuator

* 很重要的一点就是，尽管我只展现了如何使用HTTP与端点交互，但是它们还会暴露为MBean，我们可以使用任意的JMX客户端来进行访问。  （个人笔记: define a high level interface than http or jmx bean.）

