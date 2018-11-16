# 用法

Java 中，final 关键字可以在多处使用，可以修饰类、方法、变量，效果都很类似，但又各有差异。可谓是“关键字重载”的典范。

总体来说，final 带有一种“不可变”的意思。其各种用法可参考下表：

| 修饰对象 | 效果 |
|:--|:--|
| 类 | 不能被继承 |
| 静态方法 | 不能被子类隐藏 |
| 实例方法 | 不能被子类重写 |
| 变量 | 不能重新赋值 |

# 知识点

虽然，看起来用法也不算很多，但整理一下相关知识点，发现还是有不少的。

* 变量
  * 不能重新赋值
  * 初始化
    * 静态变量初始化时机
    * 实例变量初始化时机
    * 局部变量初始化时机
    * final 域“冻结”时机
    * 空白 final 
    * 常量
  * 隐式 final
  * 事实 final
  * 写受保护的域
* 方法
  * 静态绑定
* 综合
  * 多线程
  * 兼容性
* 比较
  * final vs. finally vs. finalize()
  * final vs. abstract

# 说明

## 变量

### 不能重新赋值



### 初始化

关于 final 变量初始化，首先需要知道“空白 final”的概念。空白 final 指声明缺少初始化器的 final 变量。换句话说，如果声明时赋值了，那就不是空白 final。

各种变量都可以声明为空白 final，但其后续赋值时机限制是不同的。

* 对于局部变量而言，只需要在使用该变量前为其赋值即可。

* 对于静态变量而言，需要在类初始化完成前赋值。
* 对于实例变量而言，需要在实例初始化完成前赋值。

因此，静态变量只有 2 个赋值时机：声明时和 static 初始化块中；实例变量有 3 个赋值时机：声明时、普通初始化块中和构造器中。



另外一种常见的特殊的变量是常量变量，也就是我们常说的“常量”。

常量变量指用常量表达式初始化的 final 变量。至于**常量表达式**的准确含义与组成成分可参考 [JLS15.28](https://docs.oracle.com/javase/specs/jls/se11/html/jls-15.html#jls-15.28)。

因此，常量明显有以下特点：

* final 变量
* 声明时赋值
* 使用常量表达式赋值

然而……这些特点个个都是“坑”，我们一一来看。

首先，常量必然是 final 的，但不必是 static 的。虽然，我们最常见的是静态常量，声明类似 `public static final int i = 42;`，但常量也可以是实例变量，甚至是局部变量。

其次，常量必需声明时赋值。换句话说，空白 final 不是常量。

最后，常量必需用常量表达式赋值。因此，常量必然是基本数据类型或字符串类型。常量表达式组成成分很繁杂，简单概括来说，它是由字面量（即基本数据类型和字符串）或其它常量经过适当操作符运算的表达式。

示例1-1

```java
public class FinalTest {
    private static final int i = 42;    // 静态常量
    private final int j = 43;           // 实例常量

    @Test
    public void test() {
        final int k = 44;               // 局部常量
        final int p;                    // 空白 final，不是常量
        p = 45;
        final Integer q = 99;           // 非基本数据类型和字符串类型变量，不是常量

        System.out.println(i);
        System.out.println(j);
        System.out.println(k);
        System.out.println(p);
        System.out.println(q);
    }
}
```

对于常量，编译器会对常量表达式进行“预先计算”替换为结果字面量，其它对常量的引用也会替换为该结果字面量。

下面的代码就是上例代码编译后再反编译的代码，可以看到这些替换。

```java
public class FinalTest
{
  private static final int i = 42;
  private final int j = 43;
  
  @Test
  public void test()
  {
    int k = 44;
    
    int p = 45;
    Integer q = Integer.valueOf(99);
    System.out.println(42);
    System.out.println(43);
    System.out.println(44);
    System.out.println(p);
    System.out.println(q);
  }
}
```



### 隐式 final



### 事实 final



### 写保护的域



## 方法



## 综合



## 多线程



## 兼容性



## 比较

### final vs. finally vs. finalize()

语义上说，它们几乎没有相关性。

面试中经常把它们放在一起讨论区别，大概仅仅是它们单词的相似性而已吧！但是，你应该清楚它们各自指的是什么。

### final vs. abstract

final 与 abstract 在语义上是互斥的，因此不能同时使用。



# 意外？例外



# 遗留问题

* 常量为什么必需声明时赋值？
* 常量表达式中怎样算是必要的字符串强制转换？



final 修饰的成员变量不能仅执行默认初始化，而不显式地赋值，即所谓的“空白 final”是非法的。

一个与 final 相关的最佳实践是，所有方法参数都应该用 final 修饰。

---

另一个常讨论的问题是：为什么在内部类中引用的外部变量必须是 final 的？（Java 8 放宽了限制，只要事实上内部类中没有对外部变量重新赋值即可，不必显式使用 final 修饰）
该问题说法不尽相同，比较可信说法是：内部类使用的外部变量将以值拷贝的形式被内部类中自动生成的隐藏变量持有，因此它们是不同的引用，为了使其同步，因此必须是不可变的。