QuickStart
======
##QuickStart
只需要输入*btrace <PID> AllMethods.java*(PID是任何java应用程序的PID)就可以了
[trace script](trace_scrpt/md) (AllMethods.java)例子,会对javax.swing.*包下面的所有类的所有方法进行跟踪,跟踪器会在进入任何跟踪的方法时输出完整的类名和方法

所有的输出都是标准输出

当脚本正在运行时，可以按Ctrl-c键打开BTrace控制台菜单。
```java
package samples;

import com.sun.btrace.annotations.*;
import static com.sun.btrace.BTraceUtils.*;

/**
 * This script traces method entry into every method of
 * every class in javax.swing package! Think before using
 * this script -- this will slow down your app significantly!!
 */
@BTrace public class AllMethods {
    @OnMethod(
        clazz="/javax\\.swing\\..*/",
        method="/.*/"
    )
    public static void m(@ProbeClassName String probeClass, @ProbeMethodName String probeMethod) {
        print(Strings.strcat("entered ", probeClass));
        println(Strings.strcat(".", probeMethod));
    }
}
```

##BTrace运行详情
####动态加载应用
用于快速附加到已经运行的应用程序,获取你想要的或者想分离的数据,删除任何跟踪代码.
附加的命令如下:
```xshell
btrace [-p <port>] [-cp <classpath>] <pid> <btrace-script> [<args>]
```
- port port参数是BTrace代理监听的端口号,可选参数
- classpath 设置BTrace在编译期间搜索类的目录和jar文件的集合
- pid java程序的进程id
- btrace-script 一个跟踪程序,如果是一个".java"文件,那么会在提交之前被编译。或者它是预编译的(.class文件),那么会直接提交

brace -h命令将会打印出用法帮助

####BTrace应用启动
在此模式下，即使在运行应用程序启动之前，BTrace也会启动。 这让我们有机会跟踪在应用程序生命周期的早期执行的代码。
下面是BTrace agent启动应用程序的命令语法:
```xshell
java -javaagent:btrace-agent.jar=[<agent-arg>[,<agent-arg>]*]? <launch-args>
```
代码采用逗号分割参数列表:
- noServer 不启动socker服务
- bootClassPath 要使用的引导类路径
- systemClassPath 用使用的系统路劲
- debug 打开详细的调用信息(true/false)
- unsafe 不检查是否违反btrace的限制(true/false)
- dumpClasses 将转换后的字节码转储到文件(true/false)
- dumpDir 指定将转换的类转储到的文件夹
- stdout 将跟踪输出重定向到stdout，而不是将其写入任意文件(true/false)
- probeDescPath 搜索探针描述符XML的路径
- startupRetransform 启动所有附加的类重新转换(enable retransform of all the loaded classes at attach)(true/false)
- scriptdir 要在代理启动时运行的脚本的目录
- scriptOutputFile btrace代理将存储其输出的文件路径
- script 冒号分隔的要在代理启动时运行的跟踪脚本列表
要运行的脚本必须已经由btracec编译为字节码（.class文件）。

##btracec
btracec是编译脚本跟踪到class文件的命令。
语法是:
```shell
btracec [-cp <classpath>] [-d <directory>] <one-or-more-BTrace-.java-files>
```
- classpath 用于编译BTrace程序的路劲(默认是".")
- directory 存储编译的.class文件的输出目录。 默认为“.”。

不是常规javac，而是使用BTrace编译器 - 在编译时评估脚本，并防止在运行时报告验证错误。

##btracer
用于启动BTrace应用的辅助脚本
语法如下:
```shell
btracer <pre-compiled-btrace.class> <vm-arg> <application-args>
```
- pre-compiled-btrace.class 跟踪脚本通过btracec编译为字节码
- vm-args vm参数,比如:-cp app.jar Main.class或-jar app.jar
- application-args　应用程序特定的参数

