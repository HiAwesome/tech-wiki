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
* 让我们从定义一些核心 AOP 概念和术语开始。这些术语不是 Spring 特定的。不幸的是，AOP 术语并不是特别直观。但是，如果 Spring 使用它自己的术语，那就更令人困惑了。
  * Aspect：跨多个类的关注点的模块化。事务管理是企业 Java 应用程序中横切关注点的一个很好的例子。在 Spring AOP 中，方面是通过使用常规类（ [基于模式的方法](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop-schema) ）或使用 @Aspect注解注释的常规类（ [@AspectJ 样式](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop-ataspectj) ）来实现的。
  * Join point：程序执行过程中的一个点，例如方法的执行或异常的处理。在 Spring AOP 中，一个连接点总是代表一个方法执行。
  * Advice：方面在特定连接点采取的行动。不同类型的建议包括“周围”、“之前”和“之后”建议。（通知类型将在后面讨论。）包括 Spring 在内的许多 AOP 框架将通知建模为拦截器，并在连接点周围维护一个拦截器链。
  * Pointcut：匹配连接点的谓词。Advice 与切入点表达式相关联，并在与切入点匹配的任何连接点处运行（例如，执行具有特定名称的方法）。切入点表达式匹配的连接点的概念是 AOP 的核心，Spring 默认使用 AspectJ 切入点表达式语言。
  * Introduction：代表一个类型声明额外的方法或字段。Spring AOP 允许您向任何建议的对象引入新接口（和相应的实现）。例如，您可以使用介绍使 bean 实现 IsModified接口，以简化缓存。（介绍在 AspectJ 社区中称为类型间声明。）
  * Target object：一个或多个方面建议的对象。也称为“建议对象”。由于 Spring AOP 是使用运行时代理实现的，因此该对象始终是代理对象。
  * AOP proxy：由 AOP 框架创建的对象，用于实现方面协定（建议方法执行等）。在 Spring Framework 中，AOP 代理是 JDK 动态代理或 CGLIB 代理。
  * Weaving：将方面与其他应用程序类型或对象链接以创建建议对象。这可以在编译时（例如，使用 AspectJ 编译器）、加载时或运行时完成。Spring AOP 与其他纯 Java AOP 框架一样，在运行时执行编织。
* Spring AOP 包括以下类型的通知：
  * 通知前（Before advice）：在连接点之前运行但不能阻止执行流继续到连接点的通知（除非它抛出异常）。
  * 返回通知后（After returning advice）：在连接点正常完成后运行的通知（例如，如果方法返回而没有引发异常）。
  * 抛出建议后（After throwing advice）：如果方法因抛出异常而退出，则运行建议。
  * 在（最终）通知之后（After (finally) advice）：无论连接点以何种方式退出（正常或异常返回），都将运行建议。
  * 围绕建议（Around advice）：围绕连接点的建议，例如方法调用。这是最有力的建议。环绕通知可以在方法调用之前和之后执行自定义行为。它还负责选择是继续到连接点还是通过返回自己的返回值或抛出异常来缩短建议的方法执行。
* 环绕建议（Around advice）是最一般的建议。由于 Spring AOP 与 AspectJ 一样，提供了全方位的通知类型，因此我们建议您使用可以实现所需行为的最不强大的通知类型。例如，如果您只需要使用方法的返回值来更新缓存，那么您最好实现一个后返回通知而不是一个环绕通知，尽管一个环绕通知可以完成同样的事情。使用最具体的通知类型提供了一个更简单的编程模型，并且出错的可能性更小。例如，您不需要proceed() 在 used for around 建议上调用方法JoinPoint，因此，您不能不调用它。 所有建议参数都是静态类型的，因此您可以使用适当类型的建议参数（例如，方法执行的返回值的类型）而不是Object数组。 切入点匹配的连接点的概念是 AOP 的关键，这将它与仅提供拦截的旧技术区分开来。切入点使建议的目标独立于面向对象的层次结构。例如，您可以将提供声明性事务管理的环绕建议应用到一组跨越多个对象（例如服务层中的所有业务操作）的方法。
* Spring AOP 是用纯 Java 实现的。不需要特殊的编译过程。Spring AOP 不需要控制类加载器层次结构，因此适用于 servlet 容器或应用程序服务器。 Spring AOP 当前仅支持方法执行连接点（建议在 Spring bean 上执行方法）。没有实现字段拦截，尽管可以在不破坏核心 Spring AOP API 的情况下添加对字段拦截的支持。如果您需要建议字段访问和更新连接点，请考虑使用 AspectJ 等语言。 Spring AOP 的 AOP 方法不同于大多数其他 AOP 框架。目的不是提供最完整的 AOP 实现（尽管 Spring AOP 非常有能力）。相反，其目的是提供 AOP 实现和 Spring IoC 之间的紧密集成，以帮助解决企业应用程序中的常见问题。 因此，例如，Spring 框架的 AOP 功能通常与 Spring IoC 容器一起使用。方面是通过使用普通的 bean 定义语法来配置的（尽管这允许强大的“自动代理”功能）。这是与其他 AOP 实现的关键区别。您无法使用 Spring AOP 轻松或高效地做一些事情，例如建议非常细粒度的对象（通常是域对象）。在这种情况下，AspectJ 是最佳选择。然而，我们的经验是，Spring AOP 为企业 Java 应用程序中大多数适合 AOP 的问题提供了出色的解决方案。 Spring AOP 从不努力与 AspectJ 竞争以提供全面的 AOP 解决方案。我们相信 Spring AOP 等基于代理的框架和 AspectJ 等成熟框架都很有价值，它们是互补的，而不是竞争的。Spring 将 Spring AOP 和 IoC 与 AspectJ 无缝集成，以在一致的基于 Spring 的应用程序架构中实现 AOP 的所有使用。此集成不会影响 Spring AOP API 或 AOP Alliance API。Spring AOP 保持向后兼容。
* Spring 框架的核心原则之一是非侵入性。这就是不应该强迫您将特定于框架的类和接口引入您的业务或领域模型的想法。但是，在某些地方，Spring Framework 确实为您提供了将 Spring Framework 特定的依赖项引入代码库的选项。为您提供此类选项的理由是，在某些情况下，以这种方式阅读或编写某些特定功能可能更容易。但是，Spring 框架（几乎）总是为您提供选择：您可以自由地就哪个选项最适合您的特定用例或场景做出明智的决定。 与本章相关的一个这样的选择是选择哪种 AOP 框架（以及哪种 AOP 风格）。您可以选择 AspectJ、Spring AOP 或两者兼而有之。您还可以选择 @AspectJ 注释样式方法或 Spring XML 配置样式方法。本章选择首先介绍@AspectJ 样式方法这一事实不应被视为Spring 团队更喜欢@AspectJ 注释样式方法而不是Spring XML 配置样式的指示。
* Spring AOP 默认为 AOP 代理使用标准 JDK 动态代理。这使得任何接口（或一组接口）都可以被代理。 Spring AOP 也可以使用 CGLIB 代理。这是代理类而不是接口所必需的。默认情况下，如果业务对象未实现接口，则使用 CGLIB。由于对接口而不是类进行编程是一种很好的做法，因此业务类通常实现一个或多个业务接口。在那些（希望很少见）需要建议未在接口上声明的方法或需要将代理对象作为具体类型传递给方法的情况下，可以 [强制使用 CGLIB](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop-proxying) 。 重要的是要掌握 Spring AOP 是基于代理的这一事实。请参阅 [了解 AOP 代理](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop-understanding-aop-proxies), 以全面了解此实现细节的实际含义。
* 通过组件扫描自动检测方面: 您可以通过类中的@Bean方法将方面类注册为 Spring XML 配置中的常规 bean @Configuration，或者让 Spring 通过类路径扫描自动检测它们——与任何其他 Spring 管理的 bean 相同。但是，请注意， @Aspect注释不足以在类路径中进行自动检测。为此，您需要添加一个单独的@Component注释（或者，根据 Spring 组件扫描器的规则，一个符合条件的自定义构造型注释）。
* 建议方面与其他方面（Advising aspects with other aspects）？ 在 Spring AOP 中，切面本身不能成为其他切面通知的目标。类上的@Aspect注释将其标记为方面，因此将其排除在自动代理之外。
* 形成 @Pointcut 注释值的切入点表达式是常规的 AspectJ 切入点表达式。有关 AspectJ 切入点语言的完整讨论，请参阅 [AspectJ Programming Guide](https://www.eclipse.org/aspectj/doc/released/progguide/index.html) （以及扩展的 [AspectJ 5 Developer's Notebook](https://www.eclipse.org/aspectj/doc/released/adk15notebook/index.html) ）或有关 AspectJ 的书籍之一（例如Colyer 等人的Eclipse AspectJ或AspectJ in Action，拉姆尼瓦斯·拉达德）。
* Spring AOP 支持在切入点表达式中使用以下 AspectJ 切入点指示符 (PCD)：
  * execution：用于匹配方法执行连接点。这是使用 Spring AOP 时使用的主要切入点指示符。
  * within: 限制匹配到特定类型内的连接点（使用 Spring AOP 时执行匹配类型内声明的方法）。
  * this：限制匹配到连接点（使用 Spring AOP 时方法的执行），其中 bean 引用（Spring AOP 代理）是给定类型的实例。
  * target：将匹配限制在目标对象（被代理的应用程序对象）是给定类型的实例的连接点（使用 Spring AOP 时方法的执行）。
  * args: 限制匹配到参数是给定类型的实例的连接点（使用 Spring AOP 时方法的执行）。
  * @target：限制匹配到连接点（使用 Spring AOP 时方法的执行），其中执行对象的类具有给定类型的注释。
  * @args：将匹配限制为连接点（使用 Spring AOP 时方法的执行），其中传递的实际参数的运行时类型具有给定类型的注释。
  * @within：将匹配限制为具有给定注释的类型内的连接点（使用 Spring AOP 时执行在具有给定注释的类型中声明的方法）。
  * @annotation：限制匹配到连接点的主题（在 Spring AOP 中运行的方法）具有给定注释的连接点。
* 其他切入点类型：完整的 AspectJ 切入点语言支持 Spring 中不支持的其他切入点指示符：call、get、set、preinitialization、 staticinitialization、initialization、handler、adviceexecution、withincode、cflow、 cflowbelow、if、@this和@withincode。在 Spring AOP 解释的切入点表达式中使用这些切入点指示符会导致IllegalArgumentException抛出异常。 Spring AOP 支持的切入点指示符集可能会在未来的版本中扩展，以支持更多的 AspectJ 切入点指示符。
* 由于 Spring AOP 将匹配限制为仅方法执行连接点，因此前面对切入点指示符的讨论给出了比您在 AspectJ 编程指南中找到的更窄的定义。此外，AspectJ 本身具有基于类型的语义，并且在执行连接点处，两者都this引用target同一个对象：执行方法的对象。Spring AOP 是一个基于代理的系统，它区分代理对象本身（绑定到this）和代理背后的目标对象（绑定到target）。
* 由于 Spring 的 AOP 框架基于代理的特性，根据定义，目标对象内的调用不会被拦截。对于 JDK 代理，只能拦截代理上的公共接口方法调用。使用 CGLIB，代理上的公共和受保护的方法调用被拦截（如果需要，甚至包可见的方法）。但是，通过代理的常见交互应始终通过公共签名进行设计。 请注意，切入点定义通常与任何拦截的方法匹配。如果切入点严格来说是只公开的，即使在 CGLIB 代理场景中，通过代理进行潜在的非公开交互，也需要相应地定义它。 如果您的拦截需求包括目标类中的方法调用甚至构造函数，请考虑使用 Spring 驱动的原生 AspectJ 编织，而不是 Spring 的基于代理的 AOP 框架。这就构成了具有不同特点的不同AOP使用模式，所以在做决定之前一定要让自己熟悉编织。
* beanPCD 仅在 Spring AOP 中受支持，在本机 AspectJ 编织中不支持。它是 AspectJ 定义的标准 PCD 的特定于 Spring 的扩展，因此不适用于@Aspect模型中声明的方面。 PCD在bean实例级别（基于 Spring bean 名称概念）而不是仅在类型级别（基于编织的 AOP 受限）运行。基于实例的切入点指示符是 Spring 的基于代理的 AOP 框架的一种特殊功能，它与 Spring bean 工厂的紧密集成，通过名称来识别特定的 bean 是自然而直接的。
* 在使用企业应用程序时，开发人员通常希望从多个方面引用应用程序的模块和特定的操作集。我们建议CommonPointcuts为此目的定义一个捕获通用切入点表达式的切面。这样的方面通常类似于以下示例：
  ```java
  package com.xyz.myapp;
  
  import org.aspectj.lang.annotation.Aspect;
  import org.aspectj.lang.annotation.Pointcut;
  
  @Aspect
  public class CommonPointcuts {
  
      /**
       * A join point is in the web layer if the method is defined
       * in a type in the com.xyz.myapp.web package or any sub-package
       * under that.
       */
      @Pointcut("within(com.xyz.myapp.web..*)")
      public void inWebLayer() {}
  
      /**
       * A join point is in the service layer if the method is defined
       * in a type in the com.xyz.myapp.service package or any sub-package
       * under that.
       */
      @Pointcut("within(com.xyz.myapp.service..*)")
      public void inServiceLayer() {}
  
      /**
       * A join point is in the data access layer if the method is defined
       * in a type in the com.xyz.myapp.dao package or any sub-package
       * under that.
       */
      @Pointcut("within(com.xyz.myapp.dao..*)")
      public void inDataAccessLayer() {}
  
      /**
       * A business service is the execution of any method defined on a service
       * interface. This definition assumes that interfaces are placed in the
       * "service" package, and that implementation types are in sub-packages.
       *
       * If you group service interfaces by functional area (for example,
       * in packages com.xyz.myapp.abc.service and com.xyz.myapp.def.service) then
       * the pointcut expression "execution(* com.xyz.myapp..service.*.*(..))"
       * could be used instead.
       *
       * Alternatively, you can write the expression using the 'bean'
       * PCD, like so "bean(*Service)". (This assumes that you have
       * named your Spring service beans in a consistent fashion.)
       */
      @Pointcut("execution(* com.xyz.myapp..service.*.*(..))")
      public void businessService() {}
  
      /**
       * A data access operation is the execution of any method defined on a
       * dao interface. This definition assumes that interfaces are placed in the
       * "dao" package, and that implementation types are in sub-packages.
       */
      @Pointcut("execution(* com.xyz.myapp.dao.*.*(..))")
      public void dataAccessOperation() {}
  
  }
  ```
* 以下示例显示了一些常见的切入点表达式：
  * 任何公共方法的执行：`execution(public * *(..))`
  * 名称以 set 开头的任何方法的执行：`execution(* set*(..))`
  * AccountService 接口定义的任何方法的执行：`execution(* com.xyz.service.AccountService.*(..))`
  * service 包中定义的任何方法的执行：`execution(* com.xyz.service.*.*(..))`
  * 服务包或其子包之一中定义的任何方法的执行：`execution(* com.xyz.service..*.*(..))`
  * 服务包中的任何连接点（仅在 Spring AOP 中执行方法）：`within(com.xyz.service.*)`
  * 服务包或其子包之一中的任何连接点（仅在 Spring AOP 中执行方法）：`within(com.xyz.service..*)`
  * AccountService代理实现接口的任何连接点（仅在 Spring AOP 中执行方法）：`this(com.xyz.service.AccountService)`
  * AccountService目标对象实现接口的任何连接点（仅在 Spring AOP 中执行方法）：`target(com.xyz.service.AccountService)`
  * 任何接受单个参数且在运行时传递的参数为的连接点（仅在 Spring AOP 中执行方法）Serializable：`args(java.io.Serializable)`
  * 目标对象具有 @Transactional注释的任何连接点（仅在 Spring AOP 中执行方法）：`@target(org.springframework.transaction.annotation.Transactional)`
  * 目标对象的声明类型具有@Transactional注释的任何连接点（仅在 Spring AOP 中执行方法）：`@within(org.springframework.transaction.annotation.Transactional)`
  * 执行方法具有 @Transactional注释的任何连接点（仅在 Spring AOP 中执行方法）：`@annotation(org.springframework.transaction.annotation.Transactional)`
  * 任何接受单个参数的连接点（仅在 Spring AOP 中执行方法），并且传递的参数的运行时类型具有@Classified注释：`@args(com.xyz.security.Classified)`
  * Spring bean 上的任何连接点（方法仅在 Spring AOP 中执行）名为 tradeService：`bean(tradeService)`
  * 名称与通配符表达式匹配的 Spring bean 上的任何连接点（仅在 Spring AOP 中执行方法）*Service：`bean(*Service)`
* 在编译期间，AspectJ 处理切入点以优化匹配性能。检查代码并确定每个连接点是否（静态或动态）匹配给定的切入点是一个代价高昂的过程。（动态匹配意味着无法从静态分析中完全确定匹配，并且在代码中放置测试以确定代码运行时是否存在实际匹配）。在第一次遇到切入点声明时，AspectJ 将其重写为匹配过程的最佳形式。这是什么意思？基本上，切入点在 DNF（析取范式）中被重写，切入点的组件被排序，以便首先检查那些评估成本较低的组件。 然而，AspectJ 只能使用它被告知的内容。为了获得最佳匹配性能，您应该考虑他们试图实现的目标，并在定义中尽可能缩小匹配的搜索空间。现有的指示符自然属于以下三组之一：kinded、scoping 和 contextual：
  * 种类指示符选择一种特定类型的连接点： execution、get、set、call和handler。 
  * 范围指示符选择一组连接兴趣点（可能有多种）：within和withincode 
  * 上下文指示符根据上下文匹配（并且可以选择绑定）： this、、target和@annotation
* 一个写得很好的切入点应该至少包括前两种类型（种类和范围）。您可以包含上下文指示符以根据连接点上下文进行匹配，或绑定该上下文以在建议中使用。由于额外的处理和分析，只提供一个 kinded 指示符或只提供一个上下文指示符是可行的，但可能会影响编织性能（使用的时间和内存）。范围指示符的匹配速度非常快，使用它们意味着 AspectJ 可以非常快速地消除不应进一步处理的连接点组。如果可能，一个好的切入点应始终包含一个切入点。
* 请注意，@AfterThrowing这并不表示一般的异常处理回调。具体来说，@AfterThrowing建议方法只应该从连接点（用户声明的目标方法）本身接收异常，而不是从伴随的 @After/@AfterReturning方法接收异常。
* 请注意，@AfterAspectJ 中的通知被定义为“在 finally 通知之后”，类似于 try-catch 语句中的 finally 块。对于从连接点（用户声明的目标方法）抛出的任何结果、正常返回或异常，都会调用它，与之相反，@AfterReturning它仅适用于成功的正常返回。
* 最后一种建议是围绕建议。围绕通知“围绕”匹配方法的执行。它有机会在方法运行之前和之后进行工作，并确定该方法何时、如何以及是否真正开始运行。如果您需要以线程安全的方式在方法执行之前和之后共享状态（例如，启动和停止计时器），则通常使用环绕通知。始终使用满足您要求的最不强大的建议形式。 例如，如果之前的建议足以满足您的需求，请不要使用环绕建议。	
Always use the least powerful form of advice that meets your requirements.  For example, do not use around advice if before advice is sufficient for your needs.
* 如果您将周围建议方法的返回类型声明为void,null 将始终返回给调用者，有效地忽略任何调用的结果proceed()。因此，建议使用 around 通知方法声明返回类型为Object. 建议方法通常应该返回从调用返回的值proceed()，即使底层方法具有void返回类型。但是，根据用例，建议可以选择返回缓存值、包装值或其他值。	If you declare the return type of your around advice method as void, null will always be returned to the caller, effectively ignoring the result of any invocation of proceed(). It is therefore recommended that an around advice method declare a return type of Object. The advice method should typically return the value returned from an invocation of proceed(), even if the underlying method has a void return type. However, the advice may optionally return a cached value, a wrapped value, or some other value depending on the use case.
* 使用argNames属性有点笨拙，所以如果argNames没有指定属性，Spring AOP 会查看类的调试信息并尝试从局部变量表中确定参数名称。只要使用调试信息（-g:vars至少）编译了类，就会出现此信息。使用此标志进行编译的后果是：（1）您的代码更容易理解（逆向工程），（2）类文件大小稍微大一点（通常无关紧要），（3）优化以删除未使用的本地您的编译器未应用变量。换句话说，打开此标志进行构建应该不会遇到任何困难。  如果 AspectJ 编译器 ( ) 已经编译了 @AspectJ 方面，ajc即使没有调试信息，您也不需要添加argNames属性，因为编译器会保留所需的信息。
* 当多条建议都想在同一个连接点运行时会发生什么？Spring AOP 遵循与 AspectJ 相同的优先级规则来确定通知执行的顺序。最高优先级的建议首先“在进入的路上”运行（因此，给定两条之前的建议，优先级最高的一条首先运行）。从连接点“退出”时，优先级最高的通知最后运行（因此，给定两条后通知，具有最高优先级的一条将运行第二个）。 当在不同方面定义的两条通知都需要在同一个连接点运行时，除非您另外指定，否则执行顺序是未定义的。您可以通过指定优先级来控制执行顺序。这是通过在org.springframework.core.Ordered方面类中实现接口或使用注释对其进行@Order注释以正常的 Spring 方式完成的。给定两个方面，从Ordered.getOrder()（或注释值）返回较低值的方面具有较高的优先级。特定方面的每个不同的建议类型在概念上意味着直接应用于连接点。因此，建议方法不应该从伴随的/方法@AfterThrowing接收异常。@After@AfterReturning. 从 Spring Framework 5.2.7 开始，在同一个@Aspect类中定义的需要在同一个连接点运行的通知方法根据它们的通知类型按以下顺序分配优先级，从最高到最低优先级：@Around, @Before, @After, @AfterReturning, @AfterThrowing。但是请注意，根据@AfterAspectJ对.@AfterReturning@AfterThrowing@After. 当同一个类中定义的两条相同类型的advice（例如两个@Afteradvice方法）@Aspect都需要运行在同一个join point时，排序是未定义的（因为无法通过javac 编译类的反射）。考虑将此类建议方法折叠为每个@Aspect类中每个连接点的一个建议方法，或者将建议片段重构为单独的@Aspect类，您可以通过Ordered或在方面级别对这些类进行排序@Order。
* 现在您已经了解了所有组成部分的工作原理，我们可以将它们组合在一起做一些有用的事情。 由于并发问题（例如，死锁失败者），业务服务的执行有时会失败。如果该操作被重试，则很可能在下一次尝试时成功。对于在这种情况下适合重试的业务服务（不需要返回给用户解决冲突的幂等操作），我们希望透明地重试操作以避免客户端看到 PessimisticLockingFailureException. 这是一个明确跨越服务层中多个服务的要求，因此非常适合通过方面实现。 因为我们要重试操作，所以我们需要使用around通知，以便我们可以proceed多次调用。以下清单显示了基本方面的实现:
  ```java
  @Aspect
  public class ConcurrentOperationExecutor implements Ordered {

      private static final int DEFAULT_MAX_RETRIES = 2;

      private int maxRetries = DEFAULT_MAX_RETRIES;
      private int order = 1;

      public void setMaxRetries(int maxRetries) {
          this.maxRetries = maxRetries;
      }

      public int getOrder() {
          return this.order;
      }

      public void setOrder(int order) {
          this.order = order;
      }

      @Around("com.xyz.myapp.CommonPointcuts.businessService()")
      public Object doConcurrentOperation(ProceedingJoinPoint pjp) throws Throwable {
          int numAttempts = 0;
          PessimisticLockingFailureException lockFailureException;
          do {
              numAttempts++;
              try {
                  return pjp.proceed();
              }
              catch(PessimisticLockingFailureException ex) {
                  lockFailureException = ex;
              }
          } while(numAttempts <= this.maxRetries);
          throw lockFailureException;
      }
  }
  ```
* 一旦你决定一个切面是实现给定需求的最佳方法，你如何在使用 Spring AOP 或 AspectJ 以及在 Aspect 语言（代码）风格、@AspectJ 注释风格或 Spring XML 风格之间做出决定？这些决策受到许多因素的影响，包括应用程序需求、开发工具和团队对 AOP 的熟悉程度。使用可以工作的最简单的东西。Spring AOP 比使用完整的 AspectJ 更简单，因为不需要将 AspectJ 编译器/编织器引入您的开发和构建过程。如果您只需要建议对 Spring bean 执行操作，那么 Spring AOP 是正确的选择。如果您需要通知不由 Spring 容器管理的对象（例如域对象，通常是），则需要使用 AspectJ。如果您希望建议连接点而不是简单的方法执行（例如，字段获取或设置连接点等），您还需要使用 AspectJ。 当您使用 AspectJ 时，您可以选择 AspectJ 语言语法（也称为“代码样式”）或 @AspectJ 注释样式。显然，如果您不使用 Java 5+，那么已经为您做出了选择：使用代码风格。如果方面在您的设计中扮演重要角色，并且您能够使用 Eclipse 的AspectJ 开发工具 (AJDT)插件，那么 AspectJ 语言语法是首选选项。它更简洁，因为该语言是专门为编写方面而设计的。如果您不使用 Eclipse 或只有几个方面在您的应用程序中没有发挥主要作用，您可能需要考虑使用 @AspectJ 样式，在您的 IDE 中坚持常规 Java 编译，并添加一个方面编织阶段你的构建脚本。
* 如果您选择使用 Spring AOP，您可以选择 @AspectJ 或 XML 样式。有各种权衡需要考虑。 现有 Spring 用户可能最熟悉 XML 样式，并且它由真正的 POJO 支持。当使用 AOP 作为配置企业服务的工具时，XML 可能是一个不错的选择（一个很好的测试是您是否将切入点表达式视为您可能想要独立更改的配置的一部分）。使用 XML 样式，可以说从您的配置中更清楚系统中存在哪些方面。 XML 样式有两个缺点。首先，它没有将它所解决的需求的实现完全封装在一个地方。**DRY 原则说，系统内的任何知识都应该有一个单一的、明确的、权威的表示。The DRY principle says that there should be a single, unambiguous, authoritative representation of any piece of knowledge within a system.** 使用 XML 样式时，如何实现需求的知识被拆分为支持 bean 类的声明和配置文件中的 XML。当您使用@AspectJ 样式时，此信息被封装在一个模块中：方面。其次，与@AspectJ 风格相比，XML 风格在表达方面稍有限制：仅支持“单例”切面实例化模型，并且无法组合 XML 中声明的命名切入点。XML 方法的缺点是不能通过 accountPropertyAccess组合这些定义来定义切入点。 @AspectJ 样式支持额外的实例化模型和更丰富的切入点组合。它具有将方面保持为模块化单元的优点。它还具有以下优点：Spring AOP 和 AspectJ 都可以理解（并因此使用）@AspectJ 方面。因此，如果您以后决定需要 AspectJ 的功能来实现其他要求，您可以轻松迁移到经典的 AspectJ 设置。**总的来说，Spring 团队更喜欢 @AspectJ 风格的自定义方面，而不是简单的企业服务配置。**
* Spring AOP 使用 JDK 动态代理或 CGLIB 为给定的目标对象创建代理。JDK 动态代理内置在 JDK 中，而 CGLIB 是一个通用的开源类定义库（重新打包到spring-core). 如果要代理的目标对象实现了至少一个接口，则使用 JDK 动态代理。目标类型实现的所有接口都被代理。如果目标对象没有实现任何接口，则创建一个 CGLIB 代理。 如果您想强制使用 CGLIB 代理（例如，代理为目标对象定义的每个方法，而不仅仅是那些由其接口实现的方法），您可以这样做。但是，您应该考虑以下问题： 
  * 使用 CGLIB，final不能建议方法，因为它们不能在运行时生成的子类中被覆盖。 
  * 从 Spring 4.0 开始，代理对象的构造函数不再被调用两次，因为 CGLIB 代理实例是通过 Objenesis 创建的。仅当您的 JVM 不允许绕过构造函数时，您可能会看到来自 Spring 的 AOP 支持的双重调用和相应的调试日志条目。
* 多个 `<aop:config/>` 部分在运行时被折叠成一个统一的自动代理创建器，它应用任何 部分（通常来自不同的 XML bean 定义文件）指定的最强代理设置。`<aop:config/>` 这也适用于`<tx:annotation-driven/>` and `<aop:aspectj-autoproxy/>` 元素。 需要明确的是，使用 proxy-target-class="true" on `<tx:annotation-driven/>`、 `<aop:aspectj-autoproxy/>` 或 `<aop:config/>` 元素会强制对所有三个使用 CGLIB 代理。
* 这里要理解的关键是类的main(..)方法内部的客户端代码Main有对代理的引用。这意味着对该对象引用的方法调用是对代理的调用。因此，代理可以委托给与该特定方法调用相关的所有拦截器（建议）。但是，一旦调用最终到达目标对象（SimplePojo在这种情况下为引用），它可能对自身进行的任何方法调用，例如this.bar()or this.foo()，都将针对this引用而不是代理调用。这具有重要意义。这意味着自调用不会导致与方法调用相关的建议有机会运行。 好的，那该怎么办呢？最好的方法（术语“最好”在这里被松散地使用）是重构你的代码，这样自调用就不会发生。这确实需要您做一些工作，但它是最好的、侵入性最小的方法。下一种方法绝对可怕，我们不愿指出，正是因为它太可怕了。您可以（对我们来说很痛苦）将您的类中的逻辑完全绑定到 Spring AOP.
* Spring 附带了一个小的 AspectJ 方面库，它在您的发行版中以spring-aspects.jar. 您需要将其添加到您的类路径中才能使用其中的方面。[Using AspectJ to Dependency Inject Domain Objects with Spring](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop-atconfigurable) 和 [AspectJ 的其他 Spring 方面](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop-ajlib-other) 讨论了这个库的内容以及如何使用它。[使用 Spring IoC 配置 AspectJ 方面](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop-aj-configure) 讨论了如何依赖注入使用 AspectJ 编译器编织的 AspectJ 方面。最后， 在 [Spring Framework](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop-aj-ltw) 中使用 AspectJ 进行加载时编织介绍了使用 AspectJ 的 Spring 应用程序的加载时编织。
* Spring 控制流切入点在概念上类似于 AspectJcflow切入点，但功能较弱。（目前无法指定一个切入点在与另一个切入点匹配的连接点下方运行。）控制流切入点匹配当前调用堆栈。例如，如果连接点被com.mycompany.web包中的方法或SomeCaller类调用，它可能会触发。控制流切入点是通过使用 org.springframework.aop.support.ControlFlowPointcut 类来指定的。与其他动态切入点相比，控制流切入点在运行时的评估成本要高得多。在 Java 1.4 中，成本大约是其他动态切入点的五倍。
* 因为 Spring AOP 中的切入点是 Java 类而不是语言特性（如在 AspectJ 中），所以您可以声明自定义切入点，无论是静态的还是动态的。Spring 中的自定义切入点可以任意复杂。但是，如果可以，我们建议使用 AspectJ 切入点表达式语言。 Spring 的更高版本可能会支持 JAC 提供的“语义切入点”——例如，“所有更改目标对象中实例变量的方法”。 Later versions of Spring may offer support for “semantic pointcuts” as offered by JAC — for example, “all methods that change instance variables in the target object.”
* 如果 throws-advice 方法本身抛出异常，它会覆盖原始异常（即，它会更改抛出给用户的异常）。覆盖异常通常是 RuntimeException，它与任何方法签名兼容。但是，如果 throws-advice 方法抛出检查异常，它必须匹配目标方法声明的异常，因此在某种程度上与特定目标方法签名耦合。不要抛出与目标方法的签名不兼容的未声明的检查异常！
* 本节是关于如何ProxyFactoryBean 选择为特定目标对象（将被代理）创建基于 JDK 的代理或基于 CGLIB 的代理的权威文档。如果要代理的目标对象的类（以下简称目标类）没有实现任何接口，则创建基于CGLIB的代理。这是最简单的场景，因为 JDK 代理是基于接口的，没有接口意味着 JDK 代理甚至是不可能的。您可以插入目标 bean 并通过设置interceptorNames属性来指定拦截器列表。请注意，即使 的proxyTargetClass属性 ProxyFactoryBean已设置为，也会创建基于 CGLIB 的代理false。（这样做毫无意义，最好从 bean 定义中删除，因为它充其量是多余的，最坏的情况是令人困惑。） 如果目标类实现一个（或多个）接口，则创建的代理类型取决于ProxyFactoryBean. 如果 的proxyTargetClass属性ProxyFactoryBean已设置为true，则创建基于 CGLIB 的代理。这是有道理的，并且符合最小意外原则。即使 的proxyInterfaces属性 ProxyFactoryBean已设置为一个或多个完全限定的接口名称，该proxyTargetClass属性设置为这一事实也会true导致基于 CGLIB 的代理生效。 如果 的proxyInterfaces属性ProxyFactoryBean已设置为一个或多个完全限定的接口名称，则会创建一个基于 JDK 的代理。proxyInterfaces 创建的代理实现了属性中指定的所有接口。如果目标类碰巧实现了比proxyInterfaces属性中指定的接口多得多的接口，那很好，但是返回的代理不会实现这些额外的接口。 如果尚未设置 的proxyInterfaces属性，但目标类确实实现了一个（或多个）接口，则自动检测目标类确实实现了至少一个接口这一事实，并创建了一个基于 JDK 的代理。实际代理的接口是目标类实现的所有接口。实际上，这与为属性提供目标类实现的每个接口的列表相同。但是，它的工作量大大减少，而且更不容易出现印刷错误。ProxyFactoryBeanProxyFactoryBeanproxyInterfaces.
* **在大多数应用程序中，将 AOP 代理创建与 IoC 框架集成是最佳实践。我们建议您使用 AOP 从 Java 代码外部化配置，就像您通常应该做的那样。**
* 在生产中修改关于业务对象的建议是否可取（不是双关语）是值得怀疑的，尽管毫无疑问，有合法的使用案例。但是，它在开发中非常有用（例如，在测试中）。我们有时发现能够以拦截器或其他建议的形式添加测试代码非常有用，进入我们想要测试的方法调用。（例如，通知可以进入为该方法创建的事务中，可能在将事务标记为回滚之前运行 SQL 以检查数据库是否正确更新。）
* 使用池化目标源提供了与无状态会话 EJB 类似的编程模型，其中维护了相同实例的池，方法调用将释放池中的对象。 Spring pooling 和 SLSB pooling 的一个关键区别是 Spring pooling 可以应用于任何 POJO。与一般的 Spring 一样，此服务可以以非侵入方式应用。 Spring 提供对 Commons Pool 2.2 的支持，它提供了相当高效的池化实现。您需要commons-pool应用程序类路径中的 Jar 才能使用此功能。您还可以子类化 org.springframework.aop.target.AbstractPoolingTargetSource以支持任何其他池 API。 Commons Pool 1.5+ 也受支持，但自 Spring Framework 4.2 起已弃用。
* **通常不需要池化无状态服务对象。我们不认为它应该是默认选择，因为大多数无状态对象自然是线程安全的，如果资源被缓存，实例池是有问题的。**
* ThreadLocal当在多线程和多类加载器环境中错误地使用它们时，实例会出现严重的问题（可能导致内存泄漏）。您应该始终考虑将 threadlocal 包装在其他类中，并且永远不要直接使用它ThreadLocal本身（包装类除外）。此外，您应该始终记住正确设置和取消设置（后者仅涉及对 的调用 ThreadLocal.set(null)）线程本地的资源。在任何情况下都应该取消设置，因为不取消设置可能会导致有问题的行为。Spring 的 ThreadLocal支持为您执行此操作，并且应该始终考虑支持在 ThreadLocal没有其他适当处理代码的情况下使用实例。
* Spring 注释使用JSR 305 注释（一种休眠但广泛传播的 JSR）进行元注释。JSR-305 元注释让 IDEA 或 Kotlin 等工具供应商以通用方式提供空安全支持，而无需对 Spring 注释进行硬编码支持。 没有必要也不建议将 [JSR-305](https://jcp.org/en/jsr/detail?id=305) 依赖项添加到项目类路径以利用 Spring 空安全 API。只有在代码库中使用空安全注解的项目（例如基于 Spring 的库）才应添加 com.google.code.findbugs:jsr305:3.0.2 Gradle compileOnly 配置或 Maven provided范围以避免编译警告。
* 正如 [ByteBuffer](https://docs.oracle.com/javase/8/docs/api/java/nio/ByteBuffer.html) 的 Javadoc 中所解释的，字节缓冲区可以是直接的或非直接的。直接缓冲区可以驻留在 Java 堆之外，这消除了对本地 I/O 操作进行复制的需要。这使得直接缓冲区对于通过套接字接收和发送数据特别有用，但它们的创建和释放成本也更高，这导致了池化缓冲区的想法。 PooledDataBuffer是它的扩展，DataBuffer它有助于引用计数，这对于字节缓冲池至关重要。它是如何工作的？当 aPooledDataBuffer被分配时，引用计数为 1。调用retain()递增计数，调用release()递减计数。只要计数大于0，就保证缓冲区不会被释放。当计数减少到 0 时，可以释放池化缓冲区，这实际上可能意味着为缓冲区保留的内存返回到内存池。 请注意，PooledDataBuffer与其直接操作，在大多数情况下，最好使用DataBufferUtils应用版本中的便捷方法或 DataBuffer仅当它是PooledDataBuffer.
* spring-jcl从 Spring Framework 5.0 开始，Spring 在模块中实现了自己的 Commons Logging 桥接器。该实现检查类路径中是否存在 Log4j 2.x API 和 SLF4J 1.7 API，并使用找到的第一个作为日志实现，回退到 Java 平台的核心日志工具（也称为JUL或java.util.logging）如果 Log4j 2.x 和 SLF4J 都不可用。 将 Log4j 2.x 或 Logback（或其他 SLF4J 提供程序）放入您的类路径中，无需任何额外的桥梁，并让框架自动适应您的选择。有关详细信息，请参阅 [Spring Boot 日志记录参考文档](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-logging). Spring 的 Commons Logging 变体仅用于核心框架和扩展中的基础设施日志记录目的。对于应用程序代码中的日志记录需求，更喜欢直接使用 Log4j 2.x、SLF4J 或 JUL。
* The util Schema: As the name implies, the util tags deal with common, utility configuration issues, such as configuring collections, referencing constants, and so forth. To use the tags in the util schema, you need to have the following preamble at the top of your Spring XML configuration file (the text in the snippet references the correct schema so that the tags in the util namespace are available to you): 顾名思义，util标签处理常见的实用程序配置问题，例如配置集合、引用常量等。要使用util架构中的标签，您需要在 Spring XML 配置文件的顶部添加以下前导码（片段中的文本引用正确的架构，以便util您可以使用命名空间中的标签）：
  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:util="http://www.springframework.org/schema/util"
          <!-- bean definitions here -->
  </beans>
  ```
* XML 模式创作: 从 2.0 版开始，Spring 提供了一种机制，可以将基于模式的扩展添加到基本的 Spring XML 格式中，用于定义和配置 bean。本节介绍如何编写自己的自定义 XML bean 定义解析器并将此类解析器集成到 Spring IoC 容器中。 为了便于编写使用模式感知 XML 编辑器的配置文件，Spring 的可扩展 XML 配置机制基于 XML Schema。如果您不熟悉标准 Spring 发行版附带的 Spring 当前 XML 配置扩展，您应该首先阅读 [XML Schemas](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#xsd-schemas) 的上一节。 要创建新的 XML 配置扩展：
  1. [创作](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#xsd-custom-schema) 一个 XML 模式来描述您的自定义元素。
  2. 编写自定义 NamespaceHandler 实现 [代码](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#xsd-custom-namespacehandler).
  3. 编写一个或多个实现 [代码](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#xsd-custom-parser) BeanDefinitionParser（这是完成实际工作的地方）。 
  4. 使用 Spring [注册您的新工件](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#xsd-custom-registration).
* 创建配置格式的基于模式的方法允许与具有模式感知 XML 编辑器的 IDE 紧密集成。通过使用正确编写的模式，您可以使用自动完成功能让用户在枚举中定义的多个配置选项之间进行选择。
* 除了模式之外，我们还需要NamespaceHandler解析 Spring 在解析配置文件时遇到的这个特定命名空间的所有元素。对于这个例子， NamespaceHandler应该负责myns:dateformat 元素的解析。 该NamespaceHandler接口具有三种方法： 
  * init(): 允许NamespaceHandler在使用处理程序之前由 Spring 调用 and 的初始化。 
  * BeanDefinition parse(Element, ParserContext)：当 Spring 遇到顶级元素（未嵌套在 bean 定义或不同的命名空间内）时调用。此方法本身可以注册 bean 定义、返回 bean 定义或两者兼而有之。 
  * BeanDefinitionHolder decorate(Node, BeanDefinitionHolder, ParserContext)：当 Spring 遇到不同命名空间的属性或嵌套元素时调用。一个或多个 bean 定义的装饰（例如）与 [Spring 支持的范围](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes) 一起使用。我们首先突出一个简单的例子，不使用装饰，然后我们在一个更高级的例子中展示装饰。
* “普通”元素的自定义属性: 编写您自己的自定义解析器和相关的工件并不难。但是，有时这不是正确的做法。考虑一个场景，您需要将元数据添加到已经存在的 bean 定义中。在这种情况下，您当然不想编写自己的整个自定义扩展。相反，您只想向现有的 bean 定义元素添加一个附加属性。
* [应用程序启动步骤](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#application-startup-steps): 附录的这一部分列出了StartupSteps核心容器被检测的现有内容。每个启动步骤的名称和详细信息不是公共合同的一部分，可能会发生变化；这被认为是核心容器的实现细节，并将跟随其行为变化。
* 本章介绍 Spring 对集成测试的支持和单元测试的最佳实践。**Spring 团队提倡测试驱动开发 (TDD)。**Spring 团队发现，正确使用控制反转 (IoC) 确实确实使单元测试和集成测试更容易（因为类上存在 setter 方法和适当的构造函数使它们更容易在测试中连接在一起，而不必建立服务定位器注册表和类似结构）。与传统 Java EE 开发相比，依赖注入应该使您的代码对容器的依赖更少。组成应用程序的 POJO 应该可以在 JUnit 或 TestNG 测试中进行测试，使用new 操作符实例化对象，无需 Spring 或任何其他容器。您可以使用 [模拟对象](https://docs.spring.io/spring-framework/docs/current/reference/html/testing.html#mock-objects) （结合其他有价值的测试技术）来单独测试您的代码。如果您遵循 Spring 的架构建议，那么代码库的干净分层和组件化将有助于更轻松的单元测试。例如，您可以通过存根或模拟 DAO 或存储库接口来测试服务层对象，而无需在运行单元测试时访问持久数据。 真正的单元测试通常运行得非常快，因为不需要设置运行时基础设施。强调真正的单元测试作为开发方法的一部分可以提高您的生产力。您可能不需要测试章节的这一部分来帮助您为基于 IoC 的应用程序编写有效的单元测试。然而，对于某些单元测试场景，Spring 框架提供了模拟对象和测试支持类，本章将对此进行介绍。
* 该org.springframework.test.util软件包包含几个用于单元和集成测试的通用实用程序。 **ReflectionTestUtils 是基于反射的实用方法的集合。您可以在需要更改常量值、设置非 public 字段、调用非public setter 方法或 public 在测试应用程序代码等用例时调用非配置或生命周期回调方法的测试场景中使用这些方法如下**： 
  * ORM 框架（例如 JPA 和 Hibernate）允许private或protected字段访问，而不是public域实体中属性的 setter 方法。 
  * Spring 对注解（例如 、 和 ）的支持，这些注解@Autowired为@Injector字段、setter 方法和配置方法@Resource提供依赖注入。private protected 
  * @PostConstruct使用注解，例如@PreDestroy生命周期回调方法。
* [AopTestUtils](https://docs.spring.io/spring-framework/docs/5.3.18/javadoc-api/org/springframework/test/util/AopTestUtils.html) 是 AOP 相关实用方法的集合。您可以使用这些方法来获取对隐藏在一个或多个 Spring 代理后面的底层目标对象的引用。例如，如果您使用 EasyMock 或 Mockito 等库将 bean 配置为动态模拟，并且模拟被包装在 Spring 代理中，您可能需要直接访问底层模拟以对其配置期望并执行验证. 有关 Spring 的核心 AOP 实用程序，请参阅 [AopUtils](https://docs.spring.io/spring-framework/docs/5.3.18/javadoc-api/org/springframework/aop/support/AopUtils.html) 和 [AopProxyUtils](https://docs.spring.io/spring-framework/docs/5.3.18/javadoc-api/org/springframework/aop/framework/AopProxyUtils.html).
* 单元测试 Spring MVC 控制器: 要将 Spring MVCController类作为 POJO 进行单元测试，请使用Spring 的 [Servlet API 模拟](https://docs.spring.io/spring-framework/docs/current/reference/html/testing.html#mock-objects-servlet) ModelAndViewAssert中的 with MockHttpServletRequest、等组合。 要结合 Spring MVC 的配置对 Spring MVC 和 REST类进行彻底的集成测试，请改用 [Spring MVC 测试框架](https://docs.spring.io/spring-framework/docs/current/reference/html/testing.html#spring-mvc-test-framework). MockHttpSession Controller WebApplicationContext
* spring-testSpring Framework 为模块中的集成测试提供了一流的支持 。实际 JAR 文件的名称可能包括发布版本，也可能是长org.springframework.test格式，具体取决于您从何处获取它（有关说明，请参阅 [依赖管理部分](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#dependency-management) ）。这个库包括org.springframework.test包，其中包含用于与 Spring 容器进行集成测试的有价值的类。此测试不依赖于应用程序服务器或其他部署环境。这样的测试运行起来比单元测试慢，但比等效的 [Selenium](https://github.com/SeleniumHQ/selenium) 测试或依赖部署到应用程序服务器的远程测试快得多。
* Spring TestContext Framework 提供了 Spring ApplicationContext实例和WebApplicationContext实例的一致加载以及这些上下文的缓存。支持加载上下文的缓存很重要，因为启动时间可能会成为一个问题——不是因为 Spring 本身的开销，而是因为 Spring 容器实例化的对象需要时间来实例化。例如，具有 50 到 100 个 Hibernate 映射文件的项目可能需要 10 到 20 秒来加载映射文件，并且在每个测试夹具中运行每个测试之前产生该成本会导致整体测试运行速度变慢，从而降低开发人员的工作效率。 测试类通常声明一组用于 XML 或 Groovy 配置元数据的资源位置（通常在类路径中）或一组用于配置应用程序的组件类。web.xml这些位置或类与生产部署的其他配置文件中指定的位置或类相同或相似。 默认情况下，一旦加载，配置ApplicationContext就会被重复用于每个测试。因此，每个测试套件只产生一次设置成本，随后的测试执行速度要快得多。在这种情况下，术语“测试套件”意味着所有测试都在同一个 JVM 中运行——例如，所有测试都从给定项目或模块的 Ant、Maven 或 Gradle 构建中运行。在不太可能的情况下，测试破坏了应用程序上下文并需要重新加载（例如，通过修改 bean 定义或应用程序对象的状态），可以将 TestContext 框架配置为在执行下一个之前重新加载配置并重建应用程序上下文测试。
* 访问真实数据库的测试中的一个常见问题是它们对持久存储状态的影响。即使您使用开发数据库，状态更改也可能会影响未来的测试。此外，许多操作——例如插入或修改持久数据——不能在事务之外执行（或验证）。 TestContext 框架解决了这个问题。默认情况下，框架会为每个测试创建并回滚一个事务。您可以编写可以假设事务存在的代码。如果您在测试中调用事务代理对象，它们会根据其配置的事务语义正确运行。此外，如果测试方法在为测试管理的事务中运行时删除了选定表的内容，则事务默认回滚，并且数据库返回到执行测试之前的状态。PlatformTransactionManager通过使用在测试的应用程序上下文中定义的 bean向测试提供事务支持。 [@Commit](https://docs.spring.io/spring-framework/docs/current/reference/html/testing.html#integration-testing-annotations) 如果您希望提交事务（不寻常，但在您希望特定测试填充或修改数据库时偶尔有用），您可以使用注释告诉 TestContext 框架导致事务提交而不是回滚 。
* 该org.springframework.test.jdbc软件包包含JdbcTestUtilsJDBC 相关实用函数的集合，旨在简化标准数据库测试场景。具体来说，JdbcTestUtils提供以下静态实用程序方法。
  * countRowsInTable(..)：计算给定表中的行数。
  * countRowsInTableWhere(..)WHERE：使用提供的子句计算给定表中的行数。
  * deleteFromTables(..)：从指定的表中删除所有行。
  * deleteFromTableWhere(..)：使用提供的 WHERE子句从给定表中删除行。
  * dropTables(..)：删除指定的表。
* [AbstractTransactionalJUnit4SpringContextTests](https://docs.spring.io/spring-framework/docs/current/reference/html/testing.html#testcontext-support-classes-junit4) 并 [AbstractTransactionalTestNGSpringContextTests](https://docs.spring.io/spring-framework/docs/current/reference/html/testing.html#testcontext-support-classes-testng) 提供方便的方法来委托给 JdbcTestUtils. 该 spring-jdbc 模块为配置和启动嵌入式数据库提供支持，您可以在与数据库交互的集成测试中使用它。有关详细信息，请参阅 [嵌入式数据库支持](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#jdbc-embedded-database-support) 和 [使用嵌入式数据库测试数据访问逻辑](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#jdbc-embedded-database-dao-testing).
* JSR-250 生命周期注解: @PostConstruct在 Spring TestContext Framework 中，您可以@PreDestroy在ApplicationContext. 但是，这些生命周期注释在实际测试类中的用途有限。 如果测试类中的方法使用 注释@PostConstruct，则该方法在底层测试框架的任何之前的方法之前运行（例如，使用 JUnit Jupiter 注释的方法@BeforeEach），并且适用于测试类中的每个测试方法。另一方面，如果测试类中的方法用 注释 @PreDestroy，则该方法永远不会运行。因此，在测试类中，我们建议您使用来自底层测试框架的测试生命周期回调，而不是 @PostConstructand @PreDestroy。
* 更改默认测试构造函数自动装配模式: 可以通过将 JVM 系统属性设置为 来更改默认的测试构造函数自动装配模式。或者，可以通过该 机制设置默认模式。spring.test.constructor.autowire.mode all [SpringProperties](https://docs.spring.io/spring-framework/docs/current/reference/html/appendix.html#appendix-spring-properties). 从 Spring Framework 5.3 开始，默认模式也可以配置为 [JUnit Platform 配置参数](https://junit.org/junit5/docs/current/user-guide/#running-tests-config-params). 如果spring.test.constructor.autowire.mode 未设置该属性，则不会自动自动装配测试类构造函数。
* 从 Spring Framework 5.2 开始，@TestConstructor仅支持SpringExtension与 JUnit Jupiter 一起使用。请注意，SpringExtension通常会自动为您注册 - 例如，当使用 Spring Boot Test 中的@SpringJUnitConfig和@SpringJUnitWebConfig/或各种与测试相关的注释等注释时。
* Spring TestContext 框架（位于org.springframework.test.context 包中）提供通用的、注释驱动的单元和集成测试支持，与正在使用的测试框架无关。TestContext 框架还非常重视约定而不是配置，具有合理的默认值，您可以通过基于注释的配置覆盖这些默认值。 除了通用测试基础架构之外，TestContext 框架还为 JUnit 4、JUnit Jupiter (AKA JUnit 5) 和 TestNG 提供了明确的支持。对于 JUnit 4 和 TestNG，Spring 提供了abstract支持类。此外，Spring为 JUnit 4 和JUnit Jupiter 提供了自定义 JUnitRunner和自定义 JUnit ，让您可以编写所谓的 POJO 测试类。POJO 测试类不需要扩展特定的类层次结构，例如支持类。Rules Extension abstract
* 从 Spring Framework 5.2 开始，@TestPropertySource可以用作可重复的注解。这意味着您可以@TestPropertySource在单个测试类上拥有多个声明，locations并且properties后面的@TestPropertySource 注释会覆盖之前的@TestPropertySource注释。 此外，您可以在一个测试类上声明多个组合注释，每个都使用 元注释@TestPropertySource，所有这些@TestPropertySource 声明都将有助于您的测试属性源。 直接呈现@TestPropertySource的注释总是优先于元呈现的@TestPropertySource注释。换句话说，locationsand propertiesfrom 直接存在的@TestPropertySource注解将覆盖 locationsand propertiesfrom@TestPropertySource用作元注解的注解。
* 测试套件和分叉流程: Spring TestContext 框架将应用程序上下文存储在静态缓存中。这意味着上下文实际上存储在一个static变量中。换句话说，如果测试在不同的进程中运行，则在每次测试执行之间都会清除静态缓存，从而有效地禁用缓存机制。 要从缓存机制中受益，所有测试都必须在同一进程或测试套件中运行。这可以通过在 IDE 中作为一个组执行所有测试来实现。同样，在使用 Ant、Maven 或 Gradle 等构建框架执行测试时，确保构建框架不会在测试之间分叉很重要。例如，如果 [forkMode](https://maven.apache.org/plugins/maven-surefire-plugin/test-mojo.html#forkMode) Maven Surefire 插件的 设置为always或pertest，则 TestContext 框架无法缓存测试类之间的应用程序上下文，从而导致构建过程运行速度明显变慢。
* ApplicationContext 生命周期和控制台日志记录: 当您需要调试使用 Spring TestContext Framework 执行的测试时，分析控制台输出（即输出到SYSOUT和SYSERR 流）会很有用。一些构建工具和 IDE 能够将控制台输出与给定的测试相关联；然而，一些控制台输出不能轻易地与给定的测试相关联。 关于由 Spring Framework 本身或在 中注册的组件触发的控制台日志记录ApplicationContext，了解 ApplicationContextSpring TestContext Framework 在测试套件中加载的生命周期非常重要。 ApplicationContext通常在准备测试类的实例时加载 for 测试 - 例如，将依赖项注入到测试@Autowired 实例的字段中。这意味着在初始化期间触发的任何控制台日志记录ApplicationContext通常不能与单个测试方法相关联。但是，如果根据@DirtiesContext 语义在执行测试方法之前立即关闭上下文，则将在执行测试方法之前加载上下文的新实例。在后一种情况下，IDE 或构建工具可能会将控制台日志记录与单个测试方法相关联。 ApplicationContext测试可以通过以下场景之一关闭。 上下文根据 [@DirtiesContext](https://docs.spring.io/spring-framework/docs/current/reference/html/testing.html#spring-testing-annotation-dirtiescontext) 语义关闭。 上下文已关闭，因为它已根据 LRU 逐出策略自动从缓存中逐出。 当测试套件的 JVM 终止时，上下文通过 JVM 关闭挂钩关闭。 如果在特定测试方法之后根据@DirtiesContext语义关闭上下文，则 IDE 或构建工具可能会将控制台日志记录与单个测试方法相关联。如果在测试类之后根据@DirtiesContext语义关闭上下文，则在关闭期间触发的任何控制台日志记录 ApplicationContext都不能与单个测试方法相关联。同样，在关闭阶段通过 JVM 关闭挂钩触发的任何控制台日志记录都不能与单个测试方法相关联。 当ApplicationContext通过 JVM 关闭挂钩关闭 Spring 时，在关闭阶段执行的回调将在名为SpringContextShutdownHook. 因此，如果您希望禁用ApplicationContext通过 JVM 关闭挂钩关闭时触发的控制台日志记录，您可以使用您的日志记录框架注册一个自定义过滤器，允许您忽略该线程启动的任何日志记录。
* 对于 JUnit Jupiter 以外的测试框架，TestContext 框架不参与测试类的实例化。因此，对构造函数使用@Autowiredor @Inject对测试类没有影响。
* 尽管在生产代码中不鼓励字段注入，但在测试代码中字段注入实际上是很自然的。差异的基本原理是您永远不会直接实例化您的测试类。因此，无需能够public在您的测试类上调用构造函数或 setter 方法。
* 抢先超时和测试管理事务: 在将测试框架中的任何形式的抢占式超时与 Spring 的测试管理事务结合使用时，必须小心。 具体来说，Spring 的测试支持在调用当前测试方法之前将事务状态绑定到当前线程（通过java.lang.ThreadLocal变量） 。如果测试框架在新线程中调用当前测试方法以支持抢占式超时，则在当前测试方法中执行的任何操作都不会在测试管理事务中调用。因此，任何此类操作的结果都不会随测试管理的事务回滚。相反，即使测试管理的事务已被 Spring 正确回滚，此类操作仍将提交到持久存储（例如，关系数据库）。 可能发生这种情况的情况包括但不限于以下情况。
  * JUnit 4 的@Test(timeout = …)支持和TimeOut规则
  * JUnit Jupiter类assertTimeoutPreemptively(…)中的方法 org.junit.jupiter.api.Assertions
  * TestNG 的@Test(timeOut = …)支持
* 方法级别的生命周期方法——例如，使用 JUnit Jupiter's @BeforeEachor注释的方法——@AfterEach在测试管理的事务中运行。另一方面，套件级和类级生命周期方法——例如，使用 JUnit Jupiter 的@BeforeAllor@AfterAll注释的方法和使用 TestNG 的 @BeforeSuite、@AfterSuite、@BeforeClass或注释的方法@AfterClass——不在测试管理的事务中运行。 如果您需要在事务中的套件级或类级生命周期方法中运行代码，您可能希望将相应的注入PlatformTransactionManager到您的测试类中，然后将其与TransactionTemplate编程事务管理一起使用。
* 有时，您可能需要在事务测试方法之前或之后但在事务上下文之外运行某些代码 - 例如，在运行测试之前验证初始数据库状态或在测试运行后验证预期的事务提交行为（如果test 被配置为提交事务）。 TransactionalTestExecutionListener完全支持此类场景的@BeforeTransaction和 @AfterTransaction注释。您可以使用这些注释之一来注释void 测试类中的任何方法或void测试接口中的任何默认方法，并TransactionalTestExecutionListener确保您的事务前方法或事务后方法在适当的时间运行。任何 before 方法（例如使用 JUnit Jupiter's 注释的方法@BeforeEach）和任何 after 方法（例如使用 JUnit Jupiter's 注释的方法@AfterEach）都在事务中运行。此外，对于未配置为在事务中运行的测试方法，使用注释@BeforeTransaction或 @AfterTransaction不运行的方法。
* 除了上述以编程方式运行 SQL 脚本的机制之外，您还可以在 Spring TestContext Framework 中以声明方式配置 SQL 脚本。具体来说，您可以@Sql在测试类或测试方法上声明注释，以配置单个 SQL 语句或 SQL 脚本的资源路径，这些脚本应在集成测试方法之前或之后针对给定数据库运行。支持 @Sql由 提供SqlScriptsTestExecutionListener，默认情况下启用。默认情况下，方法级别的@Sql声明会覆盖类级别的声明。但是，从 Spring Framework 5.2 开始，可以通过每个测试类或每个测试方法配置此行为@SqlMergeMode。有关详细信息，请参阅 合并和覆盖配置@SqlMergeMode。
* Spring Framework 5.0 引入了在使用 Spring TestContext Framework 时在单个 JVM 中并行执行测试的基本支持。一般来说，这意味着大多数测试类或测试方法可以并行运行，而无需对测试代码或配置进行任何更改。有关如何设置并行测试执行的详细信息，请参阅测试框架、构建工具或 IDE 的文档。
* Spring TestContext 框架通过自定义运行程序（在 JUnit 4.12 或更高版本上受支持）提供与 JUnit 4 的完全集成。通过使用 @RunWith(SpringJUnit4ClassRunner.class)或更短的@RunWith(SpringRunner.class) 变体注释测试类，开发人员可以实现标准的基于 JUnit 4 的单元和集成测试，同时获得 TestContext 框架的好处，例如支持加载应用程序上下文、测试实例的依赖注入、事务测试方法执行， 等等。如果您想将 Spring TestContext Framework 与替代运行程序（例如 JUnit 4 的Parameterized运行程序）或第三方运行程序（例如MockitoJUnitRunner）一起使用，您可以选择使用 [Spring 对 JUnit 规则的支持](https://docs.spring.io/spring-framework/docs/current/reference/html/testing.html#testcontext-junit4-rules).
* 如果测试类的构造函数被认为是可自动装配的，那么 Spring 将负责解析构造函数中所有参数的参数。因此，ParameterResolver在 JUnit Jupiter 中注册的其他任何人都无法解析此类构造函数的参数。如果用于关闭测试之前或之后的测试方法，则测试类的构造函数注入不能与 JUnit Jupiter 的@TestInstance(PER_CLASS)支持结合使用。@DirtiesContext ApplicationContext.原因是@TestInstance(PER_CLASS)指示 JUnit Jupiter 在测试方法调用之间缓存测试实例。因此，测试实例将保留对最初从ApplicationContext随后关闭的 bean 注入的 bean 的引用。由于在这种情况下测试类的构造函数只会被调用一次，因此不会再次发生依赖注入，后续测试会从关闭状态与bean交互ApplicationContext，可能会导致错误。 要@DirtiesContext与“测试方法之前”或“测试方法之后”模式结合使用@TestInstance(PER_CLASS)，必须配置来自 Spring 的依赖项以通过字段或设置器注入提供，以便它们可以在测试方法调用之间重新注入。
* WebTestClient是为测试服务器应用程序而设计的 HTTP 客户端。它包装了 Spring 的 [WebClient](https://docs.spring.io/spring-framework/docs/current/reference/html/web-reactive.html#webflux-client) 并使用它来执行请求，但公开了一个测试外观来验证响应。WebTestClient可用于执行端到端 HTTP 测试。它还可以用于通过模拟服务器请求和响应对象在没有运行服务器的情况下测试 Spring MVC 和 Spring WebFlux 应用程序。
* Spring MVC 测试框架，也称为 MockMvc，为测试 Spring MVC 应用程序提供支持。它执行完整的 Spring MVC 请求处理，但通过模拟请求和响应对象而不是正在运行的服务器。 MockMvc 可以单独用于执行请求和验证响应。它也可以通过 [WebTestClient](https://docs.spring.io/spring-framework/docs/current/reference/html/testing.html#webtestclient) 使用，其中 MockMvc 被插入作为服务器来处理请求。其优点WebTestClient是可以选择使用更高级别的对象而不是原始数据，以及切换到针对实时服务器的完整端到端 HTTP 测试并使用相同测试 API 的能力。
* MockMvc 与端到端测试: MockMVc 建立在 spring-test模块中的 Servlet API 模拟实现之上，不依赖于正在运行的容器。因此，与使用实际客户端和实时服务器运行的完整端到端集成测试相比，存在一些差异。 考虑这一点的最简单方法是从空白开始MockHttpServletRequest。无论您添加什么，请求都会变成什么。可能会让你大吃一惊的是，默认情况下没有上下文路径；没有jsessionid饼干；没有转发、错误或异步调度；因此，没有实际的 JSP 渲染。相反，“转发”和“重定向”的 URL 保存在 中，MockHttpServletResponse并且可以根据预期进行断言。 这意味着，如果您使用 JSP，您可以验证请求被转发到的 JSP 页面，但不会呈现 HTML。换句话说，不调用 JSP。但是请注意，所有其他不依赖转发的呈现技术，例如 Thymeleaf 和 Freemarker，都会按预期将 HTML 呈现到响应正文。通过方法渲染 JSON、XML 等格式也是如此@ResponseBody。 或者，您可以考虑使用 Spring Boot 提供的完整端到端集成测试支持@SpringBootTest。请参阅 [Spring Boot 参考指南](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-testing). 每种方法都有优点和缺点。Spring MVC Test 中提供的选项是从经典单元测试到完全集成测试的不同阶段。可以肯定的是，Spring MVC Test 中没有一个选项属于经典单元测试的范畴，但它们更接近它。例如，您可以通过将模拟服务注入控制器来隔离 Web 层，在这种情况下，您仅通过DispatcherServlet实际的 Spring 配置来测试 Web 层，因为您可能会与上面的层隔离地测试数据访问层。此外，您可以使用独立设置，一次只关注一个控制器，然后手动提供使其工作所需的配置。 使用 Spring MVC 测试时的另一个重要区别是，从概念上讲，此类测试是服务器端的，因此您可以检查使用了哪些处理程序，是否使用 HandlerExceptionResolver 处理了异常，模型的内容是什么，绑定错误是什么有，和其他细节。这意味着更容易编写期望，因为服务器不是一个不透明的盒子，就像通过实际的 HTTP 客户端测试它时那样。这通常是经典单元测试的一个优势：它更容易编写、推理和调试，但不能取代完全集成测试的需要。同时，重要的是不要忽视响应是最重要的检查这一事实。简而言之，即使在同一个项目中，这里也有多种风格和测试策略的空间。
* 那么，我们如何在测试页面交互和仍然在测试套件中保持良好性能之间取得平衡呢？答案是：“通过将 MockMvc 与 HtmlUnit 集成。”
* 为什么选择 Geb 和 MockMvc？ Geb 由 WebDriver 提供支持，因此它提供了许多与 我们从 WebDriver 获得的相同的好处。然而，Geb 为我们处理了一些样板代码，使事情变得更加容易。
* 全面的事务支持是使用 Spring 框架的最令人信服的理由之一。Spring 框架为事务管理提供了一致的抽象，具有以下好处：
  * 跨不同事务 API 的一致编程模型，例如 Java Transaction API (JTA)、JDBC、Hibernate 和 Java Persistence API (JPA)。
  * 支持 [声明式事务管理](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#transaction-declarative).
  * 用于 [编程](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#transaction-programmatic) 事务管理的 API 比复杂事务 API（如 JTA）更简单。
  * 与 Spring 的数据访问抽象完美集成。
* Spring 解决了全局和本地事务的缺点。它允许应用程序开发人员在任何环境中使用一致的编程模型。您只需编写一次代码，它就可以从不同环境中的不同事务管理策略中受益。Spring Framework 提供声明式和编程式事务管理。大多数用户更喜欢声明式事务管理，我们在大多数情况下都推荐这种方式。 通过程序化事务管理，开发人员可以使用 Spring Framework 事务抽象，它可以在任何底层事务基础设施上运行。使用首选的声明性模型，开发人员通常编写很少或根本不编写与事务管理相关的代码，因此不依赖 Spring Framework 事务 API 或任何其他事务 API。
* 您需要应用服务器来进行事务管理吗？Spring Framework 的事务管理支持改变了关于企业 Java 应用程序何时需要应用程序服务器的传统规则。 特别是，您不需要纯粹用于通过 EJB 进行声明性事务的应用程序服务器。事实上，即使您的应用程序服务器具有强大的 JTA 功能，您也可能会认为 Spring Framework 的声明式事务提供了比 EJB CMT 更强大的编程模型和更高效的编程模型。 通常，只有当您的应用程序需要处理跨多个资源的事务时，您才需要应用程序服务器的 JTA 功能，而这对许多应用程序来说不是必需的。许多高端应用程序使用单一的、高度可扩展的数据库（例如 Oracle RAC）。独立的事务管理器（例如 [Atomikos Transactions](https://www.atomikos.com/) 和 [JOTM](http://jotm.objectweb.org/) ）是其他选项。当然，您可能需要其他应用服务器功能，例如 Java 消息服务 (JMS) 和 Java EE 连接器架构 (JCA)。 Spring 框架让您可以选择何时将应用程序扩展到完全加载的应用程序服务器。使用 EJB CMT 或 JTA 的唯一替代方法是使用本地事务（例如 JDBC 连接上的事务）编写代码并且如果您需要该代码在全局、容器管理的事务中运行则面临大量返工的日子已经一去不复返了。使用 Spring Framework，只需更改配置文件中的一些 bean 定义（而不是您的代码）。
* 如果您使用 JTA，那么无论您使用哪种数据访问技术，无论是 JDBC、Hibernate JPA 还是任何其他受支持的技术，您的事务管理器定义都应该看起来相同。这是因为 JTA 事务是全局事务，它可以征用任何事务资源。在所有 Spring 事务设置中，应用程序代码不需要更改。您可以仅通过更改配置来更改事务的管理方式，即使该更改意味着从本地事务转移到全局事务，反之亦然。
* 大多数 Spring Framework 用户选择声明式事务管理。此选项对应用程序代码的影响最小，因此最符合非侵入式轻量级容器的理想。
* Spring Framework 的声明式事务管理是通过 Spring 面向方面编程 (AOP) 实现的。然而，由于事务方面代码随 Spring Framework 发行版一起提供并且可以以样板方式使用，因此通常不必理解 AOP 概念即可有效地使用此代码。
* @Transactional 仅仅告诉你用注释来注释你的类，添加 @EnableTransactionManagement 到你的配置中，并期望你理解它是如何工作的是不够的 。为了提供更深入的理解，本节将在事务相关问题的上下文中解释 Spring 框架的声明式事务基础结构的内部工作原理。Spring FrameworkTransactionInterceptor为命令式和反应式编程模型提供事务管理。拦截器通过检查方法返回类型来检测所需的事务管理风格。返回响应式类型的方法，例如PublisherKotlin Flow（或其子类型）有资格进行响应式事务管理。所有其他返回类型，包括void使用代码路径进行命令式事务管理。下图显示了在事务代理上调用方法的概念视图：
  * ![](https://docs.spring.io/spring-framework/docs/current/reference/html/images/tx.png)
* 回滚规则: 回滚规则确定在抛出给定异常时是否应回滚事务，并且规则基于模式。模式可以是完全限定的类名或异常类型的完全限定类名的子字符串（必须是 的子类Throwable），目前不支持通配符。例如， "javax.servlet.ServletException"or的值"ServletException"将匹配 javax.servlet.ServletException及其子类。 回滚规则可以通过rollback-for和no-rollback-for 属性在 XML 中配置，允许将模式指定为字符串。使用 [@Transactional](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#transaction-declarative-attransactional-settings) 时 ，可以通过rollbackFor/noRollbackFor和 rollbackForClassName/noRollbackForClassName属性配置回滚规则，它们允许将模式分别指定为Class引用或字符串。当异常类型被指定为类引用时，其完全限定名称将用作模式。因此，@Transactional(rollbackFor = example.CustomException.class)等价于@Transactional(rollbackForClassName = "example.CustomException")。 您必须仔细考虑模式的具体程度以及是否包含包信息（这不是强制性的）。例如，"Exception"将匹配几乎所有内容，并且可能会隐藏其他规则。"java.lang.Exception"如果 "Exception"旨在为所有检查的异常定义规则，那将是正确的。对于更独特的异常名称，例如"BaseBusinessException"可能不需要为异常模式使用完全限定的类名。 此外，回滚规则可能会导致对类似命名的异常和嵌套类的无意匹配。这是因为如果抛出的异常的名称包含为回滚规则配置的异常模式，则抛出的异常被认为与给定的回滚规则匹配。例如，给定一个配置为匹配 on 的com.example.CustomException规则，该规则将匹配名为com.example.CustomExceptionV2的异常（与同一包中的异常 CustomException但带有附加后缀）或名为 com.example.CustomException$AnotherException的异常（声明为嵌套类的异常CustomException）。
* 您还可以以编程方式指示所需的回滚。虽然简单，但这个过程非常具有侵入性，并且将您的代码与 Spring Framework 的事务基础结构紧密耦合。如果可能的话，强烈建议您使用声明性方法进行回滚。如果您绝对需要，可以使用程序化回滚，但它的使用与实现干净的基于 POJO 的架构背道而驰。
* 方法可见性和@Transactional: 当您在 Spring 的标准配置中使用事务代理时，您应该@Transactional只将注解应用于具有public可见性的方法。如果您使用注释对protected、private或包可见的方法进行@Transactional 注释，则不会引发错误，但带注释的方法不会显示配置的事务设置。如果您需要注释非公共方法，请考虑下一段中针对基于类的代理的提示，或者考虑使用 AspectJ 编译时或加载时编织（稍后描述）。 @EnableTransactionManagement在@Configuration类中使用时，protected也可以通过注册自定义transactionAttributeSourcebean 来使基于类的代理的包可见方法具有事务性，如下例所示。但是请注意，基于接口的代理中的事务方法必须始终 public在代理接口中定义。
* **Spring 团队建议您只使用注解来注解具体类（和具体类的方法）@Transactional，而不是注解接口。**您当然可以将@Transactional注释放在接口（或接口方法）上，但这仅适用于您使用基于接口的代理时的预期。Java 注释不是从接口继承的事实意味着，如果您使用基于类的代理 ( proxy-target-class="true") 或基于编织的方面 ( mode="aspectj")，则代理和编织基础架构无法识别事务设置，并且不会包装对象在事务代理中。
* 在代理模式下（默认），只有通过代理传入的外部方法调用会被拦截。这意味着自调用（实际上，目标对象中的一个方法调用目标对象的另一个方法）不会导致运行时的实际事务，即使调用的方法被标记为@Transactional。此外，代理必须完全初始化以提供预期的行为，因此您不应在初始化代码中依赖此功能 - 例如，在@PostConstruct 方法中。
* 当您使用此方面时，您必须注释实现类（或该类中的方法或两者），而不是该类实现的接口（如果有）。AspectJ 遵循 Java 的规则，即不继承接口上的注释。
* 从 Spring 4.2 开始，事件的监听器可以绑定到事务的一个阶段。典型的例子是在事务成功完成时处理事件。当当前事务的结果实际上对侦听器很重要时，这样做可以更灵活地使用事件。 您可以使用@EventListener注解注册常规事件侦听器。如果您需要将其绑定到事务，请使用@TransactionalEventListener. 当您这样做时，默认情况下侦听器绑定到事务的提交阶段。
* Spring 通过DataSource. ADataSource是 JDBC 规范的一部分，是一个通用的连接工厂。它允许容器或框架从应用程序代码中隐藏连接池和事务管理问题。作为开发人员，您无需了解有关如何连接到数据库的详细信息。这是设置数据源的管理员的责任。在开发和测试代码时，您很可能同时担任这两个角色，但您不必知道生产数据源的配置方式。 使用 Spring 的 JDBC 层时，可以从 JNDI 获取数据源，也可以通过第三方提供的连接池实现来配置自己的数据源。传统的选择是带有 bean 样式DataSource类的 Apache Commons DBCP 和 C3P0；对于现代 JDBC 连接池，请考虑使用 HikariCP 及其构建器风格的 API。
* 您应该仅将DriverManagerDataSourceandSimpleDriverDataSource类（包含在 Spring 发行版中）用于测试目的！当对连接进行多个请求时，这些变体不提供池化并且性能很差。
* 如果您使用的数据库不是 Spring 支持的数据库，则需要显式声明。目前，Spring 支持以下数据库的存储过程调用的元数据查找：Apache Derby、DB2、MySQL、Microsoft SQL Server、Oracle 和 Sybase。我们还支持 MySQL、Microsoft SQL Server 和 Oracle 存储函数的元数据查找。
* SQL 标准允许基于包含变量值列表的表达式来选择行。一个典型的例子是select * from T_ACTOR where id in (1, 2, 3). JDBC 标准不直接支持此变量列表用于准备好的语句。您不能声明可变数量的占位符。您需要准备好所需数量的占位符的多种变体，或者您需要在知道需要多少占位符后动态生成 SQL 字符串。NamedParameterJdbcTemplate和中提供的命名参数支持JdbcTemplate采用后一种方法。您可以将值作为java.util.List原始对象传入。此列表用于在语句执行期间插入所需的占位符并传入值。	传入许多值时要小心。JDBC 标准不保证in表达式列表可以使用超过 100 个值。各种数据库都超过了这个数字，但它们通常对允许的值的数量有硬性限制。例如，Oracle 的限制是 1000。
* 该 org.springframework.jdbc.datasource.embedded 包提供对嵌入式 Java 数据库引擎的支持。本机提供对 [HSQL](http://www.hsqldb.org/), [H2](https://www.h2database.com/) 和 [Derby](https://db.apache.org/derby) 的支持。您还可以使用可扩展的 API 来插入新的嵌入式数据库类型和 DataSource实现。由于其轻量级的特性，嵌入式数据库在项目的开发阶段非常有用。好处包括易于配置、快速启动时间、可测试性以及在开发过程中快速发展 SQL 的能力。
* 怎么样null？关系数据库结果可以包含null值。Reactive Streams 规范禁止发射null值。该要求要求正确null处理提取器功能。虽然您可以null从 a 获取值Row，但不能发出null 值。您必须将任何值包装null在对象中（例如，Optional 对于奇异值），以确保null提取器函数永远不会直接返回值。What about null?  Relational database results can contain null values. The Reactive Streams specification forbids the emission of null values. That requirement mandates proper null handling in the extractor function. While you can obtain null values from a Row, you must not emit a null value. You must wrap any null values in an object (for example, Optional for singular values) to make sure a null value is never returned directly by your extractor function.
* Spring 框架支持与 Java Persistence API (JPA) 的集成，并支持本地 Hibernate 进行资源管理、数据访问对象 (DAO) 实现和事务策略。例如，对于 Hibernate，有一流的支持和几个方便的 IoC 功能，可以解决许多典型的 Hibernate 集成问题。您可以通过依赖注入为 OR（对象关系）映射工具配置所有支持的功能。它们可以参与 Spring 的资源和事务管理，并遵守 Spring 的通用事务和 DAO 异常层次结构。推荐的集成方式是针对普通的 Hibernate 或 JPA API 编写 DAO。 当您创建数据访问应用程序时，Spring 为您选择的 ORM 层添加了显着的增强功能。您可以根据需要利用尽可能多的集成支持，并且应该将这种集成工作与在内部构建类似基础架构的成本和风险进行比较。无论采用何种技术，您都可以像使用库一样使用大部分 ORM 支持，因为一切都被设计为一组可重用的 JavaBean。Spring IoC 容器中的 ORM 有助于配置和部署。因此，本节中的大多数示例都显示了 Spring 容器内的配置。 使用 Spring 框架创建 ORM DAO 的好处包括：
  * **更容易测试**。Spring 的 IoC 方法可以轻松交换 HibernateSessionFactory实例、JDBCDataSource 实例、事务管理器和映射对象实现（如果需要）的实现和配置位置。这反过来又使得单独测试每段与持久性相关的代码变得更加容易。
  * **常见的数据访问异常**。Spring 可以包装 ORM 工具中的异常，将它们从专有（可能检查的）异常转换为公共运行时 DataAccessException层次结构。此功能使您可以处理大多数不可恢复的持久性异常，仅在适当的层中，而无需烦人的样板捕获、抛出和异常声明。您仍然可以根据需要捕获和处理异常。请记住，JDBC 异常（包括特定于 DB 的方言）也被转换为相同的层次结构，这意味着您可以在一致的编程模型中使用 JDBC 执行一些操作。
  * **通用资源管理**。Spring 应用程序上下文可以处理 HibernateSessionFactory实例、JPAEntityManagerFactory 实例、JDBCDataSource实例和其他相关资源的位置和配置。这使得这些值易于管理和更改。Spring 提供了对持久性资源的高效、简单和安全的处理。例如，使用 Hibernate 的相关代码一般需要使用相同的 Hibernate Session，以确保效率和正确的事务处理。Spring通过 Hibernate暴露电流，可以轻松地Session透明地创建和绑定到当前线程。因此，对于任何本地或 JTA 事务环境，Spring 解决了典型 Hibernate 使用的许多长期问题。SessionSessionFactory
  * **综合交易管理**。@Transactional您可以通过注释或通过在 XML 配置文件中显式配置事务 AOP 建议，使用声明性、面向方面编程 (AOP) 样式的方法拦截器来包装您的 ORM 代码 。在这两种情况下，都会为您处理事务语义和异常处理（回滚等）。如 [资源和事务管理中所述](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#orm-resource-mngmnt), 你还可以互换各种事务管理器，而不影响你的ORM相关代码。例如，您可以在本地事务和 JTA 之间进行交换，在这两种情况下都可以使用相同的完整服务（例如声明性事务）。此外，与 JDBC 相关的代码可以与用于执行 ORM 的代码在事务上完全集成。这对于不适合 ORM（例如批处理和 BLOB 流）但仍需要与 ORM 操作共享公共事务的数据访问很有用。
* 从 Spring Framework 5.3 开始，Spring 需要 Hibernate ORM 5.2+ 用于 Spring HibernateJpaVendorAdapter以及原生 HibernateSessionFactory设置。对于新启动的应用程序，强烈建议使用 Hibernate ORM 5.4。要与 一起使用HibernateJpaVendorAdapter，Hibernate Search 需要升级到 5.11.6。
* 什么时候需要加载时间编织？ 并非所有 JPA 提供程序都需要 JVM 代理。Hibernate 就是一个不这样做的例子。如果您的提供者不需要代理或您有其他替代方案，例如在构建时通过自定义编译器或 Ant 任务应用增强功能，则不应使用加载时编织器。
* 默认情况下，[Kotlin 中的所有类都是final](https://discuss.kotlinlang.org/t/classes-final-by-default/166). 类上的open修饰符与 Java 的相反final：它允许其他人从该类继承。这也适用于成员函数，因为它们需要被标记为open被覆盖。 虽然 Kotlin 的 JVM 友好设计通常与 Spring 没有摩擦，但如果不考虑这一事实，这个特定的 Kotlin 特性可能会阻止应用程序启动。这是因为 Spring bean（例如由于@Configuration技术原因默认需要在运行时扩展的带注释的类）通常由 CGLIB 代理。open解决方法是在由 CGLIB 代理的 Spring bean 的每个类和成员函数上添加一个关键字，这很快就会变得很痛苦，并且违反了 Kotlin 保持代码简洁和可预测的原则。
* 如果@RequestMapping method未指定该属性，则将匹配所有 HTTP 方法，而不仅仅是GET方法。
* Spring 中动态语言支持的一个（也许是唯一的）最引人注目的增值是“可刷新 bean”特性。 可刷新 bean 是动态语言支持的 bean。通过少量配置，动态语言支持的 bean 可以监视其底层源文件资源的变化，然后在动态语言源文件发生变化时重新加载自己（例如，当您在文件系统）。 这使您可以将任意数量的动态语言源文件部署为应用程序的一部分，配置 Spring 容器以创建由动态语言源文件支持的 bean（使用本章中描述的机制），以及（稍后，随着需求的变化或其他一些外部因素起作用）编辑动态语言源文件，并让他们所做的任何更改反映在由更改的动态语言源文件支持的 bean 中。无需关闭正在运行的应用程序（或在 Web 应用程序的情况下重新部署）。如此修改的动态语言支持的 bean 从更改的动态语言源文件中获取新的状态和逻辑。**此功能默认关闭。**
* 每个 Groovy 源文件不能定义一个以上的类。虽然这在 Groovy 中是完全合法的，但它（可以说）是一种不好的做法。为了获得一致的方法，您应该（在 Spring 团队的意见中）尊重每个源文件一个（公共）类的标准 Java 约定。
* Spring Framework 提供了两种调用 REST 端点的选择：
  * [RestTemplate](https://docs.spring.io/spring-framework/docs/current/reference/html/integration.html#rest-resttemplate): 具有同步模板方法 API 的原始 Spring REST 客户端。
  * [WebClient](https://docs.spring.io/spring-framework/docs/current/reference/html/web-reactive.html#webflux-client): 一种非阻塞、反应式的替代方案，支持同步和异步以及流式场景。
* **从 5.0 开始，RestTemplate它处于维护模式，只有较小的更改请求和错误被接受。请考虑使用 提供更现代 API 并支持同步、异步和流式传输方案 的 [WebClient](https://docs.spring.io/spring-framework/docs/current/reference/html/web-reactive.html#webflux-client).**
* Spring 支持使用各种技术进行远程处理。远程支持简化了远程服务的开发，这些服务通过 Java 接口和对象作为输入和输出来实现。目前，Spring 支持以下远程技术：
  * [Java Web 服务](https://docs.spring.io/spring-framework/docs/current/reference/html/integration.html#remoting-web-services): Spring 通过 JAX-WS 为 Web 服务提供远程支持。
  * [AMQP](https://docs.spring.io/spring-framework/docs/current/reference/html/integration.html#remoting-amqp): 独立的 Spring AMQP 项目支持通过 AMQP 作为底层协议进行远程处理。
* **从 Spring Framework 5.3 开始，出于安全原因和更广泛的行业支持，现在已弃用对多种远程处理技术的支持。支持基础设施将从 Spring Framework 中删除，以用于其下一个主要版本。**
* 远程接口没有实现自动检测。 远程接口不会自动检测已实现接口的主要原因是避免为远程调用者打开太多门。目标对象可能实现内部回调接口，例如InitializingBean不DisposableBean 希望向调用者公开的接口。 在本地情况下，提供具有由目标实现的所有接口的代理通常无关紧要。但是，当您导出远程服务时，您应该公开一个特定的服务接口，以及用于远程使用的特定操作。除了内部回调接口外，目标可能实现多个业务接口，其中只有一个用于远程公开。由于这些原因，我们需要指定这样的服务接口。 这是配置便利性和内部方法意外暴露风险之间的权衡。总是指定一个服务接口并不费力，并且在特定方法的受控公开方面让您处于安全的一边。
* 这里介绍的每一项技术都有其缺点。在选择技术时，您应该仔细考虑您的需求、您公开的服务以及您通过网络发送的对象。 使用 RMI 时，您无法通过 HTTP 协议访问对象，除非您通过隧道传输 RMI 流量。RMI 是一个相当重量级的协议，因为它支持全对象序列化，这在您使用需要通过网络进行序列化的复杂数据模型时非常重要。但是，RMI-JRMP 与 Java 客户端相关联。它是一种 Java 到 Java 的远程解决方案。 如果您需要基于 HTTP 的远程处理但又依赖于 Java 序列化，那么 Spring 的 HTTP 调用程序是一个不错的选择。它与 RMI 调用程序共享基本基础架构，但使用 HTTP 作为传输。请注意，HTTP 调用程序不仅限于 Java 到 Java 远程处理，还包括客户端和服务器端的 Spring。（后者也适用于 Spring 的非 RMI 接口的 RMI 调用程序。） 在异构环境中运行时，Hessian 可能会提供重要的价值，因为它们明确允许非 Java 客户端。但是，非 Java 支持仍然有限。已知问题包括 Hibernate 对象的序列化与延迟初始化的集合相结合。如果您有这样的数据模型，请考虑使用 RMI 或 HTTP 调用程序而不是 Hessian。 JMS 可用于提供服务集群并让 JMS 代理负责负载平衡、发现和自动故障转移。默认情况下，Java 序列化用于 JMS 远程处理，但 JMS 提供者可以使用不同的机制进行有线格式化，例如 XStream 以让服务器在其他技术中实现。 最后但同样重要的是，EJB 优于 RMI，因为它支持标准的基于角色的身份验证和授权以及远程事务传播。也可以让 RMI 调用程序或 HTTP 调用程序支持安全上下文传播，尽管核心 Spring 不提供这一点。Spring 仅提供适当的钩子用于插入第三方或自定义解决方案。
* 请注意由于不安全的 Java 反序列化导致的漏洞：被操纵的输入流可能会导致在反序列化步骤期间在服务器上执行不需要的代码。因此，不要将 HTTP 调用程序端点暴露给不受信任的客户端。相反，仅在您自己的服务之间公开它们。通常，我们强烈建议使用任何其他消息格式（例如 JSON）。 如果您担心 Java 序列化导致的安全漏洞，请考虑核心 JVM 级别的通用序列化过滤机制，该机制最初是为 JDK 9 开发的，但同时向后移植到 JDK 8、7 和 6。请参阅 [https://blogs.oracle.com/java-platform-group/entry/incoming_filter_serialization_data_a](https://blogs.oracle.com/java-platform-group/entry/incoming_filter_serialization_data_a) 和 [https://openjdk.java.net/jeps/290](https://openjdk.java.net/jeps/290).
* 作为轻量级容器，Spring 通常被认为是 EJB 的替代品。我们确实相信，对于许多（如果不是大多数）应用程序和用例，Spring 作为一个容器，结合其在事务、ORM 和 JDBC 访问领域的丰富支持功能，是比通过 EJB 实现等效功能更好的选择容器和 EJB。 但是，重要的是要注意使用 Spring 不会阻止您使用 EJB。事实上，Spring 使得访问 EJB 和在其中实现 EJB 和功能变得更加容易。此外，使用 Spring 访问 EJB 提供的服务允许这些服务的实现稍后在本地 EJB、远程 EJB 或 POJO（普通旧 Java 对象）变体之间透明地切换，而无需更改客户端代码。 在本章中，我们将了解 Spring 如何帮助您访问和实现 EJB。Spring 在访问无状态会话 bean (SLSB) 时提供了特殊的价值，因此我们从讨论这个主题开始。
* 从 Spring Framework 5 开始，Spring 的 JMS 包完全支持 JMS 2.0，并且要求 JMS 2.0 API 在运行时存在。我们建议使用与 JMS 2.0 兼容的提供程序。 如果您碰巧在系统中使用了较旧的消息代理，您可以尝试为现有代理生成升级到与 JMS 2.0 兼容的驱动程序。或者，您也可以尝试针对基于 JMS 1.1 的驱动程序运行，只需将 JMS 2.0 API jar 放在类路径中，但仅针对您的驱动程序使用与 JMS 1.1 兼容的 API。Spring 的 JMS 支持默认遵循 JMS 1.1 约定，因此通过相应的配置，它确实支持这种情况。但是，请仅在过渡场景中考虑这一点。
* 一旦配置，该类的实例JmsTemplate是线程安全的。这很重要，因为这意味着您可以配置 a 的单个实例，JmsTemplate 然后安全地将这个共享引用注入到多个协作者中。需要明确的是，JmsTemplate是有状态的，因为它维护对 a 的引用 ConnectionFactory，但这种状态不是会话状态。
* Spring 中的 JMX（Java 管理扩展）支持提供了一些功能，可让您轻松、透明地将 Spring 应用程序集成到 JMX 基础架构中。JMX？ 本章不是对 JMX 的介绍。它没有试图解释为什么您可能想要使用 JMX。如果您是 JMX 新手，请参阅本章末尾的 [更多资源](https://docs.spring.io/spring-framework/docs/current/reference/html/integration.html#jmx-resources).
* 本节介绍如何使用 Spring Framework 发送电子邮件。库依赖项: 为了使用 Spring Framework 的电子邮件库，以下 JAR 需要位于应用程序的类路径中： JavaMail / [Jakarta Mail 1.6](https://eclipse-ee4j.github.io/mail/) 库 这个库可以在网络上免费获得——例如，在 Maven Central 中作为 com.sun.mail:jakarta.mail. 请确保使用最新的 1.6.x 版本而不是 Jakarta Mail 2.0（带有不同的包命名空间）。
* Spring 包括许多预构建的TaskExecutor. 很可能，您永远不需要实现自己的。Spring提供的变体如下：
  * SyncTaskExecutor：此实现不会异步运行调用。相反，每次调用都发生在调用线程中。它主要用于不需要多线程的情况，例如简单的测试用例。
  * SimpleAsyncTaskExecutor：此实现不重用任何线程。相反，它为每次调用启动一个新线程。但是，它确实支持并发限制，该限制会阻止任何超过限制的调用，直到插槽被释放。如果您正在寻找真正的池化，请参阅ThreadPoolTaskExecutor此列表后面的 。
  * ConcurrentTaskExecutor：这个实现是一个java.util.concurrent.Executor实例的适配器。有一个替代方法 ( ThreadPoolTaskExecutor) 将Executor 配置参数公开为 bean 属性。很少需要 ConcurrentTaskExecutor直接使用。但是，如果ThreadPoolTaskExecutor它对您的需求不够灵活，ConcurrentTaskExecutor则可以选择。
  * ThreadPoolTaskExecutor: 这个实现是最常用的。它公开了用于配置 a 的 bean 属性java.util.concurrent.ThreadPoolExecutor并将其包装在 a 中TaskExecutor。如果您需要适应不同类型的java.util.concurrent.Executor，我们建议您使用 aConcurrentTaskExecutor来代替。
  * WorkManagerTaskExecutor：此实现使用 CommonJWorkManager作为其支持服务提供者，并且是在 Spring 应用程序上下文中在 WebLogic 或 WebSphere 上设置基于 CommonJ 的线程池集成的中心便利类。
  * DefaultManagedTaskExecutor：此实现使用ManagedExecutorService在与 JSR-236 兼容的运行时环境（例如 Java EE 7+ 应用程序服务器）中获得的 JNDI，为此目的替换了 CommonJ WorkManager。
* 除了TaskExecutor抽象之外，Spring 3.0 还引入了TaskScheduler 一种用于调度任务以在未来某个时间点运行的多种方法。schedule最简单的方法是仅采用 aRunnable和 a的命名方法Date。这会导致任务在指定时间后运行一次。所有其他方法都能够安排任务重复运行。固定速率和固定延迟方法用于简单的周期性执行，但接受 Trigger 的方法要灵活得多。以下清单显示了TaskScheduler接口定义：
  ```java
  public interface TaskScheduler {
  
      ScheduledFuture schedule(Runnable task, Trigger trigger);
  
      ScheduledFuture schedule(Runnable task, Instant startTime);
  
      ScheduledFuture schedule(Runnable task, Date startTime);
  
      ScheduledFuture scheduleAtFixedRate(Runnable task, Instant startTime, Duration period);
  
      ScheduledFuture scheduleAtFixedRate(Runnable task, Date startTime, long period);
  
      ScheduledFuture scheduleAtFixedRate(Runnable task, Duration period);
  
      ScheduledFuture scheduleAtFixedRate(Runnable task, long period);
  
      ScheduledFuture scheduleWithFixedDelay(Runnable task, Instant startTime, Duration delay);
  
      ScheduledFuture scheduleWithFixedDelay(Runnable task, Date startTime, long delay);
  
      ScheduledFuture scheduleWithFixedDelay(Runnable task, Duration delay);
  
      ScheduledFuture scheduleWithFixedDelay(Runnable task, long delay);
  }
  ```
* 默认情况下，毫秒将用作固定延迟、固定速率和初始延迟值的时间单位。如果您想使用不同的时间单位，例如秒或分钟，您可以通过 中的timeUnit属性进行配置@Scheduled。
* 从 Spring Framework 4.3 开始，@Scheduled任何范围的 bean 都支持方法。 确保您没有@Scheduled 在运行时初始化同一注释类的多个实例，除非您确实想为每个此类实例安排回调。与此相关，请确保您不要在使用容器@Configurable注释@Scheduled并注册为常规 Spring bean 的 bean 类上使用。否则，您将获得双重初始化（一次通过容器，一次通过@Configurable方面），结果是每个 @Scheduled方法被调用两次。
* @Async方法不仅可以声明常规java.util.concurrent.Future返回类型，还可以声明 Springorg.springframework.util.concurrent.ListenableFuture或从 Spring 4.2 开始，JDK 8 的java.util.concurrent.CompletableFuture，以便与异步任务进行更丰富的交互并与进一步的处理步骤进行即时组合。
* **[Cron Expressions](https://docs.spring.io/spring-framework/docs/current/reference/html/integration.html#scheduling-cron-expression)**, 所有 Spring cron 表达式都必须符合相同的格式，无论您是在 [@Scheduled annotations](https://docs.spring.io/spring-framework/docs/current/reference/html/integration.html#scheduling-annotation-support-scheduled), [task:scheduled-tasks](https://docs.spring.io/spring-framework/docs/current/reference/html/integration.html#scheduling-task-namespace-scheduled-tasks) elements 还是在其他地方使用它们。一个格式良好的 cron 表达式，例如* * * * * *，由六个以空格分隔的时间和日期字段组成，每个字段都有自己的有效值范围。
* 从 3.1 版开始，Spring 框架支持透明地向现有 Spring 应用程序添加缓存。与 [事务](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#transaction) 支持类似，缓存抽象允许一致使用各种缓存解决方案，而对代码的影响最小。 在 Spring Framework 4.1 中，缓存抽象显着扩展，支持 [JSR-107](https://docs.spring.io/spring-framework/docs/current/reference/html/integration.html#cache-jsr-107) 注释和更多自定义选项。
* 缓存与缓冲区: “缓冲区”和“缓存”这两个术语往往可以互换使用。但是请注意，它们代表不同的事物。传统上，缓冲区用作快速实体和慢速实体之间的数据的中间临时存储。由于一方必须等待另一方（这会影响性能），缓冲区通过允许整个数据块一次移动而不是小块移动来缓解这种情况。数据仅从缓冲区写入和读取一次。此外，缓冲区对于知道它的至少一方是可见的。 另一方面，根据定义，缓存是隐藏的，并且任何一方都不知道发生了缓存。它还提高了性能，但通过让相同的数据以快速方式多次读取来实现。 [您可以在此处](https://en.wikipedia.org/wiki/Cache_(computing)#The_difference_between_buffer_and_cache) 找到有关缓冲区和缓存之间差异的进一步说明。
* 缓存抽象的核心是将缓存应用于 Java 方法，从而根据缓存中可用的信息减少执行次数。也就是说，每次调用目标方法时，抽象都会应用缓存行为来检查是否已经为给定参数调用了该方法。如果已调用，则返回缓存的结果，而无需调用实际方法。如果没有调用该方法，则调用该方法，并将结果缓存并返回给用户，以便下次调用该方法时，返回缓存的结果。这样，对于给定的一组参数，昂贵的方法（无论是 CPU 还是 IO 绑定）只能被调用一次，并且结果可以重用，而不必再次实际调用该方法。**此方法仅适用于保证为给定输入（或参数）返回相同输出（结果）的方法，无论调用多少次。**
* 缓存抽象对多线程和多进程环境没有特殊处理，因为这些功能由缓存实现处理。
* 通常强烈建议不要在同一方法上 使用@CachePut和注释，因为它们具有不同的行为。@Cacheable后者通过使用缓存导致方法调用被跳过，而前者强制调用以运行缓存更新。这会导致意外行为，并且除了特定的极端情况（例如具有将它们彼此排除的条件的注释）外，应避免此类声明。另请注意，此类条件不应依赖于结果对象（即#result变量），因为这些条件已预先验证以确认排除。
* 处理缓存注释的默认建议模式是proxy，它只允许通过代理拦截调用。同一类中的本地调用不能以这种方式被拦截。对于更高级的拦截模式，请考虑切换到aspectj结合编译时或加载时编织的模式。
* 方法可见性和缓存注释: 当您使用代理时，您应该只将缓存注释应用于具有公共可见性的方法。如果您使用这些注释对受保护的、私有的或包可见的方法进行注释，则不会引发错误，但带注释的方法不会显示配置的缓存设置。如果您需要注释非公共方法，请考虑使用 AspectJ（请参阅本节的其余部分），因为它会更改字节码本身。Spring 建议您只使用注解来注解具体类（和具体类的方法）@Cache*，而不是注解接口。您当然可以在接口（或接口方法）上放置@Cache*注释，但这仅在您使用代理模式（mode="proxy"）时才有效。如果您使用基于编织的方面 ( mode="aspectj")，则编织基础结构在接口级声明中无法识别缓存设置。	在代理模式（默认）下，仅拦截通过代理传入的外部方法调用。这意味着自调用（实际上，目标对象中的一个方法调用目标对象的另一个方法）不会导致运行时的实际缓存，即使调用的方法被标记为@Cacheable. 在这种情况下考虑使用该aspectj模式。此外，代理必须完全初始化以提供预期的行为，因此您不应在初始化代码（即@PostConstruct）中依赖此功能。
* 自定义注释和 AspectJ: 此功能仅适用于基于代理的方法，但可以通过使用 AspectJ 进行一些额外的工作来启用。 该spring-aspects模块仅为标准注释定义了一个方面。如果您已经定义了自己的注解，您还需要为这些注解定义一个方面。检查AnnotationCacheAspect一个例子。
* 























































