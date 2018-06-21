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