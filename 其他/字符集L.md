# 字符集

## ASCII

0 ~ 127

存储英文时，一个字符就够了

a：97   0110 0001

编码规则：前面补0，补齐8位

解码规则：转换为10进制



## GBK

GB2312：1980年，简体中文

BIG5：繁体中文

GBK：2000年。包含GB13000-1中的全部中日韩汉字。ANSI。

GBK字符集完全兼容ASCII字符集

### 转换规则

英文字符（单字节）：ASCII，直接使用单字节ASCII编码

二进制第一位是0

```
a → 0x61
```

中文字符（双字节）：

高位字节二进制一定以1开头，转成十进制以后是一个负数

```
汉 → 47802   10111010 10111010
```









## Unicode

万国码，国际标准字符集



### UTF-8

Unicode Transfer Format

<span style="color:red">UTF-8不是字符集，是一种编码方式</span>

用1-4个字节保存

ASCII：一个字节

中日韩文字：三个字节

```
汉 → 查询unicode 27721   01101100 01001001 → utf-8 编码 11100110 10110001 10001001
```

| 项目        | 内容                                        |
| :---------- | :------------------------------------------ |
| 字符        | 汉                                          |
| Unicode码点 | 0x6C49                                      |
| 二进制      | `01101100 01001001`                         |
| UTF-8编码   | `E6 B1 89`                                  |
| 解释        | 按3字节模板`1110xxxx 10xxxxxx 10xxxxxx`填充 |
| 特点        | 兼容ASCII，可变长编码，节省空间             |

#### 编码规则

| Unicode范围        | 字节数 | 格式（x是数据位）                     |
| :----------------- | :----- | :------------------------------------ |
| U+0000 ~ U+007F    | 1字节  | `0xxxxxxx`                            |
| U+0080 ~ U+07FF    | 2字节  | `110xxxxx 10xxxxxx`                   |
| U+0800 ~ U+FFFF    | 3字节  | `1110xxxx 10xxxxxx 10xxxxxx`          |
| U+10000 ~ U+10FFFF | 4字节  | `11110xxx 10xxxxxx 10xxxxxx 10xxxxxx` |



### UTF-16

**UTF-16**（**U**nicode **T**ransformation **F**ormat - **16** bits）是一种用于编码Unicode字符集的数据表示方式。

| 字符类型                      | Unicode范围        | UTF-16编码方式             | 占用字节数 |
| :---------------------------- | :----------------- | :------------------------- | :--------- |
| 基本字符（ASCII、中文等）     | 0x0000 ~ 0xFFFF    | 单个16位编码单元           | 2字节      |
| 扩展字符（Emoji、冷僻字符等） | 0x10000 ~ 0x10FFFF | 两个16位编码单元（代理对） | 4字节      |

#### UTF-16与ASCII的关系

- UTF-16可以编码所有ASCII字符。
- 在UTF-16编码中，**即使是ASCII字符（如`A~Z`、`0~9`），也占用2个字节**。
- 例如：
  - 字符 `'A'` 的Unicode码是`0x0041`，UTF-16编码是 `00 41`（高位在前或后，取决于字节序）。

因此，**UTF-16是兼容ASCII字符集的内容，但不是兼容ASCII的字节格式**。







## 模拟乱码

### GBK转UTF-8

```java
public static void main(String[] args) throws Exception {
    // 1. 假设原本是 GBK 编码的中文字符串
    String original = "嘿嘿嘿，还好啦";
    // 2. 按 GBK 编码成字节数组
    byte[] gbkBytes = original.getBytes("GBK");
    // 3. 错误地用 UTF-8 解码这些 GBK字节
    String wrongDecoded = new String(gbkBytes, StandardCharsets.UTF_8);
    // 4. 输出结果
    System.out.println("原始字符串：" + original);
    System.out.println("错误解码后（模拟乱码）：" + wrongDecoded);
}
```

### UTF-8转GBK

```java

```



### 字符缺失

嘿嘿嘿，还好啦。”显示为“俸俸伲�购美病”是由于该句的代码丢失了第一个字节“BA”。

```java
public static void main(String[] args) throws Exception {
    // 1. 假设原本是 GBK 编码的中文字符串
    String original = "嘿嘿嘿，还好啦。";
    // 2. 按 GBK 编码成字节数组
    byte[] gbkBytes = original.getBytes("GBK");
    // 只去掉1个字节，破坏字节对齐
    byte[] gbkBytes2 = Arrays.copyOfRange(gbkBytes, 1, gbkBytes.length);
    // 3. 错误地用 GBK 解码这些破坏后的字节
    String wrongDecoded = new String(gbkBytes2, Charset.forName("GBK"));
    // 4. 输出结果
    System.out.println("原始字符串：" + original);
    System.out.println("（模拟乱码）：" + wrongDecoded);  // 俸俸伲购美病�
}
```





# JAVA

## StandardCharsets

`StandardCharsets` 是 **Java标准库**里提供的一个**工具类**，它包含了一些**最常用字符编码(Charset)** 的**常量**。

```java
StandardCharsets.UTF_8
StandardCharsets.UTF_16
StandardCharsets.ISO_8859_1
StandardCharsets.US_ASCII
StandardCharsets.UTF_16BE
StandardCharsets.UTF_16LE
```

```java
import java.io.*;
import java.nio.charset.StandardCharsets;

public class WriteFileExample {
    public static void main(String[] args) throws IOException {
        try (Writer writer = new OutputStreamWriter(new FileOutputStream("file.txt"), StandardCharsets.UTF_8)) {
            writer.write("你好，世界！");
        }
    }
}
```

## Charset类



常用方法：

| 方法                          | 作用                         |
| :---------------------------- | :--------------------------- |
| `Charset.forName("UTF-8")`    | 根据名字找Charset            |
| `Charset.availableCharsets()` | 获取当前JVM支持的所有Charset |
| `charset.name()`              | 获取名字                     |
| `charset.aliases()`           | 获取别名集合                 |



## 编码

```java
// 推荐写法
byte[] bytes = "你好".getBytes(StandardCharsets.UTF_8);
// 等价于
byte[] bytes = "你好".getBytes("UTF-8");
```



## 解码

```java
String text = new String(bytes, StandardCharsets.UTF_8);
String text = new String(bytes, "UTF-8");
```













