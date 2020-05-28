# JavaStream

>   参考文档：
>
>   [Java8-Stream API 详解](https://blog.csdn.net/ycj_xiyang/article/details/83624642)
>
>   [java8 stream常用方法](https://blog.csdn.net/zxl646801924/article/details/90374320?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)

## 简介

Stream 作为 Java 8 的一大亮点，它与 java.io 包里的 InputStream 和 OutputStream 是完全不同的概念。

Java 8 中的 **Stream 是对集合（Collection）对象功能的增强**，它专注于对集合对象进行各种非常便利、高效的聚合操作（aggregate operation），或者大批量数据操作 (bulk data operation)。

Stream API 借助于同样新出现的 Lambda 表达式，极大的提高编程效率和程序可读性。同时它提供串行和并行两种模式进行汇聚操作，并发模式能够充分利用多核处理器的优势，使用 fork/join 并行方式来拆分任务和加速处理过程。

通常编写并行代码很难而且容易出错, 但使用 Stream API 无需编写一行多线程的代码，就可以很方便地写出高性能的并发程序。

所以说，Java 8 中首次出现的 java.util.stream 是一个函数式语言+多核时代综合影响的产物。

## Stream的概念

流（Stream）是数据通道，用于操作数据源（集合，数组等）所生成的元素序列。整个流操作就是一条流水线，将元素放在流水线上一个个地进行处理。

**集合讲的的是数据，流讲的是计算。**

其中数据源便是原始集合，然后将如 `List<T>` 的集合转换为 `Stream<T>` 类型的流，并对流进行一系列的中间操作，比如过滤保留部分元素、对元素进行排序、类型转换等；最后再进行一个终端操作，可以把 Stream 转换回集合类型，也可以直接对其中的各个元素进行处理，比如打印、比如计算总数、计算最大值等等。

很重要的一点是，很多流操作本身就会返回一个流，所以多个操作可以直接连接起来。如果是以前，进行这么一系列操作，你需要做个迭代器或者 foreach 循环，然后遍历，一步步地地去完成这些操作；但是如果使用流，你便可以直接声明式地下指令，流会帮你完成这些操作。

注意：

1.  Stream不会存储元素
2.  Stream不会改变源对象，相反他们会返回一个持有结果的新的Stream
3.  Stream操作是延迟执行的，这意味着他们等到需要结果的时候才会执行（惰性求值）

## Stream操作的三个步骤

1.  创建

    一个数据源（如：集合，数组）获取一个流

2.  中间操作

    一个中间操作链，对数据源的数据进行处理

3.  终止操作（终端操作）

    一个终止操作，执行中间操作链，并产生结果

## 创建Stream

1.  `Collection`提供了两个方法`stream()`与`paralleStream()`

```java
List<Integer> list = new ArrayList<>();
Stream<Integer> stream = list.stream();//串行流
Stream<Integer> integerStream = list.parallelStream();//并行流
```

2.  通过`Arrays`中的`Stream()`获取一个数组流。

```java
Integer[] integers ={};
Stream<Integer> stream = Arrays.stream(integers);
```

3.  通过`Stream`类中静态方法`of()`

```java
Stream<String> stream = Stream.of("aaa", "bbb");
```

## 常用方法

### filter(T -> boolean)

保留 boolean 为 true 的元素

```java
//保留年龄为 20 的 person 元素
list = list.stream()
            .filter(person -> person.getAge() == 20)
            .collect(toList());	//collect(toList()) 可以把流转换为 List 类型
//打印输出 [Person{name='jack', age=20}]
```

### distinct()

去除重复元素，这个方法是通过类的 `equals` 方法来判断两个元素是否相等的。

如例子中的 Person 类，需要先定义好 equals 方法，不然类似`[Person{name='jack', age=20}, Person{name='jack', age=20}]`这样的情况是不会处理的。

### sorted() / sorted((T, T) -> int)

如果流中的元素的类实现了 `Comparable` 接口，即有自己的排序规则，那么可以直接调用 `sorted()` 方法对元素进行排序。

反之, 需要调用 `sorted((T, T) -> int)` 实现 `Comparator` 接口

```java
//根据年龄大小来比较：
list = list.stream()
           .sorted((p1, p2) -> p1.getAge() - p2.getAge())
           .collect(toList());
```

简化：

```java
list = list.stream()
           .sorted(Comparator.comparingInt(Person::getAge))
           .collect(toList());
```

### limit(long n)

返回前 n 个元素

```java
list = list.stream()
            .limit(2)
            .collect(toList());
 
//打印输出 [Person{name='jack', age=20}, Person{name='mike', age=25}]
```

### skip(long n)

去除前 n 个元素

```java
list = list.stream()
            .skip(2)
            .collect(toList());
 
//打印输出 [Person{name='tom', age=30}]
```

*   `skip(n)`用在 `limit(n)` 前面时，先去除前 m 个元素再返回剩余元素的前 n 个元素
*   `limit(n)` 用在 `skip(m)` 前面时，先返回前 n 个元素再在剩余的 n 个元素中去除 m 个元素

```java
list = list.stream()
            .limit(2)
            .skip(1)
            .collect(toList());
 
//打印输出 [Person{name='mike', age=25}]
```

### map(T -> R)

将流中的每一个元素 T 映射为 R（类似类型转换）

```java
List<String> newlist = list.stream()
    						.map(Person::getName)
    						.collect(toList());
//newlist 里面的元素为 list 中每一个 Person 对象的 name 变量
```

### flatMap(T -> Stream)

将流中的每一个元素 T 映射为一个流，再把每一个流连接成为一个流

```java
List<String> list = new ArrayList<>();
list.add("aaa bbb ccc");
list.add("ddd eee fff");
list.add("ggg hhh iii");
 
list = list.stream()
    .map(s -> s.split(" "))
    .flatMap(Arrays::stream)
    .collect(toList());
```

我们的目的是把 List 中每个字符串元素以" "分割开，变成一个新的 List。

首先 map 方法分割每个字符串元素，但此时流的类型为 Stream<String[ ]>，因为 split 方法返回的是 String[ ] 类型；所以我们需要使用 flatMap 方法，先使用Arrays::stream将每个 String[ ] 元素变成一个 Stream 流，然后 flatMap 会将每一个流连接成为一个流，最终返回我们需要的 Stream。

### anyMatch(T -> boolean)

流中是否有一个元素匹配给定的 `T -> boolean` 条件

```java
//是否存在一个 person 对象的 age 等于 20：
boolean b = list.stream()
    .anyMatch(person -> person.getAge() == 20);
```

### allMatch(T -> boolean)

流中是否所有元素都匹配给定的 `T -> boolean` 条件

### noneMatch(T -> boolean)

流中是否没有元素匹配给定的 T -> boolean 条件

### findAny() 和 findFirst()

*   findAny()：找到其中一个元素 （使用 stream() 时找到的是第一个元素；使用 parallelStream() 并行时找到的是其中一个元素）

*   findFirst()：找到第一个元素

值得注意的是，这两个方法返回的是一个 Optional 对象，它是一个容器类，能代表一个值存在或不存在

### reduce((T, T) -> T) 和 reduce(T, (T, T) -> T)

用于组合流中的元素，如求和，求积，求最大值等

```java
//计算年龄总和：
int sum = list.stream()
    .map(Person::getAge)
    .reduce(0, (a, b) -> a + b);
//与之相同:
int sum = list.stream()
    .map(Person::getAge)
    .reduce(0, Integer::sum);
//其中，reduce 第一个参数 0 代表起始值为 0，lambda (a, b) -> a + b 即将两值相加产生一个新值

//计算年龄总乘积：
int sum = list.stream()
    .map(Person::getAge)
    .reduce(1, (a, b) -> a * b);
//与之相同:
Optional<Integer> sum = list.stream()
    .map(Person::getAge)
    .reduce(Integer::sum);
//不接受任何起始值，但因为没有初始值，需要考虑结果可能不存在的情况，因此返回的是 Optional 类型
```

### count()

返回流中元素个数，结果为 long 类型

### collect()

收集方法，我们很常用的是 `collect(toList())`，当然还有 `collect(toSet())` 等，参数是一个收集器接口

### forEach()

返回结果为 void，很明显我们可以通过它来干什么了，比方说：

```java
//打印各个元素：
list.stream()
    .forEach(System.out::println);
```

### unordered()

还有这个比较不起眼的方法，返回一个等效的无序流，当然如果流本身就是无序的话，那可能就会直接返回其本身

## 数值流

前面介绍的如`int sum = list.stream().map(Person::getAge).reduce(0, Integer::sum);` 计算元素总和的方法，其中暗含了装箱成本，`map(Person::getAge)` 方法过后流变成了 `Stream` 类型，而每个 `Integer` 都要拆箱成一个原始类型再进行 `sum` 方法求和，这样大大影响了效率。

针对这个问题 Java 8 有良心地引入了数值流 `IntStream`, `DoubleStream`, `LongStream`，这种流中的元素都是原始数据类型，分别是 `int`，`double`，`long`

### 流与数值流的转换

#### 流转换为数值流

*   mapToInt(T -> int) : return IntStream
*   mapToDouble(T -> double) : return DoubleStream
*   mapToLong(T -> long) : return LongStream

```java
IntStream intStream = list.stream()
    .mapToInt(Person::getAge);
```

#### 数值流转换为流

```java
Stream<Integer> stream = intStream.boxed();
```

### 数值流方法

*   sum()
*   max()
*   min()
*   average() 

### 数值范围

IntStream 与 LongStream 拥有 range 和 rangeClosed 方法用于数值范围处理

*   IntStream ： rangeClosed(int, int) / range(int, int)

*   LongStream ： rangeClosed(long, long) / range(long, long)

我们可以利用 IntStream.rangeClosed(1, 100) 生成 1 到 100 的数值流

```java
//求 1 到 10 的数值总和：
IntStream intStream = IntStream.rangeClosed(1, 10);
int sum = intStream.sum();
```

这两个方法的区别在于一个是闭区间，一个是半开半闭区间：

*   rangeClosed(1, 100) ：[1, 100]
*   range(1, 100) ：[1, 100)

### 汇总

#### counting

用于计算总和：

```java
long l = list.stream().collect(counting());
//推荐
long l = list.stream().count();
```

#### summingInt, summingLong, summingDouble

```java
int sum = list.stream().collect(summingInt(Person::getAge));
//推荐
int sum = list.stream().mapToInt(Person::getAge).sum();
int sum = list.stream().map(Person::getAge).reduce(Interger::sum).get();
```

#### averagingInt，averagingLong，averagingDouble

求平均数

```java
Double average = list.stream().collect(averagingInt(Person::getAge));

OptionalDouble average = list.stream().mapToInt(Person::getAge).average();
```

#### summarizingInt，summarizingLong，summarizingDouble

这三个方法比较特殊，比如 summarizingInt 会返回 IntSummaryStatistics 类型

```java
IntSummaryStatistics l = list.stream().collect(summarizingInt(Person::getAge));
```

IntSummaryStatistics 包含了计算出来的平均值，总数，总和，最值，可以通过下面这些方法获得相应的数据

*   getAverage()
*   getCount()
*   getMax()
*   getMin()
*   getSum()

### 取最值

maxBy，minBy 两个方法，需要一个 Comparator 接口作为参数

```java
Optional<Person> optional = list.stream().collect(maxBy(comparing(Person::getAge)));
Optional<Person> optional = list.stream().max(comparing(Person::getAge));
```

### joining 连接字符串

对流里面的字符串元素进行连接，**其底层实现用的是专门用于字符串连接的 StringBuilder**

```java
String s = list.stream().map(Person::getName).collect(joining());
//结果：jackmiketom
String s = list.stream().map(Person::getName).collect(joining(","));
//结果：jack,mike,tom
String s = list.stream().map(Person::getName).collect(joining(" and ", "Today ", " play games."));
//结果：Today jack and mike and tom play games
//即 Today 放开头，play games. 放结尾，and 在中间连接各个字符串
```

### groupingBy 分组

groupingBy 用于将数据分组，最终返回一个 Map 类型

```java
Map<Integer, List<Person>> map = list.stream().collect(groupingBy(Person::getAge));
//按照年龄 age 分组，每一个 Person 对象中年龄相同的归为一组
//Person::getAge 决定 Map 的键（Integer 类型），list 类型决定 Map 的值（List 类型）
```

### partitioningBy 分区

分区与分组的区别在于，分区是按照 true 和 false 来分的，因此partitioningBy 接受的参数的 lambda 也是 `T -> boolean`

```java
//根据年龄是否小于等于20来分区
Map<Boolean, List<Person>> map = list.stream()
                                     .collect(partitioningBy(p -> p.getAge() <= 20));
//打印输出
{
    false=[Person{name='mike', age=25}, Person{name='tom', age=30}], 
    true=[Person{name='jack', age=20}]
}
```

## 并行

之前我就讲到了 parallelStream 方法能生成并行流，因此你通常可以使用 parallelStream 来代替 stream 方法，但是并行的性能问题非常值得我们思考，比方说下面这个例子

```java
 int i = Stream.iterate(1, a -> a + 1).limit(100).parallel().reduce(0, Integer::sum);
```

我们通过这样一行代码来计算 1 到 100 的所有数的和，我们使用了 parallel 来实现并行。

但实际上是，这样的计算，效率是非常低的，比不使用并行还低！一方面是因为装箱问题，还有一方面就是 iterate 方法很难把这些数分成多个独立块来并行执行，因此无形之中降低了效率。

### 流的可分解性

使用并行的时候，我们要注意流背后的数据结构是否易于分解。比如众所周知的 ArrayList 和 LinkedList，明显前者在分解方面占优。

| 数据源          | 可分解性 |
| :-------------- | :------- |
| ArrayList       | 极佳     |
| LinkedList      | 差       |
| IntStream.range | 极佳     |
| Stream.iterate  | 差       |
| HashSet         | 好       |
| TreeSet         | 好       |

### 顺序性

除了可分解性，和刚刚提到的装箱问题，还有一点值得注意的是一些操作本身在并行流上的性能就比顺序流要差，比如：limit，findFirst， 因为这两个方法会考虑元素的顺序性，而并行本身就是违背顺序性的，也是因为如此 findAny 一般比 findFirst 的效率要高。