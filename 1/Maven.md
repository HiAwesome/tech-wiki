# Maven

### [Profiles on Maven](https://maven.apache.org/guides/introduction/introduction-to-profiles.html)

How can I tell which profiles are in effect during a build?

```shell
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
