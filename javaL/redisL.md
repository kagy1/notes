# Redis

Redis 是 "Remote Dictionary Server" 的缩写。





## NoSQL

NoSQL：非关系型数据库

SQL：关系型数据库



查询语法

```bash
get user:1  -- redis
db.users.find({_id:1})  -- MongoDB
GET http://localhost:9200/users/1  -- elasticsearch
```



## 客户端

命令行客户端

Redis安装完成后自带了命令行客户端：redis-cli

```bash
redis-cli -h 127.0.0.1 -p 6379
```



## 通用命令

| **命令**                | **描述**                                                    |
| ----------------------- | ----------------------------------------------------------- |
| `PING`                  | 测试连接是否正常，返回 `PONG`。                             |
| `ECHO <message>`        | 打印指定的消息。                                            |
| `SELECT <index>`        | 切换到指定编号的数据库（默认 16 个数据库，编号从 0 开始）。 |
| `DBSIZE`                | 返回当前数据库中键的数量。                                  |
| `FLUSHDB`               | 清空当前数据库中的所有键。                                  |
| `FLUSHALL`              | 清空所有数据库中的所有键。                                  |
| `INFO`                  | 查看服务器信息和统计数据。                                  |
| `CONFIG GET <param>`    | 获取 Redis 配置参数。                                       |
| `CONFIG SET <param>`    | 设置 Redis 配置参数。                                       |
| `TIME`                  | 获取当前服务器的时间（精确到微秒）。                        |
| `ROLE`                  | 查看当前实例的角色（主/从/哨兵）。                          |
| `CLIENT LIST`           | 显示所有连接到 Redis 的客户端信息。                         |
| `CLIENT KILL <ip:port>` | 关闭指定客户端连接。                                        |
| `CLIENT SETNAME <name>` | 设置当前连接的名称。                                        |
| `CLIENT GETNAME`        | 获取当前连接的名称。                                        |
| `SHUTDOWN`              | 关闭 Redis 服务器。                                         |



## 键（Key）操作

| **命令**                                 | **描述**                                           |
| ---------------------------------------- | -------------------------------------------------- |
| `KEYS <pattern>`   `keys *`、`KEYS a*`   | 查找符合给定模式的所有键（生产环境慎用，效率低）。 |
| `EXISTS <key>`                           | 检查指定的键是否存在。                             |
| `DEL <key>`                              | 删除指定的键。                                     |
| `TYPE <key>`                             | 返回键的存储类型（如 string、list、set 等）。      |
| `EXPIRE <key> <seconds>`                 | 设置键的过期时间（秒）。                           |
| `PEXPIRE <key> <ms>`                     | 设置键的过期时间（毫秒）。                         |
| `TTL <key>`                              | 获取键的剩余过期时间（秒）。                       |
| `PTTL <key>`                             | 获取键的剩余过期时间（毫秒）。                     |
| `RENAME <key> <newkey>`                  | 重命名键。                                         |
| `RENAMENX <key> <newkey>`                | 重命名键（仅当新键不存在时）。                     |
| `RANDOMKEY`                              | 随机返回一个键。                                   |
| `MOVE <key> <db>`                        | 将键移动到指定数据库。                             |
| `DUMP <key>`                             | 序列化键的值，返回序列化结果。                     |
| `RESTORE <key> <ttl> <serialized-value>` | 反序列化数据到 Redis 中。                          |



## key结构

Redis的key允许有多个单次形成层级结构，多个单词之间用':'隔开

```
项目名:业务名:类型:id
```

```bash
set heima:user:1 '{"id":2,"name":"荣耀6","price":2999}'
```





## 数据结构

在 Redis 中，所有存储的值都是以 **字符串** 的形式存储的。**所有值默认存储为字符串**

### String 字符串

最基本的数据结构，每个键对应一个字符串值，可以是文本或二进制数据。

- string：普通字符串
- int：整数类型，可以自增自减
- float：浮点类型，可以自增自减
- JSON

#### 常用命令

| **命令**                   | **描述**                       |
| -------------------------- | ------------------------------ |
| `SET <key> <value>`        | 设置键的值为指定字符串。       |
| `GET <key>`                | 获取键的值。                   |
| `GETSET <key> <value>`     | 设置新值并返回旧值。           |
| `SETNX <key> <value>`      | 设置键的值，若键不存在则成功。 |
| `MSET <key1> <value1> ...` | 批量设置多个键值对。           |
| `MGET <key1> <key2> ...`   | 批量获取多个键的值。           |
| `INCR <key>`               | 将键的值加 1（值必须是整数）。 |
| `INCRBY <key> <increment>` | 将键的值增加指定整数。         |
| `DECR <key>`               | 将键的值减 1。                 |
| `DECRBY <key> <decrement>` | 将键的值减少指定整数。         |
| `APPEND <key> <value>`     | 在键的值后追加指定字符串。     |
| `STRLEN <key>`             | 获取键值的字符串长度。         |

`SET key value`

设置键值

```bash
SET username kagy  # 如果值没有特殊字符（如空格、换行符等），可以直接写
SET username "kagy is awesome"  # 如果值包含特殊字符，建议用引号包裹
```



### Hash 哈希表

类似于一个小型的键值对集合。

每个键（field）都映射到一个值（value）。

适合存储对象的属性信息。

| **命令**                            | **描述**                                                   |
| ----------------------------------- | ---------------------------------------------------------- |
| `HSET <key> <field> <value>`        | 设置哈希表中字段的值。                                     |
| `HGET <key> <field>`                | 获取指定字段的值。                                         |
| `HDEL <key> <field1> ...`           | 删除一个或多个字段。                                       |
| `HEXISTS <key> <field>`             | 检查字段是否存在。                                         |
| `HKEYS <key>`                       | 获取哈希表中所有字段。                                     |
| `HVALS <key>`                       | 获取哈希表中所有值。                                       |
| `HGETALL <key>`                     | 获取哈希表中所有字段和值。                                 |
| `HLEN <key>`                        | 获取哈希表中的字段数量。                                   |
| `HINCRBY <key> <field> <increment>` | 将字段值增加指定整数。                                     |
| `HMSET <key> <field1> <value1> ...` | 批量设置多个字段的值（Redis 4.0 以后建议用 `HSET` 替代）。 |
| `HMGET <key> <field1> <field2> ...` | 批量获取多个字段的值。                                     |

```bash
HSET heima:user:1 age 18 name "Li"
```





### List 列表

有序的字符串列表。支持从两端插入、弹出元素。元素可重复

类似java的`LinkedList`可以看作一个双向链表结构，支持正向检索也支持反向检索

| **命令**                            | **描述**                         |
| ----------------------------------- | -------------------------------- |
| `LPUSH <key> <value>`               | 从左侧插入一个或多个值到列表。   |
| `RPUSH <key> <value>`               | 从右侧插入一个或多个值到列表。   |
| `LPOP <key>`                        | 从左侧弹出一个值并返回。         |
| `RPOP <key>`                        | 从右侧弹出一个值并返回。         |
| `LLEN <key>`                        | 返回列表的长度。                 |
| `LRANGE <key> <start> <stop>`       | 获取指定范围内的元素。           |
| `LINDEX <key> <index>`              | 获取指定索引的元素。             |
| `LSET <key> <index> <value>`        | 设置指定索引的元素值。           |
| `LTRIM <key> <start> <stop>`        | 截取列表，保留指定范围内的元素。 |
| `BLPOP <key1> <key2> ... <timeout>` | 阻塞弹出左侧元素。               |
| `BRPOP <key1> <key2> ... <timeout>` | 阻塞弹出右侧元素。               |



### Set 集合

无序的字符串集合。

元素唯一，不允许重复。

提供集合运算功能（交集、并集、差集）。

| **命令**                   | **描述**                   |
| -------------------------- | -------------------------- |
| `SADD <key> <member>`      | 添加一个或多个元素到集合。 |
| `SREM <key> <member>`      | 移除一个或多个元素。       |
| `SISMEMBER <key> <member>` | 检查元素是否存在。         |
| `SMEMBERS <key>`           | 获取集合中所有元素。       |
| `SCARD <key>`              | 获取集合中元素的数量。     |
| `SDIFF <key1> <key2>`      | 对多个集合求差集。         |
| `SINTER <key1> <key2>`     | 对多个集合求交集。         |
| `SUNION <key1> <key2>`     | 对多个集合求并集。         |



### Sorted set 有序集合







## Java客户端

### **对比总结**

| 客户端                | 特点                                                         | 适用场景                                     |
| --------------------- | ------------------------------------------------------------ | -------------------------------------------- |
| **Jedis**             | 功能全面，简单易用，但线程不安全。                           | 单线程环境，简单的 Redis 操作。              |
| **Lettuce**           | 基于 Netty，线程安全，支持异步和响应式操作，高性能。         | 高并发场景，异步、响应式编程，Redis 集群。   |
| **Redisson**          | 提供高级抽象功能（如分布式锁、队列、集合等），支持多种 Redis 模式。 | 分布式系统，高级功能需求（锁、集合、队列）。 |
| **Spring Data Redis** | 与 Spring 无缝集成，支持多种客户端，简化操作。               | Spring 项目，快速开发。                      |

### Jedis

以Redis命令作为方法名称，学习成本低，简单实用。

但是Jedis实例线程不安全，多线程环境下需要基于连接池使用。

```java
import redis.clients.jedis.Jedis;

public class JedisExample {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("localhost", 6379); // 连接 Redis
        jedis.set("key", "value");                 // 设置键值
        String value = jedis.get("key");          // 获取值
        System.out.println(value);                // 输出：value
        jedis.close();                            // 关闭连接
    }
}
```

#### 使用

```
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
</dependency>
```

```java
@SpringBootTest
public class test4 {
    private Jedis jedis;

    @BeforeEach
    void setUp() {
        jedis = new Jedis("127.0.0.1", 6379);
        // 设置密码
        // jedis.auth("password");
        jedis.select(0);
    }

    @Test
    void testString() {
        String result = jedis.set("name", "kagy");
        System.out.println("result:" + result);
        String name = jedis.get("name");
        System.out.println("name:" + name);
    }

    @Test
    void testHash() {
        jedis.hset("user:1", "name", "kagy");
        jedis.hset("user:1", "age", "25");
        Map<String, String> map = jedis.hgetAll("user:1");
        System.out.println(map);
    }
}
```

#### Jedis线程池





### Lettuce

Lettuce 是另一个非常流行的 Redis Java 客户端，基于 Netty 框架开发，支持同步和异步操作，线程安全，适合高并发环境。

同步模式

```java
import io.lettuce.core.RedisClient;
import io.lettuce.core.api.sync.RedisCommands;

public class LettuceExample {
    public static void main(String[] args) {
        RedisClient redisClient = RedisClient.create("redis://localhost:6379");
        var connection = redisClient.connect();
        RedisCommands<String, String> commands = connection.sync(); // 同步命令
        commands.set("key", "value");
        System.out.println(commands.get("key")); // 输出：value
        connection.close();
        redisClient.shutdown(); // 关闭客户端
    }
}
```

异步模式

```java
import io.lettuce.core.RedisClient;
import io.lettuce.core.api.async.RedisAsyncCommands;

public class LettuceAsyncExample {
    public static void main(String[] args) {
        RedisClient redisClient = RedisClient.create("redis://localhost:6379");
        var connection = redisClient.connect();
        RedisAsyncCommands<String, String> commands = connection.async(); // 异步命令
        commands.set("key", "value");
        commands.get("key").thenAccept(System.out::println); // 异步回调
        connection.close();
        redisClient.shutdown();
    }
}
```



### Spring Data Redis

Spring Data Redis 是 Spring 提供的一个 Redis 数据访问框架，整合了 Jedis 和 Lettuce 客户端，提供统一的模板化操作接口。

提供RedisTemplate统一API来操作redis

支持Redis哨兵和Redis集群



#### RedisTemplate

##### 常用方法

操作字符串

```java
@Autowired
private RedisTemplate<String, Object> redisTemplate;

public void stringOperations() {
    ValueOperations<String, Object> valueOps = redisTemplate.opsForValue();

    // 设置键值对
    valueOps.set("key1", "value1");

    // 获取值
    String value = (String) valueOps.get("key1");
    System.out.println("Value for key1: " + value);

    // 设置带过期时间的值
    valueOps.set("key2", "value2", 10, TimeUnit.SECONDS);

    // 追加值
    valueOps.append("key1", "_suffix");
}
```

 操作哈希

```java
public void hashOperations() {
    HashOperations<String, Object, Object> hashOps = redisTemplate.opsForHash();

    // 设置哈希值
    hashOps.put("hashKey", "field1", "value1");

    // 获取哈希值
    String value = (String) hashOps.get("hashKey", "field1");
    System.out.println("Hash field value: " + value);

    // 获取所有字段和值
    Map<Object, Object> entries = hashOps.entries("hashKey");
    System.out.println("All entries: " + entries);
}
```

操作列表

```java
public void listOperations() {
    ListOperations<String, Object> listOps = redisTemplate.opsForList();

    // 从左侧插入
    listOps.leftPush("listKey", "value1");
    listOps.leftPush("listKey", "value2");

    // 从右侧弹出
    String value = (String) listOps.rightPop("listKey");
    System.out.println("Popped value: " + value);
}
```

操作集合

```java
public void setOperations() {
    SetOperations<String, Object> setOps = redisTemplate.opsForSet();

    // 添加元素到集合
    setOps.add("setKey", "value1", "value2", "value3");

    // 获取集合中的所有元素
    Set<Object> members = setOps.members("setKey");
    System.out.println("Set members: " + members);
}
```

发布

```java
// 发布消息
redisTemplate.convertAndSend("channel", "Hello, Redis!");
```



























