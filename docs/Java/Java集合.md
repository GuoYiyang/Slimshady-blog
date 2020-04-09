## 集合概述

#### 集合分类

Java 容器分为 Collection 和 Map 两大类：

<img src="../图片/collection集合.jpg" alt="collection集合" style="zoom: 67%;" />

Collection集合主要有List和Set两大接口

-   Set：一个无序（存入和取出顺序有可能不一致）容器，不可以存储重复元素，只允许存入一个null元素，必须保证元素唯一性。Set 接口常用实现类是 HashSet、LinkedHashSet 以及 TreeSet。
-   List：一个有序（元素存入集合的顺序和取出的顺序一致）容器，元素可以重复，可以插入多个null元素，元素都有索引。常用的实现类有 ArrayList、LinkedList。

Map是一个键值对集合，存储键、值和之间的映射。 Key无序，唯一；value 不要求有序，允许重复。Map 的常用实现类：HashMap、TreeMap、HashTable、LinkedHashMap、ConcurrentHashMap

#### 底层数据结构

**List**

-   Arraylist： Object数组
-   Vector： Object数组
-   LinkedList： 双向循环链表

**Set**

-   HashSet（无序，唯一）：基于 HashMap 实现的，底层采用 HashMap 来保存元素
-   LinkedHashSet： LinkedHashSet 继承与 HashSet，并且其内部是通过 LinkedHashMap 来实现的。
-   TreeSet（有序，唯一）： 红黑树（自平衡的排序二叉树）。

**Map**

-   HashMap： JDK1.8之前HashMap由数组+链表组成的，数组是HashMap的主体，链表则是主要为了解决哈希冲突而存在的（“拉链法”解决冲突）。JDK1.8以后在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为8）时，将链表转化为==红黑树==，以减少搜索时间。
-   LinkedHashMap：LinkedHashMap 继承自 HashMap，所以它的底层仍然是基于拉链式散列结构即由数组和链表或红黑树组成。另外，LinkedHashMap 在上面结构的基础上，==增加了一条双向链表==，使得上面的结构可以保持键值对的插入顺序。同时通过对链表进行相应的操作，实现了访问顺序相关逻辑。
-   HashTable： 数组+链表组成的，数组是 HashMap 的主体，链表则是主要为了解决哈希冲突而存在的。
-   TreeMap： 红黑树（自平衡的排序二叉树）

#### 线程安全的集合类

-   vector：就比arraylist多了个同步化机制（线程安全），因为效率较低，现在已经不太建议使用。在web应用中，特别是前台页面，往往效率（页面响应速度）是优先考虑的。
-   stack：堆栈类，先进后出。
-   hashtable：就比hashmap多了个线程安全。
-   enumeration：枚举，相当于迭代器。

#### 快速失败机制 fail-fast

fail-fast是java集合的一种错误检测机制，当多个线程对集合进行结构上的改变的操作时，有可能会产生 fail-fast 机制。

**机制原理：**

迭代器在遍历时直接访问集合中的内容，并且在遍历过程中使用一个 modCount 变量。集合在被遍历期间如果内容发生变化，就会改变modCount的值。每当迭代器使用hashNext()/next()遍历下一个元素之前，都会检测modCount变量是否为expectedmodCount值，是的话就返回遍历；否则抛出异常，终止遍历。

>   例如：假设存在两个线程（线程1、线程2），线程1通过Iterator在遍历集合A中的元素，在某个时候线程2修改了集合A的结构（是结构上面的修改，而不是简单的修改集合元素的内容），那么这个时候程序就会抛出 ConcurrentModificationException 异常，从而产生fail-fast机制。
>

**解决办法：**

-   在遍历过程中，所有涉及到改变modCount值得地方全部加上synchronized。

-   使用CopyOnWriteArrayList来替换ArrayList

#### 集合和数组的区别

-   数组是固定长度的；集合可变长度的。
-   数组可以存储基本数据类型，也可以存储引用数据类型；集合只能存储引用数据类型。
-   数组存储的元素必须是同一个数据类型；集合存储的对象可以是不同数据类型。

#### 使用集合框架的好处

1.  容量自增长；
2.  提供了高性能的数据结构和算法，使编码更轻松，提高了程序速度和质量；
3.  允许不同 API 之间的互操作，API之间可以来回传递集合；
4.  可以方便地扩展或改写集合，提高代码复用性和可操作性;
5.  通过使用JDK自带的集合类，可以降低代码维护和学习新API成本。

#### 确保一个集合不能被修改

可以使用 Collections. unmodifiableCollection(Collection c) 方法来创建一个只读集合，这样改变集合的任何操作都会抛出 Java. lang. UnsupportedOperationException 异常：

```java
List<String> list = new ArrayList<>();
list. add("x");
Collection<String> clist = Collections. unmodifiableCollection(list);
clist. add("y"); // 运行时此行报错
System. out. println(list. size());
```

#### comparable 和 comparator

-   comparable接口出自java.lang包，它有一个compareTo(Object obj)方法用来排序
-   comparator接口出自 java.util 包，它有一个compare(Object obj1, Object obj2)方法用来排序

一般我们需要对一个集合使用自定义排序时，我们就要重写compareTo方法或compare方法，当我们需要对某一个集合实现两种排序方式，比如一个song对象中的歌名和歌手名分别采用一种排序方法的话，我们可以重写compareTo方法和使用自制的Comparator方法或者以两个Comparator来实现歌名排序和歌星名排序，第二种代表我们只能使用两个参数版的Collections.sort()。

#### Collection 和 Collections 

-   java.util.Collection 是一个集合接口（集合类的一个顶级接口）。它提供了对集合对象进行基本操作的通用接口方法。Collection接口在Java 类库中有很多具体的实现。Collection接口的意义是为各种具体的集合提供了最大化的统一操作方式，其直接继承接口有List与Set。
-   Collections则是集合类的一个工具类/帮助类，其中提供了一系列静态方法，用于对集合中元素进行排序、搜索以及线程安全等各种操作。

Collections 工具类的 sort 方法有两种重载的形式，

1.  第一种要求传入的待排序容器中存放的对象比较实现 Comparable 接口以实现元素的比较。

2.  第二种不强制性的要求容器中的元素必须可比较，但是要求传入第二个参数，参数是Comparator 接口的子类型（需要重写 compare 方法实现元素的比较），相当于一个临时定义的排序规则，其实就是通过接口注入比较元素大小的算法，也是对回调模式的应用（Java 中对函数式编程的支持）。

## Collection接口

### 迭代器 Iterator 

Iterator 接口提供遍历任何 ==Collection== 的接口。我们可以从一个 Collection 中使用迭代器方法来获取迭代器实例。迭代器取代了 Java 集合框架中的 Enumeration，迭代器允许调用者在迭代过程中移除元素。

**单向遍历**

Iterator 的特点是只能单向遍历，但是更加安全，因为它可以确保，在当前遍历的集合元素被更改的时候，就会抛出 ConcurrentModificationException 异常。

```java
List<String> list = new ArrayList<>();
Iterator<String> it = list.iterator();
while(it.hasNext()){
    String obj = it.next();
    System.out.println(obj);
}
```

**移除元素**

```java
Iterator<Integer> it = list.iterator();
while(it.hasNext()){
    it.remove();
}
```

**Iterator 和 ListIterator**

-   Iterator 可以遍历 Set 和 List 集合，而 ListIterator 只能遍历 List。
-   Iterator 只能单向遍历，而 ListIterator 可以双向遍历。
-   ListIterator 实现 Iterator 接口，然后添加了一些额外的功能，比如添加一个元素、替换一个元素、获取前面或后面元素的索引位置。

### List

#### 遍历方式

1.  for 循环遍历，基于计数器。在集合外部维护一个计数器，然后依次读取每一个位置的元素，当读取到最后一个元素后停止。

2.  迭代器遍历，Iterator。Iterator 是面向对象的一个设计模式，目的是屏蔽不同数据集合的特点，统一遍历集合的接口。Java 在 Collections 中支持了 Iterator 模式。

3.  foreach 循环遍历。foreach 内部也是采用了 Iterator 的方式实现，使用时不需要显式声明 Iterator 或计数器。优点是代码简洁，不易出错；缺点是只能做简单的遍历，不能在遍历过程中操作数据集合，例如删除、替换。

**RandomAccess** 

Java Collections 框架中提供了一个 RandomAccess 接口，用来标记 List 实现是否支持 Random Access。

-   如果一个数据集合实现了该接口，就意味着它支持 RandomAccess，按位置读取元素的平均时间复杂度为 ==O(1)==，如ArrayList。
-   如果没有实现该接口，表示不支持 RandomAccess，如LinkedList。
-   推荐的做法就是，支持 RandomAccess 的列表可用 for 循环遍历，否则建议用 Iterator 或 foreach 遍历。

#### ArrayList 的优缺点

ArrayList的优点如下：

-   ArrayList 底层以数组实现，是一种随机访问模式。ArrayList 实现了 RandomAccess 接口，因此查找的时候非常快。
-   ArrayList 在顺序添加一个元素的时候非常方便。

ArrayList 的缺点如下：

-   删除元素的时候，需要做一次元素复制操作。如果要复制的元素很多，那么就会比较耗费性能。
-   插入元素的时候，也需要做一次元素复制操作，缺点同上。

ArrayList 比较适合==顺序添加、随机访问==的场景。

#### 数组和 List 的转换

-   数组转 List：使用 Arrays. asList(array) 进行转换。
-   List 转数组：使用 List 自带的 toArray() 方法。

#### ArrayList 和 LinkedList 区别

-   数据结构实现：ArrayList 是动态数组的数据结构实现，而 LinkedList 是双向链表的数据结构实现。
-   随机访问效率：ArrayList 比 LinkedList 在随机访问的时候效率要高，因为 LinkedList 是线性的数据存储方式，所以需要移动指针从前往后依次查找。
-   增加和删除效率：在非首尾的增加和删除操作，LinkedList 要比 ArrayList 效率要高，因为 ArrayList 增删操作要影响数组内的其他数据的下标。
-   内存空间占用：LinkedList 比 ArrayList 更占内存，因为 LinkedList 的节点除了存储数据，还存储了两个引用，一个指向前一个元素，一个指向后一个元素。
-   线程安全：ArrayList 和 LinkedList 都是不同步的，也就是不保证线程安全。

综合来说，在需要频繁读取集合中的元素时，更推荐使用 ArrayList，而在插入和删除操作较多时，更推荐使用 LinkedList。

#### transient 

ArrayList 实现了 Serializable 接口，这意味着 ArrayList 支持序列化。transient 的作用是说不希望 elementData 数组被序列化，每次序列化时，先调用 defaultWriteObject() 方法序列化 ArrayList 中的非 transient 元素，然后遍历 elementData，只序列化已存入的元素，这样既加快了序列化的速度，又减小了序列化之后的文件大小。

### Set

#### HashSet 的实现原理

HashSet 是基于 HashMap 实现的，HashSet的值存放于HashMap的key上，HashMap的value统一为PRESENT，因此 HashSet 的实现比较简单，相关 HashSet 的操作，基本上都是直接调用底层 HashMap 的相关方法来完成，HashSet 不允许重复的值。

#### HashSet 如何检查重复

向HashSet 中add()元素时，判断元素是否存在的依据，不仅要比较hash值，同时还要结合equles()方法比较。
HashSet 中的add()方法会使用HashMap 的put()方法。

HashMap 的 key 是唯一的，由源码可以看出 HashSet 添加进去的值就是作为HashMap 的key，并且在HashMap中如果K/V相同时，会用新的V覆盖掉旧的V，然后返回旧的V，所以不会重复（ HashMap 比较key是否相等是先比较hashcode 再比较equals ）。

```java
private static final Object PRESENT = new Object();
private transient HashMap<E,Object> map;

public HashSet() {
    map = new HashMap<>();
}

public boolean add(E e) {
    // 调用HashMap的put方法,PRESENT是一个至始至终都相同的虚值
	return map.put(e, PRESENT)==null;
}
```

#### hashCode() 与 equals()

-   如果两个对象相等，则hashcode()一定也是相同的
-   两个对象相等,对两个equals()方法返回true
-   两个对象有相同的hashcode()值，它们不一定是相等的

#### HashSet与HashMap的区别

| HashMap                            | HashSet                             |
| ---------------------------------- | ----------------------------------- |
| 实现了Map接口                      | 实现Set接口                         |
| 存储键值对                         | 仅存储对象                          |
| 调用put()向map中添加元素           | 调用add()方法向Set中添加元素        |
| HashMap使用键(Key)计算hashcode     | hashSet使用成员对象来计算hashcode值 |
| 较快，因为它是使用唯一的键获取对象 | HashSet较HashMap来说比较慢          |
|                                    |                                     |

### Queue

#### 阻塞队列BlockingQueue

Java.util.concurrent.BlockingQueue是一个队列，在进行检索或移除一个元素的时候，它会等待队列变为非空；当在添加一个元素时，它会等待队列中的可用空间。BlockingQueue接口是Java集合框架的一部分，主要用于实现==生产者-消费者模式==。我们不需要担心等待生产者有可用的空间，或消费者有可用的对象，因为它都在BlockingQueue的实现类中被处理了。Java提供了集中BlockingQueue的实现，比如ArrayBlockingQueue、LinkedBlockingQueue、PriorityBlockingQueue,、SynchronousQueue等。

#### poll()和 remove()区别

-   相同点：都是返回第一个元素，并在队列中删除返回的对象。
-   不同点：如果没有元素 poll()会返回 null，而 remove()会直接抛出 NoSuchElementException 异常。

## Map接口

#### HashMap 的实现原理

put元素时，利用key的hashCode值计算出当前对象的元素在数组中的下标存储时，如果出现hash值相同的key，此时有两种情况：

1.  如果key相同，则覆盖原始值；
2.  如果key不同（出现冲突），则将当前的key-value放入链表中

获取时，直接找到hash值对应的下标，在进一步判断key是否相同，从而找到对应值。

#### put方法的具体流程

put的时候，首先计算 key的hash值，这里调用了 hash方法，hash方法实际是让key.hashCode()与key.hashCode()>>>16进行异或操作，高16bit补0，一个数和0异或不变，所以 hash 函数大概的作用就是：==高16bit不变，低16bit和高16bit做了一个异或，目的是减少碰撞==。按照函数注释，因为bucket数组大小是2的幂，计算下标index = (table.length - 1) & hash，如果不做 hash 处理，相当于散列生效的只有几个低 bit 位，为了减少散列的碰撞，设计者综合考虑了之后，使用高16bit和低16bit异或来简单处理减少碰撞，而且JDK8中用了复杂度 O（logn）的树结构来提升碰撞下的性能。

#### HashMap的扩容操作

-   在jdk1.8中，在hashmap中的键值对大于阀值时或者初始化时，就调用resize方法进行扩容；
-   每次扩展的时候，都是扩展2倍；
-   扩展后Node对象的位置要么在原位置，要么移动到原偏移量两倍的位置。

resize() 方法表示的在进行第一次初始化时会对其进行扩容，或者当该数组的实际大小大于其临界值值(第一次为12)，这个时候在扩容的同时也会伴随的桶上面的元素进行重新分发，这也是JDK1.8版本的一个优化的地方：

-   在1.7中，扩容之后需要重新去计算其Hash值，根据Hash值对其进行分发；
-   在1.8版本中，则是根据在同一个桶的位置中进行判断(e.hash & oldCap)是否为0，重新进行hash分配后，该元素的位置要么停留在原始位置，要么移动到原始位置+增加的数组大小这个位置上。

#### Hash冲突

**哈希的概念：**

Hash，一般翻译为“散列”，也有直接音译为“哈希”的，就是把任意长度的输入通过散列算法，变换成固定长度的输出，该输出就是散列值（哈希值）；这种转换是一种压缩映射，也就是，散列值的空间通常远小于输入的空间，不同的输入可能会散列成相同的输出，所以不可能从散列值来唯一的确定输入值。简单的说就是一种将任意长度的消息压缩到某一固定长度的消息摘要的函数。

所有散列函数都有如下一个基本特性**：**根据同一散列函数计算出的散列值如果不同，那么输入值肯定也不同。但是，根据同一散列函数计算出的散列值如果相同，输入值不一定相同。

**HashMap为什么不直接使用hashCode()处理后的哈希值直接作为table的下标？**

hashCode()方法返回的是int整数类型，其范围为-(2 ^ 31)~(2 ^ 31 - 1)，约有40亿个映射空间，而HashMap的容量范围是在16（初始化默认值）~2 ^ 30，HashMap通常情况下是取不到最大值的，并且设备上也难以提供这么多的存储空间，从而导致通过hashCode()计算出的哈希值可能不在数组大小范围内，进而无法匹配存储位置；

所以HashMap自己实现了自己的hash()方法，通过两次扰动使得它自己的哈希值高低位自行进行异或运算，降低哈希碰撞概率也使得数据分布更平均。

在保证数组长度为2的幂次方的时候，使用hash()运算之后的值与运算（&）（数组长度 - 1）来获取数组下标的方式进行存储，这样一来是比取余操作更加有效率，二来也是因为只有当数组长度为2的幂次方时，h&(length-1)才等价于h%length，三来解决了“哈希值与数组大小范围不匹配”的问题；

**HashMap 的长度为什么是2的幂次方**

为了能让 HashMap 存取高效，尽量较少碰撞，也就是要尽量把数据分配均匀，每个链表/红黑树长度大致相同。这个实现就是把数据存到哪个链表/红黑树中的算法。

这个算法应该如何设计呢？

我们首先可能会想到采用取余的操作来实现。但是，重点来了：取余(%)操作中如果除数是2的幂次则等价于与其除数减一的与(&)操作（也就是说 hash%length==hash&(length-1)的前提是 length 是2的 n 次方）， 并且 采用二进制位操作 &，相对于%能够提高运算效率，这就解释了 HashMap 的长度为什么是2的幂次方。

**那为什么是两次扰动呢？**

这样就是加大哈希值低位的随机性，使得分布更均匀，从而提高对应数组存储下标位置的随机性和均匀性，最终减少Hash冲突，两次就够了，已经达到了高位低位同时参与运算的目的。

**解决hash冲突的方式**

**JDK1.8之前**

JDK1.8之前采用的是拉链法。

拉链法：将链表和数组相结合。创建一个链表数组，数组中每一格就是一个链表。若遇到哈希冲突，则将冲突的值加到链表中即可。

相比于hashCode返回的int类型，我们HashMap初始的容量大小 `DEFAULT_INITIAL_CAPACITY = 1 << 4`（即2的四次方16）要远小于int类型的范围，所以我们如果只是单纯的用hashCode取余来获取对应的下标将会大大增加哈希碰撞的概率，并且最坏情况下还会将HashMap变成一个单链表，所以我们还需要对hashCode作一定的优化。

**JDK1.8之后**

**优化hash函数**

上面提到的问题，主要是因为如果使用hashCode取余，那么相当于==参与运算的只有hashCode的低位，高位是没有起到任何作用的==，所以我们的思路就是让hashCode取值出的高位也参与运算，进一步降低hash碰撞的概率，使得数据分布更平均，我们把这样的操作称为扰动，在JDK 1.8中的hash()函数如下：

```java
static final int hash(Object key) {
    int h;
    // 与自己右移16位进行异或运算（高低位异或）
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

这比在JDK 1.7中，更为简洁，相比在1.7中的4次位运算，5次异或运算（9次扰动），在1.8中，只进行了1次位运算和1次异或运算（2次扰动）。

**新增红黑树**

当链表长度大于阈值（默认为8）且数组长度大于64时，将链表转化为红黑树，以减少搜索时间，从原来的O(n)到O(logn)。（链表长度大于阈值，但是数组长度小于64时会首先进行扩容）

#### 类作为key的条件

可以使用任何类作为 Map 的 key，然而在使用之前，需要考虑以下几点：

-   如果类重写了 equals() 方法，也应该重写 hashCode() 方法。

-   类的所有实例需要遵循与 equals() 和 hashCode() 相关的规则。

-   如果一个类没有使用 equals()，不应该在 hashCode() 中使用它。

-   用户自定义 Key 类最佳实践是使之为不可变的，这样 hashCode() 值可以被缓存起来，拥有更好的性能。不可变的类也可以确保 hashCode() 和 equals() 在未来不会改变，这样就会解决与可变相关的问题了。

**为什么HashMap中String、Integer这样的包装类适合作为K？**

String、Integer等包装类的特性能够保证Hash值的不可更改性和计算准确性，能够有效的减少Hash碰撞的几率

-   都是final类型，即不可变性，保证key的不可更改性，不会存在获取hash值不同的情况。
-   内部已重写了equals()、hashCode()等方法，遵守了HashMap内部的规范，不容易出现Hash值计算错误的情况。

**如果使用Object作为HashMap的Key，应该怎么办呢？**

重写hashCode()和equals()方法

-   重写hashCode()是因为需要计算存储数据的存储位置，需要注意不要试图从散列码计算中排除掉一个对象的关键部分来提高性能，这样虽然能更快但可能会导致更多的Hash碰撞；
-   重写equals()方法，需要遵守自反性、对称性、传递性、一致性以及对于任何非null的引用值x，x.equals(null)必须返回false的这几个特性，目的是为了保证key在哈希表中的唯一性；

#### HashMap 与 HashTable 区别

1.  线程安全： HashMap 是非线程安全的，HashTable 是线程安全的；HashTable 内部的方法基本都经过 synchronized 修饰。（如果要保证线程安全的话就使用 ConcurrentHashMap 吧！）

2.  效率： 因为线程安全的问题，HashMap 要比 HashTable 效率高一点。另外，HashTable 基本被淘汰，不要在代码中使用它；

3.  对Null key 和Null value的支持： HashMap 中，null 可以作为键，这样的键只有一个，可以有一个或多个键所对应的值为 null。但是在 HashTable 中 put 进的键值只要有一个 null，直接抛NullPointerException。

4.  初始容量大小和每次扩充容量大小的不同 ：

    ①创建时如果不指定容量初始值，Hashtable 默认的初始大小为11，之后每次扩充，容量变为原来的2n+1。HashMap 默认的初始化大小为16。之后每次扩充，容量变为原来的2倍。

    ②创建时如果给定了容量初始值，那么 Hashtable 会直接使用你给定的大小，而 HashMap 会将其扩充为2的幂次方大小。也就是说 ==HashMap 总是使用2的幂作为哈希表的大小==。

5.  底层数据结构： JDK1.8 以后的 HashMap 在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为8）时，将链表转化为红黑树，以减少搜索时间。Hashtable 没有这样的机制。

在 Hashtable 的类注释可以看到，Hashtable 是保留类不建议使用，推荐在单线程环境下使用 HashMap 替代，如果需要多线程使用则用 ConcurrentHashMap 替代。

#### ConcurrentHashMap 

**底层数据结构**

JDK1.7的 ConcurrentHashMap 底层采用 分段的数组+链表实现，JDK1.8 采用的数据结构跟HashMap的结构一样，数组+链表/红黑二叉树。HashMap的键值对允许有null，但是ConCurrentHashMap都不允许。

**实现线程安全的方式** 

**JDK1.7**

在JDK1.7中，ConcurrentHashMap采用Segment + HashEntry的方式进行实现。

首先将数据分为一段一段的存储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据时，其他段的数据也能被其他线程访问。

一个 ConcurrentHashMap 里包含一个 Segment 数组。Segment 的结构和HashMap类似，是一种数组和链表结构，Segment 是一种可重入的锁 ReentrantLock，每个 Segment 守护一个HashEntry 数组里得元素，当对 HashEntry 数组的数据进行修改时，必须首先获得对应的 Segment 锁。

**JDK1.8** 

JDK1.8 的时候已经摒弃了Segment的概念，取而代之的是采用Node + CAS + Synchronized来保证并发安全进行实现。synchronized只锁定当前链表或红黑二叉树的首节点，这样只要hash不冲突，就不会产生并发。

#### TreeMap 和 TreeSet 

-   TreeSet 要求存放的对象所属的类必须实现 Comparable 接口，该接口提供了比较元素的 compareTo()方法，当插入元素时会回调该方法比较元素的大小。
-   TreeMap 要求存放的键值对映射的键必须实现 Comparable 接口，从而根据键对元素进行排序。