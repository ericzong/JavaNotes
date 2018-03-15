# 预备知识

## 创建对象的方式

1. new
2. Class.newInstance()
3. Constructor.newInstance()
4. Object.clone()
5. 反序列化

## 数据类型

Java 的数据类型可分为两大类：基本数据类型和引用类型。

简单来说，基本数据类型直接存储于栈中；而引用类型仅将引用存储于栈中，真实对象存储在堆中。

# 拷贝对象

我们使用 clone() 方法拷贝对象，该方法是 Object 对象声明的，如下：

```java
protected native Object clone() throws CloneNotSupportedException;
```

注意：clone() 方法是一个本地方法，换句话说，它不是 Java 本身实现的。

# 深拷贝 vs. 浅拷贝



## 实现深拷贝

