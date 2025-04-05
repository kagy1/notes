# JDBC

Java DataBase Connectivity（Java语言连接数据库）







# Mybatis

MyBatis 是一个优秀的持久层框架，它支持定制化 SQL、存储过程以及高级映射。MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。MyBatis 可以使用简单的 XML 或注解来配置和映射原生信息，将接口和 Java 的 POJOs（Plain Old Java Objects）映射成数据库中的记录。



## Mapper代理开发

1. 定义与SQL映射文件同名的Mapper接口，并且将Mapper接口和SQL映射文件放置在同一目录下

   可以通过配置文件修改存放路径

   ```yaml
   # 只会匹配 classpath 下的 mapper 目录中，直接包含的 .xml 文件。
   mybatis:
     mapper-locations: classpath:mapper/*.xml
   
   # 会匹配 classpath 下的 mapper 目录及其所有子目录中，所有的 .xml 文件。
   mybatis:
     mapper-locations: classpath*:mapper/**/*.xml
   ```

2. 设置SQL映射文件的namespace属性为Mapper接口全限定名

   <span style="color:red">Mapper XML文件通过namespace属性绑定到对应的Mapper接口</span>

   只要namespace和接口的全限定名一致，Mybatis就能正确绑定Mapper XML文件和接口

3. 在Mapper接口定义方法，方法名就是SQL映射文件中sql语句的id，并保持参数类型和返回值类型一致



## Mybatis配置

```yaml
# MyBatis 配置
mybatis:
  # 指定 MyBatis 配置文件的位置
  config-location: classpath:mybatis/mybatis-config.xml
  # 指定 SQL 映射文件的位置
  mapper-locations: classpath:mybatis/mapper/*.xml
  # 指定实体类的包名，用于别名解析
  type-aliases-package: com.example.domain
  # 指定执行器类型，SIMPLE 为默认执行器
  executor-type: SIMPLE
  # 配置默认的执行器。SIMPLE 执行器没有什么特别之处。REUSE 执行器重用预处理语句。BATCH 执行器重用语句和批量更新
  default-executor-type: SIMPLE
  # 设置超时时间，它决定数据库驱动等待数据库响应的秒数
  default-statement-timeout: 25000
  # 允许 JDBC 支持自动生成主键，需要数据库驱动支持
  use-generated-keys: true
  # 指定 MyBatis 应如何自动映射列到字段或属性
  # NONE 表示取消自动映射
  # PARTIAL 只会自动映射没有定义嵌套结果映射的字段
  # FULL 会自动映射任何复杂的结果集（无论是否嵌套）
  auto-mapping-behavior: PARTIAL
  # 配置默认的执行器。SIMPLE 就是普通的执行器；REUSE 执行器会重用预处理语句（PreparedStatement）；BATCH 执行器不仅重用语句还会执行批量更新
  default-executor-type: SIMPLE
  # 设置超时时间，它决定数据库驱动等待数据库响应的秒数
  default-statement-timeout: 25000
  # 是否开启驼峰命名自动映射，即从经典数据库列名 A_COLUMN 映射到经典 Java 属性名 aColumn
  map-underscore-to-camel-case: true
  # MyBatis 使用 aggressive 懒加载。当开启时，任何方法的调用都会加载该对象的所有延迟加载属性。否则，每个延迟加载属性会按需加载
  aggressive-lazy-loading: true
  # 允许或不允许多种结果集从一个单独的语句中返回（需要数据库驱动支持）
  multiple-result-sets-enabled: true
  # 使用列标签代替列名。实际表现依赖于数据库驱动，具体可参考数据库驱动的相关文档，或通过对比测试来观察
  use-column-label: true
  # 允许 JDBC 支持自动生成主键，需要数据库驱动支持。如果设置为 true，将强制使用自动生成主键
  use-generated-keys: false
  # 指定 MyBatis 增加到日志名称的前缀
  log-prefix: mybatis.
  # 指定 MyBatis 所用日志的具体实现，未指定时将自动查找
  log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  # 指定 Mapper 接口所对应的 XML 文件位置，如果你在 Mapper 接口中有自定义方法（XML 中有自定义实现），需要进行该配置
  # mapper-locations: classpath:mapper/*.xml
  # 指定项目中枚举类的路径
  type-enums-package: com.example.enums
  # 设置类型别名所在的包
  type-aliases-package: com.example.domain
  configuration:
    # 是否开启自动驼峰命名规则（camel case）映射
    map-underscore-to-camel-case: true
    # 设置但 JDBC 类型为空时，某些驱动程序要指定值，default：OTHER，H2 数据库设置为 NULL
    jdbc-type-for-null: 'NULL'
    # 使全局的映射器启用或禁用缓存
    cache-enabled: false
    # 全局启用或禁用延迟加载。当禁用时，所有关联对象都会即时加载
    lazy-loading-enabled: true
    # 当启用时，有延迟加载属性的对象在被调用时将会完全加载任意属性。否则，每种属性将会按需要加载
    aggressive-lazy-loading: false
    # 是否允许单一语句返回多结果集（需要兼容驱动）
    multiple-result-sets-enabled: true
    # 是否可以使用列的别名 (取决于驱动的兼容性) default: true
    use-column-label: true
    # 允许 JDBC 生成主键。需要驱动器支持。如果设为了 true，这个设置将强制使用被生成的主键，有一些驱动器不兼容不过仍然可以执行。default: false
    use-generated-keys: false
    # 指定 MyBatis 如何自动映射列到字段/属性。PARTIAL 只会自动映射简单，没有嵌套的结果。FULL 会自动映射任意复杂的结果（嵌套的或其他情况）
    auto-mapping-behavior: PARTIAL
    # 这是默认的执行类型（SIMPLE：普通的执行器；REUSE：执行器会重用预处理语句（prepared statements)；BATCH：执行器将重用语句并执行批量更新）
    default-executor-type: SIMPLE
    # 使用驼峰命名法转换字段
    map-underscore-to-camel-case: true
    # 设置本地缓存范围 session: 就会有数据的共享 statement: 语句范围 (这样就不会有数据的共享 ) defalut: session
    local-cache-scope: SESSION
    # 设置 JDBC 类型为空时，某些驱动程序要指定值
    jdbc-type-for-null: 'NULL'
    # 设置触发延迟加载的方法
    lazy-load-trigger-methods: equals,clone,hashCode,toString
    # 设置工厂类，用于定义如何实例化对象，最常见的就是通过反射实例化对象
    object-factory: org.apache.ibatis.reflection.DefaultObjectFactory
```

**type-aliases-package**

配置后，在Mybatis的XMl文件中直接使用类名作为类型别名，不需要使用全限定名

```yaml
mybatis:
	type-aliases-package: com.example.domain.model
```

```xml
<!-- <select id="getUser" resultType="com.example.domain.model.User"> -->
<select id="getUser" resultType="User">
  SELECT * FROM users WHERE id = #{id}
</select>
```



## 插件

MybatisX：XML和接口方法相互跳转，根据接口方法生成statement



## Mapper接口

`@Param`

为传递给 SQL 映射文件（Mapper XML）的参数指定名称

**多参数支持**

- 如果方法中有多个参数且未使用 `@Param`，MyBatis 会按位置顺序生成参数名，如 `param1`、`param2` 等。
- 使用 `@Param` 后，你可以自定义参数名称，避免因为参数位置变化导致的错误。
- 如果方法只有一个参数，通常不需要 `@Param`，但使用它可以让代码更清晰。

```java
// Mapper 接口
List<Map<String, Object>> selectUsers(@Param("name") String name, @Param("age") Integer age);
```

```xml
<select id="selectUsers" resultType="map">
    SELECT * FROM users WHERE name = #{name} AND age = #{age}
</select>
```

`#{name}` 和 `#{age}` 是通过 `@Param` 中的名称指定的。

如果不使用 `@Param`，需要通过默认生成的 `param1` 和 `param2` 来引用参数。

用于集合类型（`List`、`Map` 等）

Map参数如果不使用`@Param`，MyBatis 会直接将 `Map` 中的键作为参数名。

```xml
<select id="selectUsersByConditions" parameterType="map" resultType="map">
    select *
    from users
    <where>
        <if test="name != null and name != ''">
            name = #{name}
        </if>
        <if test="age != null">
            and age = #{age}
        </if>
    </where>
</select>
```

使用`@Param`既可以通过 `#{key}` 引用 `Map` 的键值。也可以通过 `#{conditions.key}` 引用键值（这是因为 `@Param` 创建了一个命名空间）。

```xml
<select id="selectUsersByConditions" parameterType="map" resultType="map">
    select *
    from users
    <where>
        <if test="name != null and name != ''">
            name = #{name}
        </if>
        <if test="conditions.age != null">
            and age = #{conditions.age}
        </if>
    </where>
</select>
```









## 条件查询

1. 散装参数：需要使用`@Param("SQL中的参数占位符名称")`
2. 实体类封装参数：只需要保证SQL中的参数名和实体类属性名对应上
3. map集合：保证SQL中的参数名和map集合的键名称对应上



## XML 文件语法

1. 查询语句末尾不能带分号，否则会报错



### ＜![CDATA[]]＞

CDATA是"Character Data"（字符数据）的缩写，是一种在XML文档中包含纯文本数据的方法。它的语法格式如下

```xml
<![CDATA[ Your Text Here ]]>
```

**CDATA 段中的内容不需要被解析**（即内容会被当作纯文本处理，而不是标签或代码）。

1. 避免转义 `<`、`>`、`&` 等特殊字符

```xml
<select id="findUsersByAge" parameterType="int" resultType="User">
    <![CDATA[
        SELECT * FROM users WHERE age > #{age}
    ]]>
</select>

<select id="findUsersByAge" parameterType="int" resultType="User">
    SELECT * FROM users WHERE age <![CDATA[>]]> #{age}
</select>

<!-- 等价于 -->

<select id="findUsersByAge" parameterType="int" resultType="User">
    SELECT * FROM users WHERE age &gt; #{age}
</select>
```

2. 动态 SQL 中包含复杂逻辑

```xml
<select id="findUsers" parameterType="map" resultType="User">
    SELECT * FROM users
    WHERE 1 = 1
    <if test="name != null">
        AND name LIKE <![CDATA[ CONCAT('%', #{name}, '%') ]]>
    </if>
    <if test="age != null">
        AND age <![CDATA[ >= ]]> #{age}
    </if>
</select>
```



### like拼接

```xml
<if test="name != null">
    AND name LIKE CONCAT('%', #{name}, '%')
</if>

<!-- oracle -->
<if test="name != null">
    AND name LIKE '%' || #{name} || '%'
</if>
```





### 占位符

**#{}**

\#{}是MyBatis最常用的占位符,用于SQL语句的参数设置

MyBatis会将#{}占位符替换为?,然后使用PreparedStatement的set方法来赋值

使用#{}可以有效防止SQL注入,提高系统安全性

**${}**

${}主要用于动态生成SQL语句,可以在SQL语句中插入一个不做任何修改的字符串

使用${}时,MyBatis不会修改或转义字符串,而是直接插入SQL语句中

有SQL注入的风险,一般用于ORDER BY、表名等不能使用参数化查询的场合

如果要对表名、列名 进行动态设置，只能使用${}拼接

```xml
<select id="getUserList" resultType="User">
  SELECT * FROM user ORDER BY ${sortColumn}
</select>
```



### 属性

**parameterType**

`parameterType` 通常是不需要写的

用于指定传递给 SQL 语句的参数的类型

```java
List<User> selectUsersByIds(Integer[] ids);
```

```xml
<select id="selectUsersByIds" parameterType="array" resultType="com.example.User">
    SELECT * FROM user WHERE id IN
    <foreach collection="array" item="id" separator="," open="(" close=")">
        #{id}
    </foreach>
</select>
```







**resultType**

`resultType` 用于指定查询结果直接映射到某个 Java 类型（类或数据结构）。

```xml
<select id="selectByID" resultType="map">
    select * from user where id = #{id}
</select>
```

**resultMap**

`resultMap` 是 MyBatis 的强大功能之一，用于定义复杂的结果映射规则，适合对表的字段和 Java 对象的属性之间的映射进行细粒度控制。

- **常见场景**：当数据库字段和 Java 对象字段名不一致，或者需要处理嵌套对象时使用。
- **定义方式**：通过 `<resultMap>` 标签定义映射规则，然后在查询中引用它。

定义resultMap

```xml
<resultMap id="UserResultMap" type="com.example.User">
    <id column="user_id" property="id"/>  <!-- 主键字段 column：列名 property：属性名 -->
    <result column="user_name" property="name"/>
    <result column="email" property="email"/>
</resultMap>
```

- **`<id>`** 是用来标记主键字段的映射。**`column="user_id"`**: 数据库中表的列名为 `user_id`。**`property="id"`**: Java 类中属性的名称是 `id`。MyBatis 会将查询结果中 `user_id` 列的值映射到 `User` 类的 `id` 属性上。
- **`<result>`** 用于映射非主键字段。数据库中 `user_name` 列对应 Java 类中的 `name` 属性。

使用resultMap

```xml
<select id="selectByID" resultMap="UserResultMap">
    select user_id, user_name, email from user where id = #{id}
</select>
```



### 标签

`<select>`

`<insert>`

`<update>`

`<delete>`

`<sql>`：用于定义可复用的sql片段

- 通过`<include>`标签进行引用

- 避免重复定义常用的SQL语句

  ```xml
  <sql id="userColumns">
      id, name, email, age
  </sql>
  
  <select id="selectAllUsers" resultType="com.example.User">
      SELECT <include refid="userColumns"/> FROM user
  </select>
  ```

`<include>`：用于引入 `<sql>` 标签定义的公共 SQL 片段。

#### 动态SQL标签

**`<if>`**

`test` 属性用于定义条件，条件为 `true` 时，MyBatis 会将 `<if>` 标签内的内容拼接到最终的 SQL 中；否则会忽略。

`test` 属性的值是一个 OGNL 表达式（Object-Graph Navigation Language），可以用来对参数进行判断。

```xml
<select id="selectAll6" parameterType="java.util.List" resultType="map">
    select *
    from his.pat_inpat_order_cost c
    where
    <if test="pat_id != null and pat_id != ''">
        c.pat_id = #{pat_id}
    </if>
    <if test="zylsh != null and zylsh != ''">
        and c.zylsh = #{zylsh}
    </if>
    <if test="ZYFYMX_ID != null and ZYFYMX_ID != ''">
        and c.ZYFYMX_ID = #{ZYFYMX_ID}
    </if>
</select>
```

如果第一个pat_id为空后面直接跟了and 会报错

可以使用`<where>`标签，或者使用 where 1= 1 

MyBatis 的 `<where>` 标签可以自动处理 `AND` 的问题。如果 `<where>` 内部的第一个条件被渲染，它会自动去掉多余的 `AND`。

```xml
<select id="selectAll6" parameterType="java.util.List" resultType="map">
    select *
    from his.pat_inpat_order_cost c
    <where>
        <if test="pat_id != null and pat_id != ''">
            c.pat_id = #{pat_id}
        </if>
        <if test="zylsh != null and zylsh != ''">
            and c.zylsh = #{zylsh}
        </if>
        <if test="ZYFYMX_ID != null and ZYFYMX_ID != ''">
            and c.ZYFYMX_ID = #{ZYFYMX_ID}
        </if>
    </where>
</select>
```



`<choose>`、`<when> `和 `<otherwise>`：用于实现类似`if-else`的条件逻辑

- `<choose>`：类似 `switch`。

- `<when>`：条件分支。

- `<otherwise>`：默认分支。

  ```xml
  <select id="selectUser" parameterType="map" resultType="com.example.User">
      SELECT * FROM user
      WHERE 
      <choose>
          <when test="id != null">
              id = #{id}
          </when>
          <when test="name != null">
              name = #{name}
          </when>
          <otherwise>
              1 = 1
          </otherwise>
      </choose>
  </select>
  ```



`<foreach>`：用于动态生成 SQL 的部分，特别适合处理集合（如数组、列表等）类型的参数

  **常见属性：**

  - **`collection`**：指定要遍历的集合名称（如 `list` 或 `map` 的键）。
    - 如果参数是一个 `List` 或 `数组`，如果你没有使用 `@Param` 注解，`collection="list"` 或 `collection="array"` 是默认值。
    - 如果你使用了 `@Param` 注解，MyBatis 会使用你指定的名称作为参数绑定的名称，而不再使用默认的 `list` 或 `array`。
    - 如果参数是一个 `Map`，`collection` 就是 Map 的键名。
    - 如果参数是对象，`collection` 是对象中集合属性的名称。
  - **`item`**：当前遍历的元素。和后续使用的 `#{}` 占位符中的变量名（如 `#{id}`）一致
  - **`index`**：当前元素的索引。
  - **`separator`**：分隔符，用于分隔生成的 SQL 片段。
  - **`open`**：拼接 SQL 的开头部分。

  ```java
   @Test
   public void test5() {
       List<String> patIds = new ArrayList<>();
       patIds.add("77006889");
       patIds.add("77006890");
       List<Map<String, Object>> list = testMapper.selectUsersByIds(patIds);
       System.out.println(list);
   }
  ```

  ```java
  List<Map<String, Object>> selectUsersByIds(List<String> patIds);
  ```

  ```xml
  <select id="selectUsersByIds" parameterType="java.util.List" resultType="map">
      select * from his.pat_inpat_order_cost c where c.pat_id in
      <foreach collection="list" item="pat_id" open="(" separator="," close=")">
          #{pat_id}
      </foreach>
  </select>
  ```



```java
@Test
public void test2() {
    List<String> pat_ids = new ArrayList<>(List.of("77003810","77004949"));
    List<Map<String,Object>> result = testMapper.selectUersByPatIds(pat_ids);
    System.out.println(result);
}
```

```java
List<Map<String, Object>> selectUersByPatIds(@Param("pat_ids") List<String> pat_ids);
```

```xml
<select id="selectUersByPatIds" resultType="map">
    select * from his.pat_info i
    <where>
        <if test="pat_ids != null and pat_ids.size > 0">
            i.pat_id in
            <foreach collection="pat_ids" item="pat_id" open="(" separator="," close=")">
                #{pat_id}
            </foreach>
        </if>
    </where>
</select>
```





## 注解开发

一般不建议使用

`@Select`、`@Insert`、`@Update`、`@Delete`

写在mapper接口内

```java
@Select("select * from his.pat_inpat_order_cost where pat_id = #{pat_id}")
public User selectById (int id)
```





## Example

mybatis的的配置文件可以使用mybatis-generator工具生成，它就可以帮我们生成example类。

用于封装复杂的查询条件



### Criteria

`Criteria` 是 `Example` 类中的一个内部类，表示单组查询条件。它封装了类似于 SQL 的 `WHERE` 子句中的条件逻辑，比如 `=`, `<`, `>`, `LIKE`, `IN`, 等常用操作。通过 `Criteria`，你可以灵活地添加多种条件组合。

`Criteria` 是一个构建器模式，通过链式调用的方式逐步添加条件。











# MybatisPlus

## BaseMapper

自定义Mapper继承BaseMapper接口

```java
public interface UserMapper extends BaseMapper<User> {
    
}
```

- 类名：驼峰     表名：下划线
- 名为id的字段作为主键
- 变量名：驼峰    表字段名：下划线

当表名和主键名不一致时：

- `@TableName`：指定表名

  如果实体类名与表名不一致，需要通过 `@TableName` 指定表名。

- `@TableId`：用来指定表中的主键字段信息

  标注实体类中的主键字段，并指定主键的生成策略。

  - **`value`**：标注对应的数据库主键字段名。
  - **`type`**：指定主键生成策略（例如 `AUTO`, `INPUT`, `ASSIGN_ID`, `ASSIGN_UUID` 等）。

- `@TableField`：用来指定表中的普通字段信息

  如果字段名与数据库列名不一致，需要使用 `@TableField` 指定。

```java
@Data
@TableName("user_table")  // 表名映射
public class User {
    @TableId(value = "id", type = IdType.AUTO)  // 主键字段
    private Long id;

    @TableField("user_name")  // 数据库列名为 "user_name"
    private String username;

    @TableField("user_email")  // 数据库列名为 "user_email"
    private String email;

    @TableField(exist = false)  // 表中不存在此字段
    private String tempField;
}
```





## 分页查询

```java
@GetMapping("/page")
public R<Page> page(int page, int pageSize, String name) {
    log.info("page ={}, pageSize ={}, name ={}", page, pageSize, name);
    // 构造分页构造器
    Page pageInfo = new Page(page, pageSize);
    // 构造查询条件
    LambdaQueryWrapper<Employee> queryWrapper = new LambdaQueryWrapper<>();
    // 添加过滤条件
    queryWrapper.like((name != null && !name.trim().equals("")), Employee::getName, name);
    // 添加排序条件
    queryWrapper.orderByDesc(Employee::getUpdateTime);
    // 执行查询
    employeeService.page(pageInfo, queryWrapper);
    return R.success(pageInfo);
}
```





## MetaObjectHandler

用于实现数据库操作时的字段自动填充功能。

它可以帮助开发者在插入或更新记录时，自动填充特定字段，比如创建时间、更新时间、创建人等通用字段，从而减少重复代码。

### 主要特点

1. 自动填充：无需在每次插入或更新操作中手动设置字段值
2. 集中管理：将字段填充逻辑集中在一处，便于维护
3. 可定制性：可以根据业务需求自定义填充逻辑

### 使用步骤

1. 创建自定义类实现MetaObjectHandler接口
2. 重写insertFill和updateFill方法
3. 在实体类的字段上添加@TableField注解并设置fill属性























