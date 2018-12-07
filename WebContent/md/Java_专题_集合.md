# 概述

Set 接口是数学概念“集合”的数据结构抽象，不包含重复元素。

常用的接口与类有如下一些：

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

对于 HashSet 而言，它是无序的。因此，不可能通过索引等方式来遍历，仅能使用其迭代器进行遍历（包括 foreach 循环）。

显示使用迭代器 remove() 元素是合法的。但是，在 foreach 循环中移除元素通常是非法的，这被认为是一个并发修改，会引起快速失败，抛出 java.util.ConcurrentModificationException。

> 在 foreach 循环中删除元素能正常执行的一个特例是，只要删除的元素是最后一个迭代元素即可。比如：
>
> ```java
> public void testRemoveWhileForEach() {
>     HashSet<String> set = new HashSet<>();
>     set.add("1");
>     set.add("2");
>     set.add("3");
> 
>     for (String s: set) {
>         if(s.equals("3")) {
>             set.remove(s);
>         }
>     }
>     Assert.assertEquals(set.size(), 2);
> }
> ```
>
> 这是因为并发修改的检查是在 iterator.next() 时，而移除最后一个元素后不会再调用该方法，所以不会引发异常。
>
> 但是，我们不应该基于此特例编程。HashSet 中元素的排序是“不确定”的，不应基于任何假设排序编程，这可能导致在不同 JVM 实现中呈现不确定行为。

## LinkedHashSet

继承自 HashSet，区别在于 LinkedHashSet 中的元素是按加入先后有序的。

为了代码简洁，HashSet 甚至声明了一个构造器专用于 LinkedHashSet 的各构造器调用。

```java
HashSet(int initialCapacity, float loadFactor, boolean dummy) {
    map = new LinkedHashMap<>(initialCapacity, loadFactor);
}
```

可见，LinkedHashSet 是基于 LinkedHashMap 的。

> 注意，上面 HashSet  构造器，参数 `dummy` 如字面暗示的一样，是一个“假参数”，并没有用到。它唯一的作用就是区分另一个 `HashSet(int initialCapacity, float loadFactor)` 构造器。

## TreeSet

TreeSet 基于 TreeMap。

它是有序的，但与 LinkedHashSet 不同的是，该顺序不是元素加入序，而是元素的自然顺序和指定顺序。

如果使用自然顺序，那么 add() 到 TreeSet 中的元素类型必然要实现 Comparable 接口，否则会抛出 java.lang.ClassCastException。

如果要使用指定顺序，应使用 TreeSet(Comparator<? super E> comparator) 构造器，并传入 Comparator 实现指定排序。此时，不要求元素类型实现 Comparable 接口。

## EnumSet

EnumSet 是专为枚举设计的集合，主要是为了更好的性能。

注意 EnumSet 是一个抽象类，其中有很多创建实例的工厂方法，比如：allOf()、noneOf()、of()……它们返回的是 EnumSet 的两个默认访问权限的子类 RegularEnumSet 和 JumboEnumSet。

正如两个子类名暗示的一样，当要生成常规的枚举集合时，返回 RegularEnumSet 实例；而生成特大枚举集合时，返回 JumboEnumSet 实例。

所谓“常规”和“特大”的区别是集合大小，64个元素及以下的为常规，超过为特大。

从下面 RegularEnumSet 的部分代码可以看出，它的数据是以位向量（即 elements）的形式存储的，因此它很小；相关操作基本都是位运算，所以性能很好。

```java
private long elements = 0L;

public boolean add(E e) {
    typeCheck(e);

    long oldElements = elements;
    elements |= (1L << ((Enum<?>)e).ordinal());
    return elements != oldElements;
}
```

不过，由于位向量是 long 型的，因此至多只能表示 64 个元素，超过就需要另一种实现方式了——位向量需要是一个 long 数组——这就是 JumboEnumSet 的位向量：



```java
private long elements[];
```

## CopyOnWriteArraySet

基于 CopyOnWriteArrayList 实现。

线程安全。

适用范围：尺寸小，读操作远多于写操作。

其迭代器不支持 remove()。

迭代器工作在快照版本上。