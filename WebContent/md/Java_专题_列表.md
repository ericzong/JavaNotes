# 概述

List 接口是有序列表的数据结构抽象。

常用的接口与类如下：

* List Ⓘ
  * AbstractList Ⓐ
    * ArrayList Ⓒ
    * AbstractSequentialList Ⓐ
      * LinkedList Ⓒ
    * Vector Ⓒ
      * Stack Ⓒ
  * CopyOnWriteArrayList Ⓒ

# 实现（openjdk11）

## ArrayList

```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
        
private static final int DEFAULT_CAPACITY = 10;

private static final Object[] EMPTY_ELEMENTDATA = {};
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

transient Object[] elementData; // non-private to simplify nested class access
```

从上面部分类定义分段应该注意到：

* ArrayList 实现了标记接口 RandomAccess。其随机访问特性来源于底层数组存储结构。
* 用以共享的空数组有两个。

### 构造器

```java
public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}

public ArrayList(int initialCapacity) {
    if (initialCapacity > 0) {
        this.elementData = new Object[initialCapacity];
    } else if (initialCapacity == 0) {
        this.elementData = EMPTY_ELEMENTDATA;
    } else {
        throw new IllegalArgumentException("Illegal Capacity: "+
                                           initialCapacity);
    }
}

public ArrayList(Collection<? extends E> c) {
    elementData = c.toArray();
    if ((size = elementData.length) != 0) {
        // defend against c.toArray (incorrectly) not returning Object[]
        // (see e.g. https://bugs.openjdk.java.net/browse/JDK-6260652)
        if (elementData.getClass() != Object[].class)
            elementData = Arrays.copyOf(elementData, size, Object[].class);
    } else {
        this.elementData = EMPTY_ELEMENTDATA;
    }
}
```

从 3 个构造器可以看出，仅当无参构造时内部数组使用 DEFAULTCAPACITY_EMPTY_ELEMENTDATA，而当指定容量为 0 或以空集合初始化时内部数组使用的是 EMPTY_ELEMENTDATA。

> **“不良”源代码**
>
> ArrayList(Collection<? extends E> c) 构造器内，当用以初始化的集合大小不为 0 时，有一段 bug 修正代码（行 21、22），用以修正代码 `c.toArray();` 得到非 Object[] 的情况。
>
> 这种情况下，内部数组将被复制两次，但第一次复制是不必要的。

### 添加

添加元素操作中，多是元素的拷贝和移动等，核心是理解新容量的计算即 `newCapacity(int minCapacity)` 方法：

```java
private int newCapacity(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    if (newCapacity - minCapacity <= 0) {
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA)
            return Math.max(DEFAULT_CAPACITY, minCapacity);
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return minCapacity;
    }
    return (newCapacity - MAX_ARRAY_SIZE <= 0)
        ? newCapacity
        : hugeCapacity(minCapacity);
}
```

* 默认新容量（newCapacity）为原来（oldCapacity）的 1.5 倍。// 行 4
* 如果默认新容量（newCapacity）比要求最小容量（minCapacity）小，则： // 行5
  * 如果当前列表是用无参构造器新创建的，实际新容量为默认容量（DEFAULT_CAPACITY）与要求最小容量（minCapacity）中较大的那个。// 行 6, 7
  * 如果要求最小容量（minCapacity）小于 0，说明数组大小溢出。// 行 8, 9
  * 否则，默认新容量（newCapacity）——即按 1.5 倍扩容。
* 否则，比较默认新容量（newCapacity）与列表最大容量（MAX_ARRAY_SIZE）：
  * 如果前者大，使用 hugeCapacity(minCapacity)。
  * 否则，默认新容量（newCapacity）——即按 1.5 倍扩容。

## LinkedList



## CopyOnWriteArrayList



# 过时的

## Vector



## Stack



# FAQ

ArrayList 底层数组域是 transient 的。

ArrayList 有两个用以共享的空数组。