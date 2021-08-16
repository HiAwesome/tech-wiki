# 云服务商代理

## [阿里云 云效 Maven](https://maven.aliyun.com/mvn/guide)

### Maven

```xml
<mirror>
    <id>aliyunmaven</id>
    <name>aliyun</name>
    <url>https://maven.aliyun.com/repository/public</url>
    <mirrorOf>*</mirrorOf>
</mirror>
```

### Gradle

```groovy
allProjects {
    repositories {
        maven { url 'https://maven.aliyun.com/repository/public/' }
        mavenLocal()
        mavenCentral()
    }
}
```

## [腾讯云软件源加速软件包下载和更新](https://cloud.tencent.com/document/product/213/8623)

### Maven

```xml
<mirror>
    <id>nexus-tencentyun</id>
    <name>tencent</name>
    <url>http://mirrors.cloud.tencent.com/nexus/repository/maven-public/</url>
    <mirrorOf>*</mirrorOf>
</mirror>
```

### Gradle

```groovy
allProjects {
    repositories {
        maven { url 'http://mirrors.cloud.tencent.com/nexus/repository/maven-public/' }
        mavenLocal()
        mavenCentral()
    }
}
```
