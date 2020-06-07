# Spring

## Spring框架的七大模块

-   **Spring Core**：框架的最基础部分，提供 IoC 容器，对 bean 进行管理。
-   **Spring Context**：继承BeanFactory，提供上下文信息，扩展出JNDI、EJB、电子邮件、国际化等功能。
-   **Spring DAO**：提供了JDBC的抽象层，还提供了声明性事务管理方法。
-   **Spring ORM**：提供了JPA、JDO、Hibernate、MyBatis 等ORM映射层.
-   **Spring AOP**：集成了所有AOP功能
-   **Spring Web**：提供了基础的 Web 开发的上下文信息，现有的Web框架，如JSF、Tapestry、Structs等，提供了集成
-   **Spring Web MVC**：提供了 Web 应用的 Model-View-Controller 全功能实现。

## Spring AOP

### 概念梳理

#### 什么是AOP 

1.  AOP 是 ==Aspect Oriented Programming（面向切面编程）== 的简称，和OOP（面向对象编程）一样是一种编程思想，是对OOP的一种补充。
2.  AOP旨在将横切关注点（crosscutting concern）从业务主体逻辑中进行剥离，实现关注点分离，以提高程序的模块化程度（及业务模块只需关注业务逻辑，无需关注日志、安全、事务等通用逻辑）

#### 常见术语

AOP有自己的一套术语，我们必须了解一下这些行话，才能更好地理解AOP。为了方便大家理解，下面将用电表抄表员作为例子。

1.  通知 (Advice)
    *   电表抄表员的工作是登记每家每户的电量并汇报给电力公司，记录电量是抄表员的主要工作，类似的，==切面需要完成的工作称为通知==。
    *   定义了切面在切点附近的操作 （什么时候做什么事）。
    *   常见的通知类型有：before、after、after-returning、around等
2.  连接点 (JoinPoint)
    *   电力公司为整个城市提供供电，每家都有一个电表，每一家都是抄表员的潜在目标。同样的，我们的==程序也有千万的时间点可以执行通知，这些时间点被称为连接点==。
    *   连接点指程序运行时允许插入切面的一个点，可以是一个函数、一个包路径、一个类、或者抛出的异常
3.  切点（PointCut）
    *   如果让一位抄表员访问所有电力公司的住户是不现实的，一般每一个抄表员仅负责某一个区域的住户，同样的，==切点用于指定连接点的范围==。
    *   切点用于定义切面的位置，也就是捕获哪些连接点的调用然后执行"通知"的操作（什么地点）。
4.  切面（Aspect）
    *   当抄表员开始一天的工作时，他知道自己要做的事情（记录电量）以及去哪里收集信息（去哪些地点），这个==聚合信息称为切面==。
    *   切面是切点和通知的聚合，一般是一个横跨多各类的通用逻辑抽象而成的类，切面类会定义在什么时间、什么地点、做什么事
5.  目标对象( Target )
    *   指被切面织入的对象
6.  引入（Introduction）
    *   引入允许我们向现有的类添加新方法或属性。
7.  织入（Weaving）
    *   织入是把切面应用到切点对应的连接点的过程。切面在指定连接点被织入到目标对象中。

## Spring IoC

>   IoC - Inversion of Control, 控制反转。

### 初识IoC

#### 什么是控制反转？

控制反转就是把创建和管理 bean 的过程转移给了第三方。而这个第三方，就是 Spring IoC Container，对于 IoC 来说，最重要的就是**容器**。

容器负责创建、配置和管理 bean，也就是它管理着 bean 的生命，控制着 bean 的依赖注入。通俗点讲，因为项目中每次创建对象是很麻烦的，所以使用 Spring IoC 容器来管理这些对象，需要的时候就直接用，不用管它是怎么来的、什么时候要销毁，只管用就好了。

举个例子，就好像父母没时间管孩子，就把小朋友交给托管所，就安心的去上班而不用管孩子了。托儿所，就是第三方容器，负责管理小朋友的吃喝玩乐；父母，相当于程序员，只管接送孩子，不用管他们吃喝。

#### `bean` 是什么？

Bean 其实就是包装了的 Object，无论是控制反转还是依赖注入，它们的主语都是 Object，而 bean 就是由第三方包装好了的 Object。（想一下别人送礼物给你的时候都是要包装一下的，自己造的就免了。

Bean 是 Spring 的主角，有种说法叫 Spring 就是面向 bean 的编程（Bean Oriented Programming, BOP）。

#### 几个关键问题：

##### 何为控制，控制的是什么？

答：是 bean 的创建、管理的权利，控制 bean 的整个生命周期。

##### 何为反转，反转了什么？

答：把这个权利交给了 Spring 容器，而不是自己去控制，就是反转。由之前的自己主动创建对象，变成现在被动接收别人给我们的对象的过程，这就是反转。

举个生活中的例子，主动投资和被动投资。

自己炒股、选股票的人就是主动投资，主动权掌握在自己的手中；而买基金的人就是被动投资，把主动权交给了基金经理，除非你把这个基金卖了，否则具体选哪些投资产品都是基金经理决定的。

### IoC 容器

#### IoC容器的设计

使用 `ApplicationContext`，它是 `BeanFactory` 的子类，更好的补充并实现了 `BeanFactory` 。

`BeanFactory` 简单粗暴，可以理解为 HashMap：

-   Key - bean name
-   Value - bean object

但它一般只有 get, put 两个功能，所以称之为“低级容器”。而 `ApplicationContext` 多了很多功能，因为它继承了多个接口，可称之为“高级容器”。

`ApplicationContext` 的里面有两个具体的实现子类，用来读取配置配件的：

-   `ClassPathXmlApplicationContext` - 从 class path 中加载配置文件，更常用一些；
-   `FileSystemXmlApplicationContext` - 从本地文件中加载配置文件，不是很常用，如果再到 Linux 环境中，还要改路径，不是很方便。

当我们点开 `ClassPathXmlApplicationContext` 时，发现它并不是直接继承 `ApplicationContext` 的，它有很多层的依赖关系，每层的子类都是对父类的补充实现。而再往上找，发现最上层的 class 回到了 `BeanFactory`，所以它非常重要。

>   要注意，Spring 中还有个 `FactoryBean`，两者并没有特别的关系，只是名字比较接近，所以不要弄混了顺序。

#### IoC容器的初始化

Spring IOC容器初始化的核心过程可以简单的如下图表示，主要有四个步骤（还有一些如：后置加载器，国际化，事件广播器等一些过程不展开）：

1.  Bean定义的定位，Bean 可能定义在XML中，或者一个注解，或者其他形式。这些都被用Resource来定位，读取Resource获取BeanDefinition 注册到 Bean定义注册表中。
2.  第一次向容器getBean操作会触发Bean的创建过程,实列化一个Bean时 ,根据BeanDefinition中类信息等实列化Bean。
3.  将实列化的Bean放到单列Bean缓存内。
4.  此后再次获取向容器getBean就会从缓存中获取。

### Spring Bean

#### Bean的注入方式

目前主要有五种注入方式：SET注入，构造器注入，静态工厂，实例工厂，注解方式

#### Bean解析注册过程

这个过程完成Bean的从配置文件到解析注册成bean工厂的过程（对应代码在AbstactApplicationContext.obtainFreshBeanFactory）

1.  通过读取XML配置文件获取 Resource 资源，获取这个资源包含了路径config/spring/local/appcontext-client.xml 文件下定义的BeanDefinition信息。
2.  创建一个 BeanFactory，这里使用 DefaultListableBeanFactory。
3.  创建一个载入 BeanDefinition 的解读器，这里使用 XmlBeanDefinitionReader 来载入 XML 文件形式 BeanDefinition，通过一个回调配置给 factory。
4.  从定义好的资源位置读入配置信息，具体的解析过程由 XmlBeanDefinitionReader 的 loadBeanDefinitions() 方法来完成。完成整个载入和注册 Bean 定义之后，需要的 IoC 容器就建立起来可以直接使用了。

### 依赖注入

>   DI - dependency injection, 依赖注入

#### 何为依赖，依赖什么？

程序运行需要依赖外部的资源，提供程序内对象的所需要的数据、资源。

#### 何为注入，注入什么？

配置文件把资源从外部注入到内部，容器加载了外部的文件、对象、数据，然后把这些资源注入给程序内的对象，维护了程序内外对象之间的依赖关系。

所以说，==控制反转是通过依赖注入实现的==。但是它们是有差别的，像是**从不同角度描述的同一件事**：

-   IoC 是设计思想，DI 是具体的实现方式
-   IoC 是理论，DI 是实践

从而实现对象之间的解藕。

**当然，IoC 也可以通过其他的方式来实现，而 DI 只是 Spring 的选择。**

IoC 和 DI 也并非 Spring 框架提出来的，Spring 只是应用了这个设计思想和理念到自己的框架里去。

### 使用 IoC 的好处

==解藕==。

它把对象之间的依赖关系转成用配置文件来管理，由 Spring IoC Container 来管理。

在项目中，底层的实现都是由很多个对象组成的，对象之间彼此合作实现项目的业务逻辑。但是，很多很多对象紧密结合在一起，一旦有一方出问题了，必然会对其他对象有所影响，所以才有了解藕的这种设计思想。

## Spring 创建对象的过程

通过 `ApplicationContext` 这个 IoC 容器的入口，用它的两个具体的实现子类，从 class path 或者 file path 中读取数据，用 `getBean()` 获取具体的 bean instance。

那使用 Spring 到底省略了我们什么工作？

答：`new 的过程`。把 new 的过程交给第三方来创建、管理，这就是「解藕」。Spring 也是用的 `set()` 方法，它只不过提供了一套更加完善的实现机制而已。





