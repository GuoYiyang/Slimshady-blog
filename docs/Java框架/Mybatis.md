# Mybatis

## 简介

Mybatis 最初配置信息是基于 XML ,映射语句(SQL)也是定义在 XML 中的。而到了 MyBatis 3提供了新的基于注解的配置。mybatis提供的注解有很多，大致可以分为以下几类： 

*   **增删改查：**@Insert、@Update、@Delete、@Select、@MapKey、@Options、@SelelctKey、@Param、@InsertProvider、@UpdateProvider、@DeleteProvider、@SelectProvider 
*   **结果集映射：**@Results、@Result、@ResultMap、@ResultType、@ConstructorArgs、@Arg、@One、@Many、@TypeDiscriminator、@Case 

*   **缓存：**@CacheNamespace、@Property、@CacheNamespaceRef、@Flush 

绝大部分注解，在xml映射文件中都有元素与之对应，但不是所有。此外在mybatis-spring中提供了@Mapper注解和@MapperScan注解，用于和spring进行整合。 

## 增删改查相关注解

| **注解**                                                     | **使用对象** | **相对应的 XML**                        | **描述**                                                     |
| ------------------------------------------------------------ | ------------ | --------------------------------------- | ------------------------------------------------------------ |
| @Insert @Update @Delete @Select                              | 方法         | \<insert> \<update> \<delete> \<select> | 这四个注解分别代表将会被执行的 SQL 语句。它们用字符串数组（或单个字符串）作为参数。如果传递的是字符串数组，字符串之间先会被填充一个空格再连接成单个完整的字符串。这有效避免了以 Java 代码构建 SQL 语句时的“丢失空格”的问题。然而，你也可以提前手动连接好字符串。属性有：value，填入的值是用来组成单个 SQL 语句的字符串数组。 |
| @Options                                                     | 方法         | 映射语句的属性                          | 这个注解提供访问大范围的交换和配置选项的入口，它们通常在映射语句上作为属性出现。Options 注解提供了通俗易懂的方式来访问它们，而不是让每条语句注解变复杂。属性有：useCache=true, flushCache=FlushCachePolicy.DEFAULT, resultSetType=FORWARD_ONLY, statementType=PREPARED, fetchSize=-1, timeout=-1, useGeneratedKeys=false, keyProperty="id", keyColumn="", resultSets=""。值得一提的是， Java 注解无法指定 null 值。因此，一旦你使用了 Options 注解，你的语句就会被上述属性的默认值所影响。要注意避免默认值带来的预期以外的行为。     注意： keyColumn 属性只在某些数据库中有效（如 Oracle、PostgreSQL等）。请在插入语句一节查看更多关于 keyColumn 和 keyProperty 两者的有效值详情。 |
| @MapKey                                                      | 方法         |                                         | 这是一个用在返回值为 Map 的方法上的注解。它能够将存放对象的 List 转化为 key 值为对象的某一属性的 Map。属性有： value，填入的是对象的属性名，作为 Map 的 key 值。 |
| @SelectKey                                                   | 方法         | \<selectKey>                            | 这个注解的功能与 <selectKey> 标签完全一致，用在已经被 @Insert 或 @InsertProvider 或 @Update 或 @UpdateProvider 注解了的方法上。若在未被上述四个注解的方法上作 @SelectKey 注解则视为无效。如果你指定了 @SelectKey 注解，那么 MyBatis 就会忽略掉由 @Options 注解所设置的生成主键或设置（configuration）属性。属性有：statement 填入将会被执行的 SQL 字符串数组，keyProperty 填入将会被更新的参数对象的属性的值，before 填入 true 或 false 以指明 SQL 语句应被在插入语句的之前还是之后执行。resultType 填入 keyProperty 的 Java 类型和用 Statement、 PreparedStatement 和 CallableStatement 中的 STATEMENT、 PREPARED 或 CALLABLE 中任一值填入 statementType。默认值是 PREPARED。 |
| @Param                                                       | 参数         | N/A                                     | 如果你的映射方法的形参有多个，这个注解使用在映射方法的参数上就能为它们取自定义名字。若不给出自定义名字，多参数（不包括 RowBounds 参数）则先以 "param" 作前缀，再加上它们的参数位置作为参数别名。例如 #{param1}, #{param2}，这个是默认值。如果注解是 @Param("person")，那么参数就会被命名为 #{person}。 |
| @InsertProvider @UpdateProvider @DeleteProvider @SelectProvider | 方法         | \<insert> \<update> \<delete> \<select> | 允许构建动态 SQL。这些备选的 SQL 注解允许你指定类名和返回在运行时执行的 SQL 语句的方法。（自从MyBatis 3.4.6开始，你可以用 CharSequence 代替 String 来返回类型返回值了。）当执行映射语句的时候，MyBatis 会实例化类并执行方法，类和方法就是填入了注解的值。你可以把已经传递给映射方法了的对象作为参数，"Mapper interface type" 和 "Mapper method" 会经过 ProviderContext （仅在MyBatis 3.4.5及以上支持）作为参数值。（MyBatis 3.4及以上的版本，支持多参数传入）属性有： type, method。type 属性需填入类。method 需填入该类定义了的方法名。注意 接下来的小节将会讨论类，能帮助你更轻松地构建动态 SQL。 |

#### 2.1 @Insert、@Update、@Delete、@Options、@SelectKey

mybatis会根据接口方法上的@Insert、@Update、@Delete注解，分别去调用SqlSession的insert、update、delete方法。这个几个方法返回的都是一个int，表示影响的记录行数。 

特别的，在UserMapper接口的insert方法上，除了添加了@Insert注解，还添加了@Options注解。在上面的案例中，@Options注解用于获取自动生成主键，并设置到User实体中。此外，@SelectKey注解也可以用于获取自动生成的主键，使用方式如下： 

```java
@Insert("INSERT INTO user(id,name) VALUES (#{id},#{name})")
@SelectKey(statement = "SELECT LAST_INSERT_ID()", keyProperty = "id", before = false, resultType = Integer.class) 
public int insert(User user); 
```

#### 2.2 @Select

```java
@Select("SELECT id,name FROM user WHERE id= #{id}")
public User selectById(int id);

@Select("SELECT id,name FROM user")
public List<User> selectAll();

@Select("SELECT id,name FROM user")
@MapKey("id")
public Map<Integer,User> selectMap();

@Select("SELECT id,name FROM user LIMIT #{offset},#{limit}")
public List<User> selectPage(@Param("offset") int offset, @Param("limit") int limit);
```

在上面的案例中，UserMapper的selectById、selectAll、selectMap、selectPage方法都添加了@Select注解。mybatis会根据方法的返回值类型User、List<User>、Map<Integer,User>判断是调用SqlSession的selectOne、selectList还是selectMap方法。 

#### 2.3 @MapKey

特别的，对于返回值是Map的情况，UserMapper的selectMap方法上额外添加了一个@MapKey("id")注解，表示将User实例的id属性当做Map的key。

#### 2.4  动态sql与标签

```java
@Select({ "<script>",
         "SELECT id,name " + "FROM user " + "WHERE id IN "
             + "<foreach 
                 item='id' 
                 index='index' 
                 collection=‘array' 
                 open='(' separator=',' close=')'> " 
             + "#{id}"
             + "</foreach> ",
         "</script>" })
public List<User> selectByIds(int... ids);
```

在UserMapper的selectByIds方法中，可以看到@Select注解里填写的SQL前后分别添加了\<script>、\</script>，这是因为SQL中使用了动态sql标签\<foreach>。不管是@Insert、@Update、@Delete、@Select注解，只要SQL里使用了mybatis的动态sql标签(包括：if、choose …when ...otherwise、trim 、where、 set、foreach、bind)等，都建议在sql前后分别加上\<script>、\</script>，否则可能会出现一些参数找不到的情况。 

#### 2.5 @Param

@Param注解用于给方法参数起一个名字。以下是笔者总结的使用原则：

*   在方法只接受一个参数的情况下，可以不使用@Param。
*   在方法接受多个参数的情况下，建议一定要使用@Param注解给参数命名。

```java
@Select("SELECT id,name FROM user LIMIT #{offset},#{limit}")
public List<User> selectPage(@Param("offset") int offset, @Param("limit") int limit);
```

例如上述案例的selectPage方法接受2个参数，所以其两个参数都使用了@Param注解。

#### 2.6  @InsertProvider、@UpdateProvider、@DeleteProvider、@SelectProvider

这几个注解主要用于动态sql构建。

## 结果集映射相关注解

| **注解**           | **使用对象** | **相对应的 XML** | **描述**                                                     |
| ------------------ | ------------ | ---------------- | ------------------------------------------------------------ |
| @Results           | 方法         | \<resultMap>     | 结果映射的列表，包含了一个特别结果列如何被映射到属性或字段的详情。属性有：value, id。value 属性是 Result 注解的数组。这个 id 的属性是结果映射的名称。 |
| @Result            | 方法         | \<result> \<id>  | 在列和属性或字段之间的单独结果映射。属性有：id, column, javaType, jdbcType, typeHandler, one, many。id 属性是一个布尔值，来标识应该被用于比较（和在 XML 映射中的<id>相似）的属性。one 属性是单独的联系，和 <association> 相似，而 many 属性是对集合而言的，和<collection>相似。它们这样命名是为了避免名称冲突。 |
| @ResultMap         | 方法         | N/A              | 这个注解给 @Select 或者 @SelectProvider 提供在 XML 映射中的 <resultMap> 的id。这使得注解的 select 可以复用那些定义在 XML 中的 ResultMap。如果同一 select 注解中还存在 @Results 或者 @ConstructorArgs，那么这两个注解将被此注解覆盖。 |
| @ResultType        | 方法         | N/A              | 此注解在使用了结果处理器的情况下使用。在这种情况下，返回类型为 void，所以 Mybatis 必须有一种方式决定对象的类型，用于构造每行数据。如果有 XML 的结果映射，请使用 @ResultMap 注解。如果结果类型在 XML 的 <select> 节点中指定了，就不需要其他的注解了。其他情况下则使用此注解。比如，如果 @Select 注解在一个将使用结果处理器的方法上，那么返回类型必须是 void 并且这个注解（或者@ResultMap）必选。这个注解仅在方法返回类型是 void 的情况下生效。 |
| @ConstructorArgs   | 方法         | \<constructor>   | 收集一组结果传递给一个结果对象的构造方法。属性有：value，它是形式参数数组。 |
| @Arg               | N/A          | \<arg> \<idArg>  | 单参数构造方法，是 ConstructorArgs 集合的一部分。属性有：id, column, javaType, jdbcType, typeHandler, select和 resultMap。id 属性是布尔值，来标识用于比较的属性，和<idArg> XML 元素相似。 |
| @One               | N/A          | \<association>   | 复杂类型的单独属性值映射。属性有：select，已映射语句（也就是映射器方法）的全限定名，它可以加载合适类型的实例。fetchType会覆盖全局的配置参数 lazyLoadingEnabled。**注意** 联合映射在注解 API中是不支持的。这是因为 Java 注解的限制,不允许循环引用。 |
| @Many              | N/A          | \<collection>    | 映射到复杂类型的集合属性。属性有：select，已映射语句（也就是映射器方法）的全限定名，它可以加载合适类型的实例的集合，fetchType 会覆盖全局的配置参数 lazyLoadingEnabled。**注意** 联合映射在注解 API中是不支持的。这是因为 Java 注解的限制，不允许循环引用 |
| @TypeDiscriminator | 方法         | \<discriminator> | 一组实例值被用来决定结果映射的表现。属性有：column, javaType, jdbcType, typeHandler 和 cases。cases 属性是实例数组。 |
| @Case              | N/A          | \<case>          | 单独实例的值和它对应的映射。属性有：value, type, results。results 属性是结果数组，因此这个注解和实际的 ResultMap 很相似，由下面的 Results 注解指定。 |

 

