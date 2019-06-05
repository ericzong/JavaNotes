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

# 实现（OpenJDK11）

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

* `ArrayList` 实现了标记接口 `RandomAccess`。其随机访问特性来源于底层数组存储结构。
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

从 3 个构造器可以看出，仅当无参构造时内部数组使用 `DEFAULTCAPACITY_EMPTY_ELEMENTDATA`，而当指定容量为 0 或以空集合初始化时内部数组使用的是 `EMPTY_ELEMENTDATA`。

> **“不良”源代码**
>
> `ArrayList(Collection<? extends E> c)` 构造器内，当用以初始化的集合大小不为 0 时，有一段 bug 修正代码（行 21、22），用以修正代码 `c.toArray();` 得到非 `Object[]` 的情况。
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

* 默认新容量（`newCapacity`）为原来（`oldCapacity`）的 1.5 倍。// 行 4
* 如果默认新容量（`newCapacity`）比要求最小容量（`minCapacity`）小，则： // 行 5~11
  * 如果当前列表是用无参构造器新创建的，实际新容量为默认容量（`DEFAULT_CAPACITY`）与要求最小容量（`minCapacity`）中较大的那个。// 行 6, 7
  * 如果要求最小容量（`minCapacity`）小于 0，说明数组大小溢出。// 行 8, 9
  * 否则，默认新容量（`newCapacity`）——即按 1.5 倍扩容。// 行 10
* 否则，比较默认新容量（`newCapacity`）与列表最大容量（`MAX_ARRAY_SIZE`）：// 行 12~14
  * 如果默认新容量大，使用 `hugeCapacity(minCapacity)`。// 行 14
  * 否则，默认新容量（`newCapacity`）——即按 1.5 倍扩容。// 行 13

## LinkedList

```java
public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable

transient Node<E> first;
transient Node<E> last;
```

`LinkedList` 底层是用双向链表方式实现的。

并且实现了 `Deque` 接口，说明可以从两端分别操作链表，故需要同时持有两个端点节点。也因此可以当作栈或队列来使用。

`LinkedList` 底层数据存储在嵌套类 `Node` 节点对象中，并通过双向链接进行各种操作。

```java
private static class Node<E> {
    E item;
    Node<E> next;
    Node<E> prev;

    Node(Node<E> prev, E element, Node<E> next) {
        this.item = element;
        this.next = next;
        this.prev = prev;
    }
}
```

### 查找

链表的索引查找并不复杂，根据节点前驱 `prev` 或后继 `next` 遍历链表到适当位置即可。

```java
Node<E> node(int index) {
    // assert isElementIndex(index);

    if (index < (size >> 1)) {
        Node<E> x = first;
        for (int i = 0; i < index; i++)
            x = x.next;
        return x;
    } else {
        Node<E> x = last;
        for (int i = size - 1; i > index; i--)
            x = x.prev;
        return x;
    }
}
```

这点唯一需要注意的是编程技巧，当索引过半时，是从末端开始遍历的。

### 插入

插入节点的操作都是通过 `linkLast()` 或 `linkBefore()` 完成的，无非是修改节点的前驱和后继。

如果清楚数据结构的知识的话，应该能很好理解。

```java
void linkLast(E e) {
    final Node<E> l = last;
    final Node<E> newNode = new Node<>(l, e, null);
    last = newNode;
    if (l == null)
        first = newNode;
    else
        l.next = newNode;
    size++;
    modCount++;
}

void linkBefore(E e, Node<E> succ) {
    // assert succ != null;
    final Node<E> pred = succ.prev;
    final Node<E> newNode = new Node<>(pred, e, succ);
    succ.prev = newNode;
    if (pred == null)
        first = newNode;
    else
        pred.next = newNode;
    size++;
    modCount++;
}
```

### 删除

删除节点均通过各种 `unlink` 方法。

```java
/**
 * Unlinks non-null first node f.
 */
private E unlinkFirst(Node<E> f) {
    // assert f == first && f != null;
    final E element = f.item;
    final Node<E> next = f.next;
    f.item = null;
    f.next = null; // help GC
    first = next;
    if (next == null)
        last = null;
    else
        next.prev = null;
    size--;
    modCount++;
    return element;
}

/**
 * Unlinks non-null last node l.
 */
private E unlinkLast(Node<E> l) {
    // assert l == last && l != null;
    final E element = l.item;
    final Node<E> prev = l.prev;
    l.item = null;
    l.prev = null; // help GC
    last = prev;
    if (prev == null)
        first = null;
    else
        prev.next = null;
    size--;
    modCount++;
    return element;
}

/**
 * Unlinks non-null node x.
 */
E unlink(Node<E> x) {
    // assert x != null;
    final E element = x.item;
    final Node<E> next = x.next;
    final Node<E> prev = x.prev;

    if (prev == null) {
        first = next;
    } else {
        prev.next = next;
        x.prev = null;
    }

    if (next == null) {
        last = prev;
    } else {
        next.prev = prev;
        x.next = null;
    }

    x.item = null;
    size--;
    modCount++;
    return element;
}
```

## CopyOnWriteArrayList

```java
public class CopyOnWriteArrayList<E>
    implements List<E>, RandomAccess, Cloneable, java.io.Serializable

/**
 * The lock protecting all mutators.  (We have a mild preference
 * for builtin monitors over ReentrantLock when either will do.)
 */
final transient Object lock = new Object();

/** The array, accessed only via getArray/setArray. */
private transient volatile Object[] array;
```

与 `ArrayList` 类似，`CopyOnWriteArrayList` 底层也是使用数组存储。差异在于，后者是线程安全的。

### 添加

```java
public boolean add(E e) {
    synchronized (lock) {
        Object[] es = getArray();
        int len = es.length;
        es = Arrays.copyOf(es, len + 1);
        es[len] = e;
        setArray(es);
        return true;
    }
}
```

注意几个方面：

* 所有修改操作都是加锁的，因此，修改操作是互斥的，并发修改可能会造成阻塞。
* 拷贝了底层数组副本进行修改，而不是直接修改底层数组。因此，其他线程可以同时进行读取。
* 扩容操作是在拷贝数组时通过指定副本数组长度来实现的。因此，数组中不会有预分配的空元素。

### 删除

```java
public E remove(int index) {
    synchronized (lock) {
        Object[] es = getArray();
        int len = es.length;
        E oldValue = elementAt(es, index);
        int numMoved = len - index - 1;
        Object[] newElements;
        if (numMoved == 0)
            newElements = Arrays.copyOf(es, len - 1);
        else {
            newElements = new Object[len - 1];
            System.arraycopy(es, 0, newElements, 0, index);
            System.arraycopy(es, index + 1, newElements, index,
                             numMoved);
        }
        setArray(newElements);
        return oldValue;
    }
}
```

删除操作跟添加操作要注意的点差不多是一样的，一个额外的关注点是，当删除的元素不在数组末尾，则需要分别拷贝删除位置前后两段数据。

# FAQ

## 为什么 ArrayList 底层数组域是 transient 的

答：避免默认序列化操作将整个底层数组完全序列化，而造成资源和性能浪费。

如果 `elementData` 没有 `transient` 修饰，那么默认序列化操作时会把整个数组都序列化。但是，我们知道 elementData 数组通常情况下会有不少尚未使用的空元素，它们没有必要序列化。

因此，使用 `transient` 修饰使得其不被默认序列化。不过，由于重写了 `readObject()`、`writeObject()` 两个方法，额外处理了数组非空元素的序列化。

## ArrayList 有两个用以共享的空数组

答：为了“延迟初始化”无参构造器创建的列表。

以下两个域都是空数组，用来共享空列表这一特殊值的：

* `DEFAULTCAPACITY_EMPTY_ELEMENTDATA`
* `EMPTY_ELEMENTDATA`

但为什么需要两个呢？

认真分析 3 个构造器，会发现它们都可能创建出空列表，那么，如果要共享空列表底层数组，用一个空数组应该就够了吧？

的确是这样。但是无参构造器创建的 `ArrayList` 是具有默认初始容量的，其大小由域 `DEFAULT_CAPACITY ` 定义，即 10。

因此，按常规来说，无参构造器创建的 `ArrayList` 底层的数组长度应该是 10，而不是空数组。但是，设计者为了“延迟初始化”使用了一个编程技巧，使其创建时共享空数组，直到添加元素时才真正初始化底层数组。

因此，在扩容时需要能区分出哪个空列表是用无参构造器创建的，以便应用其默认容量。故而需要有两个不同的空数组。

相关代码如下：

```java
private int newCapacity(int minCapacity) {
	// ...
    if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA)
        return Math.max(DEFAULT_CAPACITY, minCapacity);    
	// ...
}
```

## ArrayList 最大容量到底是多少？

答：通常最大容量为 `MAX_ARRAY_SIZE`，即 `Integer.MAX_VALUE - 8`，但仍可能达到 `Integer.MAX_VALUE`。

`MAX_ARRAY_SIZE` 域代表 `ArrayList` 的最大容量，其值为 `Integer.MAX_VALUE - 8`。注意它不是 `Integer` 的最大值，而少了 8。一般的解释是，数组需要 8 个字节存储头信息或说元数据。

但是，当“大容量”列表扩容时，会有一个特殊处理：

```java
private static int hugeCapacity(int minCapacity) {
    // ...
    return (minCapacity > MAX_ARRAY_SIZE)
        ? Integer.MAX_VALUE
        : MAX_ARRAY_SIZE;
}
```

可见，当扩容大小超过 `MAX_ARRAY_SIZE` 时，容量可以达到 `Integer.MAX_VALUE`。

既然容量可以达到 `Integer.MAX_VALUE`，那预留 8 字节有什么必要呢？这个问题尚没看到合理的解释。

