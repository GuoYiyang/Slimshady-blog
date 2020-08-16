## Java基础

### 数据类型

#### 基本类型

-   byte/8
-   char/16
-   short/16
-   int/32
-   float/32
-   long/64
-   double/64
-   boolean/~

>   boolean 只有两个值：true、false，可以使用 1 bit 来存储，但是具体大小没有明确规定。JVM 会在编译时期将 boolean 类型的数据转换为 int，使用 1 来表示 true，0 表示 false。JVM 支持 boolean 数组，但是是通过读写 byte 数组来实现的。

#### 包装类型

基本类型都有对应的包装类型，基本类型与其对应的包装类型之间的赋值使用**自动装箱**与**拆箱**完成。

```java
Integer x = 2;     // 装箱 调用了 Integer.valueOf(2)
int y = x;         // 拆箱 调用了 X.intValue()
```

#### 缓存池

new Integer(123) 与 Integer.valueOf(123) 的区别在于：

-   new Integer(123) 每次都会新建一个对象；
-   Integer.valueOf(123) 会使用**缓存池**中的对象，多次调用会取得同一个对象的引用。

```java
Integer x = new Integer(123);
Integer y = new Integer(123);
System.out.println(x == y);    // false
Integer z = Integer.valueOf(123);
Integer k = Integer.valueOf(123);
System.out.println(z == k);   // true
```

>   valueOf() 方法的实现比较简单，就是先判断值是否在缓存池中，如果在的话就直接返回缓存池的内容。

基本类型对应的缓冲池如下：

-   **boolean** values true and false
-   **all byte** values
-   **short** values between -128 and 127
-   **int** values between -128 and 127
-   **char** in the range \u0000 to \u007F

>   在使用这些基本类型对应的包装类型时，如果该数值范围在缓冲池范围内，就可以直接使用缓冲池中的对象。

### 字符串

#### 概览

**String 被声明为 final，因此它不可被继承。(Integer 等包装类也不能被继承）**

在 Java 8 中，String 内部使用 **char 数组**存储数据。

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final char value[];
}
```

在 Java 9 之后，String 类的实现改用 **byte 数组**存储字符串，同时使用 *coder* 来标识使用了哪种编码。

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final byte[] value;

    /** The identifier of the encoding used to encode the bytes in {@code value}. */
    private final byte coder;
}
```

#### String 的不可变性：

>   **value 数组被声明为 final**，这意味着 value 数组初始化之后就不能再引用其它数组。并且 String 内部没有改变 value 数组的方法，因此可以保证 String 不可变。

**不可变的好处：**

1.  可以缓存 hash 值

>   因为 String 的 hash 值经常被使用，例如 String 用做 HashMap 的 key。不可变的特性可以使得 **hash 值也不可变，因此只需要进行一次计算**。
>

2.  字符串常量池的需要

>   如果一个 String 对象已经被创建过了，那么就会从字符串常量池中取得引用。只有 String 是不可变的，才可能使用 String Pool。

3. 安全性

>   String 经常作为参数，String 不可变性可以保证参数不可变。例如在作为网络连接参数的情况下如果 String 是可变的，那么在网络连接过程中，String 被改变，改变 String 的那一方以为现在连接的是其它主机，而实际情况却不一定是。

4. 线程安全

>   String 不可变性天生具备线程安全，可以在多个线程中安全地使用。

#### String、StringBuffer 和 StringBuilder

1. 可变性

-   String 不可变
-   StringBuffer 和 StringBuilder 可变

2. 线程安全

-   String 不可变，因此是线程安全的
-   StringBuffer 是线程安全的，**内部使用 synchronized 进行同步**
-   StringBuilder 不是线程安全的

#### String Pool

字符串常量池（String Pool）**保存着所有字符串字面量，这些字面量在编译时期就确定**。不仅如此，还可以使用 String 的 intern() 方法在**运行过程**将字符串添加到 String Pool 中。

当一个字符串调用 intern() 方法时，如果 String Pool 中已经存在一个字符串和该字符串值相等（使用 equals() 方法进行确定），那么就会返回 String Pool 中字符串的引用；否则，就会在 String Pool 中添加一个新的字符串，并返回这个新字符串的引用。

>   下面示例中，s1 和 s2 采用 new String() 的方式新建了两个不同字符串，而 s3 和 s4 是通过 s1.intern() 方法取得同一个字符串引用。intern() 首先把 s1 引用的字符串放到 String Pool 中，然后返回这个字符串引用。因此 s3 和 s4 引用的是同一个字符串。

```java
String s1 = new String("aaa");
String s2 = new String("aaa");
System.out.println(s1 == s2);           // false
String s3 = s1.intern();
String s4 = s1.intern();
System.out.println(s3 == s4);           // true
```

>   **如果是采用 "bbb" 这种字面量的形式创建字符串，会自动地将字符串放入 String Pool 中。**

```java
String s5 = "bbb";
String s6 = "bbb";
System.out.println(s5 == s6);  // true
```

>   在 Java 7 之前，String Pool 被放在运行时常量池中，它属于永久代。而在 Java 7，String Pool 被移到堆中。这是因为永久代的空间有限，在大量使用字符串的场景下会导致 OutOfMemoryError 错误。

**new String("abc")**

>   使用这种方式一共会**创建两个**字符串对象（前提是 String Pool 中还没有 "abc" 字符串对象）。
>
>   -   "abc" 属于字符串字面量，因此编译时期会在 **String Pool 中**创建一个字符串对象，指向这个 "abc" 字符串字面量；
>   -   而使用 new 的方式会在**堆中**创建一个字符串对象。

以下是 String 构造函数的源码，可以看到，在将一个字符串对象作为另一个字符串对象的构造函数参数时，并不会完全复制 value 数组内容，而是都会**指向同一个 value 数组**。

```java
public String(String original) {
    this.value = original.value;
    this.hash = original.hash;
}
```

### 关键字

#### final

1. 修饰数据

    声明数据为常量，可以是编译时常量，也可以是在运行时被初始化后不能被改变的常量。

    -   对于基本类型，final 使数值不变；
    -   对于引用类型，final 使引用不变，也就**不能引用其它对象**，但是**被引用的对象本身是可以修改的**。

2. 修饰方法

    声明方法不能被子类重写。

    **private 方法隐式地被指定为 final**，如果在子类中定义的方法和基类中的一个 private 方法签名相同，此时子类的方法不是重写基类方法，而是在子类中**定义了一个新的方法**。

3. 修饰类

    声明类不允许被继承。

#### static

1. 静态变量
    -   静态变量：又称为类变量，也就是说这个变量属于类的，**类所有的实例都共享静态变量，可以直接通过类名来访问它**。**静态变量在内存中只存在一份**。
    -   实例变量：每创建一个实例就会产生一个实例变量，它与该实例同生共死。

2. 静态方法
    -   **静态方法在类加载的时候就存在了，它不依赖于任何实例**。所以静态方法必须有实现，也就是说它**不能是抽象方法**。
    -   **只能访问所属类的静态字段和静态方法**，方法中不能有 this 和 super 关键字，因此这两个关键字与具体对象关联。

3. 静态语句块
    -   静态语句块在**类初始化时运行一次**。

4. 静态内部类

    -   非静态内部类依赖于外部类的实例，也就是说需要先创建外部类实例，才能用这个实例去创建非静态内部类,而静态内部类不需要。

    -   静态内部类不能访问外部类的非静态的变量和方法。

5. 静态导包
    -   在使用静态变量和方法时不用再指明 ClassName，从而简化代码，但可读性大大降低。

6. 初始化顺序

    -   静态变量和静态语句块优先于实例变量和普通语句块，静态变量和静态语句块的初始化顺序取决于它们在代码中的顺序，最后才是构造函数的初始化。

    -   >   存在继承的情况下，初始化顺序为：
        >
        >   1.  父类（静态变量、静态语句块）
        >   2.  子类（静态变量、静态语句块）
        >   3.  父类（实例变量、普通语句块）
        >   4.  父类（构造函数）
        >   5.  子类（实例变量、普通语句块）
        >   6.  子类（构造函数）

### Object 中的方法

#### 概览

Object中的所有方法：

```java
public native int hashCode()

public boolean equals(Object obj)

protected native Object clone() throws CloneNotSupportedException

public String toString()

public final native Class<?> getClass()

protected void finalize() throws Throwable {}

public final native void notify()

public final native void notifyAll()

public final native void wait(long timeout) throws InterruptedException

public final void wait(long timeout, int nanos) throws InterruptedException

public final void wait() throws InterruptedException
```

#### equals()

**1. 等价关系**

两个对象具有等价关系，需要满足以下五个条件：

>   Ⅰ 自反性
>
>   ```java
>   x.equals(x); // true
>   ```
>
>   Ⅱ 对称性
>
>   ```java
>   x.equals(y) == y.equals(x); // true
>   ```
>
>   Ⅲ 传递性
>
>   ```java
>   if (x.equals(y) && y.equals(z))
>       x.equals(z); // true
>   ```
>
>   Ⅳ 一致性
>
>   多次调用 equals() 方法结果不变
>
>   ```java
>   x.equals(y) == x.equals(y); // true
>   ```
>
>   Ⅴ 与 null 的比较
>
>   对任何不是 null 的对象 x 调用 x.equals(null) 结果都为 false
>
>   ```java
>   x.equals(null); // false
>   ```

**2. 等价与相等**

-   对于基本类型，== 判断两个值是否相等，**基本类型没有 equals() 方法**。
-   对于引用类型，**== 判断两个变量是否引用同一个对象**，而 **equals() 判断引用的对象是否等价**。

```java
Integer x = new Integer(1);
Integer y = new Integer(1);
System.out.println(x.equals(y)); // true
System.out.println(x == y);      // false
```

**3. 手写equals（）方法**

1.  检查是否为同一个对象的引用，如果是直接返回 true；
2.  检查是否是同一个类型，如果不是，直接返回 false；
3.  将 Object 对象进行转型；
4.  判断指定的关键域是否相等。

```java
public class EqualExample {

    private int x;
    private int y;
    private int z;

    public EqualExample(int x, int y, int z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        EqualExample that = (EqualExample) o;

        if (x != that.x) return false;
        if (y != that.y) return false;
        return z == that.z;
    }
}
```

#### hashCode()

hashCode() 返回哈希值，而 equals() 是用来判断两个对象是否等价。

**等价的两个对象散列值一定相同，但是散列值相同的两个对象不一定等价**，这是因为计算哈希值具有随机性，两个值不同的对象可能计算出相同的哈希值。

>   在覆盖 equals() 方法时应当总是覆盖 hashCode() 方法，保证等价的两个对象哈希值也相等。

HashSet 和 HashMap 等集合类使用了 hashCode() 方法来计算对象应该存储的位置，因此要将对象添加到这些集合类中，需要让对应的类实现 hashCode() 方法。

理想的哈希函数应当具有均匀性，即不相等的对象应当均匀分布到所有可能的哈希值上。这就要求了哈希函数要把所有域的值都考虑进来。可以将每个域都当成 R 进制的某一位，然后组成一个 R 进制的整数。

R 一般取 31，因为它是一个奇素数，如果是偶数的话，当出现乘法溢出，信息就会丢失，因为与 2 相乘相当于向左移一位，最左边的位丢失。并且一个数与 31 相乘可以转换成移位和减法：`31*x == (x<<5)-x`，编译器会自动进行这个优化。

#### toString()

默认返回 `ToStringExample@4554617c` 这种形式，其中 @ 后面的数值为散列码的无符号十六进制表示。

```java
public class ToStringExample {

    private int number;

    public ToStringExample(int number) {
        this.number = number;
    }
}

ToStringExample example = new ToStringExample(123);

System.out.println(example.toString());	//ToStringExample@4554617c
```

#### clone()

**1. cloneable**

clone() 是 Object 的 protected 方法，它不是 public，**一个类不显式去重写 clone()，其它类就不能直接去调用该类实例的 clone() 方法**。

**2. 浅拷贝**

拷贝对象和原始对象的引用类型**引用同一个对象**。

**3. 深拷贝**

拷贝对象和原始对象的引用类型**引用不同对象**。

**4. clone() 的替代方案**

使用 clone() 方法来拷贝一个对象即复杂又有风险，它会抛出异常，并且还需要类型转换。Effective Java 书上讲到，最好不要去使用 clone()，可以使用**拷贝构造函数**或者**拷贝工厂**来拷贝一个对象。

### 面向对象三大特性

#### 封装

利用抽象数据类型将数据和基于数据的操作封装在一起，使其构成一个不可分割的独立实体。数据被保护在抽象数据类型的内部，尽可能地隐藏内部的细节，只保留一些对外的接口使其与外部发生联系。用户无需关心对象内部的细节，但可以通过对象对外提供的接口来访问该对象。

优点：

-   减少耦合：可以独立地开发、测试、优化、使用、理解和修改
-   减轻维护的负担：可以更容易被理解，并且在调试的时候可以不影响其他模块
-   有效地调节性能：可以通过剖析来确定哪些模块影响了系统的性能
-   提高软件的可重用性
-   降低了构建大型系统的风险：即使整个系统不可用，但是这些独立的模块却有可能是可用的

#### 继承

继承实现了 **IS-A** 关系，例如 Cat 和 Animal 就是一种 IS-A 关系，因此 Cat 可以继承自 Animal，从而获得 Animal 非 private 的属性和方法。

继承应该遵循里氏替换原则，子类对象必须能够替换掉所有父类对象。

#### 多态

多态分为编译时多态和运行时多态：

-   编译时多态主要指方法的重载
-   运行时多态指程序中定义的对象引用所指向的具体类型在运行期间才确定

运行时多态有三个条件：

-   继承
-   覆盖（重写）
-   向上转型

### 继承

#### 访问权限

Java 中有三个访问权限修饰符：private、protected 以及 public，如果不加访问修饰符，表示包级可见。

可以对类或类中的成员（字段和方法）加上访问修饰符。

-   类可见表示其它类可以用这个类创建实例对象。
-   成员可见表示其它类可以用这个类的实例对象访问到该成员；

protected 用于修饰成员，表示在继承体系中成员对于子类可见，但是这个访问修饰符对于类没有意义。

>   设计良好的模块会隐藏所有的实现细节，把它的 API 与它的实现清晰地隔离开来。模块之间只通过它们的 API 进行通信，一个模块不需要知道其他模块的内部工作情况，这个概念被称为信息隐藏或封装。因此访问权限应当尽可能地使每个类或者成员不被外界访问。

如果子类的方法重写了父类的方法，那么子类中该方法的访问级别不允许低于父类的访问级别。这是为了确保可以使用父类实例的地方都可以使用子类实例去代替，也就是确保满足**里氏替换原则**。

**字段决不能是公有的**，因为这么做的话就失去了对这个字段修改行为的控制，客户端可以对其随意修改。例如下面的例子中，AccessExample 拥有 id 公有字段，如果在某个时刻，我们想要使用 int 存储 id 字段，那么就需要修改所有的客户端代码。

```java
public class AccessExample {
    public String id;
}
```

可以使用公有的 getter 和 setter 方法来替换公有字段，这样的话就可以控制对字段的修改行为。

```java
public class AccessExample {

    private int id;

    public String getId() {
        return id + "";
    }

    public void setId(String id) {
        this.id = Integer.valueOf(id);
    }
}
```

但是也有例外，如果是包级私有的类或者私有的嵌套类，那么直接暴露成员不会有特别大的影响。

```java
public class AccessWithInnerClassExample {
	
    //私有的嵌套类
    private class InnerClass {
        int x;
    }

    private InnerClass innerClass;

    public AccessWithInnerClassExample() {
        innerClass = new InnerClass();
    }

    public int getValue() {
        return innerClass.x;  // 直接访问
    }
}
```

#### 抽象类与接口

**1. 抽象类**

抽象类和抽象方法都使用 abstract 关键字进行声明。如果一个类中包含抽象方法，那么这个类必须声明为抽象类。

抽象类和普通类最大的区别是，**抽象类不能被实例化，只能被继承**。

```java
public abstract class AbstractClassExample {

    protected int x;
    private int y;

    public abstract void func1();	//抽象方法

    public void func2() {
        System.out.println("func2");
    }
}

public class AbstractExtendClassExample extends AbstractClassExample {
    @Override
    public void func1() {
        System.out.println("func1");
    }
}

AbstractClassExample ac1 = new AbstractClassExample(); 
// 'AbstractClassExample' is abstract; cannot be instantiated
AbstractClassExample ac2 = new AbstractExtendClassExample();
ac2.func1();
```

**2. 接口**

接口是抽象类的延伸，在 Java 8 之前，它可以看成是一个完全抽象的类，也就是说它不能有任何的方法实现。

从 Java 8 开始，**接口也可以拥有默认的方法实现**，这是因为不支持默认方法的接口的维护成本太高了。在 Java 8 之前，如果一个接口想要添加新的方法，那么要修改所有实现了该接口的类，让它们都实现新增的方法。

>   接口的成员（字段 + 方法）默认都是 public 的，并且不允许定义为 private 或者 protected
>
>   接口的字段默认都是 static 和 final 的

```java
public interface InterfaceExample {

    void func1();

    default void func2(){
        System.out.println("func2");
    }

    int x = 123;
    // int y;               // Variable 'y' might not have been initialized
    public int z = 0;       // Modifier 'public' is redundant for interface fields
    // private int k = 0;   // Modifier 'private' not allowed here
    // protected int l = 0; // Modifier 'protected' not allowed here
    // private void fun3(); // Modifier 'private' not allowed here
}

public class InterfaceImplementExample implements InterfaceExample {
    @Override
    public void func1() {
        System.out.println("func1");
    }
}

// InterfaceExample ie1 = new InterfaceExample(); 
// 'InterfaceExample' is abstract; cannot be instantiated
InterfaceExample ie2 = new InterfaceImplementExample();
ie2.func1();
System.out.println(InterfaceExample.x);
```

**3. 比较**

-   从设计层面上看，**抽象类提供了一种 IS-A 关系**，需要满足里式替换原则，即子类对象必须能够替换掉所有父类对象。而**接口更像是一种 LIKE-A 关系**，它只是提供一种方法实现契约，并不要求接口和实现接口的类具有 IS-A 关系。
-   从使用上来看，一个类可以实现多个接口，但是不能继承多个抽象类。
-   接口的字段只能是 static 和 final 类型的，而抽象类的字段没有这种限制。
-   接口的成员只能是 public 的，而抽象类的成员可以有多种访问权限。

**4. 使用选择**

使用接口：

-   需要让**不相关的类**都实现一个方法，例如不相关的类都可以实现 Compareable 接口中的 compareTo() 方法；
-   需要使用**多重继承**。

使用抽象类：

-   需要在几个**相关的类**中共享代码。
-   需要能控制继承来的成员的访问权限，而不是都为 public。
-   需要继承非静态和非常量字段。

>   在很多情况下，接口优先于抽象类。因为接口没有抽象类严格的类层次结构要求，可以灵活地为一个类添加行为。并且从 Java 8 开始，接口也可以有默认的方法实现，使得修改接口的成本也变的很低。

#### super

-   访问父类的构造函数：可以使用 super() 函数访问父类的构造函数，从而委托父类完成一些初始化的工作。应该注意到，子类一定会调用父类的构造函数来完成初始化工作，一般是调用父类的默认构造函数，如果子类需要调用父类其它构造函数，那么就可以使用 super() 函数。
-   访问父类的成员：如果子类重写了父类的某个方法，可以通过使用 super 关键字来引用父类的方法实现。

```java
public class SuperExample {

    protected int x;
    protected int y;

    public SuperExample(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public void func() {
        System.out.println("SuperExample.func()");
    }
}

public class SuperExtendExample extends SuperExample {

    private int z;

    public SuperExtendExample(int x, int y, int z) {
        super(x, y);
        this.z = z;
    }

    @Override
    public void func() {
        super.func();
        System.out.println("SuperExtendExample.func()");
    }
}

SuperExample e = new SuperExtendExample(1, 2, 3);
e.func();

//SuperExample.func()
//SuperExtendExample.func()
```

#### 重写与重载

**1. 重写（Override）**

存在于继承体系中，指子类实现了一个与父类在方法声明上完全相同的一个方法。

>   为了满足里式替换原则，重写有以下三个限制：
>
>   -   子类方法的访问权限必须大于等于父类方法；
>   -   子类方法的返回类型必须是父类方法返回类型或为其子类型。
>   -   子类方法抛出的异常类型必须是父类抛出异常类型或为其子类型。

使用 `@Override` 注解，可以让编译器帮忙检查是否满足上面的三个限制条件。

下面的示例中，SubClass 为 SuperClass 的子类，SubClass 重写了 SuperClass 的 func() 方法。其中：

-   子类方法访问权限为 public，大于父类的 protected。
-   子类的返回类型为 ArrayList，是父类返回类型 List 的子类。
-   子类抛出的异常类型为 Exception，是父类抛出异常 Throwable 的子类。
-   子类重写方法使用 `@Override` 注解，从而让编译器自动检查是否满足限制条件。

```java
class SuperClass {
    protected List<Integer> func() throws Throwable {
        return new ArrayList<>();
    }
}

class SubClass extends SuperClass {
    @Override
    public ArrayList<Integer> func() throws Exception {
        return new ArrayList<>();
    }
}
```

在调用一个方法时，先从本类中查找看是否有对应的方法，如果没有再到父类中查看，看是否从父类继承来。否则就要对参数进行转型，转成父类之后看是否有对应的方法。总的来说，方法调用的优先级为：

1.  this.func(this)
2.  super.func(this)
3.  this.func(super)
4.  super.func(super)

```java
/*
    A
    |
    B
    |
    C
    |
    D
 */

class A {

    public void show(A obj) {
        System.out.println("A.show(A)");
    }

    public void show(C obj) {
        System.out.println("A.show(C)");
    }
}

class B extends A {

    @Override
    public void show(A obj) {
        System.out.println("B.show(A)");
    }
}

class C extends B {
}

class D extends C {
}

public static void main(String[] args) {

    A a = new A();
    B b = new B();
    C c = new C();
    D d = new D();

    // 在 A 中存在 show(A obj)，直接调用
    a.show(a); // A.show(A)
    // 在 A 中不存在 show(B obj)，将 B 转型成其父类 A
    a.show(b); // A.show(A)
    // 在 B 中存在从 A 继承来的 show(C obj)，直接调用
    b.show(c); // A.show(C)
    // 在 B 中不存在 show(D obj)，但是存在从 A 继承来的 show(C obj)，将 D 转型成其父类 C
    b.show(d); // A.show(C)

    // 引用的还是 B 对象，所以 ba 和 b 的调用结果一样
    A ba = new B();
    ba.show(c); // A.show(C)
    ba.show(d); // A.show(C)
}
```

**2. 重载（Overload）**

存在于同一个类中，指一个方法与已经存在的方法名称上相同，但是**参数类型、个数、顺序至少有一个不同**。

>   应该注意的是，返回值不同，其它都相同不算是重载。

### 反射

#### 概述

每个类都有一个 **Class** 对象，包含了与类有关的信息。当编译一个新类时，会产生一个同名的 .class 文件，该文件内容保存着 Class 对象。

类加载相当于 Class 对象的加载，类在第一次使用时才动态加载到 JVM 中。也可以使用 `Class.forName("com.mysql.jdbc.Driver")` 这种方式来控制类的加载，该方法会返回一个 Class 对象。

反射可以提供运行时的类信息，并且这个类可以在运行时才加载进来，甚至在编译时期该类的 .class 不存在也可以加载进来。

简而言之，**通过反射，我们可以在运行时获得程序或程序集中每一个类型的成员和成员的信息**。程序中一般的对象的类型都是在编译期就确定下来的，而 Java 反射机制可以动态地创建对象并调用其属性，这样的对象的类型在编译期是未知的。所以我们可以通过反射机制直接创建对象，即使这个对象的类型在编译期是未知的。

反射的核心是 JVM 在运行时才动态加载类或调用方法/访问属性，它不需要事先（写代码的时候或编译期）知道运行对象是谁。

>   Java 反射主要提供以下功能：
>
>   -   在运行时判断任意一个对象所属的类；
>   -   在运行时构造任意一个类的对象；
>   -   在运行时判断任意一个类所具有的成员变量和方法（通过反射甚至可以调用private方法）；
>   -   在运行时调用任意一个对象的方法

重点：**是运行时而不是编译时**

Class 和 java.lang.reflect 一起对反射提供了支持，java.lang.reflect 类库主要包含了以下三个类：

-   **Field** ：可以使用 get() 和 set() 方法读取和修改 Field 对象关联的字段；
-   **Method** ：可以使用 invoke() 方法调用与 Method 对象关联的方法；
-   **Constructor** ：可以用 Constructor 的 newInstance() 创建新的对象。

#### 基本运用

##### 获得 Class 对象

方法有三种：

(1) 使用 Class 类的 `forName` 静态方法:

```JAVA
 public static Class<?> forName(String className)
 Class.forName(driver);	//比如在 JDBC 开发中常用此方法加载数据库驱动
```

(2)直接获取某一个对象的 class，比如:

```JAVA
Class<?> klass = int.class;
Class<?> classInt = Integer.TYPE;
```

(3)调用某个对象的 `getClass()` 方法，比如:

```JAVA
StringBuilder str = new StringBuilder("123");
Class<?> klass = str.getClass();
```

##### 判断是否为某个类的实例

一般地，我们用 `instanceof` 关键字来判断是否为某个类的实例。同时我们也可以借助反射中 Class 对象的 `isInstance()` 方法来判断是否为某个类的实例，它是一个 native 方法：

```java
public native boolean isInstance(Object obj);
```

##### 创建实例

通过反射来生成对象主要有两种方式。

1.  使用Class对象的newInstance()方法来创建Class对象对应类的实例。

```java
Class<?> c = String.class;
Object str = c.newInstance();
```

2.  先通过Class对象获取指定的Constructor对象，再调用Constructor对象的newInstance()方法来创建实例。这种方法可以用指定的构造器构造类的实例。

```java
//获取String所对应的Class对象Class<?> 
c = String.class;
//获取String类带一个String参数的构造器
Constructor constructor = c.getConstructor(String.class);
//根据构造器创建实例
Object obj = constructor.newInstance("23333");
System.out.println(obj);
```

##### 获取类的成员变量（字段）

`Class`类提供了以下几个方法来获取字段：

-   Field getField(name)：根据字段名获取某个**public**的field（包括父类）
-   Field getDeclaredField(name)：根据字段名获取当前类的某个field（不包括父类）
-   Field[] getFields()：获取所有**public**的field（包括父类）
-   Field[] getDeclaredFields()：获取当前类的所有field（不包括父类）

**获取字段值：**

一个`Field`对象包含了一个字段的所有信息：

-   `getName()`：返回字段名称，例如，`"name"`；
-   `getType()`：返回字段类型，也是一个`Class`实例，例如，`String.class`；
-   `getModifiers()`：返回字段的修饰符，它是一个`int`，不同的bit表示不同的含义。

>   正常情况下，`Main`类无法访问类的`private`字段。要修复错误，可以将`private`改为`public`，或者，在调用getXxx()前，先写一句：
>
>   ```java
>   Field.setAccessible(true);
>   ```
>
>   调用`Field.setAccessible(true)`的意思是，别管这个字段是不是`public`，一律允许访问。

##### 获取方法

`Class`类提供了以下几个方法来获取`Method`：

-   `Method getMethod(name, Class...)`：获取某个`public`的`Method`（包括父类）
-   `Method getDeclaredMethod(name, Class...)`：获取当前类的某个`Method`（不包括父类）
-   `Method[] getMethods()`：获取所有`public`的`Method`（包括父类）
-   `Method[] getDeclaredMethods()`：获取当前类的所有`Method`（不包括父类）

一个`Method`对象包含一个方法的所有信息：

-   `getName()`：返回方法名称，例如：`"getScore"`；
-   `getReturnType()`：返回方法返回值类型，也是一个Class实例，例如：`String.class`；
-   `getParameterTypes()`：返回方法的参数类型，是一个Class数组，例如：`{String.class, int.class}`；
-   `getModifiers()`：返回方法的修饰符，它是一个`int`，不同的bit表示不同的含义。

**调用方法：**

当我们从类中获取了一个方法后，我们就可以用 `invoke()` 方法来调用这个方法。`invoke` 方法的原型为:

```java
public Object invoke(Object obj, Object... args)
        throws IllegalAccessException, IllegalArgumentException,
           InvocationTargetException
```

下面时一个实例：

```java
public class test1 {
    public static void main(String[] args) throws IllegalAccessException, InstantiationException, NoSuchMethodException, InvocationTargetException {
        Class<?> klass = methodClass.class;
        //创建methodClass的实例
        Object obj = klass.newInstance();
        //获取methodClass类的add方法
        Method method = klass.getMethod("add",int.class,int.class);
        //调用method对应的方法 => add(1,4)
        Object result = method.invoke(obj,1,4);
        System.out.println(result);
    }
}
class methodClass {
    public final int fun = 3;
    public int add(int a,int b) {
        return a+b;
    }
    public int sub(int a,int b) {
        return a-b;
    }
}
```

>   为了调用非public方法，我们通过`Method.setAccessible(true)`允许其调用

##### 获取构造器信息

通过Class实例获取Constructor的方法如下：

-   `getConstructor(Class...)`：获取某个`public`的`Constructor`；
-   `getDeclaredConstructor(Class...)`：获取某个`Constructor`；
-   `getConstructors()`：获取所有`public`的`Constructor`；
-   `getDeclaredConstructors()`：获取所有`Constructor`。

>   注意`Constructor`总是当前类定义的构造方法，和父类无关，因此不存在多态的问题。

>   调用非`public`的`Constructor`时，必须首先通过`setAccessible(true)`设置允许访问。`setAccessible(true)`可能会失败。

##### 获取对象实例

主要是通过Class类的getConstructor方法得到Constructor类的一个实例，而Constructor类有一个newInstance方法可以创建一个对象实例:

```java
public T newInstance(Object ... initargs)
```

此方法可以根据传入的参数来调用对应的Constructor创建对象实例。

#### 优缺点

**反射的优点：**

-   **可扩展性** ：应用程序可以利用全限定名创建可扩展对象的实例，来使用来自外部的用户自定义类。
-   **类浏览器和可视化开发环境** ：一个类浏览器需要可以枚举类的成员。可视化开发环境（如 IDE）可以从利用反射中可用的类型信息中受益，以帮助程序员编写正确的代码。
-   **调试器和测试工具** ： 调试器需要能够检查一个类里的私有成员。测试工具可以利用反射来自动地调用类里定义的可被发现的 API 定义，以确保一组测试中有较高的代码覆盖率。

**反射的缺点：**

尽管反射非常强大，但也不能滥用。如果一个功能可以不用反射完成，那么最好就不用。在我们使用反射技术时，下面几条内容应该牢记于心。

-   **性能开销** ：反射涉及了动态类型的解析，所以 JVM 无法对这些代码进行优化。因此，反射操作的效率要比那些非反射操作低得多。我们应该避免在经常被执行的代码或对性能要求很高的程序中使用反射。
-   **安全限制** ：使用反射技术要求程序必须在一个没有安全限制的环境中运行。如果一个程序必须在有安全限制的环境中运行，如 Applet，那么这就是个问题了。
-   **内部暴露** ：由于反射允许代码执行一些在正常情况下不被允许的操作（比如访问私有的属性和方法），所以使用反射可能会导致意料之外的副作用，这可能导致代码功能失调并破坏可移植性。反射代码破坏了抽象性，因此当平台发生改变的时候，代码的行为就有可能也随着变化。
