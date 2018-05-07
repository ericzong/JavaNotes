# 概述

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
