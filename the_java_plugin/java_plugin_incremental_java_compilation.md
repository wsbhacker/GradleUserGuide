## 22.12.增量Java编译

从Gradle2.1开始,可以使用Java增量编译,此功能正在孵化,参见[JavaCompile](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.compile.JavaCompile.html)如何启用这个功能. 增量编译的主要目标如下:
- 避免在没必要编译的java编译资源上浪费时间.这意味着更快构建,尤其是在改变一些class与jar的时候,不需要再次编译那些不依赖这些class与jar的文件.
- 尽可能地少输出class.类不需要重新编译意味着保持输出目录不变。一个示例场景中，真正使用JRebel的真正有用的是 - 越少的输出类被改变，JVM可以使用越快刷新。

更高级的增量编译:
- 检测陈旧类的设置是否正确是以牺牲速度为代价的,该算法分析字节码并与编译器直接交互(非私有常量内联),依赖传递等.举个例子:当一个类的公共常量改变后,我们希望避免由编译器编译内联常数产生的问题,我们将调整算法和缓存以便增量Java编译可以是每编译任务的默认设置。
- 为了使增量编译快，我们缓存会分析class的结果和jar快照。最初的增量编译应该会慢于cold caches.