### 1. 如何优雅关闭maven-default-http-blocker？

所以Maven 3.8.1就禁止了所有HTTP协议的Maven仓库。

最近升级Maven到3.8.1后，mvn编译的时候总是提示拉不到依赖，报错如下：

```
Could not validate integrity of download from http://0.0.0.0/...
```

在`~/.m2/setttings.xml`中添加同名mirror，然后指定这个mirror不对任何仓库生效即可。

```xml
<mirror>
    <id>maven-default-http-blocker</id>
    <mirrorOf>!*</mirrorOf>
    <url>http://0.0.0.0/</url>
</mirror> 
```





