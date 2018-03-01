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

 