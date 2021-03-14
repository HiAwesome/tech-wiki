# Spring

## Spring Security 碰到的问题

* [There is no PasswordEncoder mapped for the id "null" 的解决办法](https://blog.csdn.net/Hello_World_QWP/article/details/81811462)

## spring.jpa.open-in-view=true

使用 Spring Data JPA 时，Springboot 启动报警告: 2021-03-14 00:09:26.064  WARN 27833 --- \[  restartedMain\] JpaBaseConfiguration$JpaWebConfiguration : spring.jpa.open-in-view is enabled by default. Therefore, database queries may be performed during view rendering. Explicitly configure spring.jpa.open-in-view to disable this warning. 于是 Google 了一下:

* [Spring Boot 中建议关闭 Open-EntityManager-in-view](http://www.kailing.pub/article/index/arcid/266.html)
* [What is this spring.jpa.open-in-view=true property in Spring Boot?](https://stackoverflow.com/questions/30549489/what-is-this-spring-jpa-open-in-view-true-property-in-spring-boot)

## Spring Boot H2 Database

* 浏览器访问地址:  http://localhost:8080/h2-console
* 无配置时需要确保 JDBC URL 字段被设置为 jdbc:h2:mem:testdb
* 复杂情况请参考 [javatpoint](https://www.javatpoint.com/spring-boot-h2-database)

## Spring Interceptor

* [Spring MVC Interceptor Example – XML and Annotation Java Config](https://howtodoinjava.com/spring-core/spring-mvc-interceptor-example/)
* [Spring Framework 5.3.4 PathPattern](https://docs.spring.io/spring-framework/docs/5.3.4/javadoc-api/org/springframework/web/util/pattern/PathPattern.html)
* [Spring MVC Interceptor HandlerInterceptorAdapter, HandlerInterceptor Example](https://www.journaldev.com/2676/spring-mvc-interceptor-example-handlerinterceptor-handlerinterceptoradapter)

## 学习中较好的 Spring 博客

* [KL Blog: Spring](http://www.kailing.pub/index/columns/colid/10.html)
