# Tomcat

## debug

* [How to Remotely Debug Application Running on Tomcat From Within Intellij IDEA](https://blog.trifork.com/2014/07/14/how-to-remotely-debug-application-running-on-tomcat-from-within-intellij-idea/)

### docker 8.5.73 运行日志

```text
~ docker run -it --rm -p 8888:8080 tomcat:8.5.73
Using CATALINA_BASE:   /usr/local/tomcat
Using CATALINA_HOME:   /usr/local/tomcat
Using CATALINA_TMPDIR: /usr/local/tomcat/temp
Using JRE_HOME:        /usr/local/openjdk-11
Using CLASSPATH:       /usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar
Using CATALINA_OPTS:
NOTE: Picked up JDK_JAVA_OPTIONS:  --add-opens=java.base/java.lang=ALL-UNNAMED --add-opens=java.base/java.io=ALL-UNNAMED --add-opens=java.base/java.util=ALL-UNNAMED --add-opens=java.base/java.util.concurrent=ALL-UNNAMED --add-opens=java.rmi/sun.rmi.transport=ALL-UNNAMED
10-Jan-2022 06:13:27.210 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server version name:   Apache Tomcat/8.5.73
10-Jan-2022 06:13:27.213 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server built:          Nov 11 2021 13:14:36 UTC
10-Jan-2022 06:13:27.214 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server version number: 8.5.73.0
10-Jan-2022 06:13:27.214 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log OS Name:               Linux
10-Jan-2022 06:13:27.214 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log OS Version:            5.10.76-linuxkit
10-Jan-2022 06:13:27.214 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Architecture:          aarch64
10-Jan-2022 06:13:27.214 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Java Home:             /usr/local/openjdk-11
10-Jan-2022 06:13:27.215 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log JVM Version:           11.0.13+8
10-Jan-2022 06:13:27.216 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log JVM Vendor:            Oracle Corporation
10-Jan-2022 06:13:27.216 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log CATALINA_BASE:         /usr/local/tomcat
10-Jan-2022 06:13:27.216 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log CATALINA_HOME:         /usr/local/tomcat
10-Jan-2022 06:13:27.218 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: --add-opens=java.base/java.lang=ALL-UNNAMED
10-Jan-2022 06:13:27.218 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: --add-opens=java.base/java.io=ALL-UNNAMED
10-Jan-2022 06:13:27.219 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: --add-opens=java.base/java.util=ALL-UNNAMED
10-Jan-2022 06:13:27.219 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: --add-opens=java.base/java.util.concurrent=ALL-UNNAMED
10-Jan-2022 06:13:27.219 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: --add-opens=java.rmi/sun.rmi.transport=ALL-UNNAMED
10-Jan-2022 06:13:27.219 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.util.logging.config.file=/usr/local/tomcat/conf/logging.properties
10-Jan-2022 06:13:27.220 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager
10-Jan-2022 06:13:27.220 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djdk.tls.ephemeralDHKeySize=2048
10-Jan-2022 06:13:27.220 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.protocol.handler.pkgs=org.apache.catalina.webresources
10-Jan-2022 06:13:27.220 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Dorg.apache.catalina.security.SecurityListener.UMASK=0027
10-Jan-2022 06:13:27.220 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Dignore.endorsed.dirs=
10-Jan-2022 06:13:27.220 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Dcatalina.base=/usr/local/tomcat
10-Jan-2022 06:13:27.220 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Dcatalina.home=/usr/local/tomcat
10-Jan-2022 06:13:27.220 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.io.tmpdir=/usr/local/tomcat/temp
10-Jan-2022 06:13:27.220 INFO [main] org.apache.catalina.core.AprLifecycleListener.lifecycleEvent Loaded Apache Tomcat Native library [1.2.31] using APR version [1.7.0].
10-Jan-2022 06:13:27.220 INFO [main] org.apache.catalina.core.AprLifecycleListener.lifecycleEvent APR capabilities: IPv6 [true], sendfile [true], accept filters [false], random [true], UDS [{4}].
10-Jan-2022 06:13:27.221 INFO [main] org.apache.catalina.core.AprLifecycleListener.lifecycleEvent APR/OpenSSL configuration: useAprConnector [false], useOpenSSL [true]
10-Jan-2022 06:13:27.222 INFO [main] org.apache.catalina.core.AprLifecycleListener.initializeSSL OpenSSL successfully initialized [OpenSSL 1.1.1k  25 Mar 2021]
10-Jan-2022 06:13:27.247 INFO [main] org.apache.coyote.AbstractProtocol.init Initializing ProtocolHandler ["http-nio-8080"]
10-Jan-2022 06:13:27.254 INFO [main] org.apache.tomcat.util.net.NioSelectorPool.getSharedSelector Using a shared selector for servlet write/read
10-Jan-2022 06:13:27.260 INFO [main] org.apache.catalina.startup.Catalina.load Initialization processed in 275 ms
10-Jan-2022 06:13:27.290 INFO [main] org.apache.catalina.core.StandardService.startInternal Starting service [Catalina]
10-Jan-2022 06:13:27.290 INFO [main] org.apache.catalina.core.StandardEngine.startInternal Starting Servlet engine: [Apache Tomcat/8.5.73]
10-Jan-2022 06:13:27.299 INFO [main] org.apache.coyote.AbstractProtocol.start Starting ProtocolHandler ["http-nio-8080"]
10-Jan-2022 06:13:27.304 INFO [main] org.apache.catalina.startup.Catalina.start Server startup in 43 ms
^C10-Jan-2022 06:15:21.740 INFO [Thread-3] org.apache.coyote.AbstractProtocol.pause Pausing ProtocolHandler ["http-nio-8080"]
10-Jan-2022 06:15:21.750 INFO [Thread-3] org.apache.catalina.core.StandardService.stopInternal Stopping service [Catalina]
10-Jan-2022 06:15:21.756 INFO [Thread-3] org.apache.coyote.AbstractProtocol.stop Stopping ProtocolHandler ["http-nio-8080"]
10-Jan-2022 06:15:21.766 INFO [Thread-3] org.apache.coyote.AbstractProtocol.destroy Destroying ProtocolHandler ["http-nio-8080"]
```
