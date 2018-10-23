# 关键字

## static 

起初，static  关键字由 C 引入，表示退出一个块后依然存在的局部变量。变量一直存在，当再次进入该块时仍然存在。

随后，C 重用 static 关键字，表示不能被其他文件访问的全局变量和函数。

最后，C++ 再次重用该关键字，表示属于类且不属于类对象的变量和函数。这与 Java 同义。

# API 

## Timestamp

Timestamp类继承自java.util.Date，而后者的equals方法使用一个instanceof测试，这样一来就无法覆盖实现equals使之同时做到对称且正确。

# 特性

## 自动装箱（autoboxing)

  自动装箱（autoboxing）：自动打包（autowrapping）或许更加合适，而“装箱（boxing）”这个词源自于C#。

 # 惯例

## HelloWorld

大多数语言的入门第一个程序就是“Hello, World”，其功能是简单输出“Hello, World”字符串（到屏幕、终端或者控制台）。

这种简单的程序语言入门示例源自 Brian Kernighan 编写的《C 程序设计语言》，准确地说，这种形式的示例来自他本人早期写的一部 B 语言编程教程。随着 C 语言的广为流传，这种示例程序也被后来的其他语言的教程沿用。