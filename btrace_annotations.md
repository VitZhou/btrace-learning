BTrace注解
======
翻译至[原文](https://github.com/btraceio/btrace/wiki/BTrace-Annotations)

##方法注解
- *@OnMethod*
	> 用于在方法内指定目标类class(一个或多个),目标方法method(一个或者多个),以及目标位置location(一个或多个)

	```java
    @OnMethod(clazz=<cname_spec>[, method=<mname_spec>]? [, type=<signature>]? [, location=<location_spec>]?)
    ```
    - /regex/　用于标识类名的标准正则表达式
    - cname_spec 等价于:| + | /regex/
    - class_name 全限定类名
    - +class_name　一个带+前缀的全限定类名，表示前置类名的所有子类和实现者
    - mname_spec 等价于:| /regex/
    - method_name　简单(没有签名或者返回类型)的方法名
    - arg_type 在java中写入的参数类型
    - signature 等价于:((arg_type(,arg_type)*)?
    - return_type 在Java中编写的方法的返回类型(例如:void,java.lang.String)
    当到达指定的匹配的方法时,调用有此注解注解了的动作方法。在@OnMethod注解中,跟踪类名由clazz属性指定,方法名由method属性指定.clazz可以是全限定名(比如:java.awt.Component或在两个正斜杠内指定的正则表达式).这里有个例子:[NewComponent.java](https://github.com/jbachorik/btrace/blob/master/samples/NewComponent.java)和[Classload.java](https://github.com/jbachorik/btrace/blob/master/samples/Classload.java).
    正则表达式可以匹配0或多个类,使用正则表达式的话,所有匹配的类都会被检查.例如/java\.awt\..+/匹配java.awt包中的所有类。 此外，方法名也可以是正则表达式 - 匹配零个或多个方法。请参考例子[MultiClass.java](https://github.com/jbachorik/btrace/blob/master/samples/MultiClass.java);
    还有另外一种方法可以抽象的指示跟踪类和方法.跟踪的类和方法可以通过注解来指定。 例如，如果“clazz”属性指定为@javax.jws.WebService，BTrace将对所有由WebService注解标记的类进行测试。 类似地，方法级注解可以用于抽象地指定方法。请参考例子[WebServiceTracker.java](https://github.com/jbachorik/btrace/blob/master/samples/WebServiceTracker.java)。
    可以将正则表达式与注解结合起来使用,例如*@/com\.acme\..+/*正则表达式会匹配到:由正则表达式匹配的注解注解过的任何类
    可以指定超类型来匹配多个类,即匹配给定超类型的子类型的所有类。 例如:+java.lang.Runnable匹配所有实现java.lang.Runnable接口的类.请参考例子[SubtypeTracer.java](https://github.com/jbachorik/btrace/blob/master/samples/SubTypeTracer.java).

- @OnTimer
	> 用于指定必须每N毫秒周期性地运行的跟踪操作

    ```java
    @OnTimer([[value=]?<value>]?)
    ```
    - value 用于指定时间
    时间被指定为此注解的long类型“value”属性。请参考示例[Histogram.java](https://github.com/btraceio/btrace/blob/master/samples/Histogram.java)

- @OnError
	> 用于指定每当通过跟踪某个其他探测器的操作抛出任何异常时运行的操作

	当同一BTrace类中的任何其他BTrace操作方法抛出任何异常时，将调用此注解注解过的BTrace方法。

- @OnExit
	> 用于指定当BTrace代码调用“exit（int）”内置函数来完成跟踪“session”时运行的操作

	请参考例子[ProbeExit.java](https://github.com/jbachorik/btrace/blob/master/samples/ProbeExit.java).

- @OnEvent
	> 用于将跟踪方法与BTrace客户端发送的“外部”event相关联。

    ```java
    @OnEvent([[value=]?<event_name>]?)
    ```
    - event_name 此处理程序将响应的event的名称
    当BTrace客户端发送“event”时，将调用由此注解注解过的BTrace方法。 客户端可以基于某种形式的用户请求而发送event（如按Ctrl-C或GUI菜单）.字符串值可以用作event的名称。 这样，当外部event“触发”时，可以执行某些跟踪动作。
    到目前为止，命令行BTrace客户端每当按Ctrl-C（SIGINT）时发送“event”。 在SIGINT上，显示一个控制台菜单以发送event或退出客户端[这是SIGINT的默认设置]。 请参阅样本[HistoOnEvent.java](https://github.com/jbachorik/btrace/blob/master/samples/HistoOnEvent.java)

- @OnLowMemory
	> 用于跟踪超过内存阈值的event

	参考例子[MemAlerter.java](https://github.com/jbachorik/btrace/blob/master/samples/MemAlerter.java)

- @OnProbe
	> 用于指定避免在DTrace脚本中使用内部类实现

	```java
    @OnProbe(namespace=<namespace_name>, name=<probe_name>)
    ```
    - namespace_name 是以java包形式的任意命名空间名称
    - probe_name 探针名称

    @OnProbe探针格式由BTrace VM代理映射到一个或多个@OnMethod格式中。 目前，这种映射是使用XML探针描述符文件[由BTrace代理访问]完成的。 请参考示例[SocketTracker1.java](https://github.com/jbachorik/btrace/blob/master/samples/SocketTracker1.java)和相关联的探测器描述符文件[java.net.socket.xml](https://github.com/jbachorik/btrace/blob/master/samples/java.net.socket.xml)。

    运行此示例时，需要在目标JVM运行的目录中复制此xml文件（或者将btracer.bat中的probeDescPath选项修复为指向.xml文件所在的位置）。

- @Sampled
	> 启用对注解的处理程序的采样。 与@OnMethod注释结合使用。

    ```java
    @Sampled([kind=<sampler_kind>[,mean=<number>])
    ```
    - 是Sampler.Const，Sampler.Adaptive和Sampler.None之一

##参数注解
- @ProbeClassName
	> 用于标记处理程序方法的参数，使用当前拦截方法的类名。 仅适用于由@OnMethod注解的方法

- @ProbeMethodName
	> 用于标记处理程序方法的参数，使用当前拦截方法的类名。 仅适用于由@OnMethod注解的方法

	```java
    @ProbeMethodName([fqn=(true|false)]?)
    ```
    - fqn 表示应使用完全限定名而不是简单名称; 默认为false

- @Self
	> 表示用于访问当前拦截的方法的封闭实例的参数。 将此注解用于处理程序参数将有效地过滤掉所有其他匹配的静态方法（没有要使用的实例）

- @Return
	> 此注解的参数将包含当前拦截方法的返回值。 仅适用于location = @Location(Kind.RETURN)，并且将仅拦截返回非空值的方法

- @Duration
	> 方法执行持续时间将在由@Duration注解的参数提供。 仅适用于location = @Location(Kind.RETURN)和long参数

- @TargetInstance
	> 这个注解将提供对当前被拦截的方法的被调用实例（类似于@Self）的访问，但是目标方法调用或字段访问仅适用于location = @ Location（[Kind.CALL | Kind.FIELD_GET | Kind.FIELD_SET） 非静态方法调用或字段访问

- @TargetMethodOrField
	> 此注解将提供对正在调用的方法或正在访问的字段（类似于@ProbeMethodName）的名称的访问,适用于location = @ Location（[Kind.CALL | Kind.FIELD_GET | Kind.FIELD_SET）
	```java
    @TargetMethodOrField([fqn=(true|false)]?)
    ```
    - fqn 表示应使用完全限定名而不是简单名称; 默认为false

##相关实例说明
> 地址在[这里](https://github.com/btraceio/btrace/tree/master/samples)
BTrace自带的sample是学习BTrace的最后资料，熟练使用BTrace中提供的sample并且能够手动进行验证，可以快速的熟悉BTrace并加载应用，自带的sample也有很大一部分可以直接或者稍加修改就可以成为我们的定位脚本，方便使用。

-  AWTEventTracer.java - 演示了对EventQueue.dispatchEvent()事件进行trace的做法, 可以通过instanceof来对事件进行过滤, 比如这里只针对focus事件trace.
-  AllLines.java - 演示了如何在被trace的程序到达probe指定的类和指定的行号时执行指定的操作(例子中指定的行号是-1表示任意行).
-  AllSync.java - 演示了如何在进入/退出同步块进行trace.
-  ArgArray.java - 演示了打印java.io包下所有类的readXXX方法的输入参数.
-  Classload.java - 演示打印成功加载指定类以及堆栈信息.
-  CommandArg.java - 演示如何获取btrace命令行参数.
-  Deadlock.java - 演示了@OnTimer注解和内置deadlock()方法的用法
-  DTraceInline.java - 演示@DTrace注解的用法
-  DTraceDemoRef.java - 演示@DTraceRef 注解的用法.
-  FileTracker.java - 演示了如何对File{Input/Output}Stream构造函数中初始化打开文件的读写文件操作进行trace.
-  FinalizeTracker.java - 演示了如何打印一个类所有的属性, 这个在调试和故障分析中非常有用. 这里的例子是打印FileInputStream类的close() /finalize() 方法被调用时的信息.
-  Histogram.java - 演示了统计javax.swing.JComponent在一个应用中被创建了多少次.
-  HistogramBean.java - 同上例, 只不过演示了如何与JMX集成, 这里的map属性通过使用@Property注解被暴露成一个MBean.
-  HistoOnEvent.java - 同上例, 只不过演示了如何在通过按ctrl+c中断当前脚本时打印出创建次数, 而不是定时打印.
-  JdbcQueries.java - 演示了聚合(aggregation)功能. 关于聚合功能可参考DTrace.
-  JInfo.java - 演示了内置方法printVmArguments(), printProperties() 和printEnv() 的用法
-  JMap.java - 演示了内置方法dumpHeap()的用法. 即将目标应用的堆信息以二进制的形式dump出来
-  JStack.java - 演示了内置方法jstackAll()的用法, 即打印所有线程的堆栈信息.
-  LogTracer.java - 演示了如何深入实例方法(Logger.log)并调用内置方法(field() )打印私有属性内容.
-  MemAlerter.java - 演示了使用@OnLowMememory 注解监控内存使用情况. 即堆内存中的年老代达到指定值时打印出内存信息.
-  Memory.java - 演示每隔4s打印一次内存统计信息.
-  MultiClass.java - 演示了通过使用正则表达式对多个类的多个方法进行trace.
-  NewComponent.java - 使用计数器每隔一段时间检查当前应用中创建java.awt.Component的个数.
-  OnThrow.java - 当抛出异常时, 打印出异常堆栈信息.
-  ProbeExit.java - 演示@OnExit注解和内置exit(int)方法的用法
-  Profiling.java - 演示了对profile的支持.  // 我执行没成功, BTrace内部有异常
-  Sizeof.java - 演示了内置的sizeof方法的使用.
-  SocketTracker.java - 演示了对socket的creation/bind方法的trace.
-  SocketTracker1.java - 同上, 只不过使用了@OnProbe.
-  SysProp.java - 演示了使用内置方法获取系统属性, 这里是对 java.lang.System的getProperty方法进行trace.
-  SubtypeTracer.java - 演示了如何对指定超类的所有子类的指定方法进行trace.
-  ThreadCounter.java - 演示了在脚本中如何使用jvmstat 计数器. (jstat -J-Djstat.showUnsupported=true -name btrace.com.sun.btrace.samples.ThreadCounter.count 需要这样来从外部通过jstat来访问)
-  ThreadCounterBean.java - 同上, 只不过使用了JMX.
-  ThreadBean.java - 演示了对预编译器的使用(并结合了JMX).
-  ThreadStart.java - 演示了脚本中DTrace的用法.
-  Timers.java - 演示了在一个脚本中同时使用多个@OnTimer
-  URLTracker.java - 演示了在每次URL.openConnection成功返回时打印出url. 这里也使用了D语言脚本.
-  WebServiceTracker.java - 演示了如何根据注解进行trace.