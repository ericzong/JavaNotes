# 概述

Set 接口是数学概念“集合”的数据结构抽象，不包含重复元素。

常用见的接口与类有如下一些：

* Set Ⓘ
  * AbstractSet Ⓐ
    * HashSet Ⓒ
      * LinkedHashSet Ⓒ
    * EnumSet Ⓐ
    * CopyOnWriteArraySet Ⓒ
    * TreeSet Ⓒ
  * SortedSet Ⓘ
    * NavigableSet Ⓘ
    * TreeSet Ⓒ

# 实现

> 源代码以 OpenJDK11 为例。

## HashSet

### 基于 HashMap

从 HashSet 的源码可以看出，HashSet 实际上是通过 HashMap 实现的。

```java
private transient HashMap<E,Object> map;

// Dummy value to associate with an Object in the backing Map
private static final Object PRESENT = new Object();
```

### 变量 PRESENT

从上面的源代码中，注意到有一个类变量 PRESENT。它的作用是作为一个共享的“占位符”，充当内部 HashMap 所有键值对的 value 值。

```java
public boolean add(E e) {
    return map.put(e, PRESENT)==null;
}

public boolean remove(Object o) {
    return map.remove(o)==PRESENT;
}
```

从上面代码可以看出，使用 PRESENT 的原因是为了方便地返回 add 和 remove 方法实际修改集合与否，而如果 value 统一使用 null 则无法通过这种方法判断。

### 元素重复判断

HashSet 通过 hashCode 查找元素应在位置，再由 equals() 确定该位置上的元素是否相同（如果有）。

因此，需确保 hashCode() 与 equals() 方法正确重写。



### 迭代删除元素



## LinkedHashSet



## TreeSet



## EnumSet



## CopyOnWriteArraySet



