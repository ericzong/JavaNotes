# POM 配置

## 生成可运行 jar 包

需要借助 maven-shade-plugin。

```xml
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-shade-plugin</artifactId>
  <version>3.1.1</version>
  <executions>
    <execution>
      <phase>package</phase>
      <goals>
        <goal>shade</goal>
      </goals>
      <configuration>
        <transformers>
          <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
            <mainClass>com.ericzong.maven.App</mainClass>
          </transformer>
        </transformers>
      </configuration>
    </execution>
  </executions>
</plugin>
```

## 自定义绑定插件目标

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-source-plugin</artifactId>
    <version>3.0.1</version>
    <executions>
        <execution>
            <id>attach-sources</id>
            <phase>verify</phase>
            <goals>
                <goal>jar-no-fork</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

## 聚合项目

```xml
...
  <packaging>pom</packaging>
...
  <modules>
      <module>../module1</module>
      <module>../module2</module>
      <module>../module3</module>
  </modules>
```

聚合模块核心配置即为 \<modules\> 元素，且打包方式必须为 pom。

聚合模块是帮助聚合其他模块构建的工具，本身无实质内容。

反应堆，所有模块组成的构建结构。根据各模块的相互依赖情况确定反应堆构建顺序（Reactor Build Order）。

## 继承

```xml
<parent>
    <groupId>...</groupId>
    <artifactId>...</artifactId>
    <version>...</version>
    <relativePath>../xxx</relativePath>
</parent>

<dependencyManagement></dependencyManagement>

<build>
    <pluginManagement>
        ...
    </pluginManagement>
</build>
```

父模块消除配置重复，不包含除 POM 之外的项目文件。

Maven 默认父 POM 在上一层目录下。

# settings.xml 配置

## 本地仓库

```xml
<localRepository>E:/maven</localRepository>
```

## 镜像

```xml
<mirror>
	<id>nexus-mirror</id>
	<mirrorOf>*</mirrorOf>
	<name>Nexus Mirror</name>
	<url>http://localhost:8888/nexus/repository/maven-public/</url>
</mirror>
```

\<mirrorOf\> 可以使用通配符等高级配置：

| 配置值       | 说明           |
| ------------ | -------------- |
| *            |                |
| external: *  | 所有非本机仓库 |
| repo1, repo2 | 逗号分隔       |
| *, !repo1    | ! 排除         |

## profile

```xml
  <profiles>
	<profile>
		<id>jdk-1.8</id>
		<activation>
			<activeByDefault>true</activeByDefault>
			<jdk>1.8</jdk>
		</activation>
		<properties>
			<maven.compiler.source>1.8</maven.compiler.source>
			<maven.compiler.target>1.8</maven.compiler.target>
			<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
		</properties>
	</profile>
	
	<profile>
		<id>nexus-local</id>
		<repositories>
			<repository>
				<id>nexus</id>
				<url>http://localhost:8888/nexus/repository/maven-public/</url>
				<releases>
					<enabled>true</enabled>
				</releases>
				<snapshots>
					<enabled>true</enabled>
				</snapshots>
			</repository>
		</repositories>
		
		<pluginRepositories>
			<pluginRepository>
				<id>nexus</id>
				<url>http://localhost:8888/nexus/repository/maven-public/</url>
				<releases>
					<enabled>true</enabled>
				</releases>
				<snapshots>
					<enabled>true</enabled>
				</snapshots>
			</pluginRepository>
		</pluginRepositories>
	</profile>
  </profiles>

  <activeProfiles>
	<activeProfile>jdk-1.8</activeProfile>
	<activeProfile>nexus-local</activeProfile>
  </activeProfiles>
```

## 发布配置

```xml
  <servers>
	<server>
		<id>nexus-releases</id>
		<username>zonglu</username>
		<password>Eric_iqia88</password>
	</server>
	<server>
		<id>nexus-snapshots</id>
		<username>zonglu</username>
		<password>Eric_iqia88</password>
	</server>
  </servers>
```

