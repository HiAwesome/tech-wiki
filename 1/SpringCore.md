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
* 





































































