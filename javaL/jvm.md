# JVM

## 什么是jvm

JVM（Java Virtual Machine）: Java虚拟机，真正运行Java程序的地方。

JVM本质上是一个运行在计算机上的程序，他的职责是运行Java字节码文件。**只负责运行编译好的字节码（`.class` 文件）**。

JVM是Java程序运行的基础设施，它将Java字节码`(.class文件)`转换为特定平台的机器码执行。JVM的核心理念是"一次编写，到处运行"(Write Once, Run Anywhere)。





# jdk

## 组成

```
JDK
├── JRE
│   ├── JVM
│   └── Java 核心类库
└── Java 开发工具（javac、javadoc 等）
```

## 定义

**JDK（Java Development Kit）是 Java 的开发工具包**，提供了开发 Java 应用程序所需的全部组件。







## JRE

JRE（Java Runtime Environment）—— Java 运行环境

JRE = JVM + Java 核心类库（+ 一些运行时支持文件）





### 核心类库

Java 核心类库是 Java 编程语言提供的标准库集合，是 Java 程序开发的基础。

1. `java.lang`（**基本类**）

   这是最常用的包之一，**默认自动导入**，包含 Java 语言的基础类。

   | 类名                              | 说明                                      |
   | --------------------------------- | ----------------------------------------- |
   | `Object`                          | 所有类的根类                              |
   | `String`                          | 字符串处理类                              |
   | `StringBuilder` & `StringBuffer`  | 可变字符串                                |
   | `Math`                            | 数学函数，如 `sin()`, `cos()`, `pow()` 等 |
   | `System`                          | 系统相关，如标准输入输出（`System.out`）  |
   | `Runtime`                         | JVM 运行时信息                            |
   | `Thread`                          | 多线程支持                                |
   | `Throwable`, `Exception`, `Error` | 异常处理                                  |

2. `java.util`（**工具类包**）

   提供了大量工具类，核心功能如下：

   | 类/接口                                         | 功能                           |
   | ----------------------------------------------- | ------------------------------ |
   | `Collection`, `List`, `Set`, `Map`              | 集合框架接口                   |
   | `ArrayList`, `LinkedList`, `HashSet`, `HashMap` | 常用集合实现                   |
   | `Collections`                                   | 集合操作工具类（排序、查找等） |
   | `Arrays`                                        | 数组操作类                     |
   | `Date`, `Calendar`, `TimeZone`                  | 时间日期类（老 API）           |
   | `Random`                                        | 随机数生成                     |
   | `Optional`                                      | 避免空指针的封装类型           |

3. `java.io`（**输入输出**）

   用于文件和数据流的处理：

   | 类                                        | 功能                 |
   | ----------------------------------------- | -------------------- |
   | `File`                                    | 文件和目录的表示     |
   | `InputStream`, `OutputStream`             | 字节流               |
   | `Reader`, `Writer`                        | 字符流               |
   | `BufferedReader`, `BufferedWriter`        | 带缓冲的流，提高效率 |
   | `ObjectInputStream`, `ObjectOutputStream` | 对象序列化与反序列化 |

4. `java.nio`（**新 I/O**）

5. `java.net`（**网络编程**）

6. `java.math`（**数学计算**）

7. `java.time`（**现代日期时间 API**）

8. `java.util.concurrent`（**并发包**）

9. `java.lang.reflect`（**反射机制**）

10. `java.security`（**安全与加密**）

11. `javax.*` 系列（**扩展包，但常用**）





## 开发工具

这些是命令行工具，用于编译、运行、调试、打包 Java 程序

| 工具                              | 功能说明                                               |
| --------------------------------- | ------------------------------------------------------ |
| `javac`                           | Java 编译器，将 `.java` 源码编译成 `.class` 字节码文件 |
| `java`                            | 启动 Java 程序，通过 JVM 加载并运行 `.class` 文件      |
| `javadoc`                         | 生成 Java 文档（HTML 格式）                            |
| `jar`                             | 打包工具，创建 `.jar` 文件                             |
| `jdb`                             | Java 调试工具                                          |
| `javap`                           | 字节码反汇编工具（查看 `.class` 文件结构）             |
| `jcmd`, `jstat`, `jmap`, `jstack` | 性能调试和诊断工具                                     |









# 