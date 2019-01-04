# 通用工具

## 常用库

### Guava

Google的Java常用类库

https://github.com/google/guava

> Guava 中文是石榴的意思，该项目是 Google 的一个开源项目，包含许多 Google 核心的 Java 常用库。

## 文件处理

### Commons Configuration

http://commons.apache.org/proper/commons-configuration/

Commons Configuration提供了一个通用配置接口，使得一个Java程序能从各种输入源中读取配置数据。它提供对单值和多值配置参数的类型化访问。

支持的输入源：

* Properties files
* XML documents
* Windows INI files
* Property list files (plist)
* JNDI
* JDBC Datasource
* System properties
* Applet parameters
* Servlet parameters

可使用builder创建配置对象，并可混合不同配置源。
Configurations辅助类是工具入口，可以创建builder或是直接创建Configuration。
Configuration 包含众多读取属性并转换为特定类型的方法，甚至可以将多个key相同的属性读取为List或数组，也可以生成指定key前缀的配置子集（不可变）。

## AOP

### JVM-Sandbox

https://github.com/alibaba/JVM-Sandbox

JVM 沙箱容器，一种基于 JVM 的非侵入式运行期 AOP 解决方案。

## 理论实现

### java-design-patterns

设计模式的Java实现

http://java-design-patterns.com/

https://github.com/iluwatar/java-design-patterns

# 数据处理

## ElasticSearch

分布式搜索引擎

http://www.elastic.co/products/elasticsearch

> Elasticsearch 是一个分布式的 RESTful 风格的搜索和数据分析引擎，能够解决越来越多的用例。作为 Elastic Stack 的核心，它集中存储您的数据，帮助您发现意料之中以及意料之外的情况。这个实时的分布式搜索分析引擎， 它能让你以一个之前从未有过的速度和规模，去探索你的数据。

# Test

## REST Assured

对于 REST API 集成测试来说是一个很好的工具。 

## Mockito

Mock 框架，具有简单的 API、优秀的文档以及大量示例。 

## PowerMock

## JMock

## Spock 

Spock 是一款用于 Java 和 Groovy 应用程序的测试和规范框架。它用 Groovy 编写，因此它具有很强的表现力，并且非常规范。

使用 Spock 时，测试将变得更加易读易维护。此外，得益于它的 JUnit 运行器，Spock能够兼容大多数 IDE、构建工具和持续集成服务器。

参考书籍：Java testing with spock

## Tess4J

一个 OCR 工具包。

[官网](http://tess4j.sourceforge.net/)

# AI

## N-Dimensional Arrays for Java (ND4J) 

一种为 JVM 设计的科学计算库。它们被应用在生产环境中，这就意味着路径被设计成可以最小的 RAM 内存需求来快速运行。

## Deeplearning4j 

第一个为 Java 和 Scala 编写的消费级开元分布式深度学习库。它被设计成在商业环境中使用，而非研究工具。

## Encog 

一种先进的机器学习框架，支持支持向量机（Support Vector Machines），人工神经网络（Artificial Neural Networks），基因编程（Genetic Programming），贝叶斯网络（Bayesian Networks），隐马尔科夫模型（Hidden Markov Models）和 遗传算法（Genetic Algorithms）。