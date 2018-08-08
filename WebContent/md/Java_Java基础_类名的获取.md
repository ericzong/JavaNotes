# 获取类名的方法

* Class.getName()
* Class.getCanonicalName()
* Class.getSimpleName()

# 差异对比

|          | getName()                         | getCanonicalName()               | getSimpleName() |
| -------- | --------------------------------- | -------------------------------- | --------------- |
| 普通类   | pkg.ClassName                     | pkg.ClassName                    | ClassName       |
| 嵌套类   | pkg.EnclosingClass$NestedClass    | pkg.EnclosingClass.NestedClass   | NestedClass     |
| 局部类   | pkg.EnclosingClass$nLocal         | null                             | LocalClass      |
| 匿名类   | pkg.EnclosingClass$n              | null                             | 空字符串        |
| 普通数组 | [Lpkg.ClassName;                  | pkg.ClassName[]                  | ClassName[]     |
| 嵌套数组 | [Lpkg.EnclosingClass$NestedClass; | pkg.EnclosingClass.NestedClass[] | NestedClass[]   |
| 局部数组 | [Lpkg.EnclosingClass$nLocalClass; | null                             | LocalClass[]    |

小结：

* 对 getName() 而言，主要特殊之处在于：嵌套类使用“$”分隔外围类与嵌套类；数组使用 JNI 字段描述符形式
* 对 getCanonicalName() 而言，主要特殊之处在于：局部类、匿名类和局部类数组的规范名为 null
* 对 getSimpleName() 而言，主要特殊之处在于：匿名类简单名为空（字符串）

# 附录

## JNI字段描述符(JavaNative Interface FieldDescriptors)

| Java 类型 | JNI 字段描述符                                               |
| --------- | ------------------------------------------------------------ |
| boolean   | Z                                                            |
| byte      | B                                                            |
| char      | C                                                            |
| short     | S                                                            |
| int       | I                                                            |
| long      | J                                                            |
| float     | F                                                            |
| double    | D                                                            |
| void      | V                                                            |
| class     | Lclassname;<br/>（以“L”开头，“;”结尾；路径分隔符“/”；嵌套类用“$”分隔；数组前缀“[”，个数代表深度） |

注意：

getName() 返回的 JNI 字段描述符中，路径分隔符是“.”。

## 匿名类型数组

Java 没有办法创建一个匿名类型数组。反过来也是可以理解的，一个匿名类在声明时即创建了唯一的实例，并且它没有名字，故没有办法可以引用到该类型，也就不可能定义该类型的数组了。