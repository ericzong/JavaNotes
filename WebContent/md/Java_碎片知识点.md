# 基本概念

## 变量

### 字母和数字

Java 中“字母”和“数字”的范围更大。

* 字母：’A’~’Z’、’a’~’z’、’_’、’$’、某种语言中代表字母的任何Unicode字符。
* 数字：’0’~’9’、某种语言中代表数字的任何字符。

### 声明与定义

Java 不区分变量声明与定义。

> C 和 C++ 区分变量的声明与定义。如：int i = 1; 是一个定义，而 extern int i; 是一个声明。

## 数组

所有的数组类型均包含一个public 的clone方法。

# 语法 

## 块（block）

不能在嵌套的两个块中声明同名的变量。

> C++ 中，可以在嵌套块中重新定义一个变量。内层定义的变量会覆盖外层定义。
> 这有可能导致程序设计错误，Java 中不允许。

## switch

### case 表达式

case 表达式必须是常量，null 不是常量。

### case 语句中变量的作用域

switch 的 case 语句，由于结构清晰，可以不加花括号。但是，是否有花括号直接影响声明变量的作用域。
如果 case 语句在块中声明变量，则变量作用域明显在块内；否则，作用域在 switch 块内，声明之后均可使用——但要注意，所有使用该变量的 case 语句都必须初始化它，否则编译错误，提示局部变量未初始化。

### 贯穿（fallthrough）

应使用 -Xlint:fallthrough 进行编译检查；如果是编程技巧造成的，应使用注解 @SuppressWarnings("fallthrough")抑制。

> 贯穿现象很容易理解，形式上说，case 分支类似标签的概念，跳转到某个分支后除非 break 出 switcjh，否则会顺序执行。

#   面向对象

## 类型

### 强制类型转换

进行类型转换的唯一原因是：在暂时忽视对象的实际类型之后，使用对象的全部功能。

### Class 对象

 一个Class对象实际上表示的是一个类型，而这个类型未必一定是一种类。

```
例如，int不是类，但int.class是一个Class类型的对象。
```

## 继承

### 父子类

关键字extends表明正在构造的新类派生于一个已存在的类。已存在的类称为超类（superclass）、基类（base class）或父类（parent class）；新类称为子类（subclass）、派生类（derived class）或孩子类（child class）。

前缀“超”和“子”来源于计算机科学和数学理论中的集合语言的术语。

### super 

Super跟this不同，因为它不是一个对象的引用，不能将super赋给另一个对象变量，而只是一个指示编译器调用超类方法的特殊关键字。

### final

如果将一个类声明为final，只有其中的方法自动地成为final，而不包括域。

### 重写 equals

1. 显式参数命名为otherObject，稍后需要将它转换成另一个叫做other的变量。
2. 检测this与otherObject是否引用同一个对象：if(this == otherObject) return true;（优化）
3. 检测otherObject是否为null，如果为null，返回false。If(otherObject == null) return false;（必要）


4. 比较this与otherObject是否属于同一个类。如果equals的语义在每个子类中有所改变，就使用getClass检测：if(getClass() != otherObject.getClass()) return false;；如果所有子类都拥有统一的语义，就使用instanceof检测：if(!(otherObject instanceof ClassName)) return false;


5. 将otherObject转换为相应的类类型变量：ClassName other = (ClassName)otherObject;


6. 对所有需要比较的域进行比较。使用==比较基本类型域，使用equals比较对象域。如果都匹配，返回true；否则返回false。

# API

## 格式化

可以使用 s 转换符格式化任意的对象。实现 Formattable 接口的对象都将调用 fromatTo 方法；否则调用 toString 方法。

指定参数索引：索引必须紧跟 % 后，并以 \$ 终止。

前向引用：%<x，以 < 指定再次引用上一个参数。

参数索引值从 1 开始，而非 0，避免与 0 标志混淆。

## 比较

Comparable.compareTo() 应该是反对称的，即x.compareTo(y)和y.compareTo(x)返回数值应具有相反符号或同时抛出异常。

如果存在这样一种通用算法，它能够对两个不同的子类对象进行比较，则应该在超类中提供一个compareTo方法，并将这个方法声明为final。

## 断言

在启用或禁用断言时不必重新编译程序。启用或禁用断言是类加载器（class loader）的功能。当断言被禁用时，类加载器将跳过断言代码，因此，不会降低程序运行的速度。

Java -ea:MyClass -ea:com.mycompany.mylib... MyApp
将开启MyClass类以及在com.mycompany.mylib包和它的子包中的所有类的断言。选项-ea将开启默认包中的所有类的断言。

禁用断言：-disableassertions或-da
Java -ea:... -da:MyClass MyApp

启用和禁用所有断言的-ea和-da开关不能应用到那些没有类加载器的“系统类”上。对于这些系统类来说，需要使用-enablesystemassertions/-esa开关。

## 集合

应该将Java迭代器认为是位于两个元素之间。当调用next时，迭代器就超过下一个元素，并返回刚刚越过的那个元素的引用。

对于并发修改列表的检测有一个奇怪的例外。链表只负责跟踪对列表的结构性修改，例如，添加元素、删除元素。set操作不被视为结构性修改。可以将多个迭代器附加给一个链表，所有的迭代器都调用set方法对现有结点的内容进行修改。

LinkedList.get()方法做了微小的优化：如果索引大于size()/2就从列表尾端开始搜索元素。

keySet 既不是HashSet，也不是TreeSet，而是实现了Set接口的某个其他类的对象。

当对键的唯一引用来自散列表条目时，WeakHashMap将与垃圾回收器协同工作一起删除键/值对。WeakHashMap使用弱引用（weak references）保存键。

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

