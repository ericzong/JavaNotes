# API

## 格式化

可以使用 s 转换符格式化任意的对象。实现 Formattable 接口的对象都将调用 fromatTo 方法；否则调用 toString 方法。

指定参数索引：索引必须紧跟 % 后，并以 \$ 终止。

前向引用：%<x，以 < 指定再次引用上一个参数。

参数索引值从 1 开始，而非 0，避免与 0 标志混淆。

## 断言

在启用或禁用断言时不必重新编译程序。启用或禁用断言是类加载器（class loader）的功能。当断言被禁用时，类加载器将跳过断言代码，因此，不会降低程序运行的速度。

Java -ea:MyClass -ea:com.mycompany.mylib... MyApp
将开启MyClass类以及在com.mycompany.mylib包和它的子包中的所有类的断言。选项-ea将开启默认包中的所有类的断言。

禁用断言：-disableassertions或-da
Java -ea:... -da:MyClass MyApp

启用和禁用所有断言的-ea和-da开关不能应用到那些没有类加载器的“系统类”上。对于这些系统类来说，需要使用-enablesystemassertions/-esa开关。



# 特性

## 自动装箱与拆箱

自动装箱规范要求boolean、byte、char<=127，介于-128~127之间的short和int被包装到固定的对象中。

装箱和拆箱是编译器认可的，而不是虚拟机。编译器在生成类的字节码时，插入必要的方法调用。虚拟机只是执行字节码。

## 可变参数

可以将已经存在且最后一个参数是数组的方法重新定义为可变参数的方法，而不会破坏任何已经存在的代码。

# Java 7 新特性

## TWR(Try-with-resources)

要确保try-with-resources生效，正确的用法是为各个资源声明独立变量。

try 子句中出现的资源类都必须实现 AutoCloseable 接口。

# 附录

## 隐式修饰

接口方法：public

接口域：public static final 

接口内部类：public static 

