
[TOC]

---


1. 关于`Distribution Zip Files`不能下载的问题
    根据Spring4.3.x中的文档描述[Distribution Zip Files](https://docs.spring.io/spring-framework/docs/4.3.x/spring-framework-reference/htmlsingle/#overview-distribution-zip)，可以通过[https://repo.spring.io/release/org/springframework/spring](https://repo.spring.io/release/org/springframework/spring)下载distribution zip file。
    > To download a distribution zip open a web browser to https://repo.spring.io/release/org/springframework/spring and select the appropriate subfolder for the version that you want. Distribution files end -dist.zip, for example spring-framework-{spring-version}-RELEASE-dist.zip. Distributions are also published for milestones and snapshots.
    但是，实际下载的时候，却发现不再提供。原因是Spring对下载策略做了变更。
    
    [Notice of Permissions Changes to repo.spring.io, January 2023](https://spring.io/blog/2022/12/14/notice-of-permissions-changes-to-repo-spring-io-january-2023)
    > January 26, 2023
    > Anonymous clients will no longer be able to download Spring Releases (including released plugins) from https://repo.spring.io, instead they should be sourced from Maven Central. Virtual repositories will NOT resolve snapshots and milestones anonymously. Only /snapshot and /milestone will resolve anonymously.
 
    现在只有[/snapshot](https://repo.spring.io/artifactory/snapshot/)和[/milestone](https://repo.spring.io/artifactory/milestone/)可以匿名获取。
    注意路径都是`snapshot/org/springframework/spring/`。
    在这里可以下载到`spring-xxx-dist.zip`文件。

    但是，这两种文件都不是release版本的文件。如果希望获取release版本的文件，可以下载源码。
    - 源码地址：[spring-framework/releases](https://github.com/spring-projects/spring-framework/releases)
    - 编译参考：[Build from Source](https://github.com/spring-projects/spring-framework/wiki/Build-from-Source)
    - 编译示例
      ``` shell
      # 对于Spring 5，JDK版本至少是8；对于Spring 6，JDK版本至少是11。
      # 配置JAVA_HOME为jdk路径
      # 可以使用`gradlew tasks`查看所有可用任务
      # 如果像执行build，添加：-x test：排除单元测试
      # gradlew build -x test
      # 生产dist包
      gradlew distZip
      # 执行后会在子目录`build/distributions`中得到当前版本的`xxx-dist.zip`文件。
      
      # 5.x.x的生成物在`build`目录；6.x.x的生成物在`framework-docs\build`目录。
      
      ```      
      注：对于main分支最新代码，参考[编译 Spring 报错 No locally installed toolchains match](https://yuzhouwan.com/posts/190816/#%E8%B8%A9%E8%BF%87%E7%9A%84%E5%9D%91)、[Gradle 实战 _ 宇宙湾](file/compile/Gradle%20实战%20_%20宇宙湾.zip)。
      将 spring-core.gradle 配置文件中的目标 JDK 版本改成和本地版本一致，这里以 JDK 17 为例
      ```gradle
      multiRelease {
        // targetVersions 17, 21
        targetVersions 17
      }
      ```

2. 关于日志
    这里指的是日志门面。
    Spring默认使用JCL作为日志门面。从操作上看，该门面只是统一个各个日志系统的操作。
    SLF4J是现在最流行的日志门面。在操作上，增加了灵活性，例如`{}`变量占位符。
    SLF4J+Logback是之前常用的组合，可以灵活配置日志。
    SLF4J+Log4j2是目前最流行的组合，它增加了更多的优秀特性。
    在Spring5中，Spring使用自己的`spring-jcl`代替`jcl`。实现区别是，使用`spring-jcl`使用`LogAdapter`代替`jcl`的`LogFactoryImpl`来选择`Log`。具体使用时，区别不大。
    个人推荐使用`SLF4J`+`Log4j2`。即，Java代码中使用`SLF4J`作为门面，然后使用`Log4j2`作为内部实现，即配置文件为`Log4j2`的配置文件，例如`log4j2.xml`。
    注：为了健壮性，不要将`jcl`和`SLF4j`两种门面共存。由于Spring默认使用了`jcl`（或`spring-jcl`），所以，在使用`SLF4J`的时候，应该exclude对`jcl`（或`spring-jcl`）的依赖。
    参考：
    - [SpringFramework 4.3.x Logging](https://docs.spring.io/spring-framework/docs/4.3.x/spring-framework-reference/htmlsingle/#overview-logging)
    - [SpringFramework 5.3.x Logging](https://docs.spring.io/spring-framework/docs/5.3.x/reference/html/core.html#spring-jcl)
    - [log日志框架一篇足以（log4j、JUL、JCL、Slf4j、Logback、Log4j2）](https://juejin.cn/post/6964643067747909645)
    - [log日志框架一篇足以（log4j、JUL、JCL、Slf4j、Logback、Log4j2）](file/logging/log日志框架一篇足以（log4j、JUL、JCL、Slf4j、Logback、Log4j2）%20-%20掘金.zip)

2. 
