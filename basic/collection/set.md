# set
HashSet与TreeSet都是基于Set接口的实现类。其中TreeSet是Set的子接口SortedSet的实现类。Set接口及其子接口、实现类的结构如下所示：

```
         |——SortedSet接口——TreeSet实现类
Set接口——|——HashSet实现类
         |——LinkedHashSet实现类
```

## HashSet
`HashSet` 是 Java 中的一种集合类，它实现了 `Set` 接口，用于存储不重复的元素。`HashSet` 的主要特点是它不保证元素的顺序，因此在迭代时无法预测元素的顺序。

以下是 `HashSet` 的主要特点和实现细节：

1. **底层数据结构：** `HashSet` 使用哈希表作为底层数据结构。哈希表是一种用于实现关联数组（Associative Array）的数据结构，它允许快速地插入和查找元素。

2. **不允许重复元素：** `HashSet` 不允许存储重复的元素。如果试图将重复元素插入到 `HashSet` 中，插入操作将被忽略。

3. **无序性：** 由于 `HashSet` 使用哈希表，它不保证元素的顺序。因此，在迭代时元素的顺序可能是不确定的。

4. **性能：** `HashSet` 提供了较好的性能，具有平均 O(1) 的插入、删除和查找时间复杂度。但在最坏情况下，可能需要 O(n) 的时间复杂度。

5. **基于 HashMap：** `HashSet` 实际上是通过继承 `HashMap` 类并使用 `HashMap` 的键（key）部分来实现的。在 `HashSet` 中，元素被当作键，而值则为一个固定的常量。

下面是一个简单的示例，演示了如何使用 `HashSet`：

```java
import java.util.HashSet;

public class HashSetExample {
    public static void main(String[] args) {
        HashSet<String> hashSet = new HashSet<>();

        // 添加元素到 HashSet
        hashSet.add("Apple");
        hashSet.add("Banana");
        hashSet.add("Orange");

        // 打印 HashSet 中的元素（无序）
        for (String element : hashSet) {
            System.out.println(element);
        }
    }
}
```

在上述示例中，元素在 `HashSet` 中的顺序可能是不确定的。由于 `HashSet` 不保证元素的顺序，对于需要有序集合的场景，可以考虑使用 `LinkedHashSet` 或 `TreeSet`。

## LinkedHashSet
`LinkedHashSet` 是 Java 中的一个集合类，它是 `HashSet` 的子类，同时也实现了 `Set` 接口。与 `HashSet` 不同的是，`LinkedHashSet` 保留了元素的插入顺序，因此可以按照插入顺序迭代元素。

以下是 `LinkedHashSet` 的主要特点和实现细节：

1. **底层数据结构：** `LinkedHashSet` 的底层数据结构由哈希表和链接列表（Linked List）组成。哈希表用于快速查找元素，链接列表用于维护插入顺序。

2. **不允许重复元素：** 与 `Set` 接口的特性一致，`LinkedHashSet` 不允许存储重复元素。

3. **有序性：** `LinkedHashSet` 保留了元素的插入顺序，因此在迭代时可以按照插入的先后顺序访问元素。

4. **性能：** `LinkedHashSet` 提供了与 `HashSet` 相似的性能，但由于需要维护插入顺序，相对于简单的哈希表，可能有略微的性能开销。

5. **基于 `HashSet`：** `LinkedHashSet` 是通过继承 `HashSet` 类并添加链接列表来实现的。

下面是一个简单的示例，演示了如何使用 `LinkedHashSet`：

```java
import java.util.LinkedHashSet;

public class LinkedHashSetExample {
    public static void main(String[] args) {
        LinkedHashSet<String> linkedHashSet = new LinkedHashSet<>();

        // 添加元素到 LinkedHashSet
        linkedHashSet.add("Apple");
        linkedHashSet.add("Banana");
        linkedHashSet.add("Orange");

        // 打印 LinkedHashSet 中的元素（按照插入顺序）
        for (String element : linkedHashSet) {
            System.out.println(element);
        }
    }
}
```

在上述示例中，元素将按照插入顺序（"Apple"、"Banana"、"Orange"）存储和打印。这种顺序是通过维护链接列表来实现的。与 `HashSet` 一样，`LinkedHashSet` 也具有 `HashSet` 的哈希表特性，即快速查找。
## TreeSet
`TreeSet` 是 Java 中的一个基于红黑树（Red-Black Tree）实现的有序集合类。它实现了 `SortedSet` 接口，因此存储的元素会按照它们的自然顺序或者通过提供的比较器进行排序。

以下是 `TreeSet` 的主要特性和实现细节：

1. **红黑树：** `TreeSet` 使用红黑树作为底层数据结构。红黑树是一种自平衡的二叉查找树，确保在最坏情况下的基本动态集合操作的时间复杂度为 O(log n)。

2. **有序性：** 因为使用了红黑树，`TreeSet` 中的元素是有序的。具体的排序方式取决于元素的自然顺序或提供的比较器。

3. **不允许重复元素：** `TreeSet` 不允许存储重复元素。如果试图将重复元素插入到 `TreeSet` 中，插入操作将被忽略。

4. **迭代顺序：** 迭代 `TreeSet` 中的元素将按照它们的升序（自然顺序）或根据提供的比较器的规则进行。

5. **基于NavigableMap：** 在Java 6之前，`TreeSet` 是基于 `TreeMap` 实现的。从Java 6开始，`TreeSet` 类直接实现了 `NavigableSet` 接口，而 `NavigableSet` 继承自 `SortedSet` 接口。

下面是一个简单的示例，演示了如何使用 `TreeSet`：

```java
import java.util.TreeSet;

public class TreeSetExample {
    public static void main(String[] args) {
        TreeSet<Integer> treeSet = new TreeSet<>();

        // 添加元素到 TreeSet
        treeSet.add(5);
        treeSet.add(2);
        treeSet.add(8);
        treeSet.add(1);

        // 打印 TreeSet 中的元素（有序）
        for (Integer element : treeSet) {
            System.out.println(element);
        }
    }
}
```

在上述示例中，元素将按照它们的自然顺序（整数的升序）存储和打印。如果使用自定义对象，确保对象实现了 `Comparable` 接口或者在创建 `TreeSet` 时提供了比较器。

## LinkedHashSet
`LinkedHashSet` 是 Java 中的一个集合类，它是 `HashSet` 的子类，同时也实现了 `Set` 接口。与 `HashSet` 不同的是，`LinkedHashSet` 保留了元素的插入顺序，因此可以按照插入顺序迭代元素。

以下是 `LinkedHashSet` 的主要特点和实现细节：

1. **底层数据结构：** `LinkedHashSet` 的底层数据结构由哈希表和链接列表（Linked List）组成。哈希表用于快速查找元素，链接列表用于维护插入顺序。

2. **不允许重复元素：** 与 `Set` 接口的特性一致，`LinkedHashSet` 不允许存储重复元素。

3. **有序性：** `LinkedHashSet` 保留了元素的插入顺序，因此在迭代时可以按照插入的先后顺序访问元素。

4. **性能：** `LinkedHashSet` 提供了与 `HashSet` 相似的性能，但由于需要维护插入顺序，相对于简单的哈希表，可能有略微的性能开销。

5. **基于 `HashSet`：** `LinkedHashSet` 是通过继承 `HashSet` 类并添加链接列表来实现的。

下面是一个简单的示例，演示了如何使用 `LinkedHashSet`：

```java
import java.util.LinkedHashSet;

public class LinkedHashSetExample {
    public static void main(String[] args) {
        LinkedHashSet<String> linkedHashSet = new LinkedHashSet<>();

        // 添加元素到 LinkedHashSet
        linkedHashSet.add("Apple");
        linkedHashSet.add("Banana");
        linkedHashSet.add("Orange");

        // 打印 LinkedHashSet 中的元素（按照插入顺序）
        for (String element : linkedHashSet) {
            System.out.println(element);
        }
    }
}
```

在上述示例中，元素将按照插入顺序（"Apple"、"Banana"、"Orange"）存储和打印。这种顺序是通过维护链接列表来实现的。与 `HashSet` 一样，`LinkedHashSet` 也具有 `HashSet` 的哈希表特性，即快速查找。

## HashMap 和 HashSet区别
如果你看过 HashSet 源码的话就应该知道：HashSet 底层就是基于 HashMap 实现的。（HashSet 的源码非常非常少，因为除了 clone()、writeObject()、readObject()是 HashSet 自己不得不实现之外，其他方法都是直接调用 HashMap 中的方法。

HashMap	HashSet
实现了Map接口	实现Set接口
存储键值对	仅存储对象
调用 put（）向map中添加元素	调用 add（）方法向Set中添加元素
HashMap使用键（Key）计算Hashcode	HashSet使用成员对象来计算hashcode值，对于两个对象来说hashcode可能相同，所以equals()方法用来判断对象的相等性，