[TOC]

---

1. jdk需求
   官方原文：[Before You Start](https://github.com/spring-projects/spring-framework/wiki/Build-from-Source#before-you-start)
   > To build you will need [Git](https://docs.github.com/en/get-started/quickstart/set-up-git)
   and [JDK 17](https://adoptium.net/) and [JDK 21](https://jdk.java.net/21/) in
   a [location detected by Gradle toolchain support](https://docs.gradle.org/current/userguide/toolchains.html#sec:auto_detection).
   Be sure that your `JAVA_HOME` environment variable points to the `jdk17` folder extracted from the JDK download.
   You can check which Java installations are detected by gradle by running `./gradlew -q javaToolchains` in the
   project root. If your local installation is not detected, you can declare it in
   your `$HOME/.gradle/gradle.properties` file by adding the following
   property: `org.gradle.java.installations.paths=/path/to/java21/,/path/to/other/java/install/` ([see Gradle documentation](https://docs.gradle.org/current/userguide/toolchains.html#sec:custom_loc)).

   原文要求同时配置`jdk17`和`jdk21`的安装路径。方法是配置`org.gradle.java.installations.paths`属性。

2. 配置gradle属性
   配置gradle属性的方法参考：[Build Environment](https://docs.gradle.org/current/userguide/build_environment.html)、[Custom toolchain locations](https://docs.gradle.org/current/userguide/toolchains.html#sec:custom_loc)。
   gradle属性配置方法，原文如下：
   > When configuring Gradle behavior you can use these methods, listed in order of highest to lowest precedence (
   first one wins):
   [Command-line flags](https://docs.gradle.org/current/userguide/command_line_interface.html#command_line_interface)
   such as --build-cache. These have precedence over properties and environment variables.
   [System properties](https://docs.gradle.org/current/userguide/build_environment.html#sec:gradle_system_properties)
   such as systemProp.http.proxyHost=somehost.org stored in a gradle.properties file in a root
   project directory.
   [Gradle properties](https://docs.gradle.org/current/userguide/build_environment.html#sec:gradle_configuration_properties)
   such as org.gradle.caching=true that are typically stored in a gradle.properties file in a project directory or
   in the GRADLE_USER_HOME.
   [Environment variables](https://docs.gradle.org/current/userguide/build_environment.html#sec:gradle_environment_variables)
   such as GRADLE_OPTS sourced by the environment that executes Gradle.

   根据优先级，从高到低依次是：

   1. 命令行标志：
      ``` shell
      # gradle [taskName...] [--option-name...]
      gradle build --org.gradle.java.installations.paths=/path/to/java21/,/path/to/other/java/install/
      ```
   2. 系统属性：
      1. 使用`-D`命令行选项。
         > Using the `-D` command-line option, you can pass a system property to the JVM which runs Gradle.
         The `-D` option of the `gradle` command has the same effect as the `-D` option of the `java` command.

         ```shell
         gradle build -Dorg.gradle.java.installations.paths=/path/to/java21/,/path/to/other/java/install/
         ```
      2. 在`gradle.properties`中使用`systemProp`
         > You can also set system properties in `gradle.properties` files with the prefix `systemProp`.
         ```properties
         # gradle.properties
         systemProp.org.gradle.java.installations.paths=/path/to/java21/,/path/to/other/java/install/
         ```
   3. Gradle属性

   4. 环境变量