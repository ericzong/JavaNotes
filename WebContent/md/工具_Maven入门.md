# 概述

Maven 是一个跨平台项目管理工具，主要用于基于 Java 的项目构建、依赖管理和项目信息管理。

在 Maven 中，所有的依赖项，甚至项目本身都是“构件”，并使用坐标唯一标识一个构件。

坐标，是每个构件的唯一标识。其元素包括：groupId、artifactId、version、packaging、classifier。

| 坐标元素   | 定义       | 说明           | 示例                                                |
| ---------- | ---------- | -------------- | --------------------------------------------------- |
| groupId    | 必需       | 构件所属项目名 |                                                     |
| artifactId | 必需       | 构件所属模块名 |                                                     |
| version    | 必需       | 版本           |                                                     |
| packaging  | 可选       | 打包方式       | jar（默认）, war...                                 |
| classifier | 不直接定义 | 附属构件       | javadoc, sources, jdk15（testNG 5.8 的 Java5 版本） |

> classifier 不直接定义，因为附属构件不是项目直接默认生成的，而是由附加插件帮助生成的。

Maven 项目核心是 pom.xml，POM（Project Object Model，项目对象模型）。

Maven 项目优点之一是解耦/正交性，指项目对象模型最大程度地与实际代码相互独立。

# 关于依赖

依赖范围，用以指定构件在哪些 classpath 中有效。由 \<scope\> 元素定义。被依赖的构件可在 3 个 classpath 中使用，分别是编译主代码、编译测试代码、运行时。参考[这里](工具_Maven参考.md)

传递性依赖，即间接依赖。构件A依赖构件B，而构件B依赖构件C，则A对C即为传递依赖。传递依赖会影响依赖范围（参考[这里](工具_Maven参考.md)）。

依赖调解（Dependency Mediation），当传递依赖出现同一构件的不同版本时的选择机制。其原则是：路径最近者优先；第一声明者优先。

可选依赖，非必须可选依赖。由 \<optional\> 元素定义。不传递。典型应用场景是数据库驱动程序，多个驱动通常不需同时使用，只使用某一种，则驱动依赖应定义为可选的。


# 命令参考

```maven
# 生成项目骨架
mvn archetype:generate
# 查看已解析依赖（Resolved Dependency）
mvn dependency:list
# 查看依赖树
mvn dependency:tree
# 分析依赖
mvn dependency:analyze
```

> 分析依赖的命令结果中主要有两个部分。
>
> Used undeclared dependencies，项目中使用到但未显式声明的依赖。使用主要指 import 导入等，通过直接依赖传递引入，升级直接依赖时可能导致问题。
>
> Unused declared dependencies，项目中未使用但显式声明的依赖。理论上可以删除，但通常这些依赖是运行时所需的，删除前需要仔细地分析。