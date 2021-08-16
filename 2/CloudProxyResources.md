# 云服务商代理

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

## [阿里云 公共代理库](https://help.aliyun.com/document_detail/102512.html)

### Maven

```xml
<mirror>
    <id>aliyunmaven</id>
    <name>aliyun</name>
    <url>https://maven.aliyun.com/repository/public</url>
    <mirrorOf>*</mirrorOf>
</mirror>
```
