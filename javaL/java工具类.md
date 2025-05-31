# Spring工具类

## DigestUtils

`org.springframework.util.DigestUtils` 是 Spring 框架中的简单工具类，主要用于生成 **MD5 哈希值**。

```java
import org.springframework.util.DigestUtils;

public class SpringDigestUtilsExample {
    public static void main(String[] args) {
        String input = "hello world";

        // 将字符串转为字节数组
        byte[] bytes = input.getBytes();

        // 生成 MD5 的十六进制字符串
        String md5Hex = DigestUtils.md5DigestAsHex(bytes);
        System.out.println("MD5 Hex: " + md5Hex);
    }
}
```



## UUID

`java.util.UUID;`

```java
String FileName = UUID.randomUUID().toString();
```



## BeanUtils



# JSON处理库

## Jackson

Jackson 是 Java 生态中最流行的 JSON 处理库之一。用于将 Java 对象与 JSON 数据互相转换（序列化与反序列化）。

Spring Boot 默认集成了 **Jackson** 作为 JSON 的序列化与反序列化库。

当你使用 `@RestController` 返回一个对象时，Spring Boot 会自动使用 Jackson 将对象转换为 JSON。

| 特性       | 内容                                                         |
| ---------- | ------------------------------------------------------------ |
| 类型       | Java 第三方工具库                                            |
| 功能       | JSON 的序列化（Java对象 → JSON）和反序列化（JSON → Java对象） |
| 是否是框架 | ❌ 不是完整的框架（如 Spring、Hibernate 这类）                |
| 是否常用   | ✔ 非常常用，是 Java 中最流行的 JSON 处理库                   |

### 核心类：`ObjectMapper`

Jackson 的使用几乎都围绕这个类展开

```java
ObjectMapper mapper = new ObjectMapper();
```

#### 常用方法：

1. 序列化（对象 → JSON）

   `writeValueAsString(Object value)`

   ```java
   public void write() throws IOException {
       Person person = new Person();
       person.setName("Kagy");
       person.setAge(25);
       person.setEmail("666@qq.com");
       
       ObjectMapper objectMapper = new ObjectMapper();
       String json = objectMapper.writeValueAsString(person);
       System.out.println(json);
   }
   ```

2. 将 Java 对象写入 JSON 文件。

   `writeValue(File resultFile, Object value)`

3. 反序列化（JSON → 对象）

   `readValue(String json, Class<T> valueType)`

   ```java
   public void read() throws IOException {
       ClassPathResource resource = new ClassPathResource("T1.json");
       InputStream inputStream = resource.getInputStream();
       ObjectMapper objectMapper = new ObjectMapper();
       Person person = objectMapper.readValue(inputStream, Person.class);
       System.out.println(JSON.toJSONString(person));
   }
   ```

4.  树模型操作（JsonNode）将 JSON 字符串解析为 `JsonNode` 树结构

   `readTree(String json)`

   ```java
   JsonNode node = mapper.readTree(json);
   String name = node.get("name").asText();
   ```

   







### Jackson 注解体系



### 树模型 (`JsonNode`)









### 示例

```java
@Test
public void read2() throws IOException {
    ObjectMapper mapper = new ObjectMapper();
    InputStream is = JsonReader.class.getClassLoader().getResourceAsStream("T1.json");
    // 或者
    // InputStream is = new ClassPathResource("T1.json").getInputStream();
    
    if (is == null) {
        throw new RuntimeException("File not found!");
    }
    JsonNode node = mapper.readTree(is);
    System.out.println(node.toString());
}
```

| 场景                          | 做法                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| 读取 resources 下的 JSON 文件 | 使用 `getClass().getClassLoader().getResourceAsStream()`<br />`InputStream is = new ClassPathResource("T1.json").getInputStream();` |
| 解析为 Java 对象              | `mapper.readValue(inputStream, Class)`                       |
| 不定义类，读取为树结构        | `mapper.readTree(inputStream)`                               |





## FastJson

**Fastjson** 是阿里巴巴开源的 Java 库，用于**高性能**处理 JSON。它支持 JSON 的序列化、反序列化、路径提取、动态修改等功能。

### maven依赖

```xml
<dependency>
    <groupId>com.alibaba.fastjson2</groupId>
    <artifactId>fastjson2</artifactId>
    <version>2.0.57</version>
</dependency>
```

### 常用 API 总结

1. `JSON.toJSONString(Object)`

   对象转 JSON 字符串

   ```java
   public void write() throws IOException {
       Person person = new Person();
       person.setName("Kagy");
       person.setAge(25);
       person.setEmail("666@qq.com");
       String json = JSON.toJSONString(person);
       System.out.println(json);
   }
   ```

2. `JSON.parseObject(String, Class)`

   JSON 字符串转对象

   ```java
   public void read() throws IOException {
       ClassPathResource resource = new ClassPathResource("T1.json");
       InputStream inputStream = resource.getInputStream();
       Person person = JSON.parseObject(inputStream, Person.class);
       System.out.println(person.getAge());
   }
   ```

3. `JSON.parseArray(String, Class)`

   JSON 数组转集合

4. `JSONObject.getXxx(...)`

   动态访问字段

5. `Files.write(...)`

   JSON 写入文件

6. `JSONSchema.of(schema).validate(...)`

   JSONSchema 校验





## Gson

### Maven依赖

```xml
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.11.0</version> 
</dependency>
```

### GSON类

`Gson` 是 Gson 的核心类，用于执行序列化和反序列化操作。

#### 常用方法

1. `String toJson(Object src)`

   将对象转为 JSON 字符串

   ```java
   public void test1() throws IOException {
       Person p1 = new Person();
       p1.setName("Kagy");
       p1.setAge(20);
       p1.setEmail("ka@gmail.com");
       Gson gson = new Gson();
       String json = gson.toJson(p1); // 转换为 JSON 字符串
       System.out.println(json);
   }
   ```

2. `void toJson(Object src, Appendable writer)`

   将对象写入 Writer

3. `<T> T fromJson(String json, Class<T> classOfT)`

   将 JSON 字符串转为对象

4. `<T> T fromJson(String json, Type typeOfT)`

   支持泛型类型的反序列化

5. `<T> T fromJson(Reader json, Class<T> classOfT)`

   从 `Reader` 读取 JSON

   ```java
   public void test2() throws IOException {
       ClassPathResource resource = new ClassPathResource("T1.json");
       InputStream inputStream = resource.getInputStream();
       InputStreamReader inputStreamReader = new InputStreamReader(inputStream, "UTF-8");
       Gson gson = new Gson();
       Person p1 = gson.fromJson(inputStreamReader, Person.class);
       System.out.println(p1.getName());
   }
   ```

   



# 第三方库

## Hutool

Hutool是一个小而全的Java工具类库，通过静态方法封装

### 引入

```xml
<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-all</artifactId>
    <version>5.8.38</version>
</dependency>
```

### 模块

#### hutool-crypto

##### `SecureUtil`

`SecureUtil.md5()`加密

```java
String input = "hello123";
String md5 = SecureUtil.md5(input);
System.out.println("MD5 加密结果：" + md5);
```



`MD5`类

| 方法名                      | 说明                                     |
| --------------------------- | ---------------------------------------- |
| `update(byte[] input)`      | 添加要加密的数据，可多次调用实现分段加密 |
| `digest()`                  | 返回加密后的字节数组                     |
| `digestHex()`               | 返回加密后的十六进制字符串               |
| `digestHex(String data)`    | 对字符串进行加密并返回十六进制字符串     |
| `digestHex(InputStream in)` | 对输入流数据进行加密                     |
| `digestHex(File file)`      | 对文件进行加密                           |
| `reset()`                   | 重置内部摘要状态（一般用于多次加密）     |

```java
public class Md5StreamExample {
    public static void main(String[] args) {
        MD5 md5 = new MD5();
        md5.update("hello".getBytes());
        md5.update("123".getBytes());
        String result = md5.digestHex();
        System.out.println("MD5 加密结果：" + result);
    }
}
```









#### hutool-json

##### JSON工具-JSONUtil

`JSONUtil.toJsonStr`

可以将任意对象（Bean、Map、集合等）直接转换为JSON字符串。 如果对象是有序的Map等对象，则转换后的JSON字符串也是有序的。

```java
public void ObjectToJson() throws IOException {
    Person person = new Person("Jhon", 18, "kagy@gmail.com");
    String json = JSONUtil.toJsonStr(person);
    System.out.println(json);
}
```

`JSONUtil.parseObj`

JSON字符串解析

```java
public void parseJson() throws IOException {
    // 指定字符编码
    String jsonStr = ResourceUtil.readStr("T1.json", CharsetUtil.CHARSET_UTF_8);
    JSONObject jsonObject = JSONUtil.parseObj(jsonStr);
    // 或者
    // JSONObject jsonObject = new JSONObject(jsonStr);
    String name = jsonObject.getStr("name");
    int age = jsonObject.getInt("age");
    String email = jsonObject.getStr("email");
    System.out.println(name + " " + age + " " + email);
}
```

`JSONUtil.toBean`

JSON转Bean

```java
public void JsonToObject() throws IOException {
    // 直接使用 ResourceUtil 读取 classpath 下的文件内容
    String jsonStr = ResourceUtil.readUtf8Str("T1.json");
    // 将字符串转换为对象
    Person person = JSONUtil.toBean(jsonStr, Person.class);
    System.out.println(person.getName());
}
```

```java
public void JsonToObject() throws IOException {
    ClassPathResource classPathResource = new ClassPathResource("T1.json");
    InputStream inputStream = classPathResource.getInputStream();
    String jsonStr = IoUtil.readUtf8(inputStream);
    Person person = JSONUtil.toBean(jsonStr, Person.class);
    System.out.println(person.getName());
}
```



##### JSON对象-JSONObject

JSONObject代表一个JSON中的键值对象，这个对象以大括号包围，每个键值对使用`,`隔开，键与值使用`:`隔开

###### 创建

`createObj()`快速创建

```java
JSONObject json1 = JSONUtil.createObj()
  .put("a", "value1")
  .put("b", "value2")
  .put("c", "value3");
```

###### 转换

```java
String jsonStr = "{\"b\":\"value2\",\"c\":\"value3\",\"a\":\"value1\"}";
//方法一：使用工具类转换
JSONObject jsonObject = JSONUtil.parseObj(jsonStr);
//方法二：new的方式转换
JSONObject jsonObject2 = new JSONObject(jsonStr);

//JSON对象转字符串（一行）
jsonObject.toString();

// 也可以美化一下，即显示出带缩进的JSON：
jsonObject.toStringPretty();
```



##### JSON数组-JSONArray

######  创建

```java
//方法1
JSONArray array = JSONUtil.createArray();
//方法2
JSONArray array = new JSONArray();

array.add("value1");
array.add("value2");
array.add("value3");

//转为JSONArray字符串
array.toString();
```

###### 从json字符串解析

```java
String jsonArr = "[{\"id\":111,\"name\":\"test1\"},{\"id\":112,\"name\":\"test2\"}]";
JSONArray array = JSONUtil.parseArray(jsonArr);
```



## Lombok

### java17 record 类

在java17之后可以用记录类型（record类）来快速得到一个自带的构造方法，Getter以及重写ToString

自动生成的 Getter、没有 Setter

```java
package test.dto;

public record StudentRecord(String name, int age, String gender) {
}
```

```java
public class test2 {
    public static void main(String[] args) {
        StudentRecord r = new StudentRecord("Li", 12, "male");
        System.out.println(r.name());
        System.out.println(r.age());
        System.out.println(r.toString());
        System.out.println(r.hashCode());
    }
}
```

**@Data**

包含`@Getter`,`@Setter`,`@RequiredArgsConstructor`,`@ToString`,`@EqualsAndHashCode`

**@Getter**

自动生成getter方法，可以添加到类或者字段上

boolean类型的`get方法`会被编译成`isXXX`

```java
private boolean isDeleted;

@Generated
public boolean isDeleted() {
    return this.isDeleted;
}
```

限制生成为`private`的get方法

```java
@Getter(AccessLevel.PRIVATE)
```

给`get方法`加上其他注解

```java
@Getter(onMethod_ = {@Deprecated})
```

如果手动编写了该字段的getter或者setter就不会生成



**@Setter**

**@Tolerate**

忽略

### 构造方法

@AllArgsConstructor

@NoArgsConstructor

@RequiredArgsConstructor

@ToString

@EqualsAndHashCode

### var & val

从 **Java 10** 开始，Java 引入了 **`var`** 关键字，用于局部变量类型推断。这意味着你可以在声明变量时省略显式的类型，Java 编译器会根据右侧的赋值表达式推断出变量的类型。

- **`var` 只能用于局部变量**，不能用于类成员变量或方法参数。
- **类型是静态的**。虽然你省略了类型声明，但变量的类型在编译时就已经确定，不会因为赋值改变。
- **`var` 不等于动态类型**。



Lombok 的 `val` 会将变量标记为 `final`，并且会自动进行类型推断

```java
import lombok.val;

public class Main {
    public static void main(String[] args) {
        val name = "John"; // 推断为 final String
        val list = new ArrayList<String>(); // 推断为 final ArrayList<String>
    }
}
```



### 日志框架

`@Slf4j`

- 作用：生成一个 **`org.slf4j.Logger`** 类型的日志记录器。

- 常用于项目中广泛使用的 **SLF4J** 日志框架。

  ```java
  import lombok.extern.slf4j.Slf4j;
  
  @Slf4j
  public class Demo {
      public void doSomething() {
          log.info("This is an info message.");
          log.error("This is an error message.");
      }
  }
  ```

  相当于手动定义

  ```java
  private static final org.slf4j.Logger log = org.slf4j.LoggerFactory.getLogger(Demo.class);
  ```

`@Log`

- 作用：生成一个 **`java.util.logging.Logger`** 类型的日志记录器。
- 使用的是 Java 自带的日志框架 **`java.util.logging`**。

`@Log4j`

- 作用：生成一个 **`org.apache.log4j.Logger`** 类型的日志记录器。
- 使用的是 **Log4j 1.x** 框架。

`@Log4j2`

- 作用：生成一个 **`org.apache.logging.log4j.Logger`** 类型的日志记录器。
- 使用的是 **Log4j 2.x** 框架，常用于现代 Java 项目。









