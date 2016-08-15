>###写在前面

今天刚刚提交了入职以后的第一份代码，欣喜和担忧并存。喜的是，以后大家使用xxAPP的时候，都会用到我写的部分。忧的是 ，阿西，自己的代码不会把整个功能搞奔溃了吧？总之，这个日子值得纪念：  `2016-08-15`  。

>###概述

Android项目的开发，需要频繁的编译运行，但是随着项目规模的一步步扩大，gradle构建也是越来越慢。每次构建需要5，6分钟，严重影响开发的效率，网上各种找加快Android Studio编译速度的办法，总结如下。

>###解决方案

* 开启gradle单独的守护进程

在下面的目录下面创建gradle.properties文件：
  *   `/home/<username>/.gradle/ (Linux)`
  *   `/Users/<username>/.gradle/ (Mac)`
  *   `C:\Users\<username>\.gradle (Windows)`

并在文件中增加：

```
org.gradle.daemon=true 
```

以Mac为例，可以使用touch指令创建文件，通过cat指令查看文件内容。
* 配置项目的gradle.properties文件

```
# Project-wide Gradle settings.

# IDE (e.g. Android Studio) users:
# Settings specified in this file will override any Gradle settings
# configured through the IDE.

# For more details on how to configure your build environment visit
# http://www.gradle.org/docs/current/userguide/build_environment.html

# The Gradle daemon aims to improve the startup and execution time of Gradle.
# When set to true the Gradle daemon is to run the build.
# TODO: disable daemon on CI, since builds should be clean and reliable on servers
org.gradle.daemon=true

# Specifies the JVM arguments used for the daemon process.
# The setting is particularly useful for tweaking memory settings.
# Default value: -Xmx10248m -XX:MaxPermSize=256m
org.gradle.jvmargs=-Xmx2048m -XX:MaxPermSize=512m -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8

# When configured, Gradle will run in incubating parallel mode.
# This option should only be used with decoupled projects. More details, visit
# http://www.gradle.org/docs/current/userguide/multi_project_builds.html#sec:decoupled_projects
org.gradle.parallel=true

# Enables new incubating mode that makes Gradle selective when configuring projects. 
# Only relevant projects are configured which results in faster builds for large multi-projects.
# http://www.gradle.org/docs/current/userguide/multi_project_builds.html#sec:configuration_on_demand
org.gradle.configureondemand=true
```

同时上面的这些参数也可以配置到前面的用户目录下的gradle.properties文件里，那样就不是针对一个项目生效，而是针对所有项目生效。

上面的配置文件主要就是做， 增大gradle运行的java虚拟机的大小，让gradle在编译的时候使用独立进程，让gradle可以平行的运行。

* 修改android studio配置

在android studio的配置中，开启offline模式，以及修改配置。实际上下面的配置和上面的一大段一样，主要是在这个地方配置的只会在ide构建的时候生效，命令行构建不会生效。

![屏幕快照 2016-08-15 22.07.25.png](https://ooo.0o0.ooo/2016/08/15/57b1d14458711.png)
![屏幕快照 2016-08-15 22.07.43.png](https://ooo.0o0.ooo/2016/08/15/57b1d143daebe.png)

command-line options可以添加类似于  `-Xmx2048m -XX:MaxPermSize=512m` 的jvm配置信息。基于上面的配置，命令行构建时在命令后面加上这个参数即可 --daemon --parallel --offline。

* 引入依赖库时使用aar

使用网上第三方的依赖库时尽量使用aar，可以在maven http://gradleplease.appspot.com/ 或者 github https://github.com/Goddchen/mvn-repo 搜索。

自己的库模块也可以打包成aar，关于这个可以参考文章。

>###总结

经过上述设置后，Android Studio构建时间明显缩短了，基本1分钟多一点编译完成，再也不用痛苦的等待5分钟了。亲测有效哦！
