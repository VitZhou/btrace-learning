跟踪脚本
======
>翻译至[原文](https://github.com/btraceio/btrace/wiki/Trace-Scripts)

跟踪脚本定义应跟踪什么和如何跟踪。 它们是普通的java注解类。 注解指定应在何处放置探针，以及应使哪些数据可用于跟踪操作。

##脚本元素
- Probe Point
	执行一组跟踪语句的“location”或“event”。 探测点是我们想要执行一些跟踪语句的感兴趣的“location”或“event”。

- Trace Actions或Actions
	每当探测器“触发”时执行的跟踪语句。

- Action方法
	在静态方法类中定义的，BTrace在探测器触发时执行的跟踪语句。这样的方法被称为“Action”方法。

##限制
为了确保注入代码的安全性，强制执行以下限制：
-  BTrace禁止new类、数组,、抛异常、捕获异常
-  禁止调用除com.sun.btrace.BTraceUtil类的其他实例方法以及静态方法
-  BTrace1.2前不能有实例字段和方法，只能有无返回值的静态方法，所有字段也都必须是静态的。
-  禁止定义外部、内部、匿名, 本地类
-  禁止有同步块和同步方法
-  禁止有循环(for, while, do..while)
-  禁止实现接口, 不能扩展类，直接超类必须是java.lang.Object
-  禁止使用assert语句, 不能使用class字面值
-  禁止使用class字节码


