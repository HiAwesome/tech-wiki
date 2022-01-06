# Spock Framework

## 迁移 Compile Java17 碰到的问题

#### [Cannot create mock due to module java.base does not "opens java.lang.invoke"](https://github.com/spockframework/spock/issues/1406)

## [IntelliJ IDEA Writing Tests with Spock(2021)](https://www.youtube.com/watch?v=i5Qu3qYOfsM&t=168s)

## Spock with Spring

* [Baeldung: Testing with Spring and Spock](https://www.baeldung.com/spring-spock-testing)
* [Testing with Spring and Spock](https://www.baeldung.com/spring-spock-testing)

### [Spock Framework 2.0 Release Notes](https://spockframework.org/spock/docs/2.0/release_notes.html)

较为重要的升级理由：

> * Support for Groovy 3
> * Spock now supports Parallel Execution on the spec and feature level, with powerful tools for shared resource access.

#### Update to 2.0 data links

* [spock 2.0 migration guide](https://spockframework.org/spock/docs/2.0/migration_guide.html)
* [spock 2.0 interaction-based-testing](https://spockframework.org/spock/docs/2.0/interaction_based_testing.html#interaction-based-testing)
* [maven surefire plugin spock](https://maven.apache.org/surefire/maven-surefire-plugin/examples/spock.html)
* [mvnrepository: surefire-junit47](https://mvnrepository.com/artifact/org.apache.maven.surefire/surefire-junit47)
* [mvnrepository: maven-surefire-plugin](https://mvnrepository.com/artifact/org.apache.maven.plugins/maven-surefire-plugin)
* [mvnrepository: junit-jupiter-engine](https://mvnrepository.com/artifact/org.junit.jupiter/junit-jupiter-engine)
* [spock-example pom.xml](https://github.com/spockframework/spock-example/blob/master/pom.xml)
* [mvnrepository: gmavenplus-plugin](https://mvnrepository.com/artifact/org.codehaus.gmavenplus/gmavenplus-plugin)
* [spockframework 2.0 index](https://spockframework.org/spock/docs/2.0/index.html)
* [junit5 user guide](https://junit.org/junit5/docs/current/user-guide/)
* [junit4-and-junit5-tests-not-running-in-intellij](https://stackoverflow.com/questions/45040070/junit4-and-junit5-tests-not-running-in-intellij)
* [maven-surefire-plugin junit-platform](https://maven.apache.org/surefire/maven-surefire-plugin/examples/junit-platform.html)
* [junit5 issues 1773](https://github.com/junit-team/junit5/issues/1773)
* [mvnrepository: junit-jupiter](https://mvnrepository.com/artifact/org.junit.jupiter/junit-jupiter)
* [youtrack IDEA-274589](https://youtrack.jetbrains.com/issue/IDEA-274589)

#### 2.0 并行执行

**[parallel-execution](https://spockframework.org/spock/docs/2.0/parallel_execution.html#parallel-execution)**

* 对于有事务状态的集成测试来说，在不使用 ResourceLock 的情况下无法保证影响结果的正确性，而且速度不会提高，不推荐。
* 对于完全分隔的单测而言，并行执行默认启动核心数的线程可以加快速度，具体数据待测试。
