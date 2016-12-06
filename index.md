BTrace
======

##介绍
BTrace是一个用于Java平台的安全的动态跟踪工具。 BTrace可用于动态跟踪正在运行的Java程序（类似于用于OpenSolaris应用程序和操作系统的DTrace）。 BTrace动态测试目标应用程序的类以注入跟踪代码（“字节码跟踪”）。

BTrace的工作原理是动态的（字节码）改编已运行Java程序的类。 BTrace插入跟踪行动统一到一个运行Java程序的类和hotswaps跟踪的程序类。

- github代码[仓库](https://github.com/btraceio/btrace)
- 文档[地址](https://github.com/btraceio/btrace/wiki)

##Building BTrace
####Setup
- JDK(建议优先选择jdk8)
- Git
- Gradle(可选ANT,MAVEN)

<!-- toc -->
####Build
- Gradle
    ```shell
    cd <btrace>
    ./gradlew build
    ./gradlew buildDistributions
    ```
    二进制的dist包可以在<btrace>/build/distributions目录下找到,是*.tar.gz, *.zip, *.rpm和*.deb的文件

- Ant
	```shell
    cd <btrace>/make
	ant dist
	```
     二进制的dist包可以在<btrace>/dist目录下找到,是*.tar.gz和*.zip的文件

##使用BTrace
####安装
1. 首先,你需要从[release page](https://github.com/btraceio/btrace/releases/latest)下载文件.解压二进制文件(.zip或.tar.gz)到你指定的目录中
1. 设置系统环境变量BTRACE_HOME指向包含展开的分布的目录
1. 为方便起见，可以使用$ORACLE_HOME/bin添加到系统环境变量PATH中。(或者，安装* .rpm的或* .deb软件包之一)
1. 输入命令btrace -h查看是否安装成功

####运行
- <<btrace>>/bin/btrace <<PID>> <<trace_script>> 将附加到具有给定PID的java应用程序，并编译并提交跟踪脚本
- <<btrace>>/bin/tracer <<trace script>> 将编译提供的跟踪脚本
- <<btrace>>/bin/btracer <<compiled_script>> <<args to launch a java app>> 将启动指定的java应用程序，其中btrace代理运行，并且之前由btracec编译的脚本加载

####maven集成
[maven插件](maven_plugin.md)提供了对BTrace脚本的轻松编译

