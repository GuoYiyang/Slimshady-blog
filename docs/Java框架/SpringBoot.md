# SpringBoot

## 配置相关

### JavaConfig

Spring JavaConfig 是 Spring 社区的产品，它提供了配置 Spring IoC 容器的纯 Java 方法，因此它有助于避免使用 XML 配置。使用 JavaConfig 的优点在于：

*   面向对象的配置。

    由于配置被定义为 JavaConfig 中的类，因此用户可以充分利用 Java 中的面向对象功能。一个配置类可以继承另一个，重写它的@Bean 方法等。

*   减少或消除 XML 配置。

    许多开发人员不希望在 XML 和 Java 之间来回切换。JavaConfig 为开发人员提供了一种纯 Java 方法来配置与 XML 配置概念相似的 Spring 容器。从技术角度来讲，只使用 JavaConfig 配置类来配置容器是可行的，但实际上很多人认为将JavaConfig 与 XML 混合匹配是理想的。

*   类型安全和重构友好。

    JavaConfig 提供了一种类型安全的方法来配置 Spring容器。由于 Java 5.0 对泛型的支持，现在可以按类型而不是按名称检索 bean，不需要任何强制转换或基于字符串的查找。

### 配置加载顺序

在 Spring Boot 里面，可以使用以下几种方式来加载配置。

1.  properties文件

2.  YAML文件

3.  系统环境变量

4.  命令行参数

### 核心配置文件

*   bootstrap (. yml 或者 . properties)：boostrap 由 ApplicationContext 加载，比 applicaton 优先加载，配置在应用程序上下文的引导阶段生效。一般来说我们在 Spring Cloud Config 或者 Nacos 中会用到它，且 boostrap 里面的属性不能被覆盖
*   application (. yml 或者 . properties)：由ApplicatonContext 加载，用于 spring boot 项目的自动化配置

### YAML 配置文件

YAML 是一种人类可读的数据序列化语言，它通常用于配置文件。与属性文件相比，如果我们想要在配置文件中添加复杂的属性，YAML 文件就更加结构化，而且更少混淆。 YAML 具有分层配置数据。YAML 配置和传统的 properties 配置相比有如下优势：

1.  配置有序，在一些特殊的场景下，配置有序很关键
2.  支持数组，数组中的元素可以是基本数据类型也可以是对象
3.  简洁

相比 properties 配置文件，YAML 还有一个缺点，就是不支持 @PropertySource 注解导入自定义的 YAML 配置。

### XML配置文件

Spring Boot 推荐使用 Java 配置而非 XML 配置，但是 Spring Boot 中也可以使用 XML 配置，通过 @ImportResource 注解可以引入一个 XML 配置。

### Spring Profiles

Spring Profiles 允许用户根据配置文件（dev，test，prod 等）来注册 bean。因此，当应用程序在开发中运行时，只有某些 bean 可以加载，而在 PRODUCTION中，某些其他 bean 可以加载。

## 安全相关

### 实现安全性

为了实现 Spring Boot 的安全性，使用 spring-boot-starter-security 依赖项，并且必须添加安全配置，它只需要很少的代码。配置类将必须扩展WebSecurityConfigurerAdapter 并覆盖其方法。

### Spring Security 和 Shiro 的优缺点

如果是 Spring Boot 项目，一般选择 Spring Security ，当然这只是一个建议的组合，单纯从技术上来说，无论怎么组合，都是没有问题的。

1.  Spring Security 是一个重量级的安全管理框架；Shiro 则是一个轻量级的安全管理框架
2.  Spring Security 概念复杂，配置繁琐；Shiro 概念简单、配置简单
3.  Spring Security 功能强大；Shiro 功能简单

## 监视器

### 什么是监视器

Spring boot actuator 是 spring 启动框架中的重要功能之一。Spring boot 监视器可帮助您访问生产环境中正在运行的应用程序的当前状态。有几个指标必须在生产环境中进行检查和监控。即使一些外部应用程序可能正在使用这些服务来向相关人员触发警报消息。监视器模块公开了一组可直接作为 HTTP URL 访问的REST 端点来检查状态。

## 常用注解

### 1.启动相关

#### 1.1 `@SpringBootApplication`

这个注解是 Spring Boot 项目的基石，创建 SpringBoot 项目之后会默认在主类加上。

我们可以把 `@SpringBootApplication`看作是 `@Configuration`、`@EnableAutoConfiguration`、`@ComponentScan` 注解的集合。

根据 SpringBoot 官网，这三个注解的作用分别是：

-   `@EnableAutoConfiguration`：启用 SpringBoot 的自动配置机制
-   `@ComponentScan`：扫描被`@Component` (`@Service`, ` @Controller` ,`@Repository`)注解的 bean，注解默认会扫描==该类所在的包下所有的类==。
-   `@Configuration`：允许在 Spring 上下文中注册额外的 bean 或导入其他配置类

### 2. Spring Bean 相关

#### 2.1. `@Autowired`

自动导入对象到类中，被注入进的类同样要被 Spring 容器管理。

例如：将Service 类注入到 Controller 类中。

```java
@Service
public class UserService {
  ......
}

@RestController
@RequestMapping("/users")
public class UserController {
   @Autowired
   private UserService userService;
   ......
}
```

#### 2.2. `@Component`, `@Repository`, `@Service`,  `@Controller`

我们一般使用 `@Autowired` 注解让 Spring 容器帮我们自动装配 bean，要想把类标识成可用于 `@Autowired` 注解自动装配的 bean 的类，可以采用以下注解实现：

-   `@Component` ：通用的注解，可标注==任意类==为 `Spring` 组件。如果一个 bean 不知道属于哪个层，可以使用`@Component` 注解标注。
-   `@Repository` : 对应==持久层==即 Dao 层，主要用于数据库相关操作。
-   `@Service` : 对应==服务层==，主要涉及一些复杂的逻辑，需要用到 Dao 层。
-   `@Controller` : 对应 Spring MVC ==控制层==，主要用户接受用户请求并调用 Service 层返回数据给前端页面。

#### 2.3. `@RestController`,  `@Controller` + `@ResponseBody`

`@RestController`注解是`@Controller和`@`ResponseBody`的合集,表示这是个控制器 bean,并且是==将函数的返回值直接填入 HTTP 响应体中==,是 REST 风格的控制器。

单独使用 `@Controller` 不加 `@ResponseBody`的话一般使用在要==返回一个视图==的情况，这种情况属于比较传统的 Spring MVC 的应用，对应于前后端不分离的情况。

`@Controller` + `@ResponseBody` 返回 JSON 或 XML 形式数据，即==只返回对象==，对象数据直接以 JSON 或 XML 形式写入 HTTP 响应(Response)中，这种情况属于 RESTful Web服务，这也是目前日常开发所接触的最常用的情况（前后端分离）。

>   如果在Spring4之前开发 RESTful Web服务的话，则需要使用`@Controller` 并结合`@ResponseBody`注解，也就是说`@Controller` +`@ResponseBody`= `@RestController`，`@RestController`是Spring 4 之后新加的注解

`@ResponseBody` 注解的作用是将 `Controller` 的方法返回的对象通过适当的转换器转换为指定的格式之后，写入到HTTP 响应(Response)对象的 body 中，通常用来返回 JSON 或者 XML 数据，返回 JSON 数据的情况比较多。

##### 示例1: @Controller 返回一个页面

当我们需要直接在后端返回一个页面的时候，Spring 推荐使用 Thymeleaf 模板引擎。Spring MVC中`@Controller`中的方法可以直接返回模板名称，接下来 Thymeleaf 模板引擎会自动进行渲染,模板中的表达式支持Spring表达式语言（Spring EL)。如果需要用到 Thymeleaf 模板引擎，需要添加依赖。

**Maven:**

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

`src/main/java/com/example/demo/controller/HelloController.java`

```java
@Controller
public class HelloController {
    @GetMapping("/hello")
    public String greeting(@RequestParam(name = "name", required = false, defaultValue = "World") String name, Model model) {
        model.addAttribute("name", name);
        return "hello";
    }
}
```

`src/main/resources/templates/hello.html`

Spring 默认会去 resources 目录下 templates 目录下找，所以建议把页面放在 resources/templates 目录下

```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Getting Started: Serving Web Content</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8"/>
</head>
<body>
<p th:text="'Hello, ' + ${name} + '!'"/>
</body>
</html>
```

访问：http://localhost:8999/hello?name=team-c ，将看到下面的内容

```
Hello, team-c!
```

如果要对页面在templates目录下的hello文件夹中的页面话，返回页面的时候像下面这样写就可以了：

`src/main/resources/templates/hello/hello.html`

```java
@Controller
public class HelloController {
    @GetMapping("/hello")
    public String greeting(@RequestParam(name = "name", required = false, defaultValue = "World") String name, Model model) {
        model.addAttribute("name", name);
        return "hello/hello";
    }
}
```

##### 示例2: @Controller+@ResponseBody 返回 JSON 格式数据

SpringBoot 默认集成了 jackson ,因此不需要添加任何相关依赖。

`src/main/java/com/example/demo/controller/Person.java`

```java
public class Person {
    private String name;
    private Integer age;
    //......
    //省略getter/setter ，有参和无参的construtor方法
}
```

`src/main/java/com/example/demo/controller/HelloController.java`

```java
@Controller
public class HelloController {
    @PostMapping("/hello")
    @ResponseBody
    public Person greeting(@RequestBody Person person) {
        return person;
    }
}
```

使用 post 请求访问 http://localhost:8080/hello ，body 中附带以下参数,后端会以json 格式将 person 对象返回。

```json
{
    "name": "teamc",
    "age": 1
}
```

##### 示例3: @RestController 返回 JSON 格式数据

只需要将`HelloController`改为如下形式：

```java
@RestController
public class HelloController {
    @PostMapping("/hello")
    public Person greeting(@RequestBody Person person) {
        return person;
    }
}
```

#### 2.4. `@Scope`

声明 Spring Bean 的作用域，使用方法:

```java
@Bean
@Scope("singleton")
public Person personSingleton() {
    return new Person();
}
```

**四种常见的 Spring Bean 的作用域：**

*   singleton : 唯一 bean 实例，Spring 中的 bean 默认都是单例的。
*   prototype : 每次请求都会创建一个新的 bean 实例。
*   request : 每一次 HTTP 请求都会产生一个新的 bean，该 bean 仅在当前 HTTP request 内有效。
*   session : 每一次 HTTP 请求都会产生一个新的 bean，该 bean 仅在当前 HTTP session 内有效。

#### 2.5. `Configuration`

一般用来声明配置类，可以使用 `@Component`注解替代，不过使用`Configuration`注解声明配置类更加语义化。

```java
@Configuration
public class AppConfig {
    @Bean
    public TransferService transferService() {
        return new TransferServiceImpl();
    }
}
```

### 3. 处理常见的 HTTP 请求

**5 种常见的请求类型:**

*   **GET** ：请求从服务器获取特定资源。举个例子：`GET /users`（获取所有学生）
*   **POST** ：在服务器上创建一个新的资源。举个例子：`POST /users`（创建学生）
*   **PUT** ：更新服务器上的资源（客户端提供更新后的整个资源）。举个例子：`PUT /users/12`（更新编号为 12 的学生）
*   **DELETE** ：从服务器删除特定的资源。举个例子：`DELETE /users/12`（删除编号为 12 的学生）
*   **PATCH** ：更新服务器上的资源（客户端提供更改的属性，可以看做作是部分更新），使用的比较少。

#### 3.1. GET 请求

`@GetMapping("users")` 等价于 `@RequestMapping(value="/users",method=RequestMethod.GET)`

```java
@GetMapping("/users")
public ResponseEntity<List<User>> getAllUsers() {
	return userRepository.findAll();
}
```

#### 3.2. POST 请求

`@PostMapping("users")` 等价于`@RequestMapping(value="/users",method=RequestMethod.POST)`

关于`@RequestBody`注解的使用，在下面的“前后端传值”这块会讲到。

```java
@PostMapping("/users")
public ResponseEntity<User> createUser(@Valid @RequestBody UserCreateRequest userCreateRequest) {
    return userRespository.save(user);
}
```

#### 3.3. PUT 请求

`@PutMapping("/users/{userId}")` 等价于`@RequestMapping(value="/users/{userId}",method=RequestMethod.PUT)`

```java
@PutMapping("/users/{userId}")
public ResponseEntity<User> updateUser(@PathVariable(value = "userId") Long userId, @Valid @RequestBody UserUpdateRequest userUpdateRequest) {
	//......
}
```

#### 3.4. **DELETE 请求**

`@DeleteMapping("/users/{userId}")`等价于`@RequestMapping(value="/users/{userId}",method=RequestMethod.DELETE)`

```java
@DeleteMapping("/users/{userId}")
public ResponseEntity deleteUser(@PathVariable(value = "userId") Long userId){
	//......
}
```

#### 3.5. **PATCH 请求**

一般实际项目中，我们都是 PUT 不够用了之后才用 PATCH 请求去更新数据。

```java
@PatchMapping("/profile")
public ResponseEntity updateStudent(@RequestBody StudentUpdateRequest studentUpdateRequest) {
	studentRepository.updateDetail(studentUpdateRequest);
	return ResponseEntity.ok().build();
}
```

### 4. 前后端传值

掌握前后端传值的正确姿势，是开始 CRUD 的第一步！

#### 4.1. `@PathVariable` 和 `@RequestParam`

`@PathVariable`用于获取路径中的参数，`@RequestParam`用于获取查询中的参数（`?`后面的参数）。

举个简单的例子：

```java
@GetMapping("/klasses/{klassId}/teachers")
public List<Teacher> getKlassRelatedTeachers(
         @PathVariable("klassId") Long klassId,
         @RequestParam(value = "type", required = false) String type ) {
	//...
}
```

如果我们请求的 url 是：`/klasses/{123456}/teachers?type=web`

那么我们服务获取到的数据就是：`klassId=123456,type=web`。

#### 4.2. `@RequestBody`

用于读取 Request 请求（可能是 POST,PUT,DELETE,GET 请求）的 body 部分并且**Content-Type 为 application/json** 格式的数据，接收到数据之后会自动将数据绑定到 Java 对象上去。系统会使用`HttpMessageConverter`或者自定义的`HttpMessageConverter`将请求的 body 中的 json 字符串转换为 java对象。

例如：我们有一个注册的接口：

```java
@PostMapping("/sign-up")
public ResponseEntity signUp(@RequestBody @Valid UserRegisterRequest userRegisterRequest) {
	userService.save(userRegisterRequest);
	return ResponseEntity.ok().build();
}
```

`UserRegisterRequest`对象：

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class UserRegisterRequest {
    @NotBlank
    private String userName;
    @NotBlank
    private String password;
    @FullName
    @NotBlank
    private String fullName;
}
```

我们发送 post 请求到这个接口，并且 body 携带 JSON 数据：

```json
{
    "userName":"coder",
    "fullName":"shuangkou",
    "password":"123456"
}
```

这样我们的后端就可以直接把 json 格式的数据映射到我们的 `UserRegisterRequest` 类上。

>   需要注意的是：**一个请求方法只可以有一个`@RequestBody`，但是可以有多个`@RequestParam`和`@PathVariable`**。如果你的方法必须要用两个 `@RequestBody`来接受数据的话，大概率是你的数据库设计或者系统设计出问题了

### 5. 读取配置信息

很多时候我们需要将一些常用的配置信息比如阿里云 oss、发送短信、微信认证的相关配置信息等等放到配置文件中，下面我们来看一下 Spring 为我们提供了哪些方式帮助我们从配置文件中读取这些配置信息。

数据源`application.yml`内容如下：：

```yml
wuhan2020: 2020年初武汉爆发了新型冠状病毒，疫情严重，但是，我相信一切都会过去！武汉加油！中国加油！

my-profile:
  name: Guide哥
  email: koushuangbwcx@163.com

library:
  location: 湖北武汉加油中国加油
  books:
    - name: 天才基本法
      description: 二十二岁的林朝夕在父亲确诊阿尔茨海默病这天，得知自己暗恋多年的校园男神裴之即将出国深造的消息——对方考取的学校，恰是父亲当年为她放弃的那所。
    - name: 时间的秩序
      description: 为什么我们记得过去，而非未来？时间“流逝”意味着什么？是我们存在于时间之内，还是时间存在于我们之中？卡洛·罗韦利用诗意的文字，邀请我们思考这一亘古难题——时间的本质。
    - name: 了不起的我
      description: 如何养成一个新习惯？如何让心智变得更成熟？如何拥有高质量的关系？ 如何走出人生的艰难时刻？
```

#### 5.1. `@value`(常用)

使用 `@Value("${property}")` 读取比较简单的配置信息：

```java
@Value("${wuhan2020}")
String wuhan2020;
```

>   需要注意的是`@value`这种方式是不被推荐的，Spring 比较建议的是下面几种读取配置信息的方式。

#### 5.2. `@ConfigurationProperties`(常用)

通过`@ConfigurationProperties`读取配置信息并与 bean 绑定。

```java
@Component
@ConfigurationProperties(prefix = "library")
@Setter
@Getter
@ToString
class LibraryProperties {
    @NotEmpty
    private String location;
    private List<Book> books;

    @Setter
    @Getter
    @ToString
    static class Book {
        String name;
        String description;
    }
  	//省略getter/setter
  	//......
}
```

>   `LibraryProperties`类上加了`@Component`注解，我们可以像使用普通 bean 一样将其注入到类中使用。

#### 5.3. `PropertySource`（不常用）

`@PropertySource`读取指定 properties 文件

```java
@Component
@PropertySource("classpath:website.properties")
@Getter
@Setter
class WebSite {
    @Value("${url}")
    private String url;

  //省略getter/setter
  //......
}
```

使用：

```java
@Autowired
private WebSite webSite;
System.out.println(webSite.getUrl());//https://javaguide.cn/
```

### 6. JSR参数校验

数据的校验的重要性就不用说了，即使在前端对数据进行校验的情况下，我们还是要对传入后端的数据再进行一遍校验，避免用户绕过浏览器直接通过一些 HTTP 工具直接向后端请求一些违法数据。

**JSR(Java Specification Requests）** 是一套 JavaBean 参数校验的标准，它定义了很多常用的校验注解，我们可以直接将这些注解加在我们 JavaBean 的属性上面，这样就可以在需要校验的时候进行校验了。校验的时候我们实际用的是 **Hibernate Validator** 框架。SpringBoot 项目的 spring-boot-starter-web 依赖中已经有 hibernate-validator 包，不需要引用相关依赖。

>   需要注意的是：**所有的注解，推荐使用 JSR 注解，即`javax.validation.constraints`，而不是`org.hibernate.validator.constraints`**

#### 6.1. 一些常用的字段验证的注解

*   `@NotEmpty` 被注释的字符串的不能为 null 也不能为空
*   `@NotBlank` 被注释的字符串非 null，并且必须包含一个非空白字符
*   `@Null` 被注释的元素必须为 null
*   `@NotNull` 被注释的元素必须不为 null
*   `@AssertTrue` 被注释的元素必须为 true
*   `@AssertFalse` 被注释的元素必须为 false
*   `@Pattern(regex=,flag=)`被注释的元素必须符合指定的正则表达式
*   `@Email` 被注释的元素必须是 Email 格式。
*   `@Min(value)`被注释的元素必须是一个数字，其值必须大于等于指定的最小值
*   `@Max(value)`被注释的元素必须是一个数字，其值必须小于等于指定的最大值
*   `@DecimalMin(value)`被注释的元素必须是一个数字，其值必须大于等于指定的最小值
*   `@DecimalMax(value)` 被注释的元素必须是一个数字，其值必须小于等于指定的最大值
*   `@Size(max=, min=)`被注释的元素的大小必须在指定的范围内
*   `@Digits (integer, fraction)`被注释的元素必须是一个数字，其值必须在可接受的范围内
*   `@Past`被注释的元素必须是一个过去的日期
*   `@Future` 被注释的元素必须是一个将来的日期

#### 6.2. 验证请求体(RequestBody)

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Person {

    @NotNull(message = "classId 不能为空")
    private String classId;

    @Size(max = 33)
    @NotNull(message = "name 不能为空")
    private String name;

    @Pattern(regexp = "((^Man$|^Woman$|^UGM$))", message = "sex 值不在可选范围")
    @NotNull(message = "sex 不能为空")
    private String sex;

    @Email(message = "email 格式不正确")
    @NotNull(message = "email 不能为空")
    private String email;

}
```

我们在需要验证的参数上加上了`@Valid`注解，如果验证失败，它将抛出`MethodArgumentNotValidException`。默认情况下，Spring会将此异常转换为HTTP Status 400（错误请求）。

```java
@RestController
@RequestMapping("/api")
public class PersonController {

    @PostMapping("/person")
    public ResponseEntity<Person> getPerson(@RequestBody @Valid Person person) {
        return ResponseEntity.ok().body(person);
    }
}
```

#### 6.3. 验证请求参数(Path Variables 和 Request Parameters)

一定不要忘记在类上加上 `Validated` 注解了，这个参数可以告诉 Spring 去校验方法参数。

```java
@RestController
@RequestMapping("/api")
@Validated
public class PersonController {

    @GetMapping("/person/{id}")
    public ResponseEntity<Integer> getPersonByID(@Valid @PathVariable("id") @Max(value = 5,message = "超过 id 的范围了") Integer id) {
        return ResponseEntity.ok().body(id);
    }
}
```

### 7. 全局处理 Controller 层异常

**相关注解：**

1.  `@ControllerAdvice` :注解定义全局异常处理类
2.  `@ExceptionHandler` :注解声明异常处理方法

### 8. JPA 相关

#### 8.1. 创建表

`@Entity`声明一个类对应一个数据库实体。

`@Table` 设置表名

```java
@Entity
@Table(name = "role")
public class Role {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String description;
    //省略getter/setter......
}
```

#### 8.2. 创建主键

`@Id` ：声明一个字段为主键。

使用`@Id`声明之后，我们还需要定义主键的生成策略。我们可以使用 `@GeneratedValue` 指定主键生成策略。

**1.通过 `@GeneratedValue`直接使用 JPA 内置提供的四种主键生成策略来指定主键生成策略。**

```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
```

JPA 使用枚举定义了 4 中常见的主键生成策略，`@GeneratedValue`注解默认使用的策略是`GenerationType.AUTO`。

```java
public enum GenerationType {

    /**
     * 使用一个特定的数据库表格来保存主键
     * 持久化引擎通过关系数据库的一张特定的表格来生成主键,
     */
    TABLE,

    /**
     *在某些数据库中,不支持主键自增长,比如Oracle、PostgreSQL其提供了一种叫做"序列(sequence)"的机制生成主键
     */
    SEQUENCE,

    /**
     * 主键自增长
     */
    IDENTITY,

    /**
     *把主键生成策略交给持久化引擎(persistence engine),
     *持久化引擎会根据数据库在以上三种主键生成 策略中选择其中一种
     */
    AUTO
}
```

>   一般使用 MySQL 数据库的话，使用`GenerationType.IDENTITY`策略比较普遍一点（分布式系统的话需要另外考虑使用分布式 ID）。

**2.我们可以通过 `@GenericGenerator`自己声明一个主键策略，然后 `@GeneratedValue`使用这个策略**

```java
@Id
@GeneratedValue(generator = "IdentityIdGenerator")
@GenericGenerator(name = "IdentityIdGenerator", strategy = "identity")
private Long id;
```

等价于：

```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
```

jpa 提供的主键生成策略有如下几种：

```java
public class DefaultIdentifierGeneratorFactory
implements MutableIdentifierGeneratorFactory, Serializable, ServiceRegistryAwareService {

    @SuppressWarnings("deprecation")
    public DefaultIdentifierGeneratorFactory() {
        register( "uuid2", UUIDGenerator.class );
        register( "guid", GUIDGenerator.class );   // can be done with UUIDGenerator + strategy
        register( "uuid", UUIDHexGenerator.class );   // "deprecated" for new use
        register( "uuid.hex", UUIDHexGenerator.class );  // uuid.hex is deprecated
        register( "assigned", Assigned.class );
        register( "identity", IdentityGenerator.class );
        register( "select", SelectGenerator.class );
        register( "sequence", SequenceStyleGenerator.class );
        register( "seqhilo", SequenceHiLoGenerator.class );
        register( "increment", IncrementGenerator.class );
        register( "foreign", ForeignGenerator.class );
        register( "sequence-identity", SequenceIdentityGenerator.class );
        register( "enhanced-sequence", SequenceStyleGenerator.class );
        register( "enhanced-table", TableGenerator.class );
    }

    public void register(String strategy, Class generatorClass) {
        LOG.debugf( "Registering IdentifierGenerator strategy [%s] -> [%s]", strategy, generatorClass.getName() );
        final Class previous = generatorStrategyToClassNameMap.put( strategy, generatorClass );
        if ( previous != null ) {
        	LOG.debugf( "    - overriding [%s]", previous.getName() );
        }
    }
}
```

#### 8.3. 设置字段类型

`@Column` 声明字段。

示例：

设置属性 userName 对应的数据库字段名为 user_name，长度为 32，非空

```java
@Column(name = "user_name", nullable = false, length=32)
private String userName;
```

设置字段类型并且加默认值，这个还是挺常用的。

```java
@Column(columnDefinition = "tinyint(1) default 1")
private Boolean enabled;
```

#### 8.4. 指定不持久化特定字段

`@Transient` ：声明不需要与数据库映射的字段，在保存的时候不需要保存进数据库 。

如果我们想让`secrect` 这个字段不被持久化，可以使用 `@Transient`关键字声明。

```java
Entity(name="USER")
public class User {
    ......
    @Transient
    private String secrect; // not persistent because of @Transient
}
```

除了 `@Transient`关键字声明， 还可以采用下面几种方法：

```java
static String secrect; // not persistent because of static
final String secrect = “Satish”; // not persistent because of final
transient String secrect; // not persistent because of transient
```

一般使用注解的方式比较多。

#### 8.5. 声明大字段

`@Lob`:声明某个字段为大字段。

```java
@Lob
private String content;
```

更详细的声明：

```java
@Lob
//指定 Lob 类型数据的获取策略， FetchType.EAGER 表示非延迟 加载，而 FetchType. LAZY 表示延迟加载 ；
@Basic(fetch = FetchType.EAGER)
//columnDefinition 属性指定数据表对应的 Lob 字段类型
@Column(name = "content", columnDefinition = "LONGTEXT NOT NULL")
private String content;
```

#### 8.6. 创建枚举类型的字段

可以使用枚举类型的字段，不过枚举字段要用`@Enumerated`注解修饰。

```java
public enum Gender {
    MALE("男性"),
    FEMALE("女性");

    private String value;
    Gender(String str){
        value=str;
    }
}
```

```java
@Entity
@Table(name = "role")
public class Role {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String description;
    @Enumerated(EnumType.STRING)
    private Gender gender;
    省略getter/setter......
}
```

数据库里面对应存储的是 MAIL/FEMAIL。

### 9. 事务 `@Transactional`

在要开启事务的方法上使用`@Transactional`注解即可!

```java
@Transactional(rollbackFor = Exception.class)
public void save() {
  //......
}
```

我们知道 Exception 分为运行时异常和非运行时异常。在`@Transactional`注解中如果不配置`rollbackFor`属性,那么事物只会在遇到`RuntimeException`的时候才会回滚,加上`rollbackFor=Exception.class`,可以让事物在遇到非运行时异常时也回滚。

`@Transactional` 注解一般用在可以作用在类或者方法上。

*   **作用于类**：当把`@Transactional` 注解放在类上时，表示所有该类的`public` 方法都配置相同的事务属性信息。
*   **作用于方法**：当类配置了`@Transactional`，方法也配置了`@Transactional`，方法的事务会覆盖类的事务配置信息。

### 10. json 数据处理

#### 10.1. 过滤 json 数据

`@JsonIgnoreProperties` 作用在类上用于过滤掉特定字段不返回或者不解析。

```java
//生成json时将userRoles属性过滤
@JsonIgnoreProperties({"userRoles"})
public class User {

    private String userName;
    private String fullName;
    private String password;
    @JsonIgnore
    private List<UserRole> userRoles = new ArrayList<>();
}
```

`@JsonIgnore`一般用于类的属性上，作用和上面的`@JsonIgnoreProperties` 一样。

```java
public class User {

    private String userName;
    private String fullName;
    private String password;
   //生成json时将userRoles属性过滤
    @JsonIgnore
    private List<UserRole> userRoles = new ArrayList<>();
}
```

#### 10.2. 格式化 json 数据

`@JsonFormat`一般用来格式化 json 数据。：

比如：

```java
@JsonFormat(shape=JsonFormat.Shape.STRING, pattern="yyyy-MM-dd'T'HH:mm:ss.SSS'Z'", timezone="GMT")
private Date date;
```

