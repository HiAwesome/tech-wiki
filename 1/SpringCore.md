# [SpringCore](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#spring-core)

* 配置元数据: 
  * 配置元数据传统上以简单直观的 XML 格式提供。
  * 基于注解的配置：Spring 2.5 引入了对基于注解的配置元数据的支持。 
  * 基于 Java 的配置：从 Spring 3.0 开始，Spring JavaConfig 项目提供的许多特性成为核心 Spring Framework 的一部分。因此，您可以使用 Java 而不是 XML 文件来定义应用程序类外部的 bean。要使用这些新功能，请参阅 @Configuration、 @Bean、 @Import 和 @DependsOn 注释。
* Bean 元数据和手动提供的单例实例需要尽早注册，以便容器在自动装配和其他自省步骤中正确推理它们。虽然在某种程度上支持覆盖现有元数据和现有单例实例，但官方不支持在运行时注册新 bean（同时对工厂进行实时访问），可能导致并发访问异常、bean 容器中的状态不一致等多种问题。
* Bean 命名约定: 约定是在命名 bean 时对实例字段名称使用标准 Java 约定。也就是说，bean 名称以小写字母开头，并且从那里开始是驼峰式的。此类名称的示例包括accountManager、 accountService、userDao、loginController等。 一致地命名 bean 使您的配置更易于阅读和理解。此外，如果您使用 Spring AOP，则在将建议应用于一组按名称相关的 bean 时会很有帮助。
* 实例化 Bean:
  * 使用构造函数进行实例化。
  * 使用静态工厂方法进行实例化。
  * 使用实例工厂方法进行实例化。
* 依赖注入:
  * 基于构造函数的依赖注入。
    * 构造函数参数类型匹配。
    * 构造函数参数索引。
    * 构造函数参数名称。
  * 基于 Setter 的依赖注入。
* 基于构造函数还是基于 setter 的 DI？ 
  * 由于您可以混合使用基于构造函数和基于 setter 的 DI，因此将构造函数用于强制依赖项并将 setter 方法或配置方法用于可选依赖项是一个很好的经验法则。请注意，在 setter 方法上使用@Required 注释可用于使属性成为必需的依赖项；然而，带有参数的编程验证的构造函数注入是更可取的。 
  * Spring 团队通常提倡构造函数注入，因为它允许您将应用程序组件实现为不可变对象，并确保所需的依赖项不是null. 此外，构造函数注入的组件总是以完全初始化的状态返回给客户端（调用）代码。附带说明一下，大量的构造函数参数是一种不好的代码气味，这意味着该类可能有太多的职责，应该重构以更好地解决适当的关注点分离问题。 
  * Setter 注入应该主要只用于可以在类中分配合理默认值的可选依赖项。否则，必须在代码使用依赖项的任何地方执行非空检查。setter 注入的一个好处是 setter 方法使该类的对象可以在以后重新配置或重新注入。因此，通过JMX MBean进行管理是 setter 注入的一个引人注目的用例。 
  * 使用对特定类最有意义的 DI 样式。有时，在处理您没有源代码的第三方类时，会为您做出选择。例如，如果第三方类没有公开任何 setter 方法，那么构造函数注入可能是 DI 的唯一可用形式。
* idref元素_: 第一种形式比第二种形式更可取，因为使用idref标签可以让容器在部署时验证所引用的命名 bean 确实存在。在第二个变体中，不对传递给beantargetName属性的值执行验证。只有在实际实例化 beanclient时才会发现拼写错误（很可能是致命的结果） 。client如果client bean 是一个原型bean，那么这个拼写错误和产生的异常可能只有在容器部署很久之后才会被发现。
  ```xml
  <!-- 第一种 -->
  <bean id="theTargetBean" class="..."/>
  <bean id="theClientBean" class="...">
      <property name="targetName">
          <idref bean="theTargetBean"/>
      </property>
  </bean>
  
  <!-- 第二种 -->
  <bean id="theTargetBean" class="..." />
  <bean id="client" class="...">
      <property name="targetName" value="theTargetBean"/>
  </bean>
  ```
* 内部 bean 定义不需要定义的 ID 或名称。如果指定，容器不会使用这样的值作为标识符。容器在创建时也会忽略该scope标志，因为内部 bean 始终是匿名的，并且始终使用外部 bean 创建。不可能独立访问内部 bean 或将它们注入到协作 bean 中，而不是注入封闭 bean。
* 带有 p 命名空间的 XML 快捷方式: p-namespace 允许您使用bean元素的属性（而不是嵌套 <property/>元素）来描述协作 bean 或两者的属性值。 Spring 支持带有命名空间的可扩展配置格式，它基于 XML 模式定义。本章讨论的beans配置格式在 XML Schema 文档中定义。但是，p-namespace 没有在 XSD 文件中定义，仅存在于 Spring 的核心中。 以下示例显示了解析为相同结果的两个 XML 片段（第一个使用标准 XML 格式，第二个使用 p-namespace）：
  ```text
  <beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean name="john-classic" class="com.example.Person">
        <property name="name" value="John Doe"/>
        <property name="spouse" ref="jane"/>
    </bean>

    <bean name="john-modern"
        class="com.example.Person"
        p:name="John Doe"
        p:spouse-ref="jane"/>

    <bean name="jane" class="com.example.Person">
        <property name="name" value="Jane Doe"/>
    </bean>
  </beans>
  ```
* p 命名空间不如标准 XML 格式灵活。例如，声明属性引用的格式与以 结尾的属性冲突Ref，而标准 XML 格式则不会。我们建议您仔细选择您的方法并将其传达给您的团队成员，以避免生成同时使用所有三种方法的 XML 文档。
* 带有 c 命名空间的 XML 快捷方式: 与带有 p-namespace 的 XML Shortcut类似，Spring 3.1 中引入的 c-namespace 允许内联属性来配置构造函数参数，而不是嵌套constructor-arg元素。 以下示例使用c:命名空间执行与 基于构造函数的依赖注入相同的操作：
  ```text
  <beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:c="http://www.springframework.org/schema/c"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
          https://www.springframework.org/schema/beans/spring-beans.xsd">
  
      <bean id="beanTwo" class="x.y.ThingTwo"/>
      <bean id="beanThree" class="x.y.ThingThree"/>
  
      <!-- traditional declaration with optional argument names -->
      <bean id="beanOne" class="x.y.ThingOne">
          <constructor-arg name="thingTwo" ref="beanTwo"/>
          <constructor-arg name="thingThree" ref="beanThree"/>
          <constructor-arg name="email" value="something@somewhere.com"/>
      </bean>
  
      <!-- c-namespace declaration with argument names -->
      <bean id="beanOne" class="x.y.ThingOne" c:thingTwo-ref="beanTwo"
          c:thingThree-ref="beanThree" c:email="something@somewhere.com"/>
  
  </beans>
  ```
* 1.4.3. 使用depends-on: 如果一个 bean 是另一个 bean 的依赖项，这通常意味着一个 bean 被设置为另一个 bean 的属性。通常，您使用基于 XML 的配置元数据中的<ref/> 元素来完成此操作。但是，有时 bean 之间的依赖关系不那么直接。例如，当需要触发类中的静态初始化程序时，例如用于数据库驱动程序注册。在初始化使用此元素的 bean 之前，该depends-on属性可以显式强制初始化一个或多个 bean。以下示例使用该depends-on属性来表达对单个 bean 的依赖; 要表达对多个 bean 的依赖，请提供 bean 名称列表作为depends-on属性值（逗号、空格和分号是有效的分隔符）：
  ```xml
  <!-- single depends-on -->
  <bean id="beanOne" class="ExampleBean" depends-on="manager"/>
  <bean id="manager" class="ManagerBean" />
  
  
  <!-- multi depends-on -->
  <bean id="beanOne" class="ExampleBean" depends-on="manager,accountDao">
      <property name="manager" ref="manager" />
  </bean>
  
  <bean id="manager" class="ManagerBean" />
  <bean id="accountDao" class="x.y.jdbc.JdbcAccountDao" />
  ```
* 任意方法替换: 与查找方法注入相比，一种不太有用的方法注入形式是能够用另一种方法实现替换托管 bean 中的任意方法。在您真正需要此功能之前，您可以放心地跳过本节的其余部分。 使用基于 XML 的配置元数据，您可以使用该replaced-method元素将现有方法实现替换为另一个，用于已部署的 bean。
* 从 Spring 3.0 开始，线程范围可用，但默认情况下未注册。有关详细信息，请参阅 SimpleThreadScope. 有关如何注册此或任何其他自定义范围的说明，请参阅 使用自定义范围。
* Spring 的单例 bean 概念不同于四人组 (GoF) 模式书中定义的单例模式。GoF 单例对对象的范围进行硬编码，以便每个 ClassLoader 创建一个且仅一个特定类的实例。Spring 单例的范围最好描述为每个容器和每个 bean。这意味着，如果您在单个 Spring 容器中为特定类定义一个 bean，则 Spring 容器会创建该 bean 的定义而定义类的一个且仅一个实例。单例范围是 Spring 中的默认范围。要将 bean 定义为 XML 中的单例，您可以定义一个 bean，如下例所示：
  ```xml
  <bean id="accountService" class="com.something.DefaultAccountService"/>
  
  <!-- the following is equivalent, though redundant (singleton scope is the default) -->
  <bean id="accountService" class="com.something.DefaultAccountService" scope="singleton"/> 
  ```
* bean 部署的非单例原型范围导致每次对特定 bean 发出请求时都会创建一个新的 bean 实例。也就是说，将 bean 注入到另一个 bean 中，或者您通过getBean()容器上的方法调用来请求它。**通常，您应该对所有有状态 bean 使用原型范围，对无状态 bean 使用单例范围。** 以下示例将 bean 定义为 XML 中的原型：`<bean id="accountService" class="com.something.DefaultAccountService" scope="prototype"/>`
* CGLIB 代理只拦截公共方法调用！不要在此类代理上调用非公共方法。它们没有委托给实际作用域的目标对象。
* bean 作用域机制是可扩展的。您可以定义自己的范围，甚至重新定义现有范围，尽管后者被认为是不好的做法，并且您不能覆盖内置singleton和prototype范围。
* JSR-250 @PostConstruct 和 @PreDestroy 注释通常被认为是在现代 Spring 应用程序中接收生命周期回调的最佳实践。使用这些注解意味着您的 bean 不会耦合到 Spring 特定的接口。有关详细信息，请参阅使用 @PostConstruct 和 @PreDestroy。 如果您不想使用 JSR-250 注释但仍想移除耦合，请考虑 init-method 和 destroy-method bean 定义元数据。
* 我们建议您不要使用该InitializingBean接口，因为它不必要地将代码耦合到 Spring。或者，我们建议使用@PostConstruct注解或指定 POJO 初始化方法。在基于 XML 的配置元数据的情况下，您可以使用该init-method属性来指定具有无效无参数签名的方法的名称。通过 Java 配置，您可以initMethod使用 @Bean. 请参阅接收生命周期回调。
* 我们建议您不要使用DisposableBean回调接口，因为它不必要地将代码耦合到 Spring。或者，我们建议使用@PreDestroy注释或指定 bean 定义支持的通用方法。使用基于 XML 的配置元数据，您可以destroy-method使用<bean/>. 通过 Java 配置，您可以destroyMethod使用@Bean. 请参阅 接收生命周期回调。
* 如果为一个 bean 配置了多个生命周期机制，并且每个机制都配置了不同的方法名称，那么每个配置的方法都按照本注释后列出的顺序运行。init()但是，如果为多个生命周期机制 配置了相同的方法名称（例如， 对于初始化方法），则该方法将运行一次。
* ApplicationContext默认情况下预实例化所有单例。因此，重要的是（至少对于单例 bean），如果您有一个（父）bean 定义，您打算仅将其用作模板，并且此定义指定了一个类，则必须确保将抽象属性设置为true，否则应用程序上下文将实际（尝试）预实例化abstract bean。
* BeanPostProcessor实例和 AOP 自动代理: 实现BeanPostProcessor接口的类是特殊的，被容器区别对待。它们直接引用的所有BeanPostProcessor实例和 bean 都在启动时实例化，作为ApplicationContext. 接下来，所有BeanPostProcessor实例都以排序方式注册并应用于容器中的所有其他 bean。因为 AOP 自动代理是作为BeanPostProcessor自身实现的，所以BeanPostProcessor 实例和它们直接引用的 bean 都没有资格进行自动代理，因此没有将方面编织到其中。 对于任何这样的 bean，您应该会看到一条信息性日志消息：Bean someBean is not eligible for getting processed by all BeanPostProcessor interfaces (for example: not eligible for auto-proxying). 如果您BeanPostProcessor使用自动装配或 @Resource（可能回退到自动装配）将 bean 连接到您的 bean，则 Spring 在搜索类型匹配依赖项候选时可能会访问意外的 bean，因此，使它们没有资格进行自动代理或其他类型的 bean 发布-加工。例如，如果您有一个依赖项，@Resource其中字段或 setter 名称不直接对应于 bean 的声明名称并且没有使用 name 属性，则 Spring 会访问其他 bean 以按类型匹配它们。
* PropertySourcesPlaceholderConfigurer不仅在您指定的文件中查找属性Properties 。默认情况下，如果在指定的属性文件中找不到属性，它会检查 SpringEnvironment属性和常规 JavaSystem属性。
* 在配置 Spring 时，注解是否比 XML 更好？基于注释的配置的引入提出了这种方法是否比 XML“更好”的问题。简短的回答是“视情况而定”。长答案是每种方法都有其优点和缺点，通常由开发人员决定哪种策略更适合他们。由于它们的定义方式，注释在其声明中提供了大量上下文，从而使配置更短、更简洁。然而，XML 擅长在不触及源代码或重新编译它们的情况下连接组件。一些开发人员更喜欢在源附近进行布线，而另一些开发人员则认为带注释的类不再是 POJO，此外，配置变得分散且更难控制。 无论选择如何，Spring 都可以同时适应这两种风格，甚至可以将它们混合在一起。值得指出的是，通过其JavaConfig选项，Spring 允许以非侵入性的方式使用注释，而无需触及目标组件的源代码，并且在工具方面， Spring Tools for Eclipse支持所有配置样式。
* 注解注入在 XML 注入之前执行。因此，XML 配置覆盖了通过这两种方法连接的属性的注释。
* 从Spring Framework 5.1 开始，@Required注解和RequiredAnnotationBeanPostProcessor正式弃用，支持使用构造函数注入进行所需设置（或自定义实现InitializingBean.afterPropertiesSet() 或自定义@PostConstruct方法以及 bean 属性设置方法）。
* 从 Spring Framework 4.3 开始，@Autowired如果目标 bean 仅定义一个构造函数开始，则不再需要对此类构造函数进行注释。但是，如果有多个构造函数可用并且没有主/默认构造函数，则必须至少对其中一个构造函数进行注释，@Autowired以指示容器使用哪一个。
* 确保您的目标组件（例如，MovieCatalog或CustomerPreferenceDao）始终由您用于带@Autowired注释的注入点的类型声明。否则，注入可能会由于运行时出现“找不到类型匹配”错误而失败。对于通过类路径扫描找到的 XML 定义的 bean 或组件类，容器通常预先知道具体类型。但是，对于@Bean工厂方法，您需要确保声明的返回类型具有足够的表现力。对于实现多个接口的组件或可能由其实现类型引用的组件，请考虑在您的工厂方法中声明最具体的返回类型（至少与引用您的 bean 的注入点所要求的一样具体）。
* 如果您希望数组或列表中的项目按特定顺序排序，您的目标 bean 可以实现org.springframework.core.Ordered接口或使用@Order标准注释。@Priority否则，它们的顺序遵循容器中相应目标 bean 定义的注册顺序。 您可以@Order在目标类级别和@Bean方法上声明注释，可能针对单个 bean 定义（在使用相同 bean 类的多个定义的情况下）。@Order值可能会影响注入点的优先级，但请注意它们不会影响单例启动顺序，这是由依赖关系和@DependsOn声明确定的正交问题。 请注意，标准javax.annotation.Priority注释在该 @Bean级别不可用，因为它不能在方法上声明。它的语义可以通过@Order结合@Primary每个类型的单个 bean 的值来建模。
* 默认情况下，当给定注入点没有匹配的候选 bean 时，自动装配会失败。在声明的数组、集合或映射的情况下，至少需要一个匹配元素。默认行为是将带注释的方法和字段视为指示所需的依赖项。您可以按照以下示例所示更改此行为，使框架能够通过将其标记为非必需（即，通过将required属性设置@Autowired为false）来跳过不可满足的注入点。
* 任何给定 bean 类的@Autowired构造函数只能声明其required 属性设置为true，指示构造函数在用作 Spring bean 时自动装配。因此，如果属性保留为默认值，则只有一个构造函数可以用 注释。如果多个构造函数声明了注解，它们都必须声明才能被视为自动装配的候选者（类似于requiredtrue@Autowiredrequired=falseautowire=constructor在 XML 中）。将选择在 Spring 容器中通过匹配 bean 可以满足的依赖项数量最多的构造函数。如果不能满足任何候选者，则将使用主/默认构造函数（如果存在）。类似地，如果一个类声明了多个构造函数但没有一个用 注释@Autowired，那么将使用主/默认构造函数（如果存在）。如果一个类只声明了一个构造函数，即使没有注释，它也会一直被使用。请注意，带注释的构造函数不必是公共的。 建议使用required属性 of@Autowired而不是 setter 方法上已弃用的@Required 注释。将required属性设置为false表示自动装配不需要该属性，如果不能自动装配，则忽略该属性。@Required另一方面，它更强大，因为它强制通过容器支持的任何方式设置属性，如果没有定义值，则会引发相应的异常。
* 您还可以@Autowired用于众所周知的可解析依赖项的接口：BeanFactory、ApplicationContext、Environment、ResourceLoader、 ApplicationEventPublisher和MessageSource. 这些接口及其扩展接口，例如ConfigurableApplicationContextor ResourcePatternResolver，会自动解析，无需特殊设置。
* 由于按类型自动装配可能会导致多个候选者，因此通常需要对选择过程进行更多控制。实现这一点的一种方法是使用 Spring 的 @Primary注释。@Primary指示当多个 bean 是自动装配到单值依赖项的候选对象时，应该优先考虑特定的 bean。如果候选中恰好存在一个主 bean，则它将成为自动装配的值。
* @Primary当可以确定一个主要候选者时，是一种通过类型使用多个实例的自动装配的有效方法。当您需要对选择过程进行更多控制时，可以使用 Spring 的@Qualifier注解。您可以将限定符值与特定参数相关联，缩小类型匹配的范围，以便为每个参数选择特定的 bean。
* 在类型匹配的候选对象中，让限定符值针对目标 bean 名称进行选择，不需要@Qualifier在注入点进行注释。如果没有其他解析指示符（例如限定符或主标记），对于非唯一依赖情况，Spring 将注入点名称（即字段名称或参数名称）与目标 bean 名称匹配并选择同名候选人（如有）。
* **也就是说，如果您打算按名称表达注解驱动的注入，请不要主要使用 @Autowired，即使它能够在类型匹配候选者中按 bean 名称进行选择。相反，使用 JSR-250 @Resource注释，它在语义上定义为通过其唯一名称标识特定目标组件，声明的类型与匹配过程无关。** @Autowired具有相当不同的语义：在按类型选择候选 bean 后，指定的String 限定符值仅在那些类型选择的候选者中考虑（例如，将account限定符与标记有相同限定符标签的 bean 匹配）。
* 试图在同一配置类上注入@Bean方法的结果实际上也是一种自引用场景。要么在实际需要的方法签名中懒洋洋地解析此类引用（与配置类中的自动连接字段相反），要么将受影响的@Bean方法声明为静态，将它们与包含的配置类实例及其生命周期分离。否则，这类bean只会在回退阶段考虑，而其他配置类上的匹配bean会被选为主要候选项（如果可用）。
* 除了 @Qualifier 注释之外，您还可以使用 Java 泛型类型作为限定的隐式形式。
* Spring 还通过在字段或 bean 属性设置器方法上使用 JSR-250@Resource注释 ( )来支持注入。javax.annotation.Resource这是 Java EE 中的常见模式：例如，在 JSF 管理的 bean 和 JAX-WS 端点中。Spring 也支持 Spring 管理的对象的这种模式。 @Resource采用名称属性。默认情况下，Spring 将该值解释为要注入的 bean 名称。
* @Value 通常用于注入外部属性。
* 配置PropertySourcesPlaceholderConfigurer使用 JavaConfig 时， @Bean方法必须是static.
* Spring Boot 默认配置一个PropertySourcesPlaceholderConfigurerbean，该 bean 将从application.properties和application.yml文件中获取属性。
* 当 @Value 包含 SpEL 表达式时，该值将在运行时动态计算。
* 就像@Resource，@PostConstruct和@PreDestroy注解类型是从 JDK 6 到 8 的标准 Java 库的一部分。但是，整个javax.annotation 包在 JDK 9 中与核心 Java 模块分离，并最终在 JDK 11 中被删除。如果需要，javax.annotation-api工件需要现在通过 Maven Central 获得，只需像任何其他库一样添加到应用程序的类路径中。
* 从 Spring 3.0 开始，Spring JavaConfig 项目提供的许多特性都是核心 Spring Framework 的一部分。这允许您使用 Java 而不是使用传统的 XML 文件来定义 bean。查看@Configuration、@Bean、 @Import和@DependsOn注释，了解如何使用这些新功能的示例。
* @Repository注释是满足存储库（也称为数据访问对象或 DAO）角色或原型的任何类的标记。此标记的用途之一是异常的自动翻译，如 [Exception Translation](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#orm-exception-translation) 中所述。Spring 提供了更多的原型注解：@Component、@Service和 @Controller. @Component是任何 Spring 管理的组件的通用构造型。 @Repository, @Service, 和@Controller是@Component针对更具体用例（分别在持久层、服务层和表示层）的特化。因此，您可以使用 注释组件类 @Component，但是通过使用 、 注释它们@Repository，@Service或者@Controller 相反，您的类更适合工具处理或与方面关联。例如，这些原型注释是切入点的理想目标。@Repository, @Service, 并且@Controller还可以在 Spring 框架的未来版本中携带额外的语义。因此，如果您在使用@Component或者@Service对于你的服务层，@Service显然是更好的选择。同样，如前所述，@Repository已经支持作为持久层中自动异常转换的标记。
* 要自动检测这些类并注册相应的 bean，您需要添加 @ComponentScan到您的@Configuration类中，其中basePackages属性是两个类的公共父包。（或者，您可以指定一个逗号或分号或空格分隔的列表，其中包括每个类的父包。）为简洁起见，前面的示例可能使用value了注释的属性（即 @ComponentScan("org.example") ）。
* 使用 `<context:component-scan>` 隐式启用 `<context:annotation-config>`. 使用 `<context:component-scan>`  时通常不需要包含 `<context:annotation-config>`.
* 类路径包的扫描需要类路径中存在相应的目录条目。当您使用 Ant 构建 JAR 时，请确保您没有激活 JAR 任务的仅文件开关。此外，在某些环境中，类路径目录可能不会根据安全策略公开——例如，JDK 1.7.0_45 及更高版本上的独立应用程序（ 需要在清单中设置“Trusted-Library”——请参阅 [https://stackoverflow.com/问题/19394570/java-jre-7u45-breaks-classloader-getresources](https://stackoverflow.com/questions/19394570/java-jre-7u45-breaks-classloader-getresources) ）。在 JDK 9 的模块路径（Jigsaw）上，Spring 的类路径扫描通常按预期工作。但是，请确保您的组件类在您的module-info 描述符中导出。如果您希望 Spring 调用类的非公共成员，请确保它们是“打开的”（即，它们使用opens声明而不是描述符中的 exports声明module-info）。
* 除了用于组件初始化之外，您还可以将注解放置在标有或@Lazy 的注入点上。在这种情况下，它会导致延迟解析代理的注入。然而，这种代理方法相当有限。对于复杂的惰性交互，特别是结合可选依赖项，我们建议改为。 @Autowired @Inject ObjectProvider<MyTargetBean>.
* 您可以将@Bean方法声明为static，允许在不创建包含它们的配置类作为实例的情况下调用它们。BeanFactoryPostProcessor 这在定义后处理器 bean（例如，类型or ）时特别有意义BeanPostProcessor，因为此类 bean 在容器生命周期的早期被初始化，并且应该避免在那个时候触发配置的其他部分。 由于技术限制，对静态@Bean方法的调用永远不会被容器拦截，甚至在 @Configuration类中（如本节前面所述）也不会被拦截：CGLIB 子类化只能覆盖非静态方法。因此，直接调用另一个@Bean方法具有标准的 Java 语义，从而导致直接从工厂方法本身返回一个独立的实例。 方法的 Java 语言可见性@Bean不会立即影响 Spring 容器中生成的 bean 定义。您可以自由地声明您认为适合非@Configuration类的工厂方法，也可以在任何地方声明静态方法。但是，类中的常规@Bean方法@Configuration需要是可覆盖的——也就是说，它们不能被声明为privateor final。 @Bean方法也在给定组件或配置类的基类上发现，以及在组件或配置类实现的接口中声明的 Java 8 默认方法上发现。这为组合复杂的配置安排提供了很大的灵活性，甚至可以通过 Spring 4.2 的 Java 8 默认方法实现多重继承。 最后，单个类可以@Bean为同一个 bean 保存多个方法，作为多个工厂方法的排列，根据运行时可用的依赖关系使用。这与在其他配置场景中选择“最贪婪”的构造函数或工厂方法的算法相同：在构造时选择具有最多可满足依赖项的变体，类似于容器如何在多个@Autowired构造函数之间进行选择。
* 当一个组件作为扫描过程的一部分被自动检测时，它的 bean 名称由该BeanNameGenerator扫描器已知的策略生成。默认情况下，任何包含名称的Spring 原型注解 ( @Component、@Repository、@Service和 ) 都会将该名称提供给相应的 bean 定义。
* @Scope注释仅在具体 bean 类（用于注释组件）或工厂方法（用于@Bean方法）上进行自省。与 XML bean 定义相比，没有 bean 定义继承的概念，并且类级别的继承层次结构与元数据无关。
* 与大多数基于注解的替代方案一样，请记住注解元数据绑定到类定义本身，而 XML 的使用允许相同类型的多个 bean 提供其限定符元数据的变体，因为元数据是根据每个-实例而不是每个类。 As with most annotation-based alternatives, keep in mind that the annotation metadata is bound to the class definition itself, while the use of XML allows for multiple beans of the same type to provide variations in their qualifier metadata, because that metadata is provided per-instance rather than per-class.
* 虽然类路径扫描非常快，但可以通过在编译时创建静态候选列表来提高大型应用程序的启动性能。在这种模式下，作为组件扫描目标的所有模块都必须使用这种机制。	您现有的 @ComponentScan 或 `<context:component-scan/>` 指令必须保持不变，才能请求上下文以扫描某些包中的候选人。当 ApplicationContext检测到这样的索引时，它会自动使用它而不是扫描类路径。在 IDE 中使用此模式时，spring-context-indexer必须将其注册为注释处理器，以确保更新候选组件时索引是最新的。当在类路径中找到文件时，索引会自动启用META-INF/spring.components。如果索引对某些库（或用例）部分可用，但无法为整个应用程序构建，您可以通过设置回退到常规类路径安排（好像根本不存在索引）spring.index.ignore， true或者作为JVM 系统属性或通过 SpringProperties机制。
* 从 Spring 3.0 开始，Spring 提供对 JSR-330 标准注解（依赖注入）的支持。这些注释的扫描方式与 Spring 注释相同。要使用它们，您需要在类路径中有相关的 jar。	
如果您使用 Maven，则该javax.inject工件在标准 Maven 存储库 [javax.inject](https://repo1.maven.org/maven2/javax/inject/javax.inject/1/) 中可用。您可以将以下依赖项添加到文件 pom.xml 中。
* @Component: 您可以使用@javax.inject.Namedor代替javax.annotation.ManagedBean. 与 @Component 相比，JSR-330 @Named和 JSR-250 @ManagedBean 注释是不可组合的。您应该使用 Spring 的原型模型来构建自定义组件注释。
* Spring 新的 Java 配置支持中的核心工件是带 @Configuration 注释的类和带 @Bean 注释的方法。 @Bean 注解用于表示一个方法实例化、配置和初始化一个由 Spring IoC 容器管理的新对象。对于熟悉 Spring 的 `<beans/>` XML 配置的人来说，注解与元素 @Bean 的作用相同。`<bean/>` 您可以将 @Bean-annotated 方法与任何 Spring @Component 一起使用 。但是它们最常与 @Configuration Bean 类一起使用。 用注释一个类 @Configuration 表明它的主要目的是作为 bean 定义的来源。此外，@Configuration 类允许通过调用 @Bean 同一类中的其他方法来定义 bean 间的依赖关系。
* 完整的@Configuration 与“精简”@Bean 模式？当@Bean方法在没有用 注释的类中声明时 @Configuration，它们被称为以“精简”模式处理。在一个或什至在一个普通的旧类中声明的 Bean 方法@Component被认为是“精简版”，包含类的不同主要目的和一种@Bean方法在那里是一种奖励。例如，服务组件可以通过@Bean每个适用组件类上的附加方法向容器公开管理视图。在这种情况下，@Bean方法是一种通用的工厂方法机制。 与 full 不同@Configuration，lite@Bean方法不能声明 bean 间的依赖关系。相反，它们对其包含组件的内部状态进行操作，并且可以选择对它们可能声明的参数进行操作。因此，这种@Bean方法不应调用其他 @Bean方法。每个这样的方法实际上只是特定 bean 引用的工厂方法，没有任何特殊的运行时语义。这里的积极副作用是在运行时不必应用 CGLIB 子类化，因此在类设计方面没有限制（即包含类可能是final等等）。 在常见情况下，@Bean方法将在@Configuration类中声明，确保始终使用“完整”模式，并且跨方法引用因此被重定向到容器的生命周期管理。这可以防止 @Bean通过常规 Java 调用意外调用相同的方法，这有助于减少在“精简”模式下操作时难以追踪的细微错误。
* 请记住，@Configuration类是用元注释 的@Component，因此它们是组件扫描的候选对象。在前面的示例中，假设AppConfig在com.acme包（或下面的任何包）中声明了 ，在调用scan(). 在 之后refresh()，它的所有@Bean 方法都被处理并注册为容器中的 bean 定义。 Remember that @Configuration classes are meta-annotated with @Component, so they are candidates for component-scanning. In the preceding example, assuming that AppConfig is declared within the com.acme package (or any package underneath), it is picked up during the call to scan(). Upon refresh(), all its @Bean methods are processed and registered as bean definitions within the container.
* 对于编程用例， GenericWebApplicationContext 可以用作 AnnotationConfigWebApplicationContext. 有关详细信息，请参阅 [GenericWebApplicationContext](https://docs.spring.io/spring-framework/docs/5.3.17/javadoc-api/org/springframework/web/context/support/GenericWebApplicationContext.html) javadoc。
* 要声明一个 bean，你可以用注解来注解一个方法@Bean。ApplicationContext您可以使用此方法在指定为方法返回值的类型中注册 bean 定义。默认情况下，bean 名称与方法名称相同。
* 如果您始终通过声明的服务接口引用您的类型，则您的 @Bean返回类型可以安全地加入该设计决策。但是，对于实现多个接口的组件或可能由其实现类型引用的组件，声明最具体的返回类型可能更安全（至少与引用您的 bean 的注入点所要求的一样具体）。
* @Bean 注解支持指定任意初始化和销毁回调方法，很像 Spring XML bean 元素中 init-method 和 destroy-method 属性。
* 默认情况下，使用 Java 配置定义的具有公共close或shutdown 方法的 bean 会自动加入销毁回调。如果您有一个公共 close或shutdown方法并且您不希望在容器关闭时调用它，您可以添加@Bean(destroyMethod="")到您的 bean 定义以禁用默认(inferred)模式。
* 当您直接在 Java 中工作时，您可以对您的对象做任何您喜欢的事情，而不必总是依赖容器生命周期。 When you work directly in Java, you can do anything you like with your objects and do not always need to rely on the container lifecycle.
* 默认情况下，配置类使用 @Bean 方法的名称作为生成的 bean 的名称。但是，可以使用 name 属性覆盖此功能。
* 正如命名 Bean中所讨论的，有时需要为单个 bean 提供多个名称，也称为 bean 别名。注释的name属性@Bean 为此目的接受一个字符串数组。以下示例显示了如何为 bean 设置多个别名：`@Bean({"dataSource", "subsystemA-dataSource", "subsystemB-dataSource"})`.
* @Configuration 是一个类级别的注解，表明一个对象是 bean 定义的来源。类通过-annotated 方法 @Configuration 声明 bean 。对类上的方法的@Bean调用也可用于定义 bean 间的依赖关系。
* 这种声明 bean 间依赖关系的方法仅在该@Bean方法在@Configuration类中声明时才有效。 您不能使用普通@Component类来声明 bean 间的依赖关系。
  ```java
  @Configuration
  public class AppConfig {
  
      @Bean
      public BeanOne beanOne() {
          return new BeanOne(beanTwo());
      }
  
      @Bean
      public BeanTwo beanTwo() {
          return new BeanTwo();
      }
  }
  ```
* 考虑下面的例子，它显示了一个@Bean被注解的方法被调用了两次。clientDao()已被调用一次clientService1()和一次clientService2()。由于此方法会创建一个新实例ClientDaoImpl并返回它，因此您通常会期望有两个实例（每个服务一个实例）。那肯定会有问题：在 Spring 中，实例化的 beansingleton默认有一个作用域。这就是神奇之处：所有@Configuration类在启动时都使用CGLIB. 在子类中，子方法在调用父方法并创建新实例之前，首先检查容器中是否有任何缓存（作用域）bean。根据 bean 的范围，行为可能会有所不同。我们在这里谈论单例。从 Spring 3.2 开始，不再需要将 CGLIB 添加到类路径中，因为 CGLIB 类已被重新打包org.springframework.cglib并直接包含在 spring-core JAR 中。由于 CGLIB 在启动时动态添加功能，因此存在一些限制。特别是，配置类不能是最终的。但是，从 4.3 开始，配置类上允许使用任何构造函数，包括使用 @Autowired或使用单个非默认构造函数声明进行默认注入。 如果您希望避免任何 CGLIB 强加的限制，请考虑@Bean 在非@Configuration类上声明您的方法（例如，@Component改为在普通类上）。方法之间的跨方法调用@Bean不会被拦截，因此您必须完全依赖构造函数或方法级别的依赖注入。
  ```java
  @Configuration
  public class AppConfig {
  
      @Bean
      public ClientService clientService1() {
          ClientServiceImpl clientService = new ClientServiceImpl();
          clientService.setClientDao(clientDao());
          return clientService;
      }
  
      @Bean
      public ClientService clientService2() {
          ClientServiceImpl clientService = new ClientServiceImpl();
          clientService.setClientDao(clientDao());
          return clientService;
      }
  
      @Bean
      public ClientDao clientDao() {
          return new ClientDaoImpl();
      }
  }
  ```
* 就像 `<import/>` 在 Spring XML 文件中使用该元素来帮助模块化配置一样，@Import 注释允许 @Bean 从另一个配置类加载定义。
* 从 Spring Framework 4.2 开始，@Import还支持对常规组件类的引用，类似于AnnotationConfigApplicationContext.register方法。如果您想通过使用一些配置类作为入口点来显式定义所有组件来避免组件扫描，这将特别有用。
* 确保您以这种方式注入的依赖项只是最简单的类型。@Configuration 类在上下文初始化期间很早就被处理，并且强制以这种方式注入依赖项可能会导致意外的早期初始化。尽可能使用基于参数的注入，如前面的示例所示。 此外，请特别注意BeanPostProcessor和的BeanFactoryPostProcessor定义@Bean。这些通常应该被声明为static @Bean方法，而不是触发它们包含的配置类的实例化。否则，@Autowired可能@Value无法在配置类本身上工作，因为可以将其创建为早于 AutowiredAnnotationBeanPostProcessor.
* @Configuration仅从 Spring Framework 4.3 开始支持类中的 构造函数注入。另请注意，如果目标 bean 仅定义一个构造函数 ，则无需指定 @Autowired。
* 如果您想影响某些 bean 的启动创建顺序，请考虑将其中一些声明为@Lazy（用于在首次访问时创建而不是在启动时创建）或@DependsOn某些其他 bean（确保在当前 bean 之前创建特定的其他 bean，超出后者的直接依赖意味着什么）。
* Spring 的@Configuration类支持并非旨在 100% 完全替代 Spring XML。一些工具，例如 Spring XML 命名空间，仍然是配置容器的理想方式。在 XML 方便或必要的情况下，您可以选择：或者以“以 XML 为中心”的方式实例化容器，例如使用 ， ClassPathXmlApplicationContext或者以“以 Java 为中心”的方式实例化它 AnnotationConfigApplicationContext，@ImportResource使用根据需要导入 XML。
* 在system-test-config.xml文件中，AppConfig <bean/>不声明id 元素。虽然这样做是可以接受的，但这是不必要的，因为没有其他 bean 曾经引用过它，并且不太可能通过名称从容器中显式获取。类似地，DataSourcebean 仅按类型自动装配，因此id 并不严格要求显式 bean。
* 在@Configuration类是配置容器的主要机制的应用程序中，仍然可能至少需要使用一些 XML。在这些场景中，您可以根据@ImportResource需要使用和定义尽可能多的 XML。这样做实现了一种“以 Java 为中心”的方法来配置容器并将 XML 保持在最低限度。
* Environment接口是集成在容器中的抽象，它对应用程序环境的两个关键方面进行建模：配置文件 和属性。 配置文件是一个命名的、逻辑的 bean 定义组，仅当给定的配置文件处于活动状态时才向容器注册。可以将 Bean 分配给配置文件，无论是在 XML 中定义还是使用注释定义。与配置文件相关的对象的作用Environment是确定哪些配置文件（如果有）当前处于活动状态，以及哪些配置文件（如果有）默认情况下应该是活动的。 属性在几乎所有应用程序中都发挥着重要作用，并且可能源自多种来源：属性文件、JVM 系统属性、系统环境变量、JNDI、servlet 上下文参数、ad-hocProperties对象、Map对象等等。与属性相关的对象的作用Environment是为用户提供一个方便的服务接口，用于配置属性源并从中解析属性。
* 配置文件字符串可能包含一个简单的配置文件名称（例如，production）或配置文件表达式。配置文件表达式允许表达更复杂的配置文件逻辑（例如，production & us-east）。不能在不使用括号的情况下混合使用&and运算符。|例如， production & us-east | eu-central不是一个有效的表达式。它必须表示为 production & (us-east | eu-central)。
* 如果一个@Configuration类用 标记，则与该类关联@Profile的所有@Bean方法和 注释都将被绕过，除非一个或多个指定的配置文件处于活动状态。@Import如果一个@Component或@Configuration类标有@Profile({"p1", "p2"})，则除非已激活配置文件“p1”或“p2”，否则不会注册或处理该类。如果给定配置文件以 NOT 运算符 ( ) 为前缀，!则仅当配置文件不活动时才注册带注释的元素。例如，@Profile({"p1", "!p2"})如果配置文件“p1”处于活动状态或配置文件“p2”未处于活动状态，则会发生注册。
* 使用@Profileon@Bean方法，可能会应用一种特殊情况：在@Bean相同 Java 方法名称的重载方法的情况下（类似于构造函数重载），@Profile需要在所有重载方法上一致地声明一个条件。如果条件不一致，则仅重载方法中第一个声明的条件重要。因此，@Profile不能用于选择具有特定参数签名的重载方法而不是另一个。同一 bean 的所有工厂方法之间的解析在创建时遵循 Spring 的构造函数解析算法。 如果要定义具有不同配置文件条件的替代 bean，请使用不同的 Java 方法名称，这些方法名称通过使用@Beanname 属性指向相同的 bean 名称，如前面的示例所示。如果参数签名都相同（例如，所有变体都有无参数工厂方法），这是首先在有效 Java 类中表示这种安排的唯一方法（因为只能有一个特定名称和参数签名的方法）。
* 激活配置文件可以通过多种方式完成，但最直接的方式是以编程方式针对Environment通过 ApplicationContext. 以下示例显示了如何执行此操作：
  ```java
  AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext();
  ctx.getEnvironment().setActiveProfiles("development");
  ctx.register(SomeConfig.class, StandaloneDataConfig.class, JndiDataConfig.class);
  ctx.refresh();
  ```
* default 配置文件表示默认启用的配置文件。如果没有激活的配置文件，dataSource则创建。您可以将此视为一种为一个或多个 bean 提供默认定义的方法。如果启用了任何配置文件，则默认配置文件不适用。 setDefaultProfiles() 您可以使用 onEnvironment 或以声明方式使用 spring.profiles.default 属性来更改默认配置文件的名称。
* 根据@PropertySourceJava 8 约定，注释是可重复的。但是，所有此类@PropertySource注释都需要在同一级别声明，或者直接在配置类上声明，或者作为同一自定义注释中的元注释。不建议混合直接注释和元注释，因为直接注释有效地覆盖了元注释。
* 因为 SpringMessageSource是基于 Java 的ResourceBundle，所以它不会合并具有相同基本名称的包，而只会使用找到的第一个包。具有相同基本名称的后续消息包将被忽略。
* 作为 的替代方案ResourceBundleMessageSource，Spring 提供了一个 ReloadableResourceBundleMessageSource类。此变体支持相同的捆绑文件格式，但比基于标准 JDK 的 ResourceBundleMessageSource实现更灵活。特别是，它允许从任何 Spring 资源位置（不仅从类路径）读取文件，并支持捆绑属性文件的热重载（同时在它们之间有效地缓存它们）。有关详细信息，请参阅ReloadableResourceBundleMessageSource javadoc。
* ApplicationContext 中的事件处理是通过 ApplicationEvent 类和 ApplicationListener 接口提供的。如果将实现 ApplicationListener接口的 bean 部署到上下文中，则每次 ApplicationEvent发布到 时ApplicationContext，都会通知该 bean。本质上，这是标准的观察者设计模式。从 Spring 4.2 开始，事件基础结构得到了显着改进，并提供了基于注释的模型以及发布任意事件的能力（即，不一定从 扩展的对象ApplicationEvent）。当这样的对象发布时，我们会为您将其包装在一个事件中。
* 请注意，ApplicationListener它通常使用自定义事件的类型进行参数化（BlockedListEvent在前面的示例中）。这意味着该 onApplicationEvent()方法可以保持类型安全，避免任何向下转换的需要。您可以根据需要注册任意数量的事件侦听器，但请注意，默认情况下，事件侦听器会同步接收事件。这意味着该publishEvent()方法会阻塞，直到所有侦听器都完成了对事件的处理。这种同步和单线程方法的一个优点是，当侦听器接收到事件时，如果事务上下文可用，它会在发布者的事务上下文中运行。如果需要另一种事件发布策略，请参阅 javadoc 了解 Spring 的 ApplicationEventMulticaster接口和SimpleApplicationEventMulticaster 配置选项的实现。
* Spring 的事件机制是为同一应用程序上下文中的 Spring bean 之间的简单通信而设计的。然而，对于更复杂的企业集成需求，单独维护的 Spring Integration项目为 构建基于众所周知的 Spring 编程模型的 轻量级、面向模式、事件驱动的架构提供了完整的支持。
* 异步侦听器: 如果您希望特定侦听器异步处理事件，则可以重用 常规@Async支持。使用异步事件时请注意以下限制： 
  * 如果异步事件侦听器抛出Exception，它不会传播给调用者。有关 AsyncUncaughtExceptionHandler 更多详细信息，请参阅。 
  * 异步事件侦听器方法不能通过返回值来发布后续事件。如果您需要发布另一个事件作为处理的结果，请 ApplicationEventPublisher 手动注入一个来发布该事件。
* ApplicationStartup仅在应用程序启动期间和核心容器中使用；这绝不是 Java 分析器或Micrometer等指标库的替代品。
* "spring.*"开发人员在创建自定义启动步骤时 不应使用命名空间。这个命名空间是为内部 Spring 使用而保留的，并且可能会发生变化。
* BeanFactory还是ApplicationContext？ BeanFactory本节解释了容器级别和 容器级别之间的差异ApplicationContext以及对引导的影响。 ApplicationContext除非您有充分的理由不这样做，否则 您应该使用 anGenericApplicationContext及其子类AnnotationConfigApplicationContext 作为自定义引导的常见实现。这些是 Spring 核心容器的主要入口点，用于所有常见目的：加载配置文件、触发类路径扫描、以编程方式注册 bean 定义和带注释的类，以及（从 5.0 开始）注册功能性 bean 定义。 因为 anApplicationContext包含 a 的所有功能BeanFactory，所以通常建议在 plain 上使用BeanFactory，除了需要完全控制 bean 处理的场景。在一个ApplicationContext（例如 GenericApplicationContext实现）中，按照约定（即按 bean 名称或按 bean 类型——特别是后处理器）检测几种 bean，而 plainDefaultListableBeanFactory对任何特殊 bean 是不可知的。 对于许多扩展容器特性，例如注解处理和 AOP 代理，BeanPostProcessor扩展点是必不可少的。如果您仅使用普通DefaultListableBeanFactory的，则默认情况下不会检测和激活此类后处理器。这种情况可能会令人困惑，因为您的 bean 配置实际上没有任何问题。相反，在这种情况下，需要通过额外的设置来完全引导容器。
* 在这两种情况下，显式注册步骤都不方便，这就是为什么在 Spring 支持的应用程序中，各种ApplicationContext变体比普通的更受青睐 ，尤其是在典型企业设置中依赖实例来扩展容器功能时DefaultListableBeanFactory。BeanFactoryPostProcessorBeanPostProcessor. AnAnnotationConfigApplicationContext已注册所有常见的注释后处理器，并且可以通过配置注释引入额外的处理器，例如@EnableTransactionManagement. 在 Spring 的基于注解的配置模型的抽象级别上，bean 后处理器的概念变成了单纯的内部容器细节。
* Spring 本身广泛使用抽象 Resource，在需要资源时作为许多方法签名中的参数类型。某些 Spring API 中的其他方法（例如各种ApplicationContext实现的构造函数）采用 String朴素或简单的形式来创建 Resource 适合于该上下文实现的方法，或者通过 String 路径上的特殊前缀，让调用者指定特定的 Resource 实现必须创建和使用。 虽然Resource接口在 Spring 和 Spring 中被大量使用，但在您自己的代码中将其本身用作通用实用程序类实际上非常方便，用于访问资源，即使您的代码不知道或不关心任何 Spring 的其他部分。虽然这会将您的代码与 Spring 耦合，但它实际上只是将它耦合到这一小部分实用程序类，它可以作为更强大的替代品，URL并且可以被认为等同于您将用于此目的的任何其他库。
* Resource 抽象不会取代功能，它尽可能地包裹它。例如，UrlResource 包装一个 URL 并使用被包装 URL 的来完成它的工作。
* 所有应用程序上下文都实现了该ResourceLoader接口。因此，所有应用程序上下文都可以用于获取Resource实例。 当您调用getResource()特定的应用程序上下文，并且指定的位置路径没有特定的前缀时，您将返回Resource适合该特定应用程序上下文的类型。例如，假设下面的代码片段是针对一个ClassPathXmlApplicationContext实例运行的: `Resource template = ctx.getResource("some/resource/path/myTemplate.txt");`, 针对 a ClassPathXmlApplicationContext，该代码返回 a ClassPathResource。如果对FileSystemXmlApplicationContext实例运行相同的方法，它将返回一个FileSystemResource. 对于 a WebApplicationContext，它将返回 a ServletContextResource。它同样会为每个上下文返回适当的对象。 因此，您可以以适合特定应用程序上下文的方式加载资源。
* ResourceLoader 任何标准中的默认值 ApplicationContext 实际上都是 PathMatchingResourcePatternResolver 实现 ResourcePatternResolver  接口的实例。 ApplicationContext 实例本身也是如此，它也实现了 ResourcePatternResolver 接口并委托给  default PathMatchingResourcePatternResolver 。
* Resource要为包含通配符或使用特殊资源前缀的资源路径 加载一个或多个对象classpath*:，请考虑将 autowired 实例 ResourcePatternResolver而不是ResourceLoader. To load one or more Resource objects for a resource path that contains wildcards or makes use of the special classpath*: resource prefix, consider having an instance of ResourcePatternResolver autowired into your application components instead of ResourceLoader.
* 请注意，资源路径没有前缀。因此，由于应用程序上下文本身将用作ResourceLoader，因此资源通过 ClassPathResource、 FileSystemResource或 加载ServletContextResource，具体取决于应用程序上下文的确切类型。 如果需要强制使用特定Resource类型，可以使用前缀。以下两个示例展示了如何强制使用 ClassPathResource和 UrlResource（后者用于访问文件系统中的文件）
  * `<property name="template" value="classpath:some/resource/path/myTemplate.txt">`
  * `<property name="template" value="file:///some/resource/path/myTemplate.txt"/>`
* 通配符类路径依赖于getResources()底层的方法 ClassLoader。由于现在大多数应用程序服务器都提供自己的ClassLoader 实现，因此行为可能会有所不同，尤其是在处理 jar 文件时。检查是否classpath*有效的一个简单测试是使用ClassLoader从类路径上的 jar 中加载文件： getClass().getClassLoader().getResources("<someFileInsideTheJar>"). 尝试使用具有相同名称但位于两个不同位置的文件进行此测试 - 例如，具有相同名称和相同路径但在类路径上的不同 jar 中的文件。如果返回不适当的结果，请检查应用程序服务器文档以获取可能影响ClassLoader行为的设置。
* Spring 使用java.beans.PropertyEditorManager为可能需要的属性编辑器设置搜索路径。搜索路径还包括sun.bean.editors，其中包括PropertyEditor诸如Font、Color和大多数基本类型的实现。另请注意，标准 JavaBeans 基础结构会自动发现PropertyEditor类（无需显式注册它们），如果它们与它们处理的类在同一个包中并且与该类具有相同的名称，并带有Editor附加。例如，可以具有以下类和包结构，这足以使SomethingEditor该类被识别并用作PropertyEditorforSomething类型的属性。请注意，您也可以在此处使用标准JavaBeans 机制（在 [此处](https://docs.oracle.com/javase/tutorial/javabeans/advanced/customization.html) BeanInfo进行了一定程度的描述 ）。以下示例使用该机制显式注册一个或多个 具有关联类属性的实例：BeanInfoPropertyEditor.
* Spring 3 引入了一个core.convert提供通用类型转换系统的包。系统定义了一个 SPI 来实现类型转换逻辑和一个 API 来在运行时执行类型转换。在 Spring 容器中，您可以使用此系统作为实现的替代PropertyEditor方案，将外部化的 bean 属性值字符串转换为所需的属性类型。您还可以在应用程序中需要类型转换的任何地方使用公共 API。参考: [Spring Core Converter](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/core/convert/converter/Converter.html).
* 因为GenericConverter是比较复杂的 SPI 接口，所以应该只在需要的时候使用。支持Converter或ConverterFactory满足基本类型转换需求。Because GenericConverter is a more complex SPI interface, you should use it only when you need it. Favor Converter or ConverterFactory for basic type conversion needs.
* 默认情况下，未使用注释的日期和时间字段是使用样式@DateTimeFormat从字符串转换而来的。DateFormat.SHORT如果您愿意，可以通过定义自己的全局格式来更改它。 为此，请确保 Spring 不注册默认格式化程序。相反，请在以下帮助下手动注册格式化程序：
  * org.springframework.format.datetime.standard.DateTimeFormatterRegistrar
  * org.springframework.format.datetime.DateFormatterRegistrar
* 为了符合 Spring 驱动的方法验证的条件，所有目标类都需要使用 Spring 的@Validated注解进行注解，它还可以选择声明要使用的验证组。有关 [MethodValidationPostProcessor](https://docs.spring.io/spring-framework/docs/5.3.18/javadoc-api/org/springframework/validation/beanvalidation/MethodValidationPostProcessor.html) Hibernate Validator 和 Bean Validation 1.1 提供程序的设置详细信息，请参阅。
* 方法验证依赖于目标类周围的 [AOP 代理，要么是接口上方法的 JDK 动态代理，要么是 CGLIB 代理](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop-introduction-proxies). 使用代理有某些限制，其中一些在 [了解 AOP 代理](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop-understanding-aop-proxies) 中进行了描述。此外，请记住始终在代理类上使用方法和访问器；直接字段访问将不起作用。
* 从 Spring 3 开始，您可以DataBinder使用Validator. 配置完成后，您可以通过调用Validator来调用binder.validate(). 任何验证都会 Errors自动添加到活页夹的BindingResult.
* Spring 表达式语言（简称“SpEL”）是一种强大的表达式语言，支持在运行时查询和操作对象图。语言语法类似于 Unified EL，但提供了额外的功能，最值得注意的是方法调用和基本的字符串模板功能。 
* 虽然还有其他几种可用的 Java 表达式语言——OGNL、MVEL 和 JBoss EL 等等——但创建 Spring 表达式语言的目的是为 Spring 社区提供一种受良好支持的表达式语言，该语言可以在所有产品中使用春季投资组合。它的语言特性由 Spring 产品组合中的项目需求驱动，包括Spring Tools for Eclipse中代码完成支持的工具需求。也就是说，SpEL 基于与技术无关的 API，如果需要，可以集成其他表达式语言实现。 
* 虽然 SpEL 是 Spring 产品组合中表达式评估的基础，但它不直接与 Spring 绑定，可以独立使用。为了独立起见，本章中的许多示例都使用 SpEL，就好像它是一种独立的表达语言一样。这需要创建一些引导基础设施类，例如解析器。大多数 Spring 用户不需要处理这个基础设施，而是可以只编写表达式字符串进行评估。这种典型用途的一个示例是将 SpEL 集成到创建 XML 或基于注释的 bean 定义中，如 定义 bean 定义的表达式支持中所示。
* EvaluationContext在评估表达式以解析属性、方法或字段并帮助执行类型转换时，使用该接口。Spring 提供了两种实现。
  * SimpleEvaluationContext：针对不需要完整范围的 SpEL 语言语法且应受到有意义限制的表达式类别，公开了基本 SpEL 语言功能和配置选项的子集。示例包括但不限于数据绑定表达式和基于属性的过滤器。 
  * StandardEvaluationContext：公开全套 SpEL 语言功能和配置选项。您可以使用它来指定默认根对象并配置每个可用的评估相关策略。
* 默认情况下，SpEL 使用 Spring 核心 ( org.springframework.core.convert.ConversionService) 中可用的转换服务。此转换服务带有许多用于常见转换的内置转换器，但也是完全可扩展的，因此您可以在类型之间添加自定义转换。此外，它是泛型感知的。这意味着，当您在表达式中使用泛型类型时，SpEL 会尝试转换以维护它遇到的任何对象的类型正确性。 这在实践中意味着什么？假设使用 赋值setValue()来设置List属性。属性的类型实际上是List<Boolean>. BooleanSpEL 认识到列表中的元素在放入之前需要转换为。
* 可以使用解析器配置对象 ( org.springframework.expression.spel.SpelParserConfiguration) 来配置 SpEL 表达式解析器。配置对象控制一些表达式组件的行为。例如，如果您对数组或集合进行索引，并且指定索引处的元素是null，SpEL可以自动创建元素。这在使用由属性引用链组成的表达式时很有用。如果您对数组或列表进行索引并指定超出数组或列表当前大小末尾的索引，SpEL 可以自动增长数组或列表以适应该索引。为了在指定索引处添加元素，SpEL 将在设置指定值之前尝试使用元素类型的默认构造函数创建元素。如果元素类型没有默认构造函数，null将被添加到数组或列表中。如果没有知道如何设置值的内置或自定义转换器，null则将保留在数组或列表中指定索引处。
* Spring Framework 4.1 包含一个基本的表达式编译器。通常会解释表达式，这在评估期间提供了很大的动态灵活性，但不能提供最佳性能。对于偶尔的表达式使用，这很好，但是，当被其他组件（例如 Spring Integration）使用时，性能可能非常重要，并且不需要动态。 SpEL 编译器旨在满足这一需求。在评估期间，编译器生成一个体现表达式运行时行为的 Java 类，并使用该类来实现更快的表达式评估。由于没有围绕表达式键入，编译器在执行编译时使用在表达式的解释评估期间收集的信息。例如，它不能仅从表达式中知道属性引用的类型，但在第一次解释评估期间，它会找出它是什么。当然，如果各种表达式元素的类型随着时间的推移而变化，那么基于这些派生信息进行编译可能会在以后造成麻烦。出于这个原因，编译最适合那些类型信息不会在重复计算中改变的表达式。 考虑以下基本表达式：`someArray[0].someProperty.someOtherProperty < 0.1`, 因为前面的表达式涉及数组访问、一些属性取消引用和数值运算，所以性能提升可能非常明显。在 50000 次迭代的示例微基准测试中，使用解释器评估需要 75 毫秒，而使用表达式的编译版本只需要 3 毫秒。
* 编译器可以在 org.springframework.expression.spel.SpelCompilerMode枚举中捕获的三种模式之一中运行。模式如下：
  * OFF（默认）：编译器关闭。
  * IMMEDIATE: 在立即模式下，表达式会尽快编译。这通常是在第一次解释评估之后。如果编译表达式失败（通常是由于类型更改，如前所述），则表达式求值的调用者会收到异常。
  * MIXED：在混合模式下，表达式会随着时间在解释模式和编译模式之间静默切换。在经过一定次数的解释运行后，它们会切换到编译形式，如果编译形式出现问题（例如类型更改，如前所述），表达式会自动再次切换回解释形式。一段时间后，它可能会生成另一个已编译的表单并切换到它。基本上，用户进入IMMEDIATE模式的异常是在内部处理的。
* 您可以将 SpEL 表达式与基于 XML 或基于注释的配置元数据一起使用来定义BeanDefinition实例。在这两种情况下，定义表达式的语法都是 `#{ <expression string> }.`:
  * 可以使用表达式设置属性或构造函数参数值，如以下示例所示：
    ```xml
    <bean id="numberGuess" class="org.spring.samples.NumberGuess">
        <property name="randomNumber" value="#{ T(java.lang.Math).random() * 100.0 }"/>
    
        <!-- other properties -->
    </bean>
    ```
  * 以下示例显示了对systemProperties作为 SpEL 变量的 bean 的访问：
    ```xml
    <bean id="taxCalculator" class="org.spring.samples.TaxCalculator">
        <property name="defaultLocale" value="#{ systemProperties['user.region'] }"/>
    
        <!-- other properties -->
    </bean>
    ```
  * 您还可以按名称引用其他 bean 属性，如以下示例所示：
    ```xml
    <bean id="numberGuess" class="org.spring.samples.NumberGuess">
        <property name="randomNumber" value="#{ T(java.lang.Math).random() * 100.0 }"/>
    
        <!-- other properties -->
    </bean>
    
    <bean id="shapeGuess" class="org.spring.samples.ShapeGuess">
        <property name="initialShapeSeed" value="#{ numberGuess.randomNumber }"/>
    
        <!-- other properties -->
    </bean>
    ```
* 属性名称的第一个字母允许不区分大小写。因此，上述示例中的表达式可以分别写为Birthdate.Year + 1900和 PlaceOfBirth.City。此外，可以选择通过方法调用来访问属性——例如，getPlaceOfBirth().getCity()而不是 placeOfBirth.city.
* 大于和小于比较null遵循一个简单的规则：null被视为无（即不为零）。因此，任何其他值总是大于null（X > null总是true），并且没有其他值永远小于零（X < null总是false）。 如果您更喜欢数字比较，请避免基于数字的比较，而null倾向于与零进行比较（例如，X > 0或X < 0）。
* 小心原始类型，因为它们会立即被装箱到它们的包装类型。例如，1 instanceof T(int)计算结果为false，而 1 instanceof T(Integer)计算结果为true，如预期的那样。
* 如果评估上下文已经配置了 bean 解析器，您可以使用 @ 符号从表达式中查找 bean。要访问工厂 bean 本身，您应该在 bean 名称前加上一个 & 符号。
* 安全导航运算符用于避免 NullPointerException 和来自 [Groovy](http://www.groovy-lang.org/operators.html#_safe_navigation_operator) 语言。通常，当您有一个对象的引用时，您可能需要在访问该对象的方法或属性之前验证它不为空。为避免这种情况，安全导航运算符返回 null 而不是抛出异常。
* 选择是一种强大的表达式语言功能，可让您通过从其条目中进行选择将源集合转换为另一个集合。 选择使用 `.?[selectionExpression]`. 它过滤集合并返回一个包含原始元素子集的新集合。数组或任何实现 java.lang.Iterable 的东西都支持选择。对于列表或数组，将针对每个单独的元素评估选择标准。针对映射，针对每个映射条目（Java 类型的对象）评估选择标准Map.Entry。每个地图条目都有其key可value 访问的属性，以便在选择中使用。
  * `List<Inventor> list = (List<Inventor>) parser.parseExpression("members.?[nationality == 'Serbian']").getValue(societyContext);`
  * `Map newMap = parser.parseExpression("map.?[value<27]").getValue();`
* 除了返回所有选定的元素之外，您还可以只检索第一个或最后一个元素。要获取与选择匹配的第一个元素，语法为 `.^[selectionExpression]`. 要获得最后一个匹配的选择，语法是 `.$[selectionExpression]`.
* 投影让集合驱动子表达式的评估，结果是一个新的集合。投影的语法是 `.![projectionExpression]`. 例如，假设我们有一个发明者列表，但想要他们出生的城市列表。实际上，我们希望为发明者列表中的每个条目评估“placeOfBirth.city”。以下示例使用投影来执行此操作：`// returns ['Smiljan', 'Idvor' ]
List placesOfBirth = (List)parser.parseExpression("members.![placeOfBirth.city]");`.
* 面向方面编程 (AOP) 通过提供另一种思考程序结构的方式来补充面向对象编程 (OOP)。OOP 中模块化的关键单元是类，而 AOP 中模块化的单元是方面。方面支持跨多种类型和对象的关注点（例如事务管理）的模块化。（这种关注点在 AOP 文献中通常被称为“横切”关注点。） Spring 的关键组件之一是 AOP 框架。虽然 Spring IoC 容器不依赖 AOP（这意味着如果您不想使用 AOP，则不需要使用 AOP），AOP 补充了 Spring IoC 以提供非常强大的中间件解决方案。
* 带有 AspectJ 切入点的 Spring AOP: Spring 通过使用 [基于模式的方法](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop-schema) 或 [@AspectJ 注释样式](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop-ataspectj) 提供了编写自定义切面的简单而强大的方法。这两种风格都提供了完全类型化的建议和使用 AspectJ 切入点语言，同时仍然使用 Spring AOP 进行编织。AOP 在 Spring Framework 中用于：
  * 提供声明式企业服务。最重要的此类服务是 [声明式事务管理](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#transaction-declarative).
  * 让用户实现自定义方面，用 AOP 补充他们对 OOP 的使用。
* 




































































