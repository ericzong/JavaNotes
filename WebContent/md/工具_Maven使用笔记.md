# 最佳实践

## 设置 MAVEN_OPTS 环境变量

通常，应该通过 MAVEN_OPTS 环境变量设置 Maven 命令运行时相关 JVM 参数，比如内存参数。

MAVEN_OPTS=-Xms128m -Xmx512m

恰当的内存参数可避免当项目过大时 Maven 命令执行报 java.lang.OutOfMemoryError。

> 不应修改 mvn.bat 或 mvn 脚本文件，否则，升级 Maven 后需要再次修改。

## 使用用户配置

应使用用户配置 ~/.m2/settings.xml。

> 应避免使用全局配置 $M2_HOME/conf/settings.xml ，否则，升级 Maven 后需要再次修改。