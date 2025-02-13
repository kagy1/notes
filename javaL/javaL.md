# JAVA

## 面向对象

### 构造方法

在创建对象的时候给成员对象赋值

有空参构造，带参构造

格式

1. 方法名与类名相同，大小写也要一样
2. 没有返回值类型，void也没有
3. 没有具体的返回值类型（不能用return带回结果）

```java
public class Student {
    修饰符 类名() {
        方法体;
    }
}
```

执行时机

1. 创建对象的时候由虚拟机调用，不能手动调用构造方法
2. 每创建一次对象就调用一次构造方法



构造方法定义：

- 如果没有定义构造方法，系统会给出一个默认的无参构造
- 如果定义了构造方法，系统不再提供默认的构造方法



### 静态方法

在Java中，**静态方法**是使用 `static` 关键字修饰的方法。它属于类本身，而不是类的实例。静态方法可以在不创建类的对象的情况下直接调用，因此常用于工具类、工厂方法或全局逻辑的实现。

特点：

1. **属于类本身**
   静态方法是类级别的，直接与类相关，而不是与某个实例相关。
2. **不依赖于实例**
   调用静态方法不需要创建对象，可以通过类名直接调用。
3. **不能直接访问实例成员**
   静态方法只能访问类变量（静态变量）和调用其他静态方法，无法直接访问实例变量或调用实例方法，除非通过对象引用。
4. **生命周期**
   静态方法随着类的加载而存在，直到类卸载。

```java
public class MathUtils {
    // 定义一个静态方法
    public static int add(int a, int b) {
        return a + b;
    }
}

public class Main {
    public static void main(String[] args) {
        // 直接通过类名调用静态方法
        int result = MathUtils.add(5, 3);
        System.out.println("结果是: " + result);
    }
}
```

**静态方法的限制**

1. **不能使用 `this` 或 `super`**
   静态方法与实例无关，因此无法访问当前实例的 `this` 引用或调用父类的方法。

2. **不能直接访问实例变量或实例方法**
   静态方法只能直接访问静态变量和调用静态方法。若要访问实例成员，需要显式传递对象引用。

   ```java
   public class Example {
       private int instanceVar = 10;
    
       public static void printInstanceVar() {
           // 以下代码会报错，因为 instanceVar 是实例变量
           System.out.println(instanceVar);
    
           // 正确方式：通过对象引用访问实例变量
           Example obj = new Example();
           System.out.println(obj.instanceVar);
       }
   }
   ```

3. **不适合特定于实例的操作**
   如果方法需要依赖于实例的状态（例如实例变量或方法），则不应将其定义为静态方法。

**静态方法的常见用途**

1. **工具类**
   静态方法常用于实现通用逻辑，例如数学计算、字符串处理等（如 `Math`、`Arrays` 和 `Collections` 类）。
2. **工厂方法**
   静态方法可以用来创建对象，例如 `Integer.valueOf()` 和 `Collections.emptyList()`。
3. **全局逻辑**
   静态方法可以实现一些与实例无关的逻辑，例如配置加载器或应用程序入口点（`main` 方法）。







## 内部类

类的五大成员：属性、方法、构造方法、代码块、内部类

### 什么是内部类

在一个类里面再定义一个类

```java
public class Outer {  // 外部类 
    public class inner {  // 内部类
        
    }
}
```

**内部类的访问特点**

- 内部类可以直接访问外部类的成员，包括私有
- 外部类要访问内部类的成员，必须创建对象



### 成员内部类

写在成员位置的，属于外部类的成员

1. 成员内部类可以被一些修饰符所修饰，比如：private，默认，public，static等
2. 在成员内部类里面，JDK16之前不能定义静态变量。

获得成员内部类对象的两种方式：

1. 外部类编写类方法，对外提供内部类对象

   ```java
   public class OuterClass {
       private class InnerClass {
           public void innerMethod() {
               System.out.println("这是成员内部类的方法");
           }
       }
   
       public InnerClass getInnerClass() {
           return new InnerClass();
       }
   }
   
   public class Main {
       public static void main(String[] args) {
           OuterClass outerClass = new OuterClass();
           OuterClass.InnerClass innerClass = outerClass.getInnerClass();
           innerClass.innerMethod();  // 这是成员内部类的方法
       }
   }
   ```

2. 直接创建

   ```java
   public class OuterClass {
       public class InnerClass {
           public void innerMethod() {
               System.out.println("这是成员内部类的方法");
           }
       }
   }
   
   public class Main {
       public static void main(String[] args) {
           OuterClass.InnerClass innerClass =new OuterClass.new InnerClass();
           innerClass.innerMethod();  // 这是成员内部类的方法
       }
   }
   ```

   

#### 获取变量

```java
package classTest;

public class Outer {
    private int a = 10;

    class Inner {
        private int a = 20;

        public void show() {
            int a = 30;
            System.out.println(Outer.this.a);  // 10 获取外部类的成员变量
            System.out.println(this.a);  // 20
            System.out.println(a);   // 30 就近原则

        }
    }

    public static void main(String[] args) {
        Inner i = new Outer().new Inner();
        i.show();
    }
}

```



### 静态内部类

静态内部类只能访问外部类中的静态变量和静态方法，如果要访问非静态的需要创建对象。

创建静态内部类对象的格式： 外部类名.内部类名 对象名 = new 外部类名.内部类名();

```java
public class Test {
    private final String name;

    public Test(String name){
        this.name = name;
    }

    public static class Inner {
        public void test(){
            System.out.println("我是静态内部类！");
        }
    }
}
```

创建对象

```java
public class Main {
    public static void main(String[] args) {
        // 创建静态内部类的对象
        Test.Inner inner = new Test.Inner();

        // 调用静态内部类的方法
        inner.test();
    }
}
```





### 匿名内部类

本质上就是隐藏了名字的内部类

匿名内部类是 Java 编程语言中一种特殊的类，它没有显式地定义类名，而是在创建对象时通过传递实现了某个接口或继承了某个类的代码块来定义类。通常，我们使用它来简化代码、减少类的数量和更高效地编写事件处理程序等。



## 线程

并发：同一时刻，有多个指令在单个CPU上交替执行

并行：同一时刻，有多个指令在多个CPU上同时执行

### 多线程实现方式

#### 继承Thread类

**实现步骤**

1. 创建一个类并继承自 `Thread` 类。
2. 重写 `Thread` 类的 `run()` 方法，将线程的任务逻辑写在 `run()` 方法中。
3. 创建该类的实例。
4. 调用 `start()` 方法启动线程。

```java
public class ThreadDemo {
    public static void main(String[] args) {
        /*  1.定义一个类继承Thread
            2.重写run方法
            3. 创建子类的对象并启动线程 */
        myTrhead t1 = new myTrhead();
        myTrhead t2 = new myTrhead();
        t1.setName("线程1");
        t2.setName("线程2");
        t1.start();
        t2.start();
    }
}

class myTrhead extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 10000; i++) {
            System.out.println(getName() + "hello world");
        }
    }
}

```

**优点**

1. **简单直接**：通过继承 `Thread` 类，可以直接调用 `start()` 方法启动线程，代码较为直观。
2. **易于实现**：适合快速创建单一功能的线程。

**缺点**

1. **单继承限制**：Java 是单继承语言，继承了 `Thread` 类后就无法再继承其他类，限制了类的扩展性。
2. **线程与任务耦合**：继承 `Thread` 类后，线程的任务逻辑与线程本身耦合在一起，难以分离任务逻辑和线程控制。



#### 实现Runnable接口

**实现步骤**

1. 创建一个类并实现 `Runnable` 接口。
2. 在实现类中重写 `run()` 方法，将线程的任务逻辑写在 `run()` 方法中。
3. 创建 `Runnable` 实现类的实例。
4. <span style="color:red">将该实例作为参数传递给 `Thread` 类的构造方法，创建线程对象。</span>
5. 调用线程对象的 `start()` 方法启动线程。

```java
public class ThreadDemo1 {
    public static void main(String[] args) {
        MyRun mr = new MyRun();
        Thread t1 = new Thread(mr);
        Thread t2 = new Thread(mr); 
        t1.setName("线程1");
        t2.setName("线程2");
        t1.start();
        t2.start();
    }
}

class MyRun implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            // 获取到当前线程的对象
            Thread t = Thread.currentThread();
            System.out.println(t.getName()+"hello world");
        }
    }
}
```



#### Callable接口和Future接口

可以获取到多线程的结果





## Lambda

Lambda是jdk8中的一个语法糖，可以简化匿名内部类的书写。让我们不用关注是什么对象，而是更关注我们对数据进行了什么操作。

Lambda只能简化函数式接口的匿名内部类的写法

函数式接口：有且只有一个抽象方法的接口叫做函数式接口，接口上方可以加@FunctionalInterface注解



### runnable

匿名内部类写法

```java
public class LambdaDemo {
    public static void main(String[] args) {
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("run方法执行了");
            }
        }).start();
    }
}
```

lambda简化

```java
public class LambdaDemo {
    public static void main(String[] args) {
        new Thread(() -> System.out.println("run方法执行了")).start();
    }
}
```



### 数组排序

原方法：

```java
String[] s1 = {"1", "2", "333", "22"};

Arrays.sort(s1, new Comparator<String>() {
    @Override
    public int compare(String o1, String o2) {
        return Integer.parseInt(o1) - Integer.parseInt(o2);
    }
});
```

lambda：

```java
String[] s1 = {"1", "2", "333", "22"};

Arrays.sort(s1, (String o1, String o2) -> {
    return o1.compareTo(o2);
});
```

流：

```java
String[] s1 = {"1", "2", "333", "22"};
Arrays.stream(s1).sorted().forEach(System.out::println);
```



### 示例

#### 示例1

匿名内部类

```java
import java.util.function.IntConsumer;

public class Main {
    public static void main(String[] args) {
        foreachArr(new IntConsumer() {
            @Override
            public void accept(int value) {
                System.out.println(value);
            }
        });

    }

    public static void foreachArr(IntConsumer consumer) {
        int[] arr = {1, 2, 3, 4, 5, 6, 7};
        for (int i : arr) {
            consumer.accept(i);
        }
    }
}
```

Lambda

```java
import java.util.function.IntConsumer;

public class Main {
    public static void main(String[] args) {
        foreachArr(value -> System.out.println(value));
    }

    public static void foreachArr(IntConsumer consumer) {
        int[] arr = {1, 2, 3, 4, 5, 6, 7};
        for (int i : arr) {
            consumer.accept(i);
        }
    }
}
```



## Stream流

### 示例

示例1

```java
//  打印姓张的并且长度为3的名字
public class Main {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("张三");
        list.add("李四");
        list.add("李四一");
        list.add("王五");
        list.add("张伟伟");
        list.add("张森弟");

        list.stream().filter(name -> name.length() == 3).filter(name -> name.startsWith("张")).forEach(System.out::println);
    }
}
```

存入list

```java
public class Main {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("张三");
        list.add("李四");
        list.add("李四一");
        list.add("王五");
        list.add("张伟伟");
        list.add("张森弟");

        // 将过滤后的结果存入一个新的列表
        List<String> filteredList = list.stream()
                .filter(name -> name.length() == 3)
                .filter(name -> name.startsWith("张"))
                .collect(Collectors.toList());
    }
}
```

提取出内容形成字符串

通过Collectors.joining用分号分隔并在 前后插入

```java
package test1;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

public class test {
    public static void main(String[] args) {
        Map<Object, String> map1 = new HashMap<>();
        map1.put("name", "John");
        map1.put("age", "30");
        map1.put("city", "New York");
        Map<Object, String> map2 = new HashMap<>();
        map2.put("name", "Jane");
        map2.put("age", "25");
        map2.put("city", "Los Angeles");
        List<Map<Object, String>> list = new ArrayList<>();
        list.add(map1);
        list.add(map2);
        String result = list.stream().map(s -> "姓名" + s.get("name") + "，年龄" + s.get("age") + "，城市" + s.get("city"))
                .collect(Collectors.joining(";", "start ", " end"));
        System.out.println(result);
        // start 姓名John，年龄30，城市New York;姓名Jane，年龄25，城市Los Angeles end
    }
}

```



### 中间方法

`Stream` 是 Java 8 引入的功能，用于支持函数式编程和数据处理。`Stream` 的操作分为 **中间操作（Intermediate Operations）** 和 **终结操作（Terminal Operations）**。以下是尽可能完整的列出它们，并配上代码示例和注释。

1. **`filter`**

   - 用于筛选符合条件的元素。

   ```java
   List<Integer> list = List.of(1, 2, 3, 4, 5);
   list.stream()
       .filter(x -> x % 2 == 0) // 保留偶数
       .forEach(System.out::println);
   ```

2. **`map`**

   - 将每个元素映射为另一个值。

   ```java
   List<String> list = List.of("a", "b", "c");
   list.stream()
       .map(String::toUpperCase) // 转换为大写
       .forEach(System.out::println);
   ```

3. **`flatMap`**

   - 将流中的每个元素转换为另一个流，然后合并。

   ```java
   List<List<Integer>> nestedList = List.of(List.of(1, 2), List.of(3, 4));
   nestedList.stream()
             .flatMap(List::stream) // 扁平化
             .forEach(System.out::println);
   ```

4. **`distinct`**

   - 去重。

   ```java
   List<Integer> list = List.of(1, 2, 2, 3, 4, 4);
   list.stream()
       .distinct() // 去重
       .forEach(System.out::println);
   ```

5. **`sorted`**

   - 对流中的元素排序（自然排序或自定义排序）。

   ```java
   List<Integer> list = List.of(5, 3, 1, 4, 2);
   list.stream()
       .sorted() // 自然排序
       .forEach(System.out::println);
   
   list.stream()
       .sorted((a, b) -> b - a) // 自定义排序（降序）
       .forEach(System.out::println);
   ```

6. **`peek`**

   - 对每个元素执行操作，用于调试。
   - 对流中的每个元素执行一个操作而不改变元素本身

   ```java
   List<String> list = List.of("a", "b", "c");
   list.stream()
       .peek(System.out::println) // 打印每个元素
       .map(String::toUpperCase)
       .forEach(System.out::println);
   ```

7. **`limit`**

   - 截取前 n 个元素。

   ```java
   List<Integer> list = List.of(1, 2, 3, 4, 5);
   list.stream()
       .limit(3) // 只保留前 3 个元素
       .forEach(System.out::println);
   ```

8. **`skip`**

   - 跳过前 n 个元素。

   ```java
   List<Integer> list = List.of(1, 2, 3, 4, 5);
   list.stream()
       .skip(2) // 跳过前 2 个元素
       .forEach(System.out::println);
   ```

### 终结方法

1. **`forEach`**

   - 对每个元素执行操作。

   ```
   java复制代码List<String> list = List.of("a", "b", "c");
   list.stream()
       .forEach(System.out::println); // 打印每个元素
   ```

2. **`toArray`**

   - 将流转换为数组。

   ```
   java复制代码List<Integer> list = List.of(1, 2, 3);
   Integer[] array = list.stream()
                         .toArray(Integer[]::new); // 转换为数组
   ```

3. **`collect`**

   - 收集流的元素到集合或其他容器中。

   ```
   java复制代码List<String> list = List.of("a", "b", "c");
   List<String> result = list.stream()
                             .collect(Collectors.toList()); // 转换为列表
   ```

4. **`count`**

   - 返回流中的元素数量。

   ```
   java复制代码List<Integer> list = List.of(1, 2, 3, 4, 5);
   long count = list.stream().filter(x -> x > 2).count(); // 统计大于 2 的元素数量
   ```

5. **`reduce`**

   - 聚合操作，结合流的元素为一个值。

   ```
   java复制代码List<Integer> list = List.of(1, 2, 3, 4, 5);
   int sum = list.stream()
                 .reduce(0, Integer::sum); // 计算总和
   ```

6. **`anyMatch`**

   - 检查是否有任意元素匹配条件。

   ```
   java复制代码List<Integer> list = List.of(1, 2, 3);
   boolean result = list.stream().anyMatch(x -> x > 2); // 是否有大于 2 的元素
   ```

7. **`allMatch`**

   - 检查是否所有元素都匹配条件。

   ```
   java复制代码List<Integer> list = List.of(1, 2, 3);
   boolean result = list.stream().allMatch(x -> x > 0); // 是否所有元素大于 0
   ```

8. **`noneMatch`**

   - 检查是否没有元素匹配条件。

   ```
   java复制代码List<Integer> list = List.of(1, 2, 3);
   boolean result = list.stream().noneMatch(x -> x > 5); // 是否没有元素大于 5
   ```

9. **`findFirst`**

   - 返回第一个元素。

   ```
   java复制代码List<String> list = List.of("a", "b", "c");
   String first = list.stream().findFirst().orElse("default");
   ```

10. **`findAny`**

    - 返回任意一个元素（并行流中表现不确定）。

    ```
    java复制代码List<String> list = List.of("a", "b", "c");
    String any = list.stream().findAny().orElse("default");
    ```

11. **`max`**

    - 返回最大值。

    ```
    java复制代码List<Integer> list = List.of(1, 2, 3);
    int max = list.stream().max(Integer::compareTo).orElse(-1);
    ```

12. **`min`**

    - 返回最小值。

    ```
    java复制代码List<Integer> list = List.of(1, 2, 3);
    int min = list.stream().min(Integer::compareTo).orElse(-1);
    ```



## String

### 方法

#### 字符串长度

1. length()

   返回字符串中字符的个数。

   ```java
   String s = "Hello";
   int len = s.length();  // len 为 5
   ```

2. charAt(int index)

   返回指定索引处的字符（索引从 0 开始）。

   ```java
   char c = s.charAt(1);  // 返回 'e'
   ```

3. toCharArray()

   将字符串转换为字符数组。

   ```java
   char[] chars = s.toCharArray();
   // chars = ['H', 'e', 'l', 'l', 'o']
   ```

#### 字符串替换

1. concat(String str)

   连接两个字符串，相当于用 + 运算符。

   ```java
   String s1 = "Hello", s2 = "World";
   String joined = s1.concat(" ").concat(s2);  // 结果 "Hello World"
   ```

2. **replace(char oldChar, char newChar)** 和 **replace(CharSequence target, CharSequence replacement)**

   替换所有匹配的旧字符或字符序列为新字符或字符序列。

   ```java
   String original = "banana";
   String replaced = original.replace('a', 'o');  // 输出 "bonono"
   
   String original = "I like Java. Java is powerful.";
   String replaced = original.replace("Java", "Python");  // "I like Python. Python is powerful."。
   ```

   

#### 大小写和空白处理

1. toLowerCase() 和 toUpperCase()

2. trim()

   去除字符串首尾的空白字符。

   ```java
   String padded = "  hello  ";
   System.out.println(padded.trim());  // "hello"
   ```

#### 其他常用方法

1. isEmpty()

   如果字符串长度为0，返回true

   ```java
   "".isEmpty();     // 返回 true
   "abc".isEmpty();  // 返回 false
   ```

2. format(String format, Object... args)

   按指定格式和参数生成格式化字符串。

   ```java
   String formatted = String.format("Hello, %s! You have %d new messages.", "Alice", 5);
   // 结果 "Hello, Alice! You have 5 new messages."
   ```

3. join( 分隔符 ,  元素 ) (Java 8+)

   用指定的分隔符将多个字符串连接成一个字符串。

   ```java
   String joined = String.join("-", "a", "b", "c");  // "a-b-c"
   ```

   









## 集合

单列集合：Collection

双列集合：Map   



### Map

#### 实现类

Java 中常见的 Map 实现类包括：

1. HashMap：最常用的 Map 实现类，允许 null 键和 null 值，不保证元素的顺序，并且不是线程安全的。
2. LinkedHashMap：类似于 HashMap，但是保证了元素的插入顺序，也允许 null 键和 null 值，不是线程安全的。
3. TreeMap：基于红黑树实现，可以对键进行排序，不允许 null 键，但允许 null 值，不是线程安全的。
4. ConcurrentHashMap：线程安全的 HashMap 实现，适用于并发环境，不允许 null 键和 null 值。
5. Hashtable：较旧的线程安全的 Map 实现，不允许 null 键和 null 值，通常不推荐使用，应该使用 ConcurrentHashMap 替代。

#### 方法

1. put(K key, V value)：将指定的键值对插入到 Map 中，如果键已经存在，则替换旧值并返回旧值，否则返回 null
```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
map.put("a", 3); // 键 "a" 已存在，替换旧值 1 为 3
```

2. get(Object key)：根据指定的键获取对应的值，如果找不到则返回 null
```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
map.put("b", 2);
Integer value = map.get("a"); // 返回 1
Integer notFound = map.get("c"); // 返回 null
```

3. remove(Object key)：删除指定键对应的键值对，并返回对应的值，如果找不到则返回 null
```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
map.put("b", 2);
Integer removedValue = map.remove("a"); // 返回 1
Integer notFound = map.remove("c"); // 返回 null
```

4. containsKey(Object key)：检查 Map 中是否包含指定的键，返回一个布尔值
```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
map.put("b", 2);
boolean containsA = map.containsKey("a"); // 返回 true
boolean containsC = map.containsKey("c"); // 返回 false
```

5. containsValue(Object value)：检查 Map 中是否包含指定的值，返回一个布尔值
```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
map.put("b", 2);
boolean containsOne = map.containsValue(1); // 返回 true
boolean containsThree = map.containsValue(3); // 返回 false
```

6. size()：返回 Map 中键值对的数量
```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
map.put("b", 2);
int size = map.size(); // 返回 2
```

7. isEmpty()：检查 Map 是否为空，返回一个布尔值
```java
Map<String, Integer> map = new HashMap<>();
boolean isEmpty = map.isEmpty(); // 返回 true
map.put("a", 1);
isEmpty = map.isEmpty(); // 返回 false
```

8. clear()：删除 Map 中的所有键值对
```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
map.put("b", 2);
map.clear();
boolean isEmpty = map.isEmpty(); // 返回 true
```

9. keySet()：
```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
map.put("b", 2);
Set<String> keys = map.keySet(); // 返回 ["a", "b"] 的 Set 视图
```

10. values()：
```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
map.put("b", 2);
Collection<Integer> values = map.values(); // 返回 [1, 2] 的 Collection 视图
```

11. entrySet()：
```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
map.put("b", 2);
Set<Map.Entry<String, Integer>> entries = map.entrySet(); // 返回 [("a", 1), ("b", 2)] 的 Set 视图
```

12. putAll(Map<? extends K, ? extends V> m)：
```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
map.put("b", 2);
Map<String, Integer> otherMap = new HashMap<>();
otherMap.put("c", 3);
otherMap.put("d", 4);
map.putAll(otherMap); // map 现在包含 {"a"=1, "b"=2, "c"=3, "d"=4}
```

13. getOrDefault(Object key, V defaultValue)：
```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
map.put("b", 2);
Integer value = map.getOrDefault("a", 0); // 返回 1
Integer defaultValue = map.getOrDefault("c", 0); // 返回 0
```

14. forEach(BiConsumer<? super K, ? super V> action)：
```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
map.put("b", 2);
map.forEach((key, value) -> System.out.println(key + "=" + value)); // 打印 "a=1" 和 "b=2"
```

15. replaceAll(BiFunction<? super K, ? super V, ? extends V> function)：
```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
map.put("b", 2);
map.replaceAll((key, value) -> value * 10); // map 现在包含 {"a"=10, "b"=20}
```

16. putIfAbsent(K key, V value)：
```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
Integer oldValue = map.putIfAbsent("a", 2); // 返回 1，map 不变
Integer nullValue = map.putIfAbsent("b", 2); // 返回 null，map 现在包含 {"a"=1, "b"=2}
```

17. remove(Object key, Object value)：
```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
map.put("b", 2);
boolean removed = map.remove("a", 1); // 返回 true，map 现在包含 {"b"=2}
boolean notRemoved = map.remove("b", 1); // 返回 false，map 不变
```

18. replace(K key, V oldValue, V newValue)：
```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
map.put("b", 2);
boolean replaced = map.replace("a", 1, 10); // 返回 true，map 现在包含 {"a"=10, "b"=2}
boolean notReplaced = map.replace("b", 1, 20); // 返回 false，map 不变
```

19. replace(K key, V value)：
```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
Integer replacedValue = map.replace("a", 10); // 返回 1，map 现在包含 {"a"=10}
Integer nullValue = map.replace("b", 20); // 返回 null，map 不变
```

20. computeIfAbsent(K key, Function<? super K, ? extends V> mappingFunction)：
```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
Integer value = map.computeIfAbsent("a", k -> 10); // 返回 1，map 不变
Integer newValue = map.computeIfAbsent("b", k -> 20); // 返回 20，map 现在包含 {"a"=1, "b"=20}
```

21. computeIfPresent(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction)：
```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
Integer value = map.computeIfPresent("a", (k, v) -> v * 10); // 返回 10，map 现在包含 {"a"=10}
Integer nullValue = map.computeIfPresent("b", (k, v) -> 20); // 返回 null，map 不变
```

22. compute(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction)：
```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
Integer value = map.compute("a", (k, v) -> (v == null) ? 10 : v * 10); // 返回 10，map 现在包含 {"a"=10}
Integer newValue = map.compute("b", (k, v) -> (v == null) ? 20 : v * 10); // 返回 20，map 现在包含 {"a"=10, "b"=20}
```

23. merge(K key, V value, BiFunction<? super V, ? super V, ? extends V> remappingFunction)：
```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
Integer value = map.merge("a", 10, (oldValue, newValue) -> oldValue + newValue); // 返回 11，map 现在包含 {"a"=11}
Integer newValue = map.merge("b", 20, (oldValue, newValue) -> oldValue + newValue); // 返回 20，map 现在包含 {"a"=11, "b"=20}
```



### List

#### 实现类

ArrayList 和 LinkedList 



#### 不可变列表

##### List.of

创建的列表是 **完全不可变的**，即无法修改内容（如添加、删除、或更改元素）

```java
List<String> l1 = List.of("a", "b", "c");
l1.add("d"); // 抛出 UnsupportedOperationException
```



##### Array.asList()

创建的列表是 **固定大小的**，即不能增加或减少元素，但可以修改现有元素

Arrays.asList() 是 Java 中 Arrays 类提供的一个静态工厂方法，用于将一个数组或者多个元素转换为一个固定大小的 List 集合。

```java
String[] array = {"apple", "banana", "orange"};
List<String> list = Arrays.asList(array);

List<String> l2 = Arrays.asList("a", "b", "c");
l2.set(0, "x"); // 可以修改现有元素
l2.add("d");    // 抛出 UnsupportedOperationException
```

通过 Arrays.asList() 方法返回的 List 集合有以下特点：

1. 固定大小：返回的 List 是一个固定大小的集合，不能增加或删除元素，否则会抛出 UnsupportedOperationException 异常。
2. backed by original array：返回的 List 是原始数组的一个视图，对 List 的任何修改都会反映到原始数组上。
3. 不支持基本数据类型：Arrays.asList() 方法不支持传入基本数据类型的数组，如 int[]、double[] 等，需要使用包装类型的数组，如 Integer[]、Double[] 等。

如果需要一个可变大小的 List，可以将 Arrays.asList() 返回的 List 作为构造函数参数创建一个新的 ArrayList 或 LinkedList：

```java
List<String> mutableList = new ArrayList<>(Arrays.asList("apple", "banana", "orange"));
```

```java
String[] array = {"apple", "banana", "orange"};
List<String> list = Arrays.stream(array).collect(Collectors.toList());

// 依然是可变长度的
list.add("grape");
System.out.println(list); // 输出：[apple, banana, orange, grape]
```

toList()不可变（java16引入）

```java
String[] arr = {"apple", "banana", "orange", "grape"};
List<String> li = Arrays.stream(arr).toList();  // li不可变
```



#### 方法

1. `boolean add(E element)`: 在 List 的末尾添加指定的元素。

```java
List<String> list = new ArrayList<>();
list.add("apple");
list.add("banana");
System.out.println(list); // 输出: [apple, banana]
```

2. `void add(int index, E element)`: 在 List 的指定位置插入指定的元素。

```java
List<String> list = new ArrayList<>();
list.add("apple");
list.add("banana");
list.add(1, "orange");
System.out.println(list); // 输出: [apple, orange, banana]
```

3. `boolean addAll(Collection<? extends E> c)`: 将指定集合中的所有元素添加到 List 的末尾。

```java
List<String> list = new ArrayList<>();
list.add("apple");
list.add("banana");
List<String> newList = Arrays.asList("orange", "pear");
list.addAll(newList);
System.out.println(list); // 输出: [apple, banana, orange, pear]
```

4. `boolean addAll(int index, Collection<? extends E> c)`: 将指定集合中的所有元素插入到 List 的指定位置。

```java
List<String> list = new ArrayList<>();
list.add("apple");
list.add("banana");
List<String> newList = Arrays.asList("orange", "pear");
list.addAll(1, newList);
System.out.println(list); // 输出: [apple, orange, pear, banana]
```

5. `void clear()`: 从 List 中删除所有元素。

```java
List<String> list = new ArrayList<>();
list.add("apple");
list.add("banana");
list.clear();
System.out.println(list); // 输出: []
```

6. `boolean contains(Object o)`: 如果 List 包含指定的元素,则返回 true。

```java
List<String> list = new ArrayList<>();
list.add("apple");
list.add("banana");
System.out.println(list.contains("apple")); // 输出: true
System.out.println(list.contains("orange")); // 输出: false
```

7. `E get(int index)`: 返回 List 中指定位置的元素。

```java
List<String> list = new ArrayList<>();
list.add("apple");
list.add("banana");
System.out.println(list.get(0)); // 输出: apple
System.out.println(list.get(1)); // 输出: banana
```

8. `int indexOf(Object o)`: 返回 List 中第一次出现指定元素的索引,如果 List 不包含该元素,则返回 -1。

```java
List<String> list = new ArrayList<>();
list.add("apple");
list.add("banana");
list.add("apple");
System.out.println(list.indexOf("apple")); // 输出: 0
System.out.println(list.indexOf("orange")); // 输出: -1
```

9. `boolean isEmpty()`: 如果 List 不包含元素,则返回 true。

```java
List<String> list = new ArrayList<>();
System.out.println(list.isEmpty()); // 输出: true
list.add("apple");
System.out.println(list.isEmpty()); // 输出: false
```

10. `Iterator<E> iterator()`: 返回按适当顺序在 List 的元素上进行迭代的迭代器。

```java
List<String> list = new ArrayList<>();
list.add("apple");
list.add("banana");
Iterator<String> iterator = list.iterator();
while (iterator.hasNext()) {
    String fruit = iterator.next();
    System.out.println(fruit);
}
// 输出:
// apple
// banana
```

11. `int lastIndexOf(Object o)`: 返回 List 中最后一次出现指定元素的索引,如果 List 不包含该元素,则返回 -1。

```java
List<String> list = new ArrayList<>();
list.add("apple");
list.add("banana");
list.add("apple");
System.out.println(list.lastIndexOf("apple")); // 输出: 2
System.out.println(list.lastIndexOf("orange")); // 输出: -1
```

12. `ListIterator<E> listIterator()`: 返回 List 中的元素的列表迭代器(按适当顺序)。

```java
List<String> list = new ArrayList<>();
list.add("apple");
list.add("banana");
ListIterator<String> listIterator = list.listIterator();
while (listIterator.hasNext()) {
    String fruit = listIterator.next();
    System.out.println(fruit);
}
// 输出:
// apple
// banana
```

13. `E remove(int index)`: 删除 List 中指定位置的元素。

```java
List<String> list = new ArrayList<>();
list.add("apple");
list.add("banana");
list.remove(0);
System.out.println(list); // 输出: [banana]
```

14. `boolean remove(Object o)`: 从 List 中删除第一次出现的指定元素(如果存在)。

```java
List<String> list = new ArrayList<>();
list.add("apple");
list.add("banana");
list.remove("apple");
System.out.println(list); // 输出: [banana]
```

15. `boolean removeAll(Collection<?> c)`: 从 List 中删除指定集合中包含的所有元素。

```java
List<String> list = new ArrayList<>();
list.add("apple");
list.add("banana");
list.add("orange");
List<String> removeList = Arrays.asList("apple", "orange");
list.removeAll(removeList);
System.out.println(list); // 输出: [banana]
```

16. `void replaceAll(UnaryOperator<E> operator)`: 将 List 中的每个元素替换为对该元素应用运算符的结果。

```java
List<String> list = new ArrayList<>();
list.add("apple");
list.add("banana");
list.replaceAll(String::toUpperCase);
System.out.println(list); // 输出: [APPLE, BANANA]
```

17. `boolean retainAll(Collection<?> c)`: 仅保留 List 中包含在指定集合中的元素。

```java
List<String> list = new ArrayList<>();
list.add("apple");
list.add("banana");
list.add("orange");
List<String> retainList = Arrays.asList("apple", "orange");
list.retainAll(retainList);
System.out.println(list); // 输出: [apple, orange]
```

18. `E set(int index, E element)`: 用指定的元素替换 List 中指定位置的元素。

```java
List<String> list = new ArrayList<>();
list.add("apple");
list.add("banana");
list.set(1, "orange");
System.out.println(list); // 输出: [apple, orange]
```

19. `void sort(Comparator<? super E> c)`: 根据指定的比较器引发的顺序对 List 中的元素进行排序。

```java
List<String> list = new ArrayList<>();
list.add("banana");
list.add("apple");
list.add("orange");
list.sort(String::compareTo);
System.out.println(list); // 输出: [apple, banana, orange]
```

20. `List<E> subList(int fromIndex, int toIndex)`: 返回 List 中指定的 fromIndex(包含)和 toIndex(不包含)之间的部分视图。

```java
List<String> list = new ArrayList<>();
list.add("apple");
list.add("banana");
list.add("orange");
List<String> subList = list.subList(1, 3);
System.out.println(subList); // 输出: [banana, orange]
```



### Set

#### 不可变Set集合



```java
Set<String> a = Set.of("a", "b", "c");
```



## 接口

Java接口是一系列方法的声明，是一些方法特征的集合，一个接口只有方法的特征没有方法的实现，因此这些方法可以在不同的地方被不同的类实现，而这些实现可以具有不同的行为（功能）。

接口可以理解为一种特殊的类型，它可以包含**常量**（`public static final` 默认修饰）和**方法的定义**，包括**抽象方法**、**默认方法**（`default`）、**静态方法**（`static`）以及**私有方法**（`private`，Java 9 及之后支持）。

接口是解决 Java 无法使用多继承的一种手段，因为一个类可以实现多个接口，而 Java 的类只能继承一个父类。但接口在实际中的主要作用是**制定标准**，为实现类提供一组必须实现的规范或行为。

在 Java 8 之前，接口可以被理解为**100% 抽象类**，因为接口中的方法必须全部是抽象方法，且不能包含任何实现。然而，从 Java 8 开始，接口可以包含**默认方法**和**静态方法**，因此接口已不再是严格意义上的“100% 抽象类”。

- 接口中的方法必须是公开的，因为接口的目的是对外提供功能。
- 接口中的方法默认是抽象方法，必须由实现类实现（除非是 `default` 或 `static` 方法）
- 接口本身无法创建对象，但它的实现类可以创建对象。

**示例**

```java
public interface Flyable {
    void fly();
}

public interface Swimmable {
    void swim();
}

public class Bird implements Flyable, Swimmable {
    @Override
    public void fly() {
        System.out.println("Bird is flying.");
    }

    @Override
    public void swim() {
        System.out.println("Bird is swimming.");
    }
}
```

接口定义

```java
// 定义一个接口
public interface AdvancedInterface {

    // 1. 常量（public static final 自动添加）
    int MAX_COUNT = 100; // 等价于 public static final int MAX_COUNT = 100;

    // 2. 抽象方法（必须由实现类实现）默认是 public abstract
    void abstractMethod1();
    // 等同于 public void abstractMethod1();
    // 等同于 public abstract void abstractMethod1();
    String abstractMethod2(String input);

    // 3. 默认方法（default）：可以有方法体，供实现类直接使用或重写
    default void defaultMethod() {
        System.out.println("This is a default method.");
    }

    default void anotherDefaultMethod() {
        System.out.println("Another default method.");
    }

    // 4. 静态方法（static）：直接通过接口名调用，不能被实现类重写
    static void staticMethod() {
        System.out.println("This is a static method.");
    }

    static void anotherStaticMethod() {
        System.out.println("Another static method.");
    }
}
```

实现接口

```java
// 实现接口的类
public class AdvancedInterfaceImpl implements AdvancedInterface {

    // 必须实现抽象方法
    @Override
    public void abstractMethod1() {
        System.out.println("Implementation of abstractMethod1.");
    }

    @Override
    public String abstractMethod2(String input) {
        return "Input received: " + input;
    }

    // 可以选择重写默认方法（这里重写了 defaultMethod）
    @Override
    public void defaultMethod() {
        System.out.println("Overridden default method in AdvancedInterfaceImpl.");
    }

    // 默认方法 anotherDefaultMethod 没有重写，直接继承接口的实现
}
```



## 工具类

### `java.util.uuid`

专门用于表示和生成 **通用唯一标识符 (UUID)** 的类。UUID 是一种 128 位的标识符，通常用于确保唯一性，如数据库主键、分布式系统中的对象标识符等。

Java 的 `UUID.randomUUID()` 基于随机数。

```java
System.out.println(UUID.randomUUID().toString());
// 1865dcaa-6349-48f2-8878-b86acb1b5ca2
System.out.println(UUID.randomUUID().toString().replaceAll("-", ""));
// 80c05a54cc9e49b2bf4cb439e4ae9833
```


| 方法                             | 描述                                            |
| -------------------------------- | ----------------------------------------------- |
| `randomUUID()`                   | 生成一个随机 UUID（版本 4）。                   |
| `fromString(String name)`        | 将字符串解析为 UUID。                           |
| `nameUUIDFromBytes(byte[] name)` | 根据字节数组生成一个基于名称的 UUID（版本 3）。 |
| `getMostSignificantBits()`       | 获取 UUID 的高 64 位。                          |
| `getLeastSignificantBits()`      | 获取 UUID 的低 64 位。                          |
| `version()`                      | 获取 UUID 的版本号（1、3、4、5）。              |
| `variant()`                      | 获取 UUID 的变体号（通常是 2）。                |
| `toString()`                     | 将 UUID 转换为标准格式的字符串表示。            |
| `equals(Object obj)`             | 比较两个 UUID 是否相等。                        |
| `hashCode()`                     | 获取 UUID 的哈希值。                            |
| `compareTo(UUID val)`            | 比较两个 UUID 的大小。                          |

