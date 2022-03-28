# Spring

#### [How Fast is Spring?](https://spring.io/blog/2018/12/12/how-fast-is-spring)

#### [设计模式 -- 策略模式+Spring Bean代替if/else](https://developer.aliyun.com/article/775879)

#### [How to use Log4j 2 with Spring Boot](https://www.callicoder.com/spring-boot-log4j-2-example/)

### [Spring Bean Names](https://www.baeldung.com/spring-bean-names)

### [spring cloud config client not loading configuration from config server](https://stackoverflow.com/a/65539287)

If you are using 2020.0.0 version of spring cloud than you need to this dependency in your maven dependencies to enable bootstrap, which is desabled by default in 2020.0.0.

[Breaking changes in 2020.0.0](https://github.com/spring-cloud/spring-cloud-release/wiki/Spring-Cloud-2020.0-Release-Notes#breaking-changes)

It work for me.

```text
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bootstrap</artifactId>
</dependency>
```

### [Properties with Spring and Spring Boot](https://www.baeldung.com/properties-with-spring)

### [Guide to @ConfigurationProperties in Spring Boot](https://www.baeldung.com/configuration-properties-in-spring-boot)

### [Validation in Spring Boot](https://www.baeldung.com/spring-boot-bean-validation)

#### [Springboot 2.3: Validation Starter no longer included in web starters](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.3-Release-Notes#validation-starter-no-longer-included-in-web-starters)

### [Guide to Spring Retry](https://www.baeldung.com/spring-retry)

### [Overview of Spring Boot Dev Tools](https://www.baeldung.com/spring-boot-devtools)

### [Spring Native](https://docs.spring.io/spring-native/docs/current/reference/htmlsingle/)

* [Docker image names are not allowed to have upper-case characters](https://github.com/spring-projects/spring-boot/issues/21922#issuecomment-644929521)

#### [Improve native image build error message](https://github.com/quarkusio/quarkus/issues/1140)

结论：碰到这个错误时增加 Docker 引擎的内存

```text
[creator]     Error: Image build request failed with exit status 137
[creator]     unable to invoke layer creator
[creator]     unable to contribute native-image layer
[creator]     error running build
[creator]     exit status 137
[creator]     ERROR: failed to build: exit status 1
```

#### [Environment 用户自定义类导致 Springboot 启动失败](https://www.javazhiyin.com/59321.html)

### [Validation in Spring Boot](https://www.baeldung.com/spring-boot-bean-validation)

### [How can I access path variables in my custom HandlerMethodArgumentResolver](https://stackoverflow.com/a/28939816/7379661)

### [spring-boot 下划线和驼峰转换](https://adolphor.com/2019/11/16/spring-boot-under-lower-camel.html)

### [How to bind @RequestParam to object in Spring](http://dolszewski.com/spring/how-to-bind-requestparam-to-object/)

### [Spring @PathVariable without ending slash (String with IP address)](https://stackoverflow.com/a/24946723/7379661)

使用 PathVariable 传递 IP 地址会截断最后一个点及其之后的内容，详情参考上面链接。

## [Spring validate string value is a JSON](https://stackoverflow.com/a/59204301/7379661)

## [Java Bean Validation Basics](https://www.baeldung.com/javax-validation)

## Spring MVC

### [Spring @RequestParam Annotation](https://www.baeldung.com/spring-request-param)

### [Validating RequestParams and PathVariables in Spring](https://www.baeldung.com/spring-validate-requestparam-pathvariable)

## [Youtube Video, 20200914: Tutorial: Reactive Spring Boot](https://www.youtube.com/watch?v=229gPlcc5d8)

## Spring Config

* [Baeldung: Spring Profiles](https://www.baeldung.com/spring-profiles)
* [Make Spring JUnit Test run on test database](https://stackoverflow.com/a/43374987)

## [Spring Guides](https://spring.io/guides)

### [Accessing Relational Data using JDBC with Spring](https://spring.io/guides/gs/relational-data-access/)

### [JDBC Repositories](https://docs.spring.io/spring-data/jdbc/docs/current/reference/html/#jdbc.repositories)

## DAO

### [Transaction](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#transaction)

* [How to handle manually transactions while using jdbcTemplate?](https://stackoverflow.com/a/60088953)
* [@EnableTransactionManagement in Spring Boot](https://stackoverflow.com/a/40724843)
* [Spring Transaction Management: @Transactional In-Depth](https://www.marcobehler.com/guides/spring-transaction-management-transactional-in-depth)
* [Baeldung: Transactions with Spring and JPA](https://www.baeldung.com/transaction-configuration-with-jpa-and-spring)
* [Rollback transaction after @Test](https://stackoverflow.com/a/12626636), 补充：使用 org.springframework.test.annotation.Commit 可以提交测试事务。

### Spring JDBC Template

* [Baeldung: Spring JDBC](https://www.baeldung.com/spring-jdbc-jdbctemplate)
* [Springboot: Working with SQL Databases](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-sql)
* [Spring 的 NamedParameterJdbcTemplate 使用方法小结以及项目实战实例](https://blog.csdn.net/Jaiaxn/article/details/87889550)

### Mybatis

* [Baeldung: MyBatis with Spring](https://www.baeldung.com/spring-mybatis)

### DAO Unit Test

* [Stack Overflow: Using annotation @Sql, is it possible to execute scripts in Class level before Method level?](https://stackoverflow.com/a/32892436)
* [Stack Overflow: Using JUnit Categories with Maven Failsafe plugin](https://stackoverflow.com/a/18373848)
* [Baeldung: Testing in Spring Boot](https://www.baeldung.com/spring-boot-testing)
* [Baeldung: Quick Guide on Loading Initial Data with Spring Boot](https://www.baeldung.com/spring-boot-data-sql-and-schema-sql)
* [Baeldung: Spring JdbcTemplate Unit Testing](https://www.baeldung.com/spring-jdbctemplate-testing)
* [How unit test insert record in spring (no delete method)](https://stackoverflow.com/a/51277993)

### HikariCP

* [主流Java数据库连接池比较及前瞻 20180430](https://blog.didispace.com/java-datasource-pool-compare/), 推荐 HikariCP.
* [Github: brettwooldridge/HikariCP](https://github.com/brettwooldridge/HikariCP).
* [HikariCP MySQL Configuration](https://github.com/brettwooldridge/HikariCP/wiki/MySQL-Configuration), 请注意，不需要传递 DataSource class 参数，如主页 wiki 所言: The MySQL DataSource is known to be broken with respect to network timeout support. Use jdbcUrl configuration instead.
* [Baeldung: Introduction to HikariCP](https://www.baeldung.com/hikaricp)
* [Baeldung: Configuring a Hikari Connection Pool with Spring Boot](https://www.baeldung.com/spring-boot-hikari)

## Bean Problem

* [Baeldung: The BeanDefinitionOverrideException in Spring Boot](https://www.baeldung.com/spring-boot-bean-definition-override-exception)

## [Field injection is not recommended](https://blog.marcnuri.com/field-injection-is-not-recommended)

* [Field Dependency Injection Considered Harmful](https://www.vojtechruzicka.com/field-dependency-injection-considered-harmful/)
* [What exactly is Field Injection and how to avoid it?](https://stackoverflow.com/a/39892204)
* [Field injection is not recommended – Spring IOC](https://blog.marcnuri.com/field-injection-is-not-recommended/)

## Spring Security 碰到的问题

* [There is no PasswordEncoder mapped for the id "null" 的解决办法](https://blog.csdn.net/Hello_World_QWP/article/details/81811462)

### [Spring Security: Exploring JDBC Authentication](https://www.baeldung.com/spring-security-jdbc-authentication)

Spring Security JDBC Authentication 默认建表语句和测试数据：

```text
-- schema.sql

CREATE TABLE users (
  username VARCHAR(50) NOT NULL,
  password VARCHAR(100) NOT NULL,
  enabled TINYINT NOT NULL DEFAULT 1,
  PRIMARY KEY (username)
);
  
CREATE TABLE authorities (
  username VARCHAR(50) NOT NULL,
  authority VARCHAR(50) NOT NULL,
  FOREIGN KEY (username) REFERENCES users(username)
);

CREATE UNIQUE INDEX ix_auth_username
  on authorities (username,authority);
```

```text
-- data.sql

INSERT INTO users (username, password, enabled)
  values ('user',
    '$2a$10$8.UnVuG9HHgffUDAlk8qfOuVGkqRzgVymGe07xd00DMxs.AQubh4a',
    1);

INSERT INTO authorities (username, authority)
  values ('user', 'ROLE_USER');
```

#### [Spring Boot /h2-console throws 403 with Spring Security 1.5.2](https://stackoverflow.com/a/53066577)

### [Authenticating a User with LDAP](https://spring.io/guides/gs/authenticating-ldap/)

```java

@Configuration
public class SecurityConfiguration extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) {
        http.authorizeRequests()
                .antMatchers("/").permitAll()
                .antMatchers("/h2-console/**").permitAll();

        http.csrf().disable();
        http.headers().frameOptions().disable();
    }
}
```

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

* [Hello. Do you want to become a better Java developer?](https://www.marcobehler.com/)
* [KL Blog: Spring](http://www.kailing.pub/index/columns/colid/10.html)
