# JAVA

## 类

### 定义类

一个java文件中最多只能有一个`public`顶层类，并且这个`public`类的名字必须和文件名相同

在 Java 中，一个 `.java` 文件中可以定义 **多个非 `public` 的类**。

在 Java 中，**顶级类（Top-level class）** 只能使用以下两个访问修饰符之一：

- `public`
- **无修饰符（即默认的包私有，package-private）**
- 不能是**`private` 或 `protected`**。



### 多态

#### 常见的多态用法

父类变量指向子类对象

```java
Animal a1 = new Dog();
Animal a2 = new Cat();
```

未来的代码需要**处理很多不同的子类对象**，你就会发现用 `Parent` 作为统一类型**非常方便、非常强大**。

可以把它们放进一个列表里

```java
List<Animal> animals = new ArrayList<>();
animals.add(new Dog());
animals.add(new Cat());
```

`Animal a1 = new Dog();`

| 部分        | 名称             | 含义                                           |
| ----------- | ---------------- | ---------------------------------------------- |
| `Animal`    | **引用类型**     | **变量的声明类型**，也叫“编译时类型”           |
| `a1`        | 变量名           | 指向对象的引用变量                             |
| `new Dog()` | **实际对象类型** | 创建的真实对象，也叫“运行时类型”或“实例化对象” |

<span style="color:red">直接用子类类型，可以使用子类特有的方法</span>



#### 向下转型

为什么 `Child p = new Parent();` 是错误的？

这是 **向下转型（Downcasting）**，但尝试直接用父类对象赋值给子类引用会 **编译时报错** 或 **运行时抛出异常**，因为：

- 不是所有 `Parent` 类型的对象都是 `Child` 类型的实例。
- 编译器无法保证安全性。

```java
Child p = new Parent();
```

```java
Parent p = new Child();  // 向上转型
Child c = (Child) p;  // 向下转型
```



#### 成员变量隐藏

Java 字段不是多态的

- **方法是多态的**：调用哪个方法由**实际对象类型**决定（运行时绑定）。

- **字段不是多态的**：访问哪个字段由**引用变量的类型**决定（编译时绑定）。

```java
class Parent {
    String name = "parent";
}

class Child extends Parent {
    String name = "child";
}

public class Test1 {
    public static void main(String[] args) {
        Parent p = new Child();
        System.out.println(p.name);  // "parent"
    }
}
```



### instanceof

判断一个对象是否是某个类的实例，**或者是其子类/实现类的实例**。

```java
class Animal {}
class Dog extends Animal {}

public class Test {
    public static void main(String[] args) {
        Animal a = new Dog();

        System.out.println(a instanceof Dog);    // true
        System.out.println(a instanceof Animal); // true
        System.out.println(a instanceof Object); // true
        System.out.println(a instanceof String); // 编译错误 ❌
    }
}
```







## 修饰符

在 Java 中，方法不能被定义在另一个方法的内部

Java 的方法结构不支持嵌套方法声明（方法内部定义方法）。

### 访问修饰符/权限修饰符

**public** ：对所有类可见。使用对象：类、接口、变量、方法。

使用 `public` 修饰的类、方法或变量可以被 **所有类** 访问，无论它们是否在同一个包中。

需要 `public` 修饰的类必须与文件名相同

<span style="color:red">`.java`文件中只能包含一个public类，并且该类名称必须与文件名相同</span>



**protected** : 对同一包内的类和所有子类可见。使用对象：变量、方法。 注意：不能修饰类（外部类）。
使用 `protected` 修饰的成员可以被

- 同一包内的类 访问，
- 不同包中的子类 访问。



**default** (即默认，什么也不写）: 在同一包内可见，不使用任何修饰符。使用对象：类、接口、变量、方法。

没有显式声明的修饰符时，默认使用 **包访问权限**（`default`）。只有 **同一包内的类** 可以访问，包外类无法访问。



**private** : 在同一类内可见。使用对象：变量、方法。 注意：不能修饰类（外部类）。

使用 `private` 修饰的成员只能在 **当前类** 内部访问，其他类（即使是同一个包内的类）也无法访问。







### 非访问控制修饰符

### abstract

#### 抽象类

如果一个类中没有包含足够的信息来描绘一个具体的对象，这样的类就是抽象类。

<span style="color:red">抽象类不能实例化对象</span>，类的其它功能依然存在，成员变量、成员方法和构造方法的访问方式和普通类一样。

##### 定义

要定义一个抽象类，需要使用`abstract`关键字。

```java
public abstract class Animal {
    protected String name;

    public Animal(String name) {
        this.name = name;
    }

    public abstract void makeSound();
}
```

- 可以包含抽象方法和具体方法：抽象类不要求所有方法都是抽象的，可以混合使用。

- 可以有成员变量：抽象类可以包含普通的成员变量（字段）。

- 可以有构造方法：虽然抽象类不能被实例化，但它可以定义构造方法，用于子类在初始化时调用。

- 可以有静态方法：抽象类中允许存在静态方法，但静态方法不能是抽象的。

##### 限制

1. 类声明为 abstract 后，不能直接实例化。
2. 抽象方法不能有方法体。接口可以包含抽象方法。
3. 非抽象类不能包含抽象方法。
4. 抽象方法不能是 `static`、`final` 或 `private`

##### 抽象方法

抽象类中可以包含抽象方法，这些方法没有具体实现，只有方法签名。子类必须重写这些抽象方法，除非子类也是抽象类。

没有方法体（即没有大括号 `{}` 和实现代码）

```java
public abstract void makeSound();
```

##### 继承抽象类

其他类可以通过`extends`关键字继承抽象类，并实现所有的抽象方法。

特点：

- <span style="color:red">**必须在抽象类中定义**：只有抽象类才能包含抽象方法。</span>
- **子类必须实现**：如果一个类继承了包含抽象方法的抽象类，那么该类必须实现所有的抽象方法，除非该类本身也是抽象类。
- 抽象方法不能是 `static`、`final` 或 `private`：这些修饰符与抽象方法的特性冲突。
  - `static`：静态方法属于类，而抽象方法依赖于实例。
  - `final`：`final` 方法不能被重写，而抽象方法需要被实现。
  - `private`：`private` 方法无法被子类访问，而抽象方法需要被子类实现。

```java
public class Cat extends Animal {
    public Cat(String name) {
        super(name);
    }

    @Override
    public void makeSound() {
        System.out.println("Meow!");
    }
}
```

##### 抽象类的使用

抽象类不能被直接实例化，但可以通过多态的方式使用其子类的实例。

```java
Animal animal = new Cat("Tom");
animal.makeSound(); // 输出: Meow!
animal.sleep(); // 输出: Tom is sleeping.
```

##### 抽象类与接口

**相同点**

- 接口和抽象类都可以包含抽象方法，定义某种行为规范。
- 它们都不能直接实例化，必须通过具体的类来实现或继承。子类可以通过抽象类或接口实现多态行为。

**区别**

- 一个类只能继承一个抽象类，但可以实现多个接口。
- 抽象类可以有构造函数，而接口不能。
- 接口中的方法默认是 `public`；不能用 `private/protected`。抽象类中的方法和变量可以使用各种访问修饰符（`private`, `protected`, `public`）。

**使用抽象类而不是接口的情况抽象方法**

- 需要在类中定义成员变量，抽象类可以有非静态、非常量的成员变量，而接口中的变量默认都是`public static final`（常量）。
- 需要提供方法的默认实现
- 需要使用非public访问修饰符控制某些方法
- 想要实现代码重用，共享一些公共方法的实现
- 需要定义构造函数
- 类之间有明确的"is-a"关系（继承关系）



### 接口

所有抽象方法默认是 `public abstract`，如果尝试使用其他访问修饰符会报错

 Java 8+ 支持接口中的 `default` 和 `static` 方法



### static

**属于类，而不是属于类的实例**

#### 静态变量

静态变量是用 `static` 关键字修饰的成员变量（字段），它属于类本身，而不是某个特定的对象。<span style="color:red">无论创建多少个类的实例，静态变量的值都是共享的。</span>

特点：

- <span style="color:red">`static` 只能修饰**类的成员变量**，不能修饰局部变量。</span>

- **属于类本身**：静态变量存储在方法区中，且在类加载时初始化。
- **共享性**：所有该类的实例共享同一个静态变量。
- **生命周期长**：静态变量从类加载到程序结束的整个生命周期都存在。
- 可以通过类名直接访问，也可以通过对象访问（不推荐，因为没有实例绑定关系）。
- 适合存储与整个类相关的公共数据。

```java
public class StaticVariableExample {
    static int staticCount = 0; // 静态变量，所有实例共享

    int instanceCount = 0; // 普通实例变量

    public StaticVariableExample() {
        staticCount++; // 每创建一个对象，静态变量加 1
        instanceCount++; // 每个对象单独维护自己的实例变量
    }

    public static void main(String[] args) {
        StaticVariableExample obj1 = new StaticVariableExample();
        StaticVariableExample obj2 = new StaticVariableExample();

        System.out.println("Static Count: " + StaticVariableExample.staticCount); // 2
        System.out.println("Instance Count (obj1): " + obj1.instanceCount); // 1
        System.out.println("Instance Count (obj2): " + obj2.instanceCount); // 1
        System.out.println("Static Count (via obj1): " + obj1.staticCount); // 2  不推荐
    }
}
```



#### 静态方法

在 Java 中，**静态方法**属于类，不依赖于类的实例；而**非静态方法**属于类的实例（对象），必须通过实例化对象来调用。

特点：

- **不依赖对象**：可以通过类名直接调用。
- **不能访问非静态成员**：因为静态方法在类加载时就存在，而非静态成员只有在对象创建后才存在。非静态成员依赖于实例，而静态方法不依赖于实例。
- **常用于工具类方法**：如 `Math` 类中的 `Math.sqrt()`，因为不需要保存状态。
- 静态方法不能使用 `this` 和 `super` 关键字。
- 在 `static` 方法中，**没有 this 对象**，也就**无法调用实例方法**
- **静态方法**是可以通过类的实例来调用的，但 **不推荐** 这样做。
- <span style="color:blue">静态方法（`static` methods）不能被子类或实现类重写（override）</span>

```java
public class eTest {
    public static void main(String[] args) {
        say(); // 直接调用静态方法
    }

    public static void say() {
        System.out.println("hello");
    }
}
```

```java
public class eTest {
    public static void main(String[] args) {
        eTest test = new eTest(); // 创建 eTest 类的实例
        test.say(); // 通过实例调用非静态方法
    }

    public void say() {
        System.out.println("hello");
    }
}
```

```java
class Parent {
    static void sayHello() {
        System.out.println("Hello from Parent");
    }
}

class Child extends Parent {
    static void sayHello() {
        System.out.println("Hello from Child");
    }
}

public class Test {
    public static void main(String[] args) {
        Parent p = new Child();
        p.sayHello(); // 输出：Hello from Parent ❗️不是 Child
    }
}

// 尽管 Child 也定义了 sayHello()，但由于方法是静态的，调用的是 引用变量类型 的方法（Parent），而不是对象实例的实际类型（Child）。
// 所以输出的是 Parent 的方法。
```



#### 静态内部类

静态类指的是用 `static` 修饰的 **嵌套类**（即定义在另一个类内部的静态类）。<span style="color:red">在 Java 中，只有内部类可以声明为静态类，顶层类不能是静态的。</span>

静态内部类是可以实例化的，但需要通过外部类名来引用。

静态内部类可以独立于外部类的实例存在。如果内部类的行为和外部类的实例无关，就可以使用静态内部类。

```java
public class OuterClass {
    // 静态内部类
    static class StaticNestedClass {
        void display() {
            System.out.println("This is a static nested class.");
        }
    }

    public static void main(String[] args) {
        // 静态内部类可以直接实例化，通过外部类名引用
        OuterClass.StaticNestedClass nestedObj = new OuterClass.StaticNestedClass();
        nestedObj.display();
    }
}
```



#### 静态代码块

##### 底层原理

静态代码块在类的字节码被加载到JVM时执行，并且在整个JVM生命周期中只会执行一次。JVM在加载类时，先加载类的字节码，接着在类中查找静态成员和静态代码块。所有的静态代码块会按照它们在类中定义的顺序依次执行。

##### 执行时机

<span style="color:red">静态代码块是在**类初始化阶段**执行的。类的初始化发生在**主动引用**</span>

| 主动引用方式                      | 描述                                     |
| --------------------------------- | ---------------------------------------- |
| 1. 创建类的实例                   | `new MyClass()`                          |
| 2. 访问类的静态变量（非 `final`） | `MyClass.staticVar`                      |
| 3. 调用类的静态方法               | `MyClass.staticMethod()`                 |
| 4. 使用反射操作该类               | `Class.forName("com.example.MyClass")`   |
| 5. 子类初始化时，父类也会被初始化 | `new SubClass()` 会先初始化 `SuperClass` |
| 6. JVM 启动时指定的主类           | 启动类也会被初始化                       |

##### 执行顺序

在 Java 中，**静态变量和静态代码块按照它们在类中出现的顺序执行**。

```java
public class ATest {
    public static void main(String[] args) {
        A a = new A();
        System.out.println(A.m);  // 300
    }
}

class A {
    static int m = 100;
    static {
        System.out.println("A类静态代码块初始化");
        m = 300;
    }
    
    public A() {
        System.out.println("A类构造函数初始化");
    }
}
```

**执行顺序：**

1. `static int m = 100` → 初始化静态变量 `m` 为 100。
2. 执行静态代码块 → 打印 `"A类静态代码块初始化"`，然后将 `m` 设为 300。
3. 构造函数执行 → 打印 `"A类构造函数初始化"`。
4. `System.out.println(A.m)` → 输出 `300`。

```java
public class ATest {
    public static void main(String[] args) {
        A a = new A();
        System.out.println(A.m);  // 100
    }
}

class A {
    static {
        System.out.println("A类静态代码块初始化");
        m = 300;
    }

    static int m = 100;
 
    public A() {
        System.out.println("A类构造函数初始化");
    }
}
```

**执行顺序：**

1. 执行静态代码块 → 打印 `"A类静态代码块初始化"`，将 `m` 设为 300。
2. 再执行 `static int m = 100` → 把 `m` 重新赋值为 100（**覆盖了前面设置的 300**）。
3. 构造函数执行 → 打印 `"A类构造函数初始化"`。
4. `System.out.println(A.m)` → 输出 `100`。





### final

#### 变量

表示该变量一旦被初始化，就不能再次被赋值。

1. 修饰局部变量

   `final` 修饰的局部变量**必须在声明时或第一次使用之前进行初始化**，否则编译会报错。

   一旦赋值后，变量的值不能改变。

   常用于方法内部的常量，比如临时计算值或者传递参数时，确保该变量在方法内不会被意外修改。

   ```java
   public void method() {
       final int x = 10; // x 被赋值为 10
       // x = 20; // 编译错误，final 变量不能被重新赋值
   }
   ```

2. 修饰成员变量

   - 在声明时直接赋值

     ```java
     class Example {
         private final int x = 10; // x 必须被初始化
     }
     ```

   - 在构造方法中赋值

     ```java
     class Example {
         private final int x;
     
         public Example(int value) {
             this.x = value; // 必须在构造方法中初始化
         }
     }
     ```

   - 修饰静态变量 (`static final`)

     `final` 和 `static` 修饰的变量称为**常量**，必须在声明时或静态初始化块中进行赋值。

     ```java
     class Example {
         public static final double PI = 3.14159; // 声明时直接赋值
         
         public static final int MAX_VALUE;
     
         static {
             MAX_VALUE = 100; // 静态初始化块赋值
         }
     }
     ```
     
#### **final 修饰方法**

当 `final` 修饰方法时，表示该方法不能被子类重写（override）。

```java
class Parent {
    public final void display() {
        System.out.println("This is a final method.");
    }
}

class Child extends Parent {
    // @Override
    // public void display() {  // 编译错误，不能重写 final 方法
    //     System.out.println("Attempt to override final method.");
    // }
}
```

`final` 方法可以被重载（overload），因为方法重载与方法重写是两回事。

#### final 修饰类

当 `final` 修饰类时，表示该类不能被继承。

`final` 修饰的类通常是设计为不可更改的类（比如工具类或安全类），防止子类破坏其行为。

```java
final class FinalClass {
    public void display() {
        System.out.println("This is a final class.");
    }
}

// class SubClass extends FinalClass {  // 编译错误，不能继承 final 类
// }
```

`final` 类不能有子类，因此类中的所有 `final` 方法也不能被重写。

#### 其他

`final` 修饰引用类型时，表示引用本身不可变，但引用指向的对象内容可以改变。

```java
class Example {
    public static void main(String[] args) {
        final StringBuilder sb = new StringBuilder("Hello");
        sb.append(", World!"); // 合法，修改对象内容
        // sb = new StringBuilder("New"); // 编译错误，不能重新分配引用
        System.out.println(sb);
    }
}
```



### synchronized

synchronized 是 Java 语言的一个关键字，它允许多个线程同时访问共享的资源，以避免多线程编程中的竞争条件和死锁问题。

synchronized可以用来给对象或者方法进行加锁，当对某个对象或者代码块加锁时，同时就只能有一个线程去执行。这种就是互斥关系，被加锁的区域称为临界区，而里面的资源就是临界资源。当一个线程进入临界区的时候，另一个线程就必须等待。



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



## 对象类型

### 时间

#### LocalDateTime 

`LocalDateTime` 是 Java 8 引入的日期时间 API 的一部分，位于 `java.time` 包中。它表示没有时区信息的日期和时间

比如 `2025-04-04T23:18:34.266360600`。

```java
// 获取当前日期和时间
LocalDateTime now = LocalDateTime.now();
```













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
3. 可以直接访问外部类的所有成员变量和方法，包括 `private` 修饰的成员
4. **必须通过外部类的实例来访问**，且通常需要使用外部类的类名来标识。

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

   成员内部类的实例化必须通过外部类的实例来完成
   
   ```java
   public class OuterClass {
       public class InnerClass {
           public void innerMethod() {
               System.out.println("这是成员内部类的方法");
           }
       }
   }
   
   class Main {
       public static void main(String[] args) {
           // 先创建外部类的实例
           OuterClass outerClass = new OuterClass();
           // 通过外部类的实例来创建内部类的实例
        OuterClass.InnerClass innerClass = outerClass.new InnerClass();
           // 调用内部类的方法
           innerClass.innerMethod(); // 这是成员内部类的方法
       }
   }
   ```
   
   ```java
   public class OuterClass {
       public class InnerClass {
           public void innerMethod() {
               System.out.println("这是成员内部类的方法");
           }
       }
   }
   
   class Main {
       public static void main(String[] args) {
           OuterClass.InnerClass innerClass = new OuterClass().new InnerClass();
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

<span style="color:red">静态内部类只能访问外部类中的静态变量和静态方法，如果要访问非静态的需要创建对象。</span>

创建静态内部类对象的格式： 外部类名.内部类名 对象名 = new 外部类名.内部类名();

**不需要外部类的实例，可以通过外部类的类名直接访问**。

```java
package InnerClassTest;

public class OuterClass2 {
    public int age = 25;
    public static String name = "John";

    public static class InnerClass2 {
        public void printName() {
            System.out.println("Name: " + name);
        }

        public static void say() {
            System.out.println("Hello World");
        }
    }
}

class OuterClass2Test {
    public static void main(String[] args) {
        OuterClass2.InnerClass2 innerClass2 = new OuterClass2.InnerClass2();
        innerClass2.printName();
        OuterClass2.InnerClass2.say();
    }
}

```



### 局部内部类

局部内部类是定义在一个方法、构造器或者代码块中的类，类似于局部变量，其作用域仅限于定义它的那个方法或代码块。

外界无法直接使用，需要在方法内部创建对象并使用。

该类可以直接访问外部类的成员，也可以访问方法内的局部变量。

**只能在定义它的方法或代码块中使用**，不能通过外部类类名访问。

局部内部类（`InnerClass3`）不能包含静态方法或静态变量。

不能有访问修饰符（如 `public`、`protected` 或 `private`），因为它的作用域仅限于方法或代码块。

```java
package InnerClassTest;

public class OuterClass3 {
    int a = 10;

    public void show() {
        class InnerClass3 {
            public void method1() {
                System.out.println("a = " + a);
                System.out.println("method1");
            }
        }
        InnerClass3 innerClass3 = new InnerClass3();
        // 调用局部内部类的方法
        innerClass3.method1();
    }
}

class OuterClass3Test {
    public static void main(String[] args) {
        OuterClass3 outerClass3 = new OuterClass3();
        outerClass3.show();
    }
}

```



### 匿名内部类

<span style="color:blue">匿名内部类就是一个“没有名字”的类，它在创建对象时临时定义，并且立刻实例化。</span>

匿名内部类是 Java 编程语言中一种特殊的类，它没有显式地定义类名，而是在创建对象时通过传递实现了某个接口或继承了某个类的代码块来定义类。通常，我们使用它来简化代码、减少类的数量和更高效地编写事件处理程序等。

<span style="color:blue">当你 **只需要使用一次某个类的子类**（比如重写一个方法），**又不想新建一个类文件时**，就可以使用匿名内部类。</span>

- **不能通过外部类类名访问，因为匿名内部类没有名字**。

- **必须实现或继承**：<span style="color:red">匿名内部类必须继承一个类或者实现一个接口。</span>可以继承抽象类或者具体类

- **直接创建对象**：匿名内部类会在定义的同时直接实例化对象。

- **只能有一个实例**：因为没有类名，无法重复实例化。

- 匿名内部类 **必须是某个类的子类**，**或者是某个接口的实现类**

- **可以重写多个方法**，也可以不重写方法（具体类）

  ```java
  abstract class Animal {
      abstract void eat();
      abstract void sleep();
  }
  
  public class Main {
      public static void main(String[] args) {
          Animal animal = new Animal() {
              @Override
              void eat() {
                  System.out.println("吃饭！");
              }
  
              @Override
              void sleep() {
                  System.out.println("睡觉！");
              }
          };
  
          animal.eat();
          animal.sleep();
      }
  }
  ```

- 可以在匿名内部类里定义**实例变量**，但不能定义**构造方法**，也不能起类名

  ```java
  abstract class Animal {
      abstract void speak();
  }
  
  public class Main {
      public static void main(String[] args) {
          Animal animal = new Animal() {
              int age = 3; // ✅ 可以定义变量
  
              @Override
              void speak() {
                  System.out.println("这只动物 " + age + " 岁了");
              }
          };
  
          animal.speak(); // 输出：这只动物 3 岁了
      }
  }
  ```

- 匿名内部类可以继承具体类但**不重写任何方法**

  ```java
  class HelloWorld {
      public void say() {
          System.out.println("Hello World!");
      }
  }
  
  public class Test {
      public static void main(String[] args) {
          HelloWorld hw = new HelloWorld() {
              // 没有重写任何方法
          };
          hw.say(); // 输出 Hello World!
      }
  }
  ```

  

| 特点               | 解释                                   |
| ------------------ | -------------------------------------- |
| 是一个类           | 本质上就是一个类，但没有名字           |
| 在定义时就实例化   | 你不能创建多个对象，因为没有类名可引用 |
| 常用于接口或抽象类 | 直接实现接口或重写抽象类方法，简洁方便 |
| 只能用一次         | 匿名内部类没有类名，不能重复使用       |

```java
new 类名或接口名() {
    重写方法;
};
```

```java
public interface Animal1 {
    public void eat();
}
```

```java
public class Demo1Test {
    public static void main(String[] args) {
        Animal1 animal = new Animal1() {
            @Override
            public void eat() {
                System.out.println("I am eating");
            }
        };
        animal.eat();
    }
}
```

```java
public class Demo1Test {
    public static void main(String[] args) {
        new Animal1() {
            @Override
            public void eat() {
                System.out.println("I am eating");
            }
        }.eat();
    }
}
```

#### 原理

**编译之后**，Java 编译器会自动为匿名内部类**生成一个真正的类名**

```java
Animal1 animal = new Animal1() {
    public void eat() { ... }
};
```

```java
class Demo1Test$1 extends Animal1 {
    public void eat() { ... }
}
```

```java
Animal1 animal = new Demo1Test$1(); // 你没写这个类名，是编译器起的
```



#### 示例

```java
Dog dog = new Dog() {
    @Override
    void bark() {
        System.out.println("我是匿名子类的狗！");
    }
};
```

```java
class MyDog extends Dog {
    @Override
    void bark() {
        System.out.println("我是匿名子类的狗！");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new MyDog(); // 用子类创建对象
        dog.bark(); // 输出：我是匿名子类的狗！
    }
}
```







## `@FunctionalInterface`

`@FunctionalInterface` 是 Java 8 引入的一个**注解**，用于标记一个接口是 **函数式接口（Functional Interface）**。

```java
@FunctionalInterface
public interface MyFunction {
    void execute();
}
```

使用 Lambda 表达式调用

```java
MyFunction func = () -> System.out.println("Hello Functional Interface!");
func.execute();
```

**编译器会强制检查接口是否符合函数式接口的规范（即只能有一个抽象方法）**，如果不符合就会报错。

**函数式接口** 是指 **只包含一个抽象方法** 的接口。

特征：

- **只能有一个抽象方法**
- 可以有多个默认方法（`default`）或静态方法（`static`）
- 可以有多个默认方法或静态方法（但只能有一个抽象方法）
- 可以被 **Lambda 表达式** 或 **方法引用** 替代使用

```java
Runnable r1 = new Runnable() {
    public void run() {
        System.out.println("Hello from anonymous inner class");
    }
};

// 简化
Runnable r2 = () -> System.out.println("Hello from lambda");
```









## Lambda

Lambda是jdk8中的一个语法糖，可以简化匿名内部类的书写。让我们不用关注是什么对象，而是更关注我们对数据进行了什么操作。

用来实现**函数式接口的简洁写法**

函数式接口：有且只有一个抽象方法的接口叫做函数式接口，接口上方可以加@FunctionalInterface注解

<span style="color:red">函数式接口 **必须是接口（`interface`）**，而**不是抽象类（`abstract class`）**。</span>

### 条件

1. 必须用于函数式接口

2. 接口必须是 SAM 接口：**SAM**：Single Abstract Method（单一抽象方法）。

   接口中可以包含：

   - 默认方法（`default`）
   - 静态方法（`static`）
   - 但只能有**一个抽象方法**。

### 与匿名内部类的区别

| 特性                       | 匿名内部类                                   | Lambda 表达式                                        |
| -------------------------- | -------------------------------------------- | ---------------------------------------------------- |
| 是什么                     | 创建一个**子类**或**接口的实现类**的匿名实例 | 实现一个**函数式接口**（只有一个抽象方法）的方法体   |
| 是否生成类文件             | ✅ 是，会生成一个匿名内部类的 `.class` 文件   | ✅ 是，但通常是由 JVM 优化生成的 `invokedynamic` 调用 |
| 是否可以有多个方法         | ✅ 可以实现多个接口方法（如果接口有多个）     | ❌ 只能实现一个方法（函数式接口）                     |
| 使用场景                   | 任何接口或类的实现                           | 只能用于函数式接口（如 Runnable, Comparator 等）     |
| 语法是否简洁               | ❌ 相对冗长                                   | ✅ 简洁                                               |
| 是否保留外部变量作用域信息 | ✅ 是（有自己的作用域，`this` 指向匿名类）    | ✅ 是（`this` 指向外围类）                            |

### 示例

```java
Thread t1 = new Thread(() -> System.out.println("hello world"));
```

等同于

```java
Thread t1 = new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("hello world");
    }
});
```



```java
@FunctionalInterface
public interface IGreeting {
    void sayHello();
}
```

```java
public class GreetTest1 {
    public static void main(String[] args) {
        IGreeting greet = () -> System.out.println("Hello World!");
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

   ```java
   List<Integer> l2 = list.stream().map(String::length).collect(Collectors.toUnmodifiableList());
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

   ```java
   List<String> list = List.of("a", "b", "c");
   list.stream()
       .forEach(System.out::println); // 打印每个元素
   ```

2. **`toArray`**

   - 将流转换为数组。

   ```java
   List<Integer> list = List.of(1, 2, 3);
   Integer[] array = list.stream()
                         .toArray(Integer[]::new); // 转换为数组
   ```

3. **`collect`**

   - 收集流的元素到集合或其他容器中。

   ```java
   List<String> list = List.of("a", "b", "c");
   List<String> result = list.stream()
                             .collect(Collectors.toList()); // 转换为列表
   ```

4. **`count`**

   - 返回流中的元素数量。

   ```java
   List<Integer> list = List.of(1, 2, 3, 4, 5);
   long count = list.stream().filter(x -> x > 2).count(); // 统计大于 2 的元素数量
   ```

5. **`reduce`**

   - 聚合操作，结合流的元素为一个值。

   ```java
   List<Integer> list = List.of(1, 2, 3, 4, 5);
   int sum = list.stream()
                 .reduce(0, Integer::sum); // 计算总和
   ```

6. **`anyMatch`**

   - 检查是否有任意元素匹配条件。

   ```java
   List<Integer> list = List.of(1, 2, 3);
   boolean result = list.stream().anyMatch(x -> x > 2); // 是否有大于 2 的元素
   ```

7. **`allMatch`**

   - 检查是否所有元素都匹配条件。

   ```java
   List<Integer> list = List.of(1, 2, 3);
   boolean result = list.stream().allMatch(x -> x > 0); // 是否所有元素大于 0
   ```

8. **`noneMatch`**

   - 检查是否没有元素匹配条件。

   ```java
   List<Integer> list = List.of(1, 2, 3);
   boolean result = list.stream().noneMatch(x -> x > 5); // 是否没有元素大于 5
   ```

9. **`findFirst`**

   - 返回第一个元素。

   ```java
   List<String> list = List.of("a", "b", "c");
   String first = list.stream().findFirst().orElse("default");
   ```

10. **`findAny`**

    - 返回任意一个元素（并行流中表现不确定）。

    ```java
    List<String> list = List.of("a", "b", "c");
    String any = list.stream().findAny().orElse("default");
    ```

11. **`max`**

    - 返回最大值。

    ```java
    List<Integer> list = List.of(1, 2, 3);
    int max = list.stream().max(Integer::compareTo).orElse(-1);
    ```

12. **`min`**

    - 返回最小值。

    ```java
    List<Integer> list = List.of(1, 2, 3);
    int min = list.stream().min(Integer::compareTo).orElse(-1);
    ```



### 收集器





### 使用

#### 遍历属性组合字符串

```java
public class eTest {
    public static void main(String[] args) {
        List<Map<String, Object>> list = new ArrayList<>();

        // 创建一个 Map 并添加键值对
        Map<String, Object> map = new HashMap<>();
        map.put("name", "John");
        map.put("age", 25);
        list.add(map);

        Map<String, Object> map1 = new HashMap<>();
        map1.put("name", "Jane");
        map1.put("age", 28);
        list.add(map1);

        String res = list.stream().map(model -> "姓名" + model.get("name") + "年龄" + model.get("age")).collect(Collectors.joining(";", "结果：", "end"));

        System.out.println(res);
    }
}
```

#### 数组使用stream流

使用 `Arrays.stream(array)` 将数组转换为流。

```java
String[] array = {"street1", "USA", "JinHua"};
Arrays.stream(array).forEach(System.out::println);
```







## Collectors

1. `Collectors.toList()`: 将 Stream 中的元素收集到一个 List 中。
    示例: `List<String> list = stream.collect(Collectors.toList());`

   ```java
   List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
   List<String> upperCaseNames = names.stream()
                                       .map(String::toUpperCase)
                                       .collect(Collectors.toList());
   // upperCaseNames = ["ALICE", "BOB", "CHARLIE"]
   ```

2. `Collectors.toSet()`: 将 Stream 中的元素收集到一个 Set 中,自动去重。
    示例: `Set<String> set = stream.collect(Collectors.toSet());`

   ```java
   List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "Bob");
   Set<String> uniqueNames = names.stream().collect(Collectors.toSet());
   // uniqueNames = ["Alice", "Bob", "Charlie"]
   ```

3. `Collectors.toCollection(supplier)`: 将 Stream 中的元素收集到一个指定的集合中,需要提供一个集合的供应器。
    示例: `ArrayList<String> list = stream.collect(Collectors.toCollection(ArrayList::new));`

   ```java
   List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
   LinkedList<String> linkedListNames = names.stream()
                                              .collect(Collectors.toCollection(LinkedList::new));
   // linkedListNames = ["Alice", "Bob", "Charlie"]
   ```

4. `Collectors.toMap(keyMapper, valueMapper)`: 将 Stream 中的元素收集到一个 Map 中,需要提供键和值的映射函数。
    示例: `Map<String, Integer> map = stream.collect(Collectors.toMap(String::toLowerCase, String::length));`

   ```java
   List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
   Map<String, Integer> nameLengths = names.stream()
                                           .collect(Collectors.toMap(Function.identity(), String::length));
   // nameLengths = {"Alice"=5, "Bob"=3, "Charlie"=7}
   ```

5. `Collectors.joining()`: 将 Stream 中的元素连接成一个字符串。
    示例: `String str = stream.collect(Collectors.joining());`

   ```java
   List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
   String joinedNames = names.stream().collect(Collectors.joining());
   // joinedNames = "AliceBobCharlie"
   ```

6. `Collectors.joining(delimiter)`: 将 Stream 中的元素用指定的分隔符连接成一个字符串。
    示例: `String str = stream.collect(Collectors.joining(","));`

   ```java
   List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
   String joinedNamesWithComma = names.stream().collect(Collectors.joining(", "));
   // joinedNamesWithComma = "Alice, Bob, Charlie"
   ```

7. `Collectors.joining(delimiter, prefix, suffix)`: 将 Stream 中的元素用指定的分隔符连接成一个字符串,并添加前缀和后缀。
    示例: `String str = stream.collect(Collectors.joining(",", "[", "]"));`

   ```java
   List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
   String joinedNamesWithBrackets = names.stream().collect(Collectors.joining(", ", "[", "]"));
   // joinedNamesWithBrackets = "[Alice, Bob, Charlie]"
   ```

8. `Collectors.groupingBy(classifier)`: 根据指定的分类函数对 Stream 中的元素进行分组,返回一个 Map。
    示例: `Map<String, List<Student>> map = students.collect(Collectors.groupingBy(Student::getGender));`

   ```java
   List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "Dave");
   Map<Integer, List<String>> namesByLength = names.stream()
                                                    .collect(Collectors.groupingBy(String::length));
   // namesByLength = {3=["Bob"], 5=["Alice"], 7=["Charlie"], 4=["Dave"]}
   ```

9. `Collectors.partitioningBy(predicate)`: 根据指定的谓词函数对 Stream 中的元素进行分区,返回一个 Map,键为 true 和 false,值为对应的元素列表。
    示例: `Map<Boolean, List<Student>> map = students.collect(Collectors.partitioningBy(s -> s.getAge() > 18));`

   ```java
   List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);
   Map<Boolean, List<Integer>> partitionedNumbers = numbers.stream()
                                                       .collect(Collectors.partitioningBy(n -> n % 2 == 0));
   // partitionedNumbers = {false=[1, 3, 5], true=[2, 4, 6]}
   ```

10. `Collectors.counting()`: 计算 Stream 中元素的个数。
     示例: `long count = stream.collect(Collectors.counting());`

    ```java
    List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
    long count = names.stream().collect(Collectors.counting());
    // count = 3
    ```

11. `Collectors.summingInt(mapper)`: 对 Stream 中元素的某个整数属性求和。
     示例: `int totalAge = students.collect(Collectors.summingInt(Student::getAge));`

    ```java
    List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
    int totalLength = names.stream().collect(Collectors.summingInt(String::length));
    // totalLength = 15
    ```

12. `Collectors.averagingInt(mapper)`: 对 Stream 中元素的某个整数属性计算平均值。
     示例: `double averageAge = students.collect(Collectors.averagingInt(Student::getAge));`

    ```java
    List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
    double averageLength = names.stream().collect(Collectors.averagingInt(String::length));
    // averageLength = 5.0
    ```

13. `Collectors.maxBy(comparator)`: 根据指定的比较器找出 Stream 中的最大元素。
     示例: `Optional<Student> maxAgeStudent = students.collect(Collectors.maxBy(Comparator.comparing(Student::getAge)));`

    ```java
    List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
    Optional<String> longestName = names.stream()
                                        .collect(Collectors.maxBy(Comparator.comparing(String::length)));
    // longestName = Optional["Charlie"]
    ```

14. `Collectors.minBy(comparator)`: 根据指定的比较器找出 Stream 中的最小元素。
     示例: `Optional<Student> minAgeStudent = students.collect(Collectors.minBy(Comparator.comparing(Student::getAge)));`

    ```java
    List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
    Optional<String> shortestName = names.stream()
                                         .collect(Collectors.minBy(Comparator.comparing(String::length)));
    // shortestName = Optional["Bob"]
    ```

15. `Collectors.reducing(identity, mapper, operation)`: 对 Stream 中的元素执行一个归约操作,将它们组合成一个结果。
     示例: `int sum = stream.collect(Collectors.reducing(0, Integer::intValue, Integer::sum));`

    ```java
    List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
    int totalLength = names.stream().collect(Collectors.reducing(0, String::length, Integer::sum));
    // totalLength = 15
    ```

    

## 方法引用

方法引用（**Method Reference**）是 Java 8 引入的一种简洁的写法，主要目的是**简化**Lambda表达式（`lambda expressions`）的代码。

<span style="color:red">方法引用实际上是lambda表达式的一种简化形式或语法糖。</span>

<span style="color:red">方法引用 和 Lambda 表达式 都是用来创建函数式接口实例的</span>

<span style="color:blue">Lambda 表达式可以直接写逻辑，也可以调用已有方法；方法引用只能引用已有的方法，不能写新逻辑！</span>

方法引用属于**函数式编程**的一部分，它允许我们直接引用已有的方法，而不用重复写lambda表达式。

- 不是所有的lambda表达式都能转换为方法引用。方法引用是lambda表达式的一种特殊简化形式，但它有一些限制条件。
- 所有可以使用方法引用的地方都可以改写为等效的lambda表达式



**Lambda表达式创建函数式接口实例**

- 直接写逻辑

  ```java
  Runnable r = () -> {
      System.out.println("Running...");
  };
  ```

- 调用已有方法

  ```java
  public class Test4 {
      public static void main(String[] args) {
          Runnable r = () -> say();
      }
  
      public static void say() {
          System.out.println("Hello World");
      }
  }
  ```

  

**方法引用创建函数式接口实例**

- 调用已有方法

  ```java
  public class Test4 {
      public static void main(String[] args) {
          Runnable r = Test4::say;
      }
  
      public static void say() {
          System.out.println("Hello World");
      }
  }
  ```





### 基本形式

| 形式                | 示例                        | 实例                     | 说明                           |
| :------------------ | :-------------------------- | :----------------------- | ------------------------------ |
| 1. 静态方法引用     | `ClassName::staticMethod`   | `Integer::parseInt`      | 引用类的静态方法               |
| 2. 实例方法引用     | `instance::instanceMethod`  | `someObject::methodName` | 引用特定对象的实例方法         |
| 3. 类的实例方法引用 | `ClassName::instanceMethod` | `String::toLowerCase`    | 引用某个类的任意对象的实例方法 |
| 4. 构造器引用       | `ClassName::new`            | `ArrayList::new`         | 引用类的构造器（构建对象）     |



静态方法

```java
int num = Integer.parseInt("123");
```

类实例方法

```java
String str = "HELLO";
String lower = str.toLowerCase();
```



### 示例

**静态方法引用**

字符串列表中的每个元素转换为整数

```java
public class StaticMethodReferenceExample {
    public static void main(String[] args) {
        List<String> strNumbers = Arrays.asList("1", "2", "3", "4", "5");
        
        // 使用Lambda表达式
        List<Integer> numbersLambda = strNumbers.stream()
                .map(s -> Integer.parseInt(s))
                .collect(Collectors.toList());
        
        // 使用方法引用（静态方法引用）
        List<Integer> numbersMethodRef = strNumbers.stream()
                .map(Integer::parseInt)
                .collect(Collectors.toList());
        
        System.out.println("使用Lambda表达式: " + numbersLambda);
        System.out.println("使用方法引用: " + numbersMethodRef);
    }
}
```



**实例方法引用**

创建一个程序，使用方法引用对字符串列表进行排序。

```java
public class InstanceMethodReferenceExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("张三", "李四", "王五", "赵六");
        
        // 使用Lambda表达式
        Collections.sort(names, (s1, s2) -> s1.compareTo(s2));
        System.out.println("使用Lambda表达式排序: " + names);
        
        // 打乱顺序
        Collections.shuffle(names);
        
        // 使用方法引用（实例方法引用）
        Collections.sort(names, String::compareTo);
        System.out.println("使用方法引用排序: " + names);
    }
}
```

```java
// 方法引用
String prefix = "Hello, ";
Function<String, String> greeter = prefix::concat;

// 等效的lambda表达式
Function<String, String> greeter = s -> prefix.concat(s);
```

```java
// 方法引用
Comparator<String> comp = String::compareTo;

// 等效的lambda表达式
Comparator<String> comp = (s1, s2) -> s1.compareTo(s2);
```



**构造函数引用**

```java
class Person {
    private String name;
    
    public Person(String name) {
        this.name = name;
    }
    
    @Override
    public String toString() {
        return "Person{name='" + name + "'}";
    }
}

public class ConstructorReferenceExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("张三", "李四", "王五");
        
        // 使用Lambda表达式
        List<Person> peopleWithLambda = names.stream()
                .map(name -> new Person(name))
                .collect(Collectors.toList());
        
        // 使用构造函数引用
        List<Person> peopleWithConstructorRef = names.stream()
                .map(Person::new)
                .collect(Collectors.toList());
        
        System.out.println("使用Lambda表达式: " + peopleWithLambda);
        System.out.println("使用构造函数引用: " + peopleWithConstructorRef);
    }
}
```



### 其他示例

mp

```java
userWrapper.eq(User::getPhone, loginForm.getPhone());
```

```java
user -> user.getPhone()
```

```java
userWrapper.eq(new SFunction<User, String>() {
    @Override
    public String apply(User user) {
        return user.getPhone();
    }
}, loginForm.getPhone());
```





```java
@FunctionalInterface
interface Printer {
    void print(String message);
}
```

```java
public class Util {
    public static void printMsg(String msg) {
        System.out.println("Message: " + msg);
    }
}
```

可以这样写

```java
Printer p = Util::printMsg;
p.print("Hello");
```





## 常量池

### 类文件常量池

静态常量池是指每个 .class 文件中包含的常量池部分，由 Java 编译器在编译时生成。它主要用于存储在编译期间已知的各种字面量和符号引用。

存储内容

1. 字面量（Literals）：
   - 数值字面量：如 int、long、float、double、char、boolean 的常量值。
   - 字符串字面量：如 "Hello World"。
2. 符号引用（Symbolic References）：
   - 类或接口的全限定名：如 java/lang/String。
   - 字段的名称和描述符：如 age 和 I（表示 int 类型）。
   - 方法的名称和描述符：如 main 和 ([Ljava/lang/String;)V。




### 运行时常量池

运行时常量池是 JVM 在类加载时，从 `.class` 文件的常量池（Class Constant Pool）中加载的一部分数据，并存储在方法区或元空间（JDK 8 之后）。

**运行时常量池的作用**

1. **存放字面量**（`final` 修饰的常量，或者 `1.0`, `"abc"` 这种字面值）。
2. **存放符号引用**（类名、方法名、字段名等）。
3. **存放动态常量**（`ldc` 指令加载的字符串、基本类型、常量池动态计算的值）。

```java
public class TestFinalConstant {
    public static void main(String[] args) {
        final String s1 = "hello";
        final String s2 = " world";
        String s3 = s1 + s2; // 直接在编译期计算成 "hello world"
        String s4 = "hello world";

        System.out.println(s3 == s4); // true ✅（编译期优化，s3 直接指向字符串常量池）
    }
}
```

动态计算的 `String` 进入运行时常量池

```java
public class TestRuntimeConstantPool {
    public static void main(String[] args) {
        String s1 = "hello";
        String s2 = s1 + " world"; // 运行时拼接，不会放入字符串常量池
        String s3 = "hello world";

        System.out.println(s2 == s3); // false ❌（s2 运行时创建，不在字符串常量池）
        
        String s4 = s2.intern();
        System.out.println(s4 == s3); // true ✅（s4 被放入字符串常量池）
    }
}
```









### 基础类型常量池（缓存池）

基础类型常量池主要针对 Java 中的基本数据类型（如 `int`、`long` 等）以及其对应的包装类（如 `Integer`、`Long` 等），用于优化内存使用和提高性能。

Java 里 **`Byte`、`Short`、`Integer`、`Long`、`Character`、`Boolean`** **这 6 种包装类** 都有**缓存池**，但 `Float` 和 `Double` 没有缓存机制。

```java
Integer i1 = 10;
Integer i2 = 10;
System.out.println(i1 == i2); // true

Integer i3 = 128;
Integer i4 = 128;
System.out.println(i3 == i4); // false
```

#### 具体缓存范围

| **包装类**  | **缓存范围**    | **缓存池的存储位置** |
| ----------- | --------------- | -------------------- |
| `Boolean`   | `true`、`false` | Java **堆（Heap）**  |
| `Byte`      | `-128 ~ 127`    | Java **堆（Heap）**  |
| `Short`     | `-128 ~ 127`    | Java **堆（Heap）**  |
| `Integer`   | `-128 ~ 127`    | Java **堆（Heap）**  |
| `Long`      | `-128 ~ 127`    | Java **堆（Heap）**  |
| `Character` | `0 ~ 127`       | Java **堆（Heap）**  |
| `Float`     | ❌ **无缓存**    | **总是创建新对象**   |
| `Double`    | ❌ **无缓存**    | **总是创建新对象**   |

### 字符串常量池

**字符串常量池**是 JVM 中专门用于存储字符串字面量和通过 `intern()` 方法添加的字符串的区域。其主要目的是为了优化字符串的存储，避免重复创建相同内容的字符串对象，从而节省内存并提高性能。

```java
public class StringPoolTest {
    public static void main(String[] args) {
        String str1 = "Hello";
        String str2 = "Hello";
        System.out.println(str1 == str2); // 输出 true，引用同一个对象
    }
}
```



## 数据类型

### BLOB

BLOB（Binary Large Object）

`BLOB` 是用于存储**二进制数据**的大型对象，比如图像、音频、视频、PDF 文件等。

```
java.sql.Blob
```



### CLOB

CLOB（Character Large Object）

`CLOB` 是用于存储**大文本数据**的类型，比如 XML、HTML、长篇文章等。

```
java.sql.Clob
```



### BLOB 与 CLOB 的区别

| 特性     | **BLOB**                            | **CLOB**                           |
| -------- | ----------------------------------- | ---------------------------------- |
| 全称     | Binary Large Object                 | Character Large Object             |
| 存储内容 | 二进制数据（图像、音频、视频等）    | 字符数据（文本、XML、HTML等）      |
| 数据编码 | 不进行字符编码                      | 依据字符集（如 UTF-8、UTF-16）编码 |
| 操作方式 | 使用 `InputStream` / `OutputStream` | 使用 `Reader` / `Writer`           |
| 示例用途 | 存储图片、PDF、音频、加密数据等     | 存储长篇文本、文章、日志、HTML等   |
| 数据类型 | 原始字节（byte[]）                  | 字符串（String）                   |



## String

### 字符串常量池

在 Java 中，`String` 是一个特殊的对象类型，字符串字面量（如 `"hello"`）会被存储在**字符串常量池（String Pool）**中。

**Java 7及以后**：字符串常量池在**堆内存**中

```java
String s1 = "hello world";
String s2 = "hello world";
System.out.println(s1 == s2);  // true
String a1 = new String("hello world");
String a2 = new String("hello world");
System.out.println(a1 == a2);  // false
System.out.println(a1.equals(a2));  // true
```

`==` 比较的是引用地址

`equals()` 比较的是内容

```java
String a = new String("hello");
String b = new String("hello");

System.out.println(a == b);  // false
System.out.println(a.equals(b));  // true
```

这里 `a` 和 `b` 指向的是不同的 `String` 对象，所以 `a == b` 结果为 `false`，但 `a.equals(b)` 仍然是 `true`。

| **代码**                          | **存储位置**                            | **`==` 结果**          | **原因**                                                 |
| --------------------------------- | --------------------------------------- | ---------------------- | -------------------------------------------------------- |
| `String s1 = "hello world";`      | **常量池（方法区）Metaspace（元空间）** | ✅ `s1 == s2` 为 `true` | 字符串字面量直接存入常量池，多个相同字面量指向同一个对象 |
| `String a = new String("hello");` | **Java 堆（Heap）**                     | ❌ `a == b` 为 `false`  | `new` 关键字每次都会创建新的 `String` 对象               |

如果你希望让 `new String("hello")` 也使用字符串池，可以调用 `.intern()` 方法，**字符串常量池**

```java
String a = new String("hello").intern();
String b = "hello";

System.out.println(a == b);  // true
```





String a = new String("hello") 

1. String a = new String("hello");会创建两个对象:
在字符串常量池中创建字符串字面量"hello"（如果之前不存在）
在堆内存中创建一个新的String对象，它内部引用了常量池中的"hello"
2. 变量a存储在栈内存中，它引用的是堆内存中的String对象，而不是常量池中的字面量

String a = "hello"

1. 如果直接使用String a = "hello"; 变量a会直接引用常量池中的字面量，不会在堆内存中创建额外对象





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

#### 字符串截取

1. substring(int beginIndex, int endIndex)

   - **`beginIndex`**：起始索引（包含）。
   - **`endIndex`**：结束索引（不包含）。

   ```java
   String str = "hello";
   String result = str.substring(1, 2); // 取索引 1 的字符
   System.out.println(result); // 输出 "e"
   ```

   注意  <span style="color:red">oracle里substr(1,2)是从第一个字符开始截取两个字符,并且从1开始计数</span>

   ```sql
   SELECT SUBSTR('hello', 1, 2) AS result FROM dual;
   -- 返回 "he"
   ```

   JavaScript 的 `substring` 方法与 Java 类似，substr是按长度截取

   ```javascript
   let str = "hello";
   let result = str.substring(1, 2); // 取索引 1 的字符
   console.log(result); // 输出 "e"
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




### StringBuilder

`StringBuilder` 是**可变的**，即它的内容可以在创建后被修改，而不像 `String` 那样每次修改都会生成新的字符串对象。

- **可变性**：`StringBuilder` 是可变对象，对其进行的操作不会产生新的对象。
- **线程不安全**：`StringBuilder` 是非线程安全的（与 `StringBuffer` 相对），因此适用于单线程场景。
- **高性能**：由于没有线程安全的开销，`StringBuilder` 比 `StringBuffer` 更快。
- **底层实现**：`StringBuilder` 内部使用一个 `char[]` 数组来存储字符数据，初始容量可以动态扩展。

#### 构造方法

```java
StringBuilder sb = new StringBuilder();  // 无参构造
StringBuilder sb = new StringBuilder(50);  // 指定初始容量
StringBuilder sb = new StringBuilder("Hello");
```

#### 方法

**增加内容**

- `append(String str)`：在末尾追加字符串。

  ```java
  StringBuilder sb = new StringBuilder("Hello");
  sb.append(" World");
  System.out.println(sb);  // 输出：Hello World
  ```

- `insert(int offset, String str)`：在指定位置插入字符串。

  ```java
  StringBuilder sb = new StringBuilder("Hello");
  sb.insert(5, " Beautiful");
  System.out.println(sb);  // 输出：Hello Beautiful
  ```

**删除内容**

- `delete(int start, int end)`：删除从 `start` 到 `end`（不包含）的内容。

  ```java
  StringBuilder sb = new StringBuilder("Hello World");
  sb.delete(5, 11);
  System.out.println(sb);  // 输出：Hello
  ```

- `deleteCharAt(int index)`：删除指定位置的字符。

  ```java
  StringBuilder sb = new StringBuilder("Hello");
  sb.deleteCharAt(0);
  System.out.println(sb);  // 输出：ello
  ```

**替换内容**

- `replace(int start, int end, String str)`：用指定字符串替换从 `start` 到 `end`（不包含）的内容。

  ```java
  StringBuilder sb = new StringBuilder("Hello World");
  sb.replace(6, 11, "Java");
  System.out.println(sb);  // 输出：Hello Java
  ```

**反转内容**

- `reverse()`：将字符串内容反转。

  ```java
  StringBuilder sb = new StringBuilder("Hello");
  sb.reverse();
  System.out.println(sb);  // 输出：olleH
  ```

**修改单个字符**

- `setCharAt(int index, char ch)`：修改指定位置的字符。

  ```java
  StringBuilder sb = new StringBuilder("Hello");
  sb.setCharAt(0, 'Y');
  System.out.println(sb);  // 输出：Yello
  ```

**获取子字符串**

- `substring(int start)` 和 `substring(int start, int end)`：返回从 `start` 开始到 `end`（不包含）的子字符串。

  ```java
  StringBuilder sb = new StringBuilder("Hello World");
  String sub = sb.substring(6);  // 从索引 6 开始
  System.out.println(sub);  // 输出：World
  ```

**获取长度和容量**

- `length()`：返回当前字符串的长度。

  ```java
  StringBuilder sb = new StringBuilder("Hello");
  System.out.println(sb.length());  // 输出：5
  ```

- `capacity()`：返回当前容量（`char[]` 的大小）。

  ```java
  StringBuilder sb = new StringBuilder("Hello");
  System.out.println(sb.capacity());  // 输出：21 (16 默认容量 + 5 字符串长度)
  ```

**扩展容量**

- `ensureCapacity(int minimumCapacity)`：确保容量至少为 `minimumCapacity`，如果当前容量不足，则进行扩容。

  ```java
  StringBuilder sb = new StringBuilder(10);
  sb.ensureCapacity(50);
  System.out.println(sb.capacity());  // 输出：50
  ```

**清空内容**

- `setLength(int newLength)`：设置新的长度。如果新长度小于当前长度，会截断字符串；如果新长度大于当前长度，会在末尾补充空字符。

  ```java
  StringBuilder sb = new StringBuilder("Hello");
  sb.setLength(0);  // 清空内容
  System.out.println(sb);  // 输出：空字符串
  ```

#### 实现原理

- `StringBuilder` 底层维护了一个 `char[]` 数组，当容量不足时，会自动扩容。

- 扩容规则：新容量为 `(旧容量 * 2) + 2`。例如，初始容量为 16，当需要扩容时，新容量为 `16 * 2 + 2 = 34`。

- 修改操作（如 `append`、`insert`）直接在原数组上进行，因此效率高。



### StringJoiner

`StringJoiner` 位于 `java.util` 包中

#### 构造方法

```java
StringJoiner joiner = new StringJoiner("分隔符");
StringJoiner joiner = new StringJoiner("分隔符","前缀","后缀");
```

#### 示例

```java
StringJoiner joiner = new StringJoiner(", ");
joiner.add("Java");
joiner.add("Python");
joiner.add("C++");
System.out.println(joiner);  // 输出：Java, Python, C++
```

```java
StringJoiner joiner = new StringJoiner(", ", "[", "]");
joiner.add("Java");
joiner.add("Python");
joiner.add("C++");
System.out.println(joiner);  // 输出：[Java, Python, C++]
```

#### 方法

**添加字符串**

- **`add(String element)`**：将字符串添加到 `StringJoiner` 中。

  ```java
  StringJoiner joiner = new StringJoiner(", ");
  joiner.add("Apple");
  joiner.add("Banana");
  System.out.println(joiner);  // 输出：Apple, Banana
  ```

**设置默认内容**

- **`setEmptyValue(CharSequence emptyValue)`**：设置当 `StringJoiner` 中没有任何元素时的默认内容。

  ```java
  StringJoiner joiner = new StringJoiner(", ");
  joiner.setEmptyValue("No Elements");
  System.out.println(joiner);  // 输出：No Elements
  ```

**合并两个 `StringJoiner`**

- **`merge(StringJoiner other)`**：将另一个 `StringJoiner` 的内容合并到当前对象中。
  
  - 如果当前 `StringJoiner` 为空，则直接使用 `other` 的内容。
- 否则，`other` 的内容会以分隔符连接到当前对象的末尾。
  
  ```java
  StringJoiner joiner1 = new StringJoiner(", ");
joiner1.add("Apple").add("Banana");
  
  StringJoiner joiner2 = new StringJoiner("; ");
joiner2.add("Orange").add("Grapes");
  
  joiner1.merge(joiner2);
  System.out.println(joiner1);  // 输出：Apple, Banana, Orange; Grapes
  ```

**获取拼接结果**

- **`toString()`**：返回拼接后的字符串。

  ```java
  StringJoiner joiner = new StringJoiner(", ");
  joiner.add("Dog").add("Cat");
  System.out.println(joiner.toString());  // 输出：Dog, Cat
  ```

**获取长度**

- **`length()`**：返回当前拼接结果字符串的长度（包括前缀、后缀和分隔符）。

  ```java
  StringJoiner joiner = new StringJoiner(", ", "[", "]");
  joiner.add("A").add("B");
  System.out.println(joiner.length());  // 输出：5（"[A, B]"）
  ```









## 集合

单列集合：Collection

双列集合：Map   

### 单列集合

#### Collection

##### 方法

**添加元素**

- boolean add(E e)  将指定的元素添加到集合中
- boolean addAll(Collection<? extends E> c)  将指定集合中的所有元素添加到当前集合中

**删除元素**

- void clear()  清空集合中的所有元素。
- boolean remove(Object o)  从集合中删除指定的元素
- boolean removeAll(Collection<?> c)  删除当前集合中那些也在指定集合中的所有元素
- boolean retainAll(Collection<?> c)  仅保留当前集合中那些也在指定集合中的元素（取两者的交集）

**查询元素的方法**

- `boolean contains(Object o)`  判断集合中是否包含指定的元素。
- `boolean containsAll(Collection<?> c)`  判断当前集合是否包含指定集合中的所有元素。
- `boolean isEmpty()`  判断集合是否为空（即集合中没有元素）。
- `int size()`  返回集合中元素的数量。



##### 遍历方式

**迭代器遍历**

迭代器在java中的类是`Iterator`，是集合专用的遍历方式。迭代器不依赖索引。

`Iterator` 接口提供了以下三种方法：

1. **`boolean hasNext()`**
   - 检查集合中是否还有下一个元素。
   - 返回 `true` 表示还有元素可以访问，返回 `false` 表示已经遍历完成。
2. **`E next()`**
   - 返回集合中的下一个元素，同时将迭代器的指针向后移动。
   - 如果没有更多元素会抛出 `NoSuchElementException`。
3. **`void remove()`**
   - 从集合中删除迭代器返回的最后一个元素。
   - 这个方法只能在调用 `next()` 方法后使用，否则会抛出 `IllegalStateException`。

用 `Iterator` 遍历集合

```java
public class Main {
    public static void main(String[] args) {
        // 创建一个 ArrayList
        ArrayList<String> list = new ArrayList<>();
        list.add("Apple");
        list.add("Banana");
        list.add("Cherry");

        // 获取迭代器
        Iterator<String> iterator = list.iterator();

        // 使用迭代器遍历集合
        while (iterator.hasNext()) {
            String element = iterator.next();
            System.out.println(element);
        }
    }
}
```

使用 `Iterator` 删除元素

```java
public class Main {
    public static void main(String[] args) {
        // 创建一个 ArrayList
        ArrayList<String> list = new ArrayList<>();
        list.add("Apple");
        list.add("Banana");
        list.add("Cherry");
        list.add("Date");

        // 获取迭代器
        Iterator<String> iterator = list.iterator();

        // 遍历并删除以 "B" 开头的元素
        while (iterator.hasNext()) {
            String element = iterator.next();
            if (element.startsWith("B")) {
                iterator.remove(); // 安全删除当前元素
            }
        }

        // 输出删除后的集合
        System.out.println(list); // 输出: [Apple, Cherry, Date]
    }
}
```

迭代器遍历时，不能用集合的方法进行增加或删除。

循环中只能使用一次next方法。`NoSuchElementException`



**增强for遍历**

- 增强for底层就是迭代器
- 所有的单列集合和数组才能用增强for遍历
- 修改增强for里的变量不会改变集合中原本的数据

```java
public class test1 {
    public static void main(String[] args) {
        List<String> li = new ArrayList<>(Arrays.asList("apple", "banana", "orange"));
        for (String s : li) {
            System.out.println(s);
        }
    }
}
```



**Lambda表达式遍历**

```java
public class test1 {
    public static void main(String[] args) {
        List<String> li = new ArrayList<>(Arrays.asList("apple", "banana", "orange"));
        li.forEach(new Consumer<String>() {
            @Override
            public void accept(String s) {
                System.out.println(s);
            }
        });
    }
}
```

```java
public class test1 {
    public static void main(String[] args) {
        List<String> li = new ArrayList<>(Arrays.asList("apple", "banana", "orange"));
        li.forEach(s -> System.out.println(s));
    }
}
```







#### List

添加的元素有序，可重复，有索引

实现类ArrayList 、LinkedList、Vector



##### 不可变列表

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



##### 方法

1. `boolean add(E element)`: 在 List 的末尾添加指定的元素。<span style="color:red">就地修改原有 List</span>

```java
List<String> list = new ArrayList<>();
list.add("apple");
list.add("banana");
System.out.println(list); // 输出: [apple, banana]
```

2. `void add(int index, E element)`: 在 List 的指定位置插入指定的元素。<span style="color:red">就地修改原有 List</span>

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

    <span style="color:red">就地修改原有 List</span>

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

18. `E set(int index, E element)`: 用指定的元素替换 List 中指定位置的元素。<span style="color:red">返回被修改的元素。</span>

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

21. `removeIf(Predicate<? super E> filter)`：根据指定的条件删除 List 中的元素

```java
public class RemoveIfExample {
    public static void main(String[] args) {
        List<Integer> numbers = new ArrayList<>();
        numbers.add(1);
        numbers.add(2);
        numbers.add(3);
        numbers.add(4);
        numbers.add(5);
        
        System.out.println("Before removeIf: " + numbers);
        
        // 使用 removeIf() 方法删除所有偶数
        numbers.removeIf(n -> n % 2 == 0);
        
        System.out.println("After removeIf: " + numbers);
    }
}
```



##### 列表迭代器



##### ArrayList

ArrayList底层是数组结构的

底层原理

- 利用空参创建的集合，在底层创建一个默认长度为0的数组
- 添加第一个元素时，底层会创建一个新的长度为10的数组
- 存满时会扩容1.5倍
- 如果一次添加多个元素，1.5倍还放不下，则新创建数组的长度以实际为准

```java
public class test1 {
    public static void main(String[] args) {
       ArrayList<String> l1 = new ArrayList<>();
       l1.add("a");
       l1.add("b");
       l1.add("c");
       ArrayList<String> l2 = new ArrayList<>();
       l2.addAll(l1);
    }
}
```



##### LinkedList

底层是双链表，查询慢，增删快，如果操作首尾元素也极快

`LinkedList` 是一个双向链表数据结构，它实现了 `List`、`Deque` 和 `Queue` 接口。由于它继承了这些接口，`LinkedList` 提供了许多方法，包括 `List` 接口中的方法（如 `add()`、`get()`、`remove()` 等），以及 `Deque` 和 `Queue` 接口中特有的方法。

特有方法

**双端队列操作**（`addFirst`、`addLast`、`getFirst`、`getLast`、`removeFirst`、`removeLast` 等）。

**队列操作**（`offer`、`poll`、`peek` 等）。

**栈操作**（`push` 和 `pop`）。

**元素索引方法**（`indexOf` 和 `lastIndexOf`）。

**其他方法**（如 `clone` 和 `element`）。





#### SET

添加的元素无序，不重复，无索引

- 无序：存取顺序不一致
- 不重复：可以去除重复
- 无索引：没有带索引的方法，所以不能使用普通for循环遍历，也不能通过索引来获取元素
- Set接口中的方法上基本与Collection的API一致

##### 实现类

HashSet、TreeSet

HashSet下有LinkedHashSet

- HashSet：无序、不重复、无索引
- LinkedHashSet：有序、不重复、无索引
- TreeSet：可排序、不重复、无索引

##### HashSet

- 底层采用哈希表存储数据

- 哈希表是一种对于增删改查性能都较好的结果
- 哈希表组成：jdk8开始 数组 + 链表 + 红黑树 

哈希值：对象的整数表现形式

- 根据hashCode方法算出来的int类型的整数
- 该方法定义在Object类中，所有对象都可以调用，默认使用地址值进行计算
- 一般情况下会重写hashCode方法，利用对象内部的属性值计算哈希值

对象的哈希值特点

- 如果没有重写hashCode方法，不同对象计算出的哈希值不同
- 如果重写hashCode方法，不同对象只要属性值相同，计算出的哈希值就一样
- 小部分情况下，不同的属性值或者不同的地址值计算出来的哈希值也有可能一样（哈希碰撞）



```java
// Stuendt类
public class Student {
    private int id;
    private String name;
    private int age;

    @Override
    public int hashCode() {
        return Objects.hash(id, name, age);
    }
}
```

```java
public class test2 {
    public static void main(String[] args) {
        Student s1 = new Student(18, "John", 15);
        Student s2 = new Student(18, "John", 15);
        System.out.println(s1.hashCode());  // 打印出来相同
        System.out.println(s2.hashCode());
    }
}
```

###### 底层原理

当链表长度超过8，而且数组长度大于64，自动转换为红黑树

如果集合存储的是自定义对象，必须要重写`hashCode`和`equals`方法

1. 创建一个默认长度为16，默认加载因为0.75的数组，数组名为table

2. 根据元素的哈希值跟数组的长度计算出应存入的位置

   `int index = (数组长度 - 1) & 哈希值`

3. 判断当前位置是否为null，如果为null直接存入

4. 如果不为null，表示有元素，调用equals比较属性值

5. 一样：不存   不一样：存入数组，形成链表，新元素直接挂在老元素下面



无序性：

- 当元素被添加到 HashSet 中时，它们的存储位置是根据哈希值计算得出的。
- 哈希值的计算是通过哈希函数对元素的属性进行计算，不同元素可能会产生相同的哈希值。
- 由于哈希函数的特性，元素的存储位置与添加顺序无关，因此 HashSet 中的元素是无序的。

没有索引：

- HashSet 没有提供基于索引的访问方法，如 `get(int index)` 或 `set(int index, E element)`。
- 这是因为哈希表的数据结构并不支持按索引进行快速访问。
- 哈希表通过哈希值来确定元素的存储位置，而不是通过连续的索引。

HashSet 通过以下机制实现元素的去重：

1. 哈希值比较：
   - 当一个元素被添加到 HashSet 时，首先计算该元素的哈希值。
   - 如果哈希表中已经存在具有相同哈希值的元素，则说明可能发生了哈希冲突，需要进一步比较。
2. equals 方法比较：
   - 当哈希值相同时，HashSet 会调用元素的 `equals` 方法来比较两个元素的内容是否相等。
   - 如果 `equals` 方法返回 true，表示两个元素相等，HashSet 就不会重复添加该元素。
   - 如果 `equals` 方法返回 false，表示两个元素不相等，HashSet 会将新元素添加到哈希表中。
3. 哈希冲突处理：
   - 当发生哈希冲突时，即多个元素的哈希值相同，HashSet 使用链表法来解决冲突。
   - 哈希值相同的元素会以链表的形式存储在同一个哈希桶（bucket）中。
   - 当查找元素时，通过哈希值定位到对应的哈希桶，然后遍历链表找到目标元素。



##### LinkedHashSet

底层原理：

- 有序、不重复、无索引
- 有序指保证存储和取出的元素顺序一致
- 底层数据结构依然是哈希表，只是每个元素又额外多了



### Map

实现类

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



#### 复制map

```
new hashMap<>(map)
```

```java
public class eTest {
    public static void main(String[] args) {
        List<Map<String, Object>> list = new ArrayList<>();

        // 创建一个 Map 并添加键值对
        Map<String, Object> map = new HashMap<>();
        map.put("name", "John");
        map.put("age", 25);

        // 添加第一个 Map 的副本
        list.add(new HashMap<>(map));

        // 修改原始 Map 并添加副本
        map.put("name1", "Jane");
        list.add(new HashMap<>(map));

        // 打印结果
        System.out.println(list);
    }
}
```



#### HashMap

##### 钻石操作符

一下两种功能完全一样

区别只是写法上的语法糖，后者是 **Java 7 及以后版本提供的简化写法**，称为：**钻石操作符（Diamond Operator）**：`<>`

钻石操作符 `<>` **只能在赋值时使用**，不能用在声明泛型类的时候。

```java
Map<Integer, OrderStatus> map = new HashMap<Integer, OrderStatus>();
// 编译器会自动推断出右边泛型的类型是 <Integer, OrderStatus>。
Map<Integer, OrderStatus> map = new HashMap<>();
```

##### 红黑树

红黑树是在每个节点上增加一个**颜色标志（红或黑）**的二叉搜索树。它确保树大致平衡，使得插入、删除、查找操作的时间复杂度为 **O(log n)**。

红黑树必须满足的5条性质：

1. **每个节点是红色或黑色。**
2. **根节点是黑色。**
3. **每个叶子节点（NIL，空节点）是黑色。**
4. **如果一个节点是红色的，则它的子节点必须是黑色的。**（即**不能有两个连续的红色节点**）
5. **从任一节点到其所有后代叶子节点的简单路径上，必须包含相同数目的黑色节点。（称为黑高）**

衍生特性：

1. 从根节点到叶子节点的最长路径，不超过最短路径的两倍



#### Map.Entry<K, V>

当你使用 `Map`（如 `HashMap`、`TreeMap` 等）时，数据是以键值对形式存储的。`Map.Entry` 就是表示这些键值对的对象。

#### 获取方法

`map.entrySet()` 是主要方式

```java
Set<Map.Entry<String, Integer>> entries = map.entrySet();
```

```java
for (Map.Entry<K, V> entry : map.entrySet()) {
    K key = entry.getKey();
    V value = entry.getValue();
}
```

##### 常用方法

| 方法                  | 描述                   |
| --------------------- | ---------------------- |
| `K getKey()`          | 返回键                 |
| `V getValue()`        | 返回值                 |
| `V setValue(V value)` | 设置新的值，并返回旧值 |

##### 示例

```java
public static void main(String[] args) {
    // 创建一个 HashMap
    Map<String, Integer> map = new HashMap<>();
    // 添加键值对
    map.put("苹果", 3);
    map.put("香蕉", 5);
    map.put("橘子", 2);
    // 遍历 Map 中的键值对
    for (Map.Entry<String, Integer> entry : map.entrySet()) {
        String key = entry.getKey();       // 获取键
        Integer value = entry.getValue();  // 获取值
        System.out.println("水果: " + key + ", 数量: " + value);
    }
    // 修改其中一个 entry 的值
    for (Map.Entry<String, Integer> entry : map.entrySet()) {
        if (entry.getKey().equals("苹果")) {
            entry.setValue(10); // 修改值
        }
    }
    System.out.println("修改后的 Map: " + map);
}
```











## 枚举

### 什么是枚举

**枚举**是一种特殊的 Java 类型，用于定义一组常量。使用枚举可以让代码更清晰、可读性更强，同时避免魔法数字（magic numbers）或字符串带来的错误。

Java 中的枚举是 `enum` 关键字引入的，从 **JDK1.5** 开始支持。

### 基本语法

```java
public enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}
```

- 枚举成员默认是 `public static final`。
- 枚举类默认继承自 `java.lang.Enum`，不能继承其他类（因为 Java 不支持多继承）。
- 枚举不能使用 `extends`，但可以实现接口 `implements`。

### 示例

**示例1**

```java
package EnumTest;

public enum DayEnum {
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday,
    Sunday
}
```

这是最简单的枚举写法。每个枚举常量（如 `Monday`）是一个“固定的对象”，它们**没有任何额外的信息**。

```java
public class EnumTest1 {
    public static void main(String[] args) {
        printDay(DayEnum.Monday);
    }

    public static void printDay(DayEnum day) {
        switch (day) {
            case Monday:
                System.out.println("星期一");
                break;
            case Tuesday:
                System.out.println("星期二");
                break;
            case Wednesday:
                System.out.println("星期三");
                break;
            case Thursday:
                System.out.println("星期四");
                break;
            case Friday:
                System.out.println("星期五");
                break;
            case Saturday:
                System.out.println("星期六");
                break;
            case Sunday:
                System.out.println("星期日");
                break;
            default:
                System.out.println("未知");
                break;
        }
    }
}
```

**示例2**

```java
public enum DayEnum {
    Monday("星期一"),
    Tuesday("星期二"),
    Wednesday("星期三"),
    Thursday("星期四"),
    Friday("星期五"),
    Saturday("星期六"),
    Sunday("星期日");

    private String dayName;

    // 构造函数，给每个枚举常量赋值
    DayEnum(String dayName) {
        this.dayName = dayName;
    }

    public String getDayName() {
        return dayName;
    }
}
```

```java
System.out.println(DayEnum.Thursday.getDayName());  // 星期四
```

```java
public class Test {
    public static void main(String[] args) {
        for (DayEnum day : DayEnum.values()) {
            System.out.println(day + " : " + day.getDayName());
        }
    }
}
```

```java
for (DayEnum day : DayEnum.values()) {
    System.out.println(day);  // 实际上是调用了 day.toString() 方法。没有重写 toString() 方法，它默认返回的就是 name() 的值。
    System.out.println(day.name());
}
```



**示例3**

```java
public enum OrderStatus {
    // 步骤 1：定义枚举常量（每个包含 code 和 desc）
    CREATED(0, "已创建"),
    PAID(1, "已付款"),
    SHIPPED(2, "已发货"),
    COMPLETED(3, "已完成"),
    CANCELLED(4, "已取消");

    // 步骤 2：定义成员变量
    private final int code;
    private final String description;

    // 步骤 3：构造方法
    OrderStatus(int code, String description) {
        this.code = code;
        this.description = description;
    }

    // 步骤 4：提供 getter 方法
    public int getCode() {
        return code;
    }

    public String getDescription() {
        return description;
    }

    private static final Map<Integer, OrderStatus> codeMap = new HashMap<>();

    static {
        for (OrderStatus value : OrderStatus.values()) {
            codeMap.put(value.code, value);
        }
    }

    // 步骤 5：提供 toString 方法
    @Override
    public String toString() {
        return "订单状态：" + code + "，" + description;
    }

    // 步骤 6：提供一个方法，根据 code 获取枚举常量
    public static OrderStatus fromCode(int code) {
        if (codeMap.containsKey(code)) {
            return codeMap.get(code);
        }
        throw new IllegalArgumentException("无效的订单状态 " + code);
    }

    // 步骤7：是否是最终状态
    public boolean isFinalStatus() {
        return this == COMPLETED || this == CANCELLED;
    }
}
```

```java
public class EnumTest1 {
    public static void main(String[] args) {
        OrderStatus status = OrderStatus.fromCode(2);
        System.out.println("状态:" + status.getDescription());
        System.out.println(status.isFinalStatus());
        System.out.println(status.toString());
    }
}
```

#### 通过map缓存

```java
public enum OrderStatus {
    CREATED(0, "已创建"),
    PAID(1, "已付款"),
    SHIPPED(2, "已发货"),
    COMPLETED(3, "已完成"),
    CANCELLED(4, "已取消");

    private final int code;
    private final String description;

    OrderStatus(int code, String description) {
        this.code = code;
        this.description = description;
    }

    public int getCode() { return code; }
    public String getDescription() { return description; }

    // 用 Map 存储 code -> 枚举 的映射
    private static final Map<Integer, OrderStatus> CODE_MAP = new HashMap<>();

    // 静态代码块初始化映射表
    static {
        for (OrderStatus status : OrderStatus.values()) {
            CODE_MAP.put(status.getCode(), status);
        }
    }

    // 快速根据 code 查找枚举
    public static OrderStatus fromCode(int code) {
        OrderStatus status = CODE_MAP.get(code);
        if (status == null) {
            throw new IllegalArgumentException("无效的状态码：" + code);
        }
        return status;
    }

    public boolean isFinalStatus() {
        return this == COMPLETED || this == CANCELLED;
    }
}
```







### 自带方法

| 方法                   | 说明                          |
| ---------------------- | ----------------------------- |
| `values()`             | 返回所有枚举值的数组          |
| `valueOf(String name)` | 将字符串转换成对应的枚举值    |
| `name()`               | 返回枚举常量的名称            |
| `ordinal()`            | 返回枚举常量的序号，从 0 开始 |

### 语法

```java
Monday("星期一")
```

实际上在 Java 枚举中就是**调用构造方法 `DayEnum(String dayName)`**，传入的参数是 `"星期一"`。

```java
// 伪代码
public static final DayEnum Monday = new DayEnum("星期一");
public static final DayEnum Tuesday = new DayEnum("星期二");
...
```



#### 构造函数

枚举类中的构造函数不能是 `public`，只能是 `private` 或默认（包访问）权限。

在 Java 中，枚举（`enum`）的构造函数默认就是 `private` 的













## 接口

Java接口是一系列方法的声明，是一些方法特征的集合，一个接口只有方法的特征没有方法的实现，因此这些方法可以在不同的地方被不同的类实现，而这些实现可以具有不同的行为（功能）。

### 特点

接口可以理解为一种特殊的类型，它可以包含**常量**（`public static final` 默认修饰）和**方法的定义**，包括**抽象方法**、**默认方法**（`default`）、**静态方法**（`static`）（从 Java 8 开始，接口可以包含默认方法，抽象方法）以及**私有方法**（`private`，Java 9 及之后支持）。

接口是解决 Java 无法使用多继承的一种手段，因为一个类可以实现多个接口，而 Java 的类只能继承一个父类。但接口在实际中的主要作用是**制定标准**，为实现类提供一组必须实现的规范或行为。

在 Java 8 之前，接口可以被理解为**100% 抽象类**，因为接口中的方法必须全部是抽象方法，且不能包含任何实现。然而，从 Java 8 开始，接口可以包含**默认方法**和**静态方法**，因此接口已不再是严格意义上的 "100% 抽象类"。

1. 接口中的所有方法默认是 `public abstract`（在 Java 8 之前）。

2. 接口中的属性（成员变量）默认是 `public static final`。

3. 接口不能包含构造方法。

4. 接口支持多继承（类只能单继承，但接口可以多继承）。

5. 从 Java 8 开始，接口可以包含：

   - 默认方法（`default` 修饰）
   - 静态方法（`static` 修饰）

6. 从 Java 9 开始，接口可以包含：

   - 私有方法（`private` 修饰）——用于默认方法之间的共享逻辑。

   <span style="color:blue">在java9之前接口里的方法必须是公开的</span>

7. 接口中的方法默认是抽象方法，必须由实现类实现（除非是 `default` 或 `static` 方法）

8. 接口本身无法创建对象，但它的实现类可以创建对象。



### **示例**

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

### 默认方法

在 **Java 8** 之前，接口中只能有抽象方法

从Java 8开始，接口可以包含两种类型的方法：

1. **抽象方法**：传统的接口方法，只有声明，没有实现
2. **默认方法**：使用`default`关键字修饰，包含方法体的实现

```java
public interface Vehicle {
    void start();  // 抽象方法

    default void honk() {
        System.out.println("Beep beep!");
    }
}
```

```java
public class Car implements Vehicle {
    @Override
    public void start() {
        System.out.println("Car started.");
    }

    // 可以不重写honk()，直接使用接口中的默认实现
}
```

```java
public class Main {
    public static void main(String[] args) {
        Car car = new Car();
        car.start();  // 输出: Car started.
        car.honk();   // 输出: Beep beep!
    }
}
```











## 异常

在 Java 中，异常是指程序在运行时出现的错误事件。**异常是由程序或 Java 运行时环境产生的对象**，用于描述错误的类型以及错误的上下文信息。

Java 的异常是通过类和对象表示的，所有异常类都继承自 `java.lang.Throwable` 类。



### 常见的异常

 NullPointerException（空指针异常）

在尝试调用 `null` 引用的对象方法或访问字段时发生。

1. 调用 `null` 引用的方法
2. 访问 `null` 对象的字段
3. 访问 `null` 数组
4. `null` 引用的长度操作
5. ...

```java
public class NullPointerExample {
    public static void main(String[] args) {
        String str = null;
        System.out.println(str.length()); // 抛出 NullPointerException
    }
}
```

ArrayIndexOutOfBoundsException（数组索引越界异常）

访问数组时索引超出了合法范围（小于 0 或大于等于数组长度）

```java
public class ArrayIndexExample {
    public static void main(String[] args) {
        int[] arr = {1, 2, 3};
        System.out.println(arr[3]); // 抛出 ArrayIndexOutOfBoundsException
    }
}
```

ArithmeticException（算术异常）

执行非法的算术操作，如整数除以零。

```java
public class ArithmeticExample {
    public static void main(String[] args) {
        int result = 10 / 0; // 抛出 ArithmeticException
    }
}
```

ClassCastException（类转换异常）

将一个对象强制转换为不兼容的类型。

```java
public class ClassCastExample {
    public static void main(String[] args) {
        Object obj = "Hello";
        Integer num = (Integer) obj; // 抛出 ClassCastException
    }
}
```

NumberFormatException（数字格式化异常）

试图将非数字格式的字符串转换为数字。

```java
public class NumberFormatExample {
    public static void main(String[] args) {
        String str = "abc";
        int num = Integer.parseInt(str); // 抛出 NumberFormatException
    }
}
```

FileNotFoundException（文件未找到异常）

试图打开一个不存在的文件。

```java
import java.io.FileReader;
import java.io.FileNotFoundException;

public class FileNotFoundExample {
    public static void main(String[] args) {
        try {
            FileReader reader = new FileReader("nonexistent.txt");
        } catch (FileNotFoundException e) {
            System.out.println("文件未找到: " + e.getMessage());
        }
    }
}
```

IOException（输入输出异常）

在执行输入/输出操作时发生错误，例如文件读写失败、网络连接中断等。

```java
import java.io.FileWriter;
import java.io.IOException;

public class IOExceptionExample {
    public static void main(String[] args) {
        try {
            FileWriter writer = new FileWriter("/invalid/path/file.txt");
            writer.write("Hello");
            writer.close();
        } catch (IOException e) {
            System.out.println("IO 异常: " + e.getMessage());
        }
    }
}
```

IllegalArgumentException（非法参数异常）

当传递给方法的参数不合法时触发

```java
public class IllegalArgumentExample {
    public static void main(String[] args) {
        Thread thread = new Thread();
        thread.setPriority(11); // 抛出 IllegalArgumentException，线程优先级必须在 1~10
    }
}
```

StringIndexOutOfBoundsException（字符串索引越界异常）

尝试访问字符串中不存在的索引位置

```java
public class StringIndexExample {
    public static void main(String[] args) {
        String str = "Hello";
        System.out.println(str.charAt(10)); // 抛出 StringIndexOutOfBoundsException
    }
}
```



### 异常分类

1. 编译异常（Exception）：程序在编译过程中发现的异常，**受检异常**。

   在编译时必须被处理的异常，否则程序无法通过编译。

   <span style="color:red">java文件通过javac生成字节码文件时，编译阶段进行处理的异常。</span>

   在编译阶段java不会运行代码，只会检查语法是否错误，或者做一些性能优化

   需要显式地捕获或抛出。

   如`SQLException`、`IOException`、`ClassNotFoundException`

2. 运行时异常（RuntimeException）：又称为**非受检异常**。

   在编译时不强制处理的异常，在运行时出现。

   <span style="color:red">jvm通过java命令执行字节码文件</span>

   程序员可以选择处理，也可以不处理。

   如`NullPointerException`、`ArrayIndexOutOfBoundsException`、`ArithmeticException`、`ClassCastException`

   `IllegalArgumentException`、`IndexOutOfBoundsException`

3. 错误（Error）：由java虚拟机生成并抛出的异常，程序对其不作处理。

   表示 JVM 本身无法恢复的严重问题。

   通常不需要程序处理，程序员也无法控制，不建议捕获。
   
   如`StackOverflowError`、`OutOfMemoryError`、`NoClassDefFoundError`



### Throwable类

java.lang.Throwable

Throwable类是java语言所有错误或异常的超类。只有当此对象是此类（或其子类之一）的实例时，才能通过Java虚拟机或者Java throw语句抛出。类似地，只有此类或其子类之一才可以是catch子句中的参数类型。

#### 子类

**Error**

代表系统级别错误（属于严重问题）

Error是Throwable的子类，用于指示合理的应用程序不应该视图捕获的严重问题。大多数这样的错误都是异常条件。

系统一旦出现问题



**Exception**

异常，代表程序可能出现的问题。通常用exception和它的子类来封装程序出现的问题。

包含`RuntimeException`、其他异常。

#### 成员方法

**public String getMessage()**

返回此throwable的详细消息字符串

**public String toString()**

返回此可抛出的简短描述

**public void printStackTrace()**

把异常的错误信息输出在控制台

<span style="color:red">只是把异常打印在控制台，不会结束程序运行</span>



```
System.err.println("错误");  // 打印错误信息
```



### 异常处理方式

1. JVM默认的处理方式

   把异常名称，异常原因以及异常出现的位置等信息输出在了控制台

   程序停止执行，下面的代码不再执行

2. 自己处理

   try...catch...finally

   ```java
   package test;
   
   public class eTest {
       public static void main(String[] args) {
           String str = "Hello World";
           try {
               printString(str);
           } catch (Exception e) {
               System.out.println("发生异常: " + e.getMessage());
           }
       }
   
       public static void printString(String str) {
           if (str.length() < 15) {
               throw new RuntimeException("String is too short");
           } else {
               System.out.println(str);
           }
       }
   }
   ```

   如果捕获多个异常，具体的写在上面，笼统的写在下面

   catch中可以同时捕获多个异常，用 | 隔开

   <span style="color:red">在try中遇到第一个问题，try里下面的代码就不会执行了</span>

   ```java
   public class eTest {
       public static void main(String[] args) {
           int[] arr = {1, 2, 3, 4, 5};
           String s = null;
           try {
               System.out.println(arr[10]);
               System.out.println(1 / 0);
               System.out.println(s.length());
           } catch (ArrayIndexOutOfBoundsException e | ArithmeticException e) {
               System.out.println("错误");
           } catch (NullPointerException e) {
               System.out.println("空指针异常");
           } catch (Exception e) {
               System.out.println("未知异常");
           }
       }
   }
   ```

3. 抛出错误

   方法签名中如果抛出的是“受检异常（Checked Exception）”，你必须在调用处处理它（try-catch 或 throws）否则编译不会通过；
如果抛出的是“运行时异常（RuntimeException）”，你可以选择处理，也可以不处理。
   
   throws：写在方法定义处，表示声明一个异常

   - 编译时异常：必须要写
   - 运行时异常：可以不写
   
   ```java
   public class test1 {
       public static void main(String[] args) {
           try {
               test();
           } catch (Exception e) {
               e.printStackTrace();
           }
       }
   
       public static void test() throws Exception {
           int[] arr = {1, 2, 3, 4, 5};
           System.out.println(arr[9]);
    }
   }
   ```
   
   throw：写在方法内，结束方法，手动抛出异常对象，交给调用者，方法下面的代码不再执行



### 自定义异常

1. 定义异常类
2. 写继承关系
3. 空参构造
4. 带参构造

```java
public class NameFormatException extends RuntimeException {
    public NameFormatException() {
        super();
    }

    public NameFormatException(String message) {
        super(message);
    }
}
```











## 事务

### 特点ACID

1. 原子性(`atomicity`)：事务是一个原子操作, 由一系列动作组成. 事务的原子性确保动作要么全部完成要么完全不起作用。
2. 一致性(`consistency`)：一致性是指事务必须使数据库从一个一致性状态变换到另一个一致性状态，也就是说一个事务执行之前和执行之后都必须处于一致性状态。举例来说，假设用户A和用户B两者的钱加起来一共是1000，那么不管A和B之间如何转账、转几次账，事务结束后两个用户的钱相加起来应该还得是1000，这就是事务的一致性。
3. 隔离性(`isolation`)：可能有许多事务会同时处理相同的数据, 因此每个事物都应该与其他事务隔离开来, 防止数据损坏。
4. 持久性(`durability`)：一旦事务完成, 无论发生什么系统错误, 它的结果都不应该受到影响. 通常情况下, 事务的结果被写到持久化存储器中。
   

### 问题

#### 脏读

#### 不可重复读

#### 幻读



## 泛型

jdk5引入，可以在编译阶段约束操作的数据类型并进行检查

- <span style="color:red">Java的泛型只能使用引用类型（类和接口），不能使用基本数据类型（如int、double、boolean等）</span>

- 指定泛型的具体类型之后，传递数据后，可以传入该类类型或者其子类类型

  ```java
  ArrayList<Animal> l1 = new ArrayList<>();
  l1.add(new Animal());
  l1.add(new Cat());
  l1.add(new Dog());
  ```

  

- 如果不写泛型，默认是Object



### 泛型类

当一个类中，某个变量的数据类型不确定时，就可以定义带有泛型的类

类名后面定义泛型，类里所有的方法都能用

T、E、K、V

```java
修饰符 class 类名<类型> {
    
}
```

### 泛型方法

方法中形参类型不确定时，可以使用类名后面定义的泛型`<E>`

```java
修饰符 <类型> 返回值类型 方法吗 (类型 变量名) {
    
}
```

```java
public class MyArrayList {
    public <E> boolean add(E e){
        obj[size] = e;
        size++;
        return true;
    }
}
```

```java
public class GenericMethodExample {

    // 泛型方法：用于打印数组元素
    public static <T> void printArray(T[] array) {
        for (T element : array) {
            System.out.print(element + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        // 使用泛型方法处理不同类型的数组
        Integer[] intArray = {1, 2, 3, 4, 5};
        String[] stringArray = {"Hello", "Generic", "World"};
        Double[] doubleArray = {1.1, 2.2, 3.3};

        // 调用泛型方法
        System.out.println("Integer Array:");
        printArray(intArray); // 打印整数数组

        System.out.println("String Array:");
        printArray(stringArray); // 打印字符串数组

        System.out.println("Double Array:");
        printArray(doubleArray); // 打印浮点数数组
    }
}
```

### 泛型接口

```java
修饰符 interface 接口名<类型> {
    
}
```

```java
// 定义一个泛型接口
public interface Container<T> {
    // 添加元素到容器
    void add(T item);

    // 获取容器中的元素
    T get(int index);

    // 获取容器的大小
    int size();
}
```

### 泛型通配符

1. **无界通配符（`?`）**

   无界通配符表示可以接受任何类型的参数。它常用于方法参数中，表示对泛型类型没有任何限制。

   ```java
   public class WildcardExample {
   
       // 接收任意类型的 List
       public static void printList(List<?> list) {
           for (Object obj : list) {
               System.out.println(obj);
           }
       }
   
       public static void main(String[] args) {
           List<Integer> intList = List.of(1, 2, 3);
           List<String> strList = List.of("A", "B", "C");
   
           // 调用方法
           printList(intList); // 输出: 1, 2, 3
           printList(strList); // 输出: A, B, C
       }
   }
   ```

2. **上界通配符（`? extends T`）**

   上界通配符表示类型的 **上限**，即匹配类型必须是 `T` 或 `T` 的子类。

   ```java
   public class WildcardExample {
   
       // 计算数字列表的总和
       public static double sumOfList(List<? extends Number> list) {
           double sum = 0.0;
           for (Number num : list) {
               sum += num.doubleValue(); // 支持 Number 或其子类
           }
           return sum;
       }
   
       public static void main(String[] args) {
           List<Integer> intList = List.of(1, 2, 3);
           List<Double> doubleList = List.of(1.1, 2.2, 3.3);
   
           System.out.println(sumOfList(intList));    // 输出: 6.0
           System.out.println(sumOfList(doubleList)); // 输出: 6.6
       }
   }
   ```

3. **下界通配符（`? super T`）**

   下界通配符表示类型的 **下限**，即匹配类型必须是 `T` 或 `T` 的父类。

   ```java
   public class WildcardExample {
   
       // 添加元素到列表
       public static void addNumbers(List<? super Integer> list) {
           list.add(1); // 可以添加 Integer 或其子类
           list.add(2);
           list.add(3);
       }
   
       public static void main(String[] args) {
           List<Number> numList = new ArrayList<>();
           List<Object> objList = new ArrayList<>();
   
           addNumbers(numList); // 可以操作 Number 类型的列表
           addNumbers(objList); // 可以操作 Object 类型的列表
   
           System.out.println(numList); // 输出: [1, 2, 3]
           System.out.println(objList); // 输出: [1, 2, 3]
       }
   }
   ```

   





## 运算符

### 可变参数

通过 `...` 来表示一个方法可以接受可变数量的参数

```java
public class VarargsExample {
    // 定义一个可变参数的方法
    public static int sum(int... numbers) {
        int sum = 0;
        for (int num : numbers) {
            sum += num;
        }
        return sum;
    }

    public static void main(String[] args) {
        // 调用方法时可以传入任意数量的参数
        System.out.println(sum(1, 2, 3, 4)); // 输出 10
        System.out.println(sum(5, 10));      // 输出 15
    }
}
```





## 注解

不是程序本身，可以对程序作出解释

注解本身**不会直接影响代码的逻辑或行为**。它的作用主要是为工具、框架或者运行时提供**元数据**

### 注解的本质

```
The common interface extended by all annotation types
所有的注解类型都继承自这个普通的接口（Annotation）
```

<span style="color:red">注解的本质就是一个继承了 Annotation（注解） 接口的接口。</span>

### 常用注解

`@Override`

标记一个方法是从父类或接口中重写的方法。

帮助编译器检查是否正确地重写了父类或接口的方法。如果方法签名不匹配，编译器会报错。

`@Deprecated`

标记某个类、方法或字段不建议使用，可能在未来版本中被移除。

`@SuppressWarnings`

告诉编译器忽略特定的警告。

抑制编译器对特定类型的警告，比如未检查的操作（`unchecked`）、已废弃的方法（`deprecation`）等。

`@FunctionalInterface`

标记一个接口为函数式接口（只能有一个抽象方法）。

用于 Lambda 表达式或方法引用的实现。

```java
@FunctionalInterface
interface MyFunction {
    void execute();
}

public class Test {
    public static void main(String[] args) {
        MyFunction func = () -> System.out.println("Lambda Expression");
        func.execute();
    }
}
```

`@Target`

 用于指定自定义注解可以应用的程序元素（如类、方法、字段等）。

如果不指定 `@Target`，默认情况下，自定义注解可以应用于任何程序元素，这可能导致不必要的误用。

如果注解被用在不允许的地方，编译器会报错。

属性：

- `ElementType.TYPE`： 类、接口、枚举。
- `ElementType.METHOD`： 方法。
- `ElementType.FIELD`： 字段。
- `ElementType.PARAMETER`： 参数。
- `ElementType.CONSTRUCTOR`： 构造方法。
- `ElementType.LOCAL_VARIABLE`： 局部变量。

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Target;

@Target(ElementType.METHOD)
@interface MethodOnly {
}

public class Test {
    @MethodOnly
    public void testMethod() {
    }
}
```

### 元注解

负责注解其他注解，Java定义了4个标准的`meat-annotation`类型，用来对其他annotation类型作说明

`@Retention`

指定注解的生命周期，也就是注解保留的阶段。指定注解在什么级别可用。

取值类型（RetentionPolicy 枚举）:

- SOURCE: 注解只保留在源码中，编译时会被丢弃，不会存在于 `.class` 文件中。例如，`@Override` 就是一个只在源码阶段生效的注解。
- CLASS: 注解保留在 `.class` 文件中，但运行时不会加载到虚拟机中（默认行为）。
- RUNTIME：注解不仅保留在 `.class` 文件中，而且在运行时可以通过反射访问。（通常用于运行时需要读取注解的场景）

```java
// 注解只在运行时可见
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation {
    String value();
}
```

`@Target`

指定注解可以应用于哪些程序元素。如果没有指定，则默认可以使用在任意地方。

取值类型（ElementType 枚举）:

- **ANNOTATION_TYPE**: 注解只能用于另一个注解上（元注解）。
- **TYPE**: 注解可以用在类、接口（包括注解类型）或枚举上。
- **FIELD**: 注解可以用在字段或属性上。
- **METHOD**: 注解可以用在方法上。
- **PARAMETER**: 注解可以用在方法的参数上。
- **CONSTRUCTOR**: 注解可以用在构造方法上。
- **LOCAL_VARIABLE**: 注解可以用在局部变量上。
- **MODULE**: 注解可以用在模块上（Java 9 引入）。
- **PACKAGE**: 注解可以用在包声明上。
- **TYPE_USE**: 注解可以用在任何使用类型的地方（Java 8 引入）。

```java
// 注解只能标注在方法和字段上
@Target({ElementType.METHOD, ElementType.FIELD})
public @interface MyAnnotation {
    String value();
}
// 或者
@Target(value = {ElementType.METHOD, ElementType.FIELD})
public @interface MyAnnotation {
    String value();
}
```

`@Documented`

指定该注解是否会包含在 Javadoc 中。如果注解被 `@Documented` 修饰，那么在生成的 API 文档中会包含该注解的说明信息。

```java
// 这个注解会出现在 Javadoc 中
@Documented
public @interface MyAnnotation {
    String value();
}
```

`@Inherited`

指定该注解是否可以被子类继承。如果一个类使用了被 `@Inherited` 修饰的注解，那么它的子类将自动继承该注解。

只对类的注解有效，对方法、字段等无效。

`@Repeatable`

指定注解可以在同一个位置多次使用（Java 8 引入）。



### 自定义注解

使用`@interface`自定义注解时，自动继承了`java.lang.annotation.Annotation`接口

```java
// 定义一个简单的注解
public @interface MyAnnotation {
    String value(); // 定义了一个名为 MyAnnotation 的注解，包含一个名为 value 的属性。
}
```

#### 属性

注解的属性类似于接口中的方法，具有以下特点：

1. **属性声明格式**：`类型 属性名()`。
2. **默认值**：可以通过 `default` 关键字为属性设置默认值。如果注解中的属性有默认值，则使用注解时可以选择不提供该属性。
3. **本质**：抽象方法（`public abstract`），**可以有默认值（`default`）**，否则使用注解时必须显式赋值。
4. 支持的属性类型：
   - 基本类型：`int`、`float`、`double`、`boolean` 等。
   - `String`。
   - 枚举类型（enum）。
   - 注解类型。
   - 一维数组（如 `int[]`、`String[]` 等）。

```java
public @interface MyAnnotation {
    String name();               // 必须要设置的属性（没有默认值）
    int age() default 18;        // 可选属性，默认值为 18
    String[] tags() default {};  // 可选属性，默认值为空数组
}

@MyAnnotation(name = "John", age = 25, tags = {"developer", "java"})
public class MyClass {
}
```

5. 如果一个注解**只有一个名为 `value` 的属性**，那么在使用该注解时，可以**省略属性名 `value=`，只写属性值**。

   如果存在多个属性，即使有一个是 `value`，也**不能省略**属性名

   ```java
   public @interface MyAnnotation {
       String value();
   }
   ```

   ```java
   @MyAnnotation("Hello") // ✅ 合法，省略了 value=
   // 等价于@MyAnnotation(value = "Hello")
   public class MyClass {}
   ```



### 示例

定义注解 `@RunMe`，被这个注解标记的方法会被自动执行。

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface RunMe {
}
```

```java
class MyTestClass {
    @RunMe
    public void sayHello() {
        System.out.println("Hello");
    }

    @RunMe
    public void sayGoodbye() {
        System.out.println("Goodbye");
    }
}

public class RunMeTest {
    public static void main(String[] args) throws Exception {
        MyTestClass obj = new MyTestClass();
        Method[] methods = obj.getClass().getDeclaredMethods();
        for (Method method : methods) {
            if (method.isAnnotationPresent(RunMe.class)) {
                method.invoke(obj);
            }
        }
    }
}
```





## 反射

反射允许对成员变量，成员方法和构造函数的信息进行编程访问

```java
public class aTest1 {

    @t1(age = 20, address = {"street1", "USA", "JinHua"})
    public void a() {
        System.out.println("hello world");
    }

    public static void main(String[] args) {
        try {
            Class<aTest1> clazz = aTest1.class;
            Method method = clazz.getDeclaredMethod("a");
            if (method.isAnnotationPresent(t1.class)) {
                t1 ann = method.getAnnotation(t1.class);
                System.out.println("name：" + ann.name());
                System.out.println("age：" + ann.age());
                System.out.println("gender：" + ann.gender());
                System.out.print("address：");
                Arrays.stream(ann.address()).forEach(item -> System.out.print(item + " "));
            } else {
                System.out.println("no t1 annotation");
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

```java
public class SecretClass {
    private String secret = "我是一个秘密";

    private void hiddenMethod() {
        System.out.println("你不该看到我！");
    }
}

public class aTest2 {
    public static void main(String[] args) throws Exception {
        SecretClass sc = new SecretClass();
        Class clazz = sc.getClass();
        Field field = clazz.getDeclaredField("secret");
        field.setAccessible(true);
        System.out.println(field.get(sc));
        Method method = clazz.getDeclaredMethod("hiddenMethod");
        method.setAccessible(true);
        method.invoke(sc);
    }
}
```



### 静态 & 动态语言

| 特性                       | 动态语言                       | 静态语言                 |
| -------------------------- | ------------------------------ | ------------------------ |
| **类型检查时机**           | 运行时                         | 编译时                   |
| **是否需要显式声明类型**   | 不需要                         | 通常需要                 |
| **灵活性**                 | 高（易于快速开发）             | 相对较低（但更安全）     |
| **运行前能否发现类型错误** | 否                             | 是                       |
| **性能**                   | 较低（类型不确定，运行时检查） | 较高（类型已知，可优化） |

#### 动态语言

这些语言的变量类型是在**运行时**决定的：

- **Python**
- **JavaScript**
- **Ruby**
- **PHP**
- **Perl**
- **Lua**
- **Smalltalk**

#### 静态语言

这些语言的变量类型是在**编译时**决定的：

- **Java**
- **C**
- **C++**
- **C#**
- **Go**
- **Rust**
- **Kotlin**
- **Swift**
- **TypeScript**（虽然是 JavaScript 的超集，但具备静态类型）



#### Java动态特性

虽然 Java 是静态语言，但它支持一些“动态行为”，例如：

- **反射（Reflection）**：运行时查看/操作类和对象的结构。
- **动态类加载（ClassLoader）**：运行时加载类。
- **接口 + 多态**：允许在运行时决定调用哪个方法。



### 获取class对象

加载完类之后，在堆内存方法区中就产生了一个Class类型的对象（一个类只有一个Class对象）

1. `Class.forName(全类名)`
2. `类名.class`
3. `对象.getClass`

```java
public class aTest2 {
    public static void main(String[] args) throws ClassNotFoundException {

        Class clazz1 = Class.forName("test.dto.Student");

        Class clazz2 = Student.class;

        Student s1 = new Student();
        Class clazz3 = s1.getClass();

        System.out.println(clazz1 == clazz2);  // true
        System.out.println(clazz1 == clazz3);  // true
    }
}
```

4. 基本内置类型的包装类都有一个Type属性

```java
Class intClass = Integer.TYPE;
```

5. 获取父类类型

```java
Class c5 = c1.getSuperclass();
```





### 访问私有成员

默认情况下，反射无法访问私有成员，需要先调用 `setAccessible(true)` 来绕过权限检查。

```java
field.setAccessible(true);
```





### 获取构造方法

1. 获取所有的构造函数

   ```java
   Constructor<?>[] constructors = clazz.getConstructors(); // 公有构造函数
   Constructor<?>[] allConstructors = clazz.getDeclaredConstructors(); // 所有构造函数
   ```

2. 获取单个构造函数

   ```java
   Constructor<?> constructor = clazz.getConstructor(String.class, int.class); // 参数类型
   Constructor<?> declaredConstructor = clazz.getDeclaredConstructor(String.class);
   ```

3. 通过构造函数创建对象

   ```java
   Constructor<?> constructor = clazz.getConstructor(String.class, int.class);
   Object instance = constructor.newInstance("John", 25);
   ```

   

### 获取字段

1. 获取所有**公共**字段，包括继承的

   `getFields()`

   ```java
   Field[] publicFields = clazz.getFields(); // 公有字段
   Field[] allFields = clazz.getDeclaredFields(); // 所有字段
   ```

2. 获取当前类声明所有字段（包括私有的），不包括父类

   `getDeclaredFields()`

   ```java
   Car car = new Car();
   Class carClass = car.getClass();
   Field[] publicFields = carClass.getDeclaredFields();
   for (Field publicField : publicFields) {
       System.out.println(publicField.getName());
   }
   ```

3. 获取单个公共字段

   `getField(String name)`

   ```java
   Field field = clazz.getField("publicFieldName"); // 公有字段
   Field privateField = clazz.getDeclaredField("privateFieldName"); // 私有字段
   ```

4. 获取字段值

   ```java
   Field field = clazz.getDeclaredField("name");
   field.setAccessible(true); // 绕过访问限制
   Object value = field.get(obj); // 获取字段值
   ```

5. 设置字段值

   ```java
   Field field = clazz.getDeclaredField("name");
   field.setAccessible(true);
   field.set(obj, "New Value"); // 设置字段值
   ```

#### Field

- 获取类中定义的字段（成员变量），包括私有字段。
- 可以读取和修改对象的字段值。

##### 获取字段的基本信息

`field.getName()`：获取字段的名称（字符串形式）。

```java
String fieldName = field.getName();
```

`field.getType()`：获取字段的类型（`Class` 对象），例如 `int.class`、`String.class`。

```java
Class<?> fieldType = field.getType();
```

`field.getModifiers()`：获取字段的修饰符，以整数形式返回（如 `public`、`private`、`static` 等）。可结合 `Modifier` 类解析。

```java
int modifiers = field.getModifiers();
boolean isPublic = Modifier.isPublic(modifiers);
```

`field.getGenericType()`：获取字段的泛型类型（`Type` 对象），例如 `List<String>`。

```java
Type genericType = field.getGenericType();
```

##### 操作字段值

`field.get(Object obj)`：获取指定对象的此字段的值。

```java
Object value = field.get(obj);
```

```java
Car car = new Car();
car.setColor("red");
Class carClass = car.getClass();
Field[] publicFields = carClass.getDeclaredFields();
for (Field publicField : publicFields) {
    if(publicField.getName().equals("color")){
        publicField.setAccessible(true);
        System.out.println(publicField.get(car));
    }
}
```

`field.set(Object obj, Object value)`：设置指定对象的此字段的值为 `value`。

```java
field.set(obj, "New Value");
```

##### 控制字段的访问权限

`field.setAccessible(boolean flag)`：设置是否允许通过反射访问私有字段。

<span style="color:red">`private` / `protected` / 默认</span> 需要设置

```java
field.setAccessible(true); // 允许访问私有字段
```

##### 注解相关

`isAnnotationPresent(Class<? extends Annotation>)`

判断字段是否有某个注解，返回 `true/false`

```java
if (clazz.isAnnotationPresent(MyAnnotation.class)) {
    System.out.println("类上存在 @MyAnnotation");
}
```

`getAnnotation(Class<A>)`

获取指定类型的注解对象，如果不存在返回 `null`

```java
// getAnnotation
MyAnnotation classAnno = clazz.getAnnotation(MyAnnotation.class);
System.out.println("类注解 value: " + classAnno.value());
```

`getAnnotations()`

获取所有注解（包含继承的）

`getDeclaredAnnotations()`

获取本类声明的所有注解（不含继承）



**示例**

```java
import java.lang.annotation.*;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
@interface MyFieldAnnotation {
    String value();
    int count() default 1;
}
```

```java
class Person {
    @MyFieldAnnotation(value = "姓名字段", count = 2)
    private String name;

    private int age;
}
```

```java
import java.lang.reflect.*;

public class FieldAnnotationTest {
    public static void main(String[] args) throws Exception {
        Class<?> clazz = Person.class;
        Field[] fields = clazz.getDeclaredFields();

        for (Field field : fields) {
            System.out.println("字段名: " + field.getName());

            // 判断是否有注解
            if (field.isAnnotationPresent(MyFieldAnnotation.class)) {
                System.out.println("  有注解 @MyFieldAnnotation");

                // 获取注解对象
                MyFieldAnnotation annotation = field.getAnnotation(MyFieldAnnotation.class);

                // 获取注解内容
                System.out.println("  注解 value: " + annotation.value());
                System.out.println("  注解 count: " + annotation.count());
            } else {
                System.out.println("  无注解");
            }

            System.out.println("------------");
        }
    }
}
```









### 获取方法

1. `Method getMethod(String name, Class<?>... parameterTypes)`：获取**public** 方法

   方法名匹配、参数数量匹配、参数类型顺序匹配

   ```java
   public class Person {
       public void sayHello() {
           System.out.println("Hello");
       }
   
       public void setInfo(String name, int age) {
           System.out.println("Name: " + name + ", Age: " + age);
       }
   }
   ```

   ```java
   Method m1 = Person.class.getMethod("sayHello");
   ```

   ```java
   Method m2 = Person.class.getMethod("setInfo", String.class, int.class);
   ```

2. `Method[] getMethods()`获取所有 **public** 方法（包括继承的）

   ```java
   public class Test1 {
       public static void main(String[] args) throws Exception {
           Car car = new Car();
           Class clazz = car.getClass();
           Method[] methods = clazz.getMethods();
           for (Method method : methods) {
               System.out.println(method.getName());  // 当前类（Car）以及它的父类（Object）中所有的 public 方法，包括继承的！
           }
       }
   }
   
   class Car {
       public void Driving() {
           System.out.println("Driving");
       }
   
       public void Show(int i, String s) {
           System.out.println("i = " + i + ", s = " + s);
       }
   }
   ```

   来自 `Car` 类的方法：

   - `Driving`
   - `Show`

   来自 `Object` 类继承的方法（这些都是 `public` 的）：

   - `toString`
   - `equals`
   - `hashCode`
   - `getClass`
   - `notify`
   - `notifyAll`
   - `wait`
   - `wait(long)`
   - `wait(long, int)`

3. `Method getDeclaredMethod(String name, Class<?>... parameterTypes)`

   获取当前类中指定名称的方法（包括 private）

   ```java
   public class Test1 {
       public static void main(String[] args) throws Exception {
           Car car = new Car();
           Class clazz = car.getClass();
           Method drivingMethod = clazz.getDeclaredMethod("Driving");
           Method showMethod = clazz.getDeclaredMethod("Show", int.class, String.class);
   
           // 允许访问 private 方法
           drivingMethod.setAccessible(true);
           showMethod.setAccessible(true);
   		
           drivingMethod.invoke(car);
           showMethod.invoke(car, 10, "Hello");
       }
   }
   
   class Car {
       private void Driving() {
           System.out.println("Driving");
       }
   
       private void Show(int i, String s) {
           System.out.println("i = " + i + ", s = " + s);
       }
   }
   ```

4. `Method[] getDeclaredMethods()`

   获取当前类中定义的所有方法（包括 private）

   

#### Method

##### 基本信息获取

1. `getName()`

   获取方法名。

   ```java
   System.out.println(method.getName()); // 输出: sayHello
   ```

2. `getReturnType()`

   获取返回值类型。

   ```java
   Class<?> returnType = method.getReturnType();
   System.out.println(returnType.getName()); // 输出: void 或 java.lang.String 等
   ```

3. `getParameterTypes()`

   获取参数类型数组。

   ```java
   Class<?>[] params = method.getParameterTypes();
   for (Class<?> param : params) {
       System.out.println(param.getName());
   }
   ```

4. `getDeclaringClass()`

   获取声明该方法的类。

   ```java
   System.out.println(method.getDeclaringClass().getName());
   ```

5. `getModifiers()`

   获取修饰符（如 public、private），返回一个 `int`，可以用 `Modifier.toString()` 转成字符串。

   ```java
   int modifiers = method.getModifiers();
   System.out.println(Modifier.toString(modifiers)); // 输出: public, private 等
   ```

##### 参数相关

1. `getParameterCount()getParameterCount()`

   获取参数个数。

   ```java
   System.out.println(method.getParameterCount()); // 输出: 2
   ```

2. `getParameters()`

   返回 `Parameter[]`，可以获取每个参数的名字

   ````java
   Parameter[] parameters = method.getParameters();
   for (Parameter p : parameters) {
       System.out.println(p.getName()); // name1, name2 等
   }
   ````

3. `getGenericParameterTypes()`

   获取泛型参数类型（用于泛型方法）。

   ```java
   Type[] types = method.getGenericParameterTypes();
   for (Type type : types) {
       System.out.println(type.getTypeName());
   }
   ```

##### 调用方法

1. `invoke(Object obj, Object... args)`

   - 如果方法为静态方法，形参obj可为null

   ```java
   drivingMethod.invoke(car);
   showMethod.invoke(car, 10, "Hello");
   ```

##### 权限控制

1. `setAccessible(boolean flag)`

   设置是否允许访问私有方法。

   ```java
   method.setAccessible(true); // 允许访问 private 方法
   ```

##### 注解相关

1. `getAnnotations()`

   获取所有注解（包括继承的）。

   ```java
   Annotation[] annotations = method.getAnnotations();
   for (Annotation a : annotations) {
       System.out.println(a.toString());
   }
   ```

2. `getDeclaredAnnotations()`

   获取当前方法上定义的所有注解（不包含继承的）。

   ```java
   Annotation[] annotations = method.getDeclaredAnnotations();
   ```

3. `isAnnotationPresent(Class<? extends Annotation>)`

   检查是否存在某个注解。

   ```java
   if (method.isAnnotationPresent(MyAnnotation.class)) {
       System.out.println("方法上有 @MyAnnotation");
   }
   ```



### 获取构造器

构造器（Constructor）是 Java 类中的一种特殊方法，它的作用是**在创建对象时初始化对象的状态**（即为对象的属性赋初值、执行初始化逻辑）。

示例

```java
class Student {
    String name;
    int age;

    // 无参构造器
    public Student() {
        name = "默认名";
        age = 18;
    }

    // 有参构造器
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```



### 反射创建对象

调用Class对象的`newInstance()`方法

- 类必须有一个无参的构造器，只能用于无参构造器
- 类的构造器的访问权限需要足够

```java
public class Test1 {
    public static void main(String[] args) throws Exception {
        // 获取Class对象
        Class carClass = Car.class;

        // 构造一个Car对象
        Car c = (Car) carClass.newInstance();
    }
}

class Car {
    private int speed;
    private String name;

    public Car() {
    }

    public Car(int speed, String name) {
        this.speed = speed;
        this.name = name;
    }
}
```

可以通过反射获取set函数设置值

```java
public class Test1 {
    public static void main(String[] args) throws Exception {
        Class clazz = Car.class;
        Car car = (Car) clazz.newInstance();
        Method setSpeed = car.getClass().getMethod("setSpeed", int.class);
        setSpeed.invoke(car, 100);
        Method setName = car.getClass().getMethod("setName", String.class);
        setName.invoke(car, "BMW");
        System.out.println(car.getSpeed());
        System.out.println(car.getName());

    }
}

class Car {
    private int speed;
    private String name;

    public Car() {
    }

    public Car(int speed, String name) {
        this.speed = speed;
        this.name = name;
    }

    public int getSpeed() {
        return speed;
    }

    public void setSpeed(int speed) {
        this.speed = speed;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

调用`Constructor`的`newInstance`方法

使用 `Class.getConstructor(...)` 或 `getDeclaredConstructor(...)` 获取构造器对象

调用 `newInstance(...)` 传入参数创建对象

```java
public class Test1 {
    public static void main(String[] args) throws Exception {
        // 获取Class对象
        Class carClass = Car.class;

        // 获取带参数的构造器
        Constructor constructor = carClass.getConstructor(int.class, String.class);

        // 创建对象
        Car car = (Car) constructor.newInstance(100, "BMW");
    }
}

class Car {
    private int speed;
    private String name;

    public Car() {
    }
}
```



### 获取注解

#### 常用方法

| 方法名                                                       | 定义位置                                     | 作用                                   |
| ------------------------------------------------------------ | -------------------------------------------- | -------------------------------------- |
| `isAnnotationPresent(Class<? extends Annotation> annotationClass)` | `Class`, `Method`, `Field`, `Constructor` 等 | 判断是否标注了某个注解                 |
| `getAnnotation(Class<T> annotationClass)`                    | 同上                                         | 获取指定类型的注解实例                 |
| `getAnnotations()`                                           | 同上                                         | 获取所有注解（包括继承的）             |
| `getDeclaredAnnotations()`                                   | 同上                                         | 获取当前声明的所有注解（不包括继承的） |
| `getAnnotationsByType(Class<T> annotationClass)`             | 同上                                         | 获取重复注解（Java 8+ 支持）           |



#### 示例

```java
@Retention(java.lang.annotation.RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
public @interface Student {
    String name() default "";

    int age() default 0;
}
```

```java
public class Test2 {
    public static void main(String[] args) {
        Class c = TestClass.class;
        Field[] fields = c.getDeclaredFields();
        for (Field field : fields) {
            if (field.isAnnotationPresent(Student.class)) {  // 判断是否包含Student注解
                Student student = field.getAnnotation(Student.class);  // 生成一个注解的代理对象实例
                // 当你调用 student.name() 或 student.age()，其实是访问这个代理对象中保存的值。
                System.out.println(student.name() + " " + student.age());
            }
        }
    }
}

class TestClass {
    @Student(name = "John", age = 20)
    private int x;
    @Student(name = "Jane", age = 23)
    private int y;
}
```



在你调用 `field.getAnnotation(Student.class)` 的时候，**JVM 会在运行时自动生成一个注解的代理对象实例**，你就可以像操作普通对象那样调用它的方法（例如 `student.name()`）。

当你调用 `student.name()` 或 `student.age()`，其实是访问这个代理对象中保存的值。





## 类的加载和初始化

### Java 类生命周期

Java 类从被加载到内存中到最终可以被使用，主要经历以下几个阶段：

1. **类加载（Loading）**

   类加载是指将 `.class` 字节码文件从文件系统或网络中读取到内存中，并生成一个 `java.lang.Class` 对象。

   JVM 通过类加载器读取 `.class` 文件字节码，生成 `Class` 对象。

   ```java
   ClassLoader loader = MyClass.class.getClassLoader();
   ```

   - 由 `ClassLoader`（类加载器）完成，主要负责查找、读取 `.class` 文件并定义类。
   - 加载后，类的字节码内容会存放在方法区（Java 8 之前）或元空间（Java 8 及之后）。
   - 元空间不再使用 JVM 堆内存，而是使用 **本地内存（native memory）**

2. **类链接（Linking）**

   - 验证（Verification）
   - 准备（Preparation）
   - 解析（Resolution）

3. **初始化（Initialization）**

   初始化阶段是执行 `<clinit>` 方法，也就是类构造器的阶段。

   **触发条件：** JVM 遇到首次主动使用该类（如下几种情况）时，会触发初始化。

   主动使用的几种情况：

   | 场景                       | 说明                      |
   | -------------------------- | ------------------------- |
   | 创建类的实例               | `new` 一个对象时          |
   | 访问类的静态变量或静态方法 | 非 final 的 `static` 成员 |
   | 使用反射调用类的方法       | `Class.forName()`         |
   | 初始化子类                 | 父类会先初始化            |

4. **使用（Using）**

5. **卸载（Unloading）**



#### 类加载

JVM通过**类加载器**读取 `.class` 文件或其他源（网络、加密文件等）

调用 `defineClass()` 转换为 `Class` 对象

将类的定义信息保存到方法区（Java 8 以前）或元空间（Java 8+）

##### 类加载器

**类加载器（ClassLoader）**是负责将 `.class` 文件加载进 JVM 的组件。它根据类的全限定名查找字节码，并将其加载为 `Class` 对象。

类加载器的基本结构

```
ClassLoader (抽象类)
  ├── Bootstrap ClassLoader （启动类加载器）✅
  ├── Extension ClassLoader（扩展类加载器）✅
  └── Application ClassLoader（系统类加载器）✅
        └── 自定义类加载器（可扩展）
```

| 加载器                                            | 作用                                              | 加载路径               | 说明                                               |
| ------------------------------------------------- | ------------------------------------------------- | ---------------------- | -------------------------------------------------- |
| **Bootstrap ClassLoader**                         | 加载 Java 核心类，如 `java.lang.*`，`java.util.*` | `$JAVA_HOME/lib`       | 启动类加载器，用 C++ 实现，在 Java 中表现为 `null` |
| **Extension ClassLoader**                         | 加载扩展目录类，如 `javax.*`                      | `$JAVA_HOME/lib/ext`   | 扩展类加载器                                       |
| **Application ClassLoader**（System ClassLoader） | 加载用户类（开发者写的类）                        | `classpath` 指定的路径 | 系统类加载器                                       |

```java
public class classTest1 {
    public static void main(String[] args) throws ClassNotFoundException {
        // 获取系统类加载器
        ClassLoader systemClassLoader = ClassLoader.getSystemClassLoader();
        System.out.println(systemClassLoader);  // jdk.internal.loader.ClassLoaders$AppClassLoader@63947c6b

        // 获取系统类加载器的父类加载器-->扩展类加载器
        ClassLoader parentClassLoader = systemClassLoader.getParent();
        System.out.println(parentClassLoader);  // jdk.internal.loader.ClassLoaders$PlatformClassLoader@776ec8df

        // 获取扩展类加载器的父类加载器-->根类加载器
        ClassLoader parentParentClassLoader = parentClassLoader.getParent();
        System.out.println(parentParentClassLoader);  // null
        
        ClassLoader classLoader = classTest1.class.getClassLoader();
        System.out.println(classLoader);  // jdk.internal.loader.ClassLoaders$AppClassLoader@63947c6b

        ClassLoader classLoader1 = Class.forName("java.lang.Object").getClassLoader();
        System.out.println(classLoader1);
    }
}
```



##### 双亲委派机制

双亲委派机制是一种**类加载委托机制**：当一个类加载器需要加载某个类时，它并不会自己去尝试加载，而是**先将请求交给父类加载器**去加载。只有当父类加载器无法加载该类时，子类加载器才会尝试自己加载。

```
AppClassLoader
    ↓
ExtClassLoader
    ↓
BootstrapClassLoader
```

###### <span style="color:blue">加载流程</span>

假设你在代码中写了：

```java
Class.forName("java.lang.String");
```

加载过程如下：

1. 当前类加载器收到加载 `java.lang.String` 的请求。
2. 它先把请求交给父加载器。
3. 父加载器再继续往上交给它的父加载器。
4. 最终交给 **启动类加载器**。
5. 如果启动类加载器能加载（比如 `String.class` 在 `rt.jar` 中），则加载成功。
6. 如果启动类加载器不能加载（比如是用户自定义类），子加载器才尝试，才会逐层返回由下层加载器尝试加载。

这个过程就像“**自底向上的请求，自顶向下的加载**”。

###### <span style="color:blue">为什么使用双亲委派机制</span>

核心思想：防止“核心类”被篡改或伪造。

没有双亲委派机制时：

假设你写了一个类，名字就叫：

```java
package java.lang;

public class String {
    public String() {
        System.out.println("Hacked String!");
    }
}
```

你把它放在你的项目里（classpath 下），如果没有双亲委派机制，**应用类加载器就可能直接加载你这个“伪造”的 String 类**。

这样一来：

- 你就可以“冒充”JDK的核心类。
- 任何使用 `java.lang.String` 的代码都有可能调用你写的假类。
- 会造成 **严重安全隐患**。

有双亲委派机制时

当你请求加载 `java.lang.String`：

1. 应用类加载器会把请求交给它的父类加载器（扩展类加载器）。
2. 扩展类加载器再往上交给启动类加载器。
3. 启动类加载器发现它自己已经加载 `rt.jar` 中的 String 类，返回这个类。
4. 子类加载器不会加载你写的那个“伪造”的 String。

这样，你的伪造类就不会被加载，就被“拦住了”。

###### <span style="color:blue">自定义类加载器破坏双亲委派</span>

有些框架（如 Tomcat、OSGi、JDBC SPI）为了支持热部署、插件机制，**会破坏双亲委派机制**，例如：

- 先尝试自己加载（child-first）
- 加载不到再委托父加载器

这是为了实现更灵活的类加载需求，但也带来了更多的风险和复杂性。



#### 类初始化

在触发初始化之前，JVM 会判断是否为“**主动引用**”。只有在**主动引用**的情况下，类才会被**立即初始化**，否则可能是“**被动引用**”，类不会被初始化。

| 类型                              | 是否触发类的初始化？ | 说明                             |
| --------------------------------- | -------------------- | -------------------------------- |
| **主动引用（Active Reference）**  | ✅ 会触发初始化       | 对类的直接使用                   |
| **被动引用（Passive Reference）** | ❌ 不会触发初始化     | 对类的间接使用或编译器优化的情形 |

##### 类主动引用

以下这些行为会触发类的初始化：

1. **创建类的实例（new）**

```java
Test test = new Test();  // 主动使用类，触发初始化
```

2. **访问类的静态变量（非 `final`）或静态方法**

```java
System.out.println(Test.value);  // 访问静态变量（非编译期常量）
Test.staticMethod();             // 调用静态方法
```

3. **使用反射操作类**

```java
Class.forName("com.example.Test");  // 直接触发初始化
```

4. **初始化子类时，父类也会被初始化**

```java
class Parent {
    static { System.out.println("Parent init"); }
}

class Child extends Parent {
    static { System.out.println("Child init"); }
}

Child c = new Child();  // 会先初始化 Parent，再初始化 Child
```

5. **调用 `main()` 方法所在的类**

```java
public class Main {
    public static void main(String[] args) {
        // 该类本身会首先被初始化
    }
}
```

###### 示例

```java
public class classTest1 {
    static {
        System.out.println("Main 类被加载");
    }

    public static void main(String[] args) {
        Father f = new Son();
    }
}

class Father {
    static {
        System.out.println("父类被加载");
    }
}

class Son extends Father {
    static {
        System.out.println("子类被加载");
    }
}
```

输出：

```
Main 类被加载
父类被加载
子类被加载
```



##### 类被动引用

1. **访问类的静态常量（`final` 修饰的编译期常量）**

```java
public class Test {
    public static final int VALUE = 123;  // 编译期常量
    static { System.out.println("Test init"); }
}

System.out.println(Test.VALUE);  // 不会触发 Test 初始化
```

> 因为 `VALUE` 是 `final` 且是基本类型，编译器会在编译阶段将其值替换，根本不会加载 `Test` 类。

2. **通过数组定义引用类**

```java
Test[] array = new Test[10];  // 不会触发 Test 初始化
```

JVM 只为数组分配内存，并不会初始化引用的类。

3. **子类访问父类的静态变量**

```java
class Parent {
    static { System.out.println("Parent init"); }
    static int value = 123;
}

class Child extends Parent {
    static { System.out.println("Child init"); }
}

System.out.println(Child.value);  // 只初始化 Parent，不初始化 Child
```

4. **通过反射获取 `Class` 对象，但不实例化**

```java
Class cls = Test.class;  // 不会触发 Test 初始化
```

仅获取 `Class` 对象并不会触发初始化，除非使用 `Class.forName()`。











## FIle类

在 Java 中，`java.io.File` 类是用于表示文件或目录（文件夹）路径名的抽象表示。**一个 `File` 对象可以表示磁盘上的一个实际文件或目录，也可以表示尚不存在的路径**。

<span style="color:blue">`File` 仅仅是路径的抽象表示，它本身不表示文件内容，也不会直接创建文件或目录。</span>

### 创建file对象

```java
import java.io.File;

public class FileExample {
    public static void main(String[] args) {
        // 方式1：使用路径字符串
        File file1 = new File("example.txt");

        // 方式2：使用父路径字符串和子路径字符串
        File file2 = new File("/home/user", "example.txt");

        // 方式3：使用父File对象和子路径字符串
        File parentDir = new File("/home/user");
        File file3 = new File(parentDir, "example.txt");
    }
}
```

### 常用方法

#### 路径相关

| 方法                 | 说明                                          |
| :------------------- | :-------------------------------------------- |
| `getName()`          | 返回文件或目录的名称                          |
| `getPath()`          | 返回创建 `File` 对象时使用的路径字符串        |
| `getAbsolutePath()`  | 返回文件或目录的绝对路径                      |
| `getCanonicalPath()` | 返回规范化的绝对路径，处理符号链接、`.`、`..` |
| `getParent()`        | 返回父目录的路径字符串                        |
| `getParentFile()`    | 返回父路径对应的File对象                      |

#### 文件或目录属性

| 方法                                        | 说明                               |
| :------------------------------------------ | :--------------------------------- |
| `exists()`                                  | 判断文件或目录是否存在             |
| `isFile()`                                  | 判断是否是一个文件                 |
| `isDirectory()`                             | 判断是否是一个目录                 |
| `length()`                                  | 文件的字节大小（目录返回未指定值） |
| `lastModified()`                            | 文件最后修改时间（毫秒时间戳）     |
| `canRead()` / `canWrite()` / `canExecute()` | 是否可读/写/执行                   |

#### 文件或目录操作

| 方法                  | 说明                             |
| :-------------------- | :------------------------------- |
| `createNewFile()`     | 创建一个新文件（如果不存在）     |
| `mkdir()`             | 创建单层目录                     |
| `mkdirs()`            | 创建多层目录（包括必要的父目录） |
| `delete()`            | 删除文件或空目录                 |
| `renameTo(File dest)` | 重命名文件或目录                 |

#### 目录操作

| 方法          | 说明                                           |
| :------------ | :--------------------------------------------- |
| `list()`      | 返回当前目录下所有文件和目录的名称数组         |
| `listFiles()` | 返回当前目录下所有文件和目录的 `File` 对象数组 |















## IO流

IO流（Input/Output Stream）是 Java 中用于处理数据传输的机制，主要目的是在程序与外部设备（如文件、网络、内存、键盘等）之间进行数据的 **输入**（Input）和 **输出**（Output）。

### 分类

#### **按数据单位划分**

| 类型   | 描述                           | 基本类                         | 适用范围                       |
| :----- | :----------------------------- | :----------------------------- | ------------------------------ |
| 字节流 | 以字节（8 bit）为单位传输数据  | `InputStream` / `OutputStream` | 所有类型的文件                 |
| 字符流 | 以字符（16 bit）为单位传输数据 | `Reader` / `Writer`            | 纯文本文件（记事本能直接打开） |

- **字节流**适合处理所有类型的数据，如图片、视频、音频、二进制文件。
- **字符流**主要用于处理文本数据，自动处理字符编码。`.txt`、`.java`、`.xml`、`.html`

#### **按流向划分**

| 类型   | 描述           |
| :----- | :------------- |
| 输入流 | 读入数据到程序 |
| 输出流 | 从程序写出数据 |





#### 字节流

##### InputStream

**InputStream** 是所有字节输入流的**抽象基类**

###### 子类

常见的 InputStream 子类

**FileInputStream**

- 从文件中读取字节数据
- 示例：`FileInputStream fis = new FileInputStream("file.txt");`

**ByteArrayInputStream**

- 从字节数组中读取数据
- 示例：`ByteArrayInputStream bais = new ByteArrayInputStream(byteArray);`

**PipedInputStream**

- 从管道中读取数据，通常与PipedOutputStream配对使用
- 用于线程间通信

**BufferedInputStream**

- 为其他输入流提供缓冲功能，提高读取效率
- 示例：`BufferedInputStream bis = new BufferedInputStream(inputStream);`

**DataInputStream**

- 允许应用程序以与机器无关的方式从底层输入流中读取基本Java数据类型
- 示例：`DataInputStream dis = new DataInputStream(inputStream);`

**ObjectInputStream**

- 用于读取之前用ObjectOutputStream写入的对象
- 实现了对象的反序列化
- 示例：`ObjectInputStream ois = new ObjectInputStream(inputStream);`

**FilterInputStream**

- 所有过滤输入流的基类
- 其子类包括BufferedInputStream、DataInputStream等

**SequenceInputStream**

- 将多个输入流串联成一个输入流
- 示例：`SequenceInputStream sis = new SequenceInputStream(is1, is2);`



##### FileInputStream

1. 创建对象

   如果文件不存在，直接报错

```java
public class FileInputDemo1 {
    public static void main(String[] args) throws Exception {
        FileInputStream fis = new FileInputStream(".\\src\\IODemo\\output.txt");
        int data = 0;
        while ((data = fis.read()) != -1) {
            System.out.print((char) data);
        }
        fis.close();
    }
}
```



###### 方法

**构造函数**

| 构造方法                             | 描述                           |
| :----------------------------------- | :----------------------------- |
| `FileInputStream(String name)`       | 通过文件名字符串创建输入流对象 |
| `FileInputStream(File file)`         | 通过 `File` 对象创建输入流对象 |
| `FileInputStream(FileDescriptor fd)` | 通过文件描述符创建输入流对象   |

**重要方法**

| 方法                                   | 描述                                                         |
| :------------------------------------- | :----------------------------------------------------------- |
| `int read()`                           | 读取一个字节，返回值是 0-255 范围的字节值，或者返回 -1 表示读到末尾 |
| `int read(byte[] b)`                   | 读取多个字节，存入数组，返回实际读取的字节数，或 -1          |
| `int read(byte[] b, int off, int len)` | 从流中读取最多 len 个字节，存储到数组 b 的 off 位置          |
| `long skip(long n)`                    | 跳过 n 个字节                                                |
| `int available()`                      | 返回可读取（不阻塞）的字节数                                 |
| `void close()`                         | 关闭流，释放资源                                             |

一次读取一个字节速度太慢

`int read(byte[] b)`一次读取一个字节数组的数据，每次读取尽可能把数组装满



###### 循环读取

```java
public class FileInputDemo2 {
    public static void main(String[] args) throws Exception {
        // 创建输入输出流
        FileInputStream fis = new FileInputStream(".\\src\\IODemo\\output.txt");
        FileOutputStream fos = new FileOutputStream(".\\src\\IODemo\\output2.txt");
        // 读写数据
        int data = 0;
        // 读取一个字节，赋给 data
        // 判断 data 是否 != -1
        while ((data = fis.read()) != -1) {
            fos.write(data);
        }
        // 释放资源
        // 先开的最后关闭
        fos.close();
        fis.close();
    }
}
```

```java
public class FileInputDemo1 {
    public static void main(String[] args) throws Exception {
        FileInputStream fis = new FileInputStream(".\\src\\IODemo\\output.txt");
        int len = 0;  // 每次从文件中实际读取的字节数量
        byte[] b = new byte[1024];
        while ((len = fis.read(b)) != -1) {
            System.out.println(new String(b, 0, len));  // 读取len个字节，存储到数组b的0位置
        }
        fis.close();
    }
}
```











##### FileOutputStream

操作本地文件的字节输出流，可以把程序中的数据写到本地文件中

1. 创建字节输出流对象
   - 如果文件已存在会清空文件
   - 文件不存在会创建一个新的文件，文件夹不存在会报错
   - 
2. 写数据
   - int参数是写入ASCII对应的字符
3. 释放资源

```java
public class FileOutputDemo1 {
    public static void main(String[] args) throws Exception {
        // 1. 创建 FileOutputStream 对象
        FileOutputStream fos = new FileOutputStream(".\\src\\IODemo\\output.txt");
        // 2. 写入数据
        fos.write("hello world!".getBytes());
        // 3. 释放资源
        fos.close();
    }
}
```

```java
public class FileOutputWithFileDemo {
    public static void main(String[] args) {
        // 1. 创建 File 对象（指定文件路径）
        File file = new File("./src/IODemo/output.txt");

        FileOutputStream fos = null;
        try {
            // 2. 创建 FileOutputStream 对象，传入 File
            fos = new FileOutputStream(file);

            // 3. 写入数据
            String data = "Hello, this is written by File object!";
            byte[] bytes = data.getBytes(); // 字符串转字节数组
            fos.write(bytes);

            // 写完后记得刷新（可选）
            fos.flush();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            // 4. 最后一定要关闭流，释放资源
            if (fos != null) {
                try {
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

###### 方法

**构造方法**

| 方法签名                                        | 说明                                                         |
| :---------------------------------------------- | :----------------------------------------------------------- |
| `FileOutputStream(String name)`                 | 创建文件输出流，向指定文件写数据。如果文件不存在，会创建；如果存在，会**清空原内容**。 |
| `FileOutputStream(String name, boolean append)` | 创建文件输出流。`append=true` 时，不会清空，而是在文件末尾追加写入。 |
| `FileOutputStream(File file)`                   | 传入一个 `File` 对象来创建流。                               |
| `FileOutputStream(File file, boolean append)`   | 同上，支持追加模式。                                         |
| `FileOutputStream(FileDescriptor fdObj)`        | 通过已有的文件描述符创建流。**很少用**，用于底层控制。       |

**写入方法**

| 方法签名                                 | 作用                                                   | 备注                                       |
| :--------------------------------------- | :----------------------------------------------------- | :----------------------------------------- |
| `void write(int b)`                      | 写入**单个字节**（低8位有效），即写一个0~255的数字。   | 写一个字节，虽然参数是 `int`，但只用低位。 |
| `void write(byte[] b)`                   | 写入整个**字节数组**。                                 | 比较常用，写多字节数据。                   |
| `void write(byte[] b, int off, int len)` | 写**byte数组的一部分**，从 `off` 开始写 `len` 个字节。 | 灵活控制写入范围。                         |

**资源管理方法**

| 方法签名       | 作用                               | 备注                                 |
| :------------- | :--------------------------------- | :----------------------------------- |
| `void flush()` | 强制把内存中缓存的数据写到文件里。 | 避免数据丢失，尤其在写完但没关流时。 |
| `void close()` | 关闭流，释放资源。                 | 必须调用，否则资源泄露。             |

#### 字符流

##### FilerReader

###### 构造方法

| 构造方法                        | 说明                            |
| :------------------------------ | :------------------------------ |
| `FileReader(String fileName)`   | 通过文件路径创建 FileReader     |
| `FileReader(File file)`         | 通过 `File` 对象创建 FileReader |
| `FileReader(FileDescriptor fd)` | 通过文件描述符创建 FileReader   |

###### 主要方法

| 方法                                            | 说明                                                     |
| :---------------------------------------------- | :------------------------------------------------------- |
| `int read()`                                    | 读取单个字符，返回字符的 Unicode 编码，读到末尾返回 `-1` |
| `int read(char[] cbuf)`                         | 读取多个字符到字符数组中，返回实际读取的字符数           |
| `int read(char[] cbuf, int offset, int length)` | 从指定位置开始读取指定长度的字符                         |
| `void close()`                                  | 关闭流，释放资源                                         |

###### `int read(char[] cbuf)`

一个char占2字节(16位)

java内部使用`utf-16`编码表示字符串

当使用FilerReader从utf-8读取编码时

1. 首先，从文件读取原始字节数据（例如一个中文字符的 3 个 UTF-8 编码字节）
2. 然后，解码器识别这 3 个字节组成一个 UTF-8 编码的中文字符
3. 最后，将这个字符转换为 Java 内部的 UTF-16 表示（1 个 `char`，2 字节）



###### 示例

```java
public static void main(String[] args) throws Exception {
    FileReader fr = new FileReader(".\\src\\IODemo\\output.txt");
    int ch;
    while ((ch = fr.read()) != -1) {
        System.out.print((char) ch);
    }
    fr.close();
}
```

```java
public static void main(String[] args) throws Exception {
    FileReader fr = new FileReader(".\\src\\IODemo\\output.txt");
    char[] chars = new char[1024];
    int len = 0;
    while ((len = fr.read(chars)) != -1) {
        System.out.print(new String(chars, 0, len));
    }
    fr.close();
}
```

###### 缓冲区

在关联文件时，会创建缓冲区（长度为8192的字节数组）

读取数据：

1. 判断缓冲区是否有数据可以获取
2. 缓冲区没有数据：
   - 从文件中获取数据，装到缓冲区中，每次尽可能装满缓冲区
   - 如果文件里也没有数据了，返回 `-1`
3. 缓冲区有数据：从缓冲区读取
   - 空参read：一次读取一个字节，遇到中文一次读多个字节，并解码成十进制返回
   - 有参read：读取字节，解码，强转。强转后的字符放到数组中。



##### FileWriter



###### 常用方法

| 方法                                   | 说明                                 |
| :------------------------------------- | :----------------------------------- |
| `write(int c)`                         | 写入单个字符                         |
| `write(char[] cbuf)`                   | 写入字符数组                         |
| `write(char[] cbuf, int off, int len)` | 写入字符数组的一部分                 |
| `write(String str)`                    | 写入字符串                           |
| `write(String str, int off, int len)`  | 写入字符串的一部分                   |
| `flush()`                              | 刷新流。把缓冲区内容强制写出到文件中 |
| `close()`                              | 关闭流，释放资源（关闭前会自动flush) |



###### 示例

```java
public static void main(String[] args) throws Exception {
    char[] chars = new char[1024];
    try (
            FileReader fileReader = new FileReader(".\\src\\IODemo\\output.txt");
            FileWriter fileWriter = new FileWriter(".\\src\\IODemo\\output3.txt", true);
    ) {
        int len = 0;
        while ((len = fileReader.read(chars)) != -1) {
            fileWriter.write(chars, 0, len);
        }
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

```java
public static void main(String[] args) {
    String[] str = {"Hello", "World", "Java", "Programming"};
    try (FileWriter fw = new FileWriter(".\\src\\IODemo\\output4.txt")) {
        for (String s : str) {
            fw.write(s + "\r\n");
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```



###### 缓冲区

长度为8192的字节数组

写数据时，从缓冲区把数据写入：

1. 缓存区数据装满了
2. flush



#### Reader

在 Java 中，`Reader` 是 **字符流**（character streams）体系的<span style="color:red">基础抽象类</span>，属于 `java.io` 包。它专门用于读取**字符（char）**数据。

`Reader` 是 **抽象类**

设计目的是为了**处理字符数据**，而不是原始字节（byte）数据。

`FileReader` 是 `Reader` 的子类，专门用于从**文件**中读取字符数据。

##### 核心方法

| 方法                                      | 说明                                                     |
| :---------------------------------------- | :------------------------------------------------------- |
| `int read()`                              | 读取单个字符，返回字符的 int 表示，若到达流末尾返回 -1。 |
| `int read(char[] cbuf)`                   | 读取多个字符到数组中，返回读取的字符数量。               |
| `int read(char[] cbuf, int off, int len)` | 将字符读入数组的指定部分。                               |
| `void close()`                            | 关闭流，释放资源。                                       |



##### InputStreamReader

`InputStreamReader` 是 Java 中的一个类，位于 `java.io` 包中，用于将 **字节输入流（InputStream）转换为字符输入流（Reader）**。

这使得我们可以读取以字节形式输入的数据，并将其正确地转换为字符，尤其适用于处理文本数据（如文件、网络流）时涉及字符编码的问题。

<span style="color:red">现在在实际开发中读取文本文件，推荐并且普遍使用的是 `InputStreamReader`（+ `BufferedReader`）</span>

```java
public class InputStreamReader extends Reader
```

###### 主要作用

- 将字节流转换为字符流
- 处理字符编码问题（如 UTF-8、GBK 等）
- 通常与 `BufferedReader` 结合使用，提高读取效率

```
字节流（InputStream）
        ↓
InputStreamReader（编码转换）
        ↓
字符流（Reader）
```

###### 为什么不直接使用FIleReader

1. `FIleReader`、`FileWriter`不能指定编码，默认使用系统编码

   而 `InputStreamReader` 和 `OutputStreamWriter` 支持

   ```java
   new InputStreamReader(new FileInputStream("a.txt"), "UTF-8");
   ```

2. `FileReader` / `FileWriter` 已经过时（不推荐使用）

###### 构造方法

```java
// 使用平台默认字符集
public InputStreamReader(InputStream in)

// 指定字符集名称（如 "UTF-8"、"GBK"）
public InputStreamReader(InputStream in, String charsetName)
InputStreamReader reader = new InputStreamReader(new FileInputStream("file.txt"), "UTF-8");

// 指定 Charset 对象
public InputStreamReader(InputStream in, Charset cs)

// 指定 CharsetDecoder 对象
public InputStreamReader(InputStream in, CharsetDecoder dec)
```

###### 方法

| 方法签名                                  | 说明                                                  |
| ----------------------------------------- | ----------------------------------------------------- |
| `int read()`                              | 读取一个字符，返回字符的 Unicode 编码，或 -1 表示 EOF |
| `int read(char[] cbuf, int off, int len)` | 批量读取字符                                          |
| `void close()`                            | 关闭流，释放资源                                      |

###### 示例

```java
InputStream input = new FileInputStream("example.txt");
InputStreamReader reader = new InputStreamReader(input, "UTF-8");
```

```java
import java.io.*;

public class Main {
    public static void main(String[] args) {
        try {
            // 创建 FileInputStream（字节流）
            FileInputStream fis = new FileInputStream("example.txt");

            // 创建 InputStreamReader（字节流 -> 字符流），指定 UTF-8 编码
            InputStreamReader reader = new InputStreamReader(fis, "UTF-8");

            // 读取字符
            int data = reader.read();
            while (data != -1) {
                System.out.print((char) data);
                data = reader.read();
            }

            // 关闭流
            reader.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

使用`BufferedReader`

```java
import java.io.*;

public class Main {
    public static void main(String[] args) {
        try {
            BufferedReader br = new BufferedReader(
                new InputStreamReader(
                    new FileInputStream("example.txt"), "UTF-8"
                )
            );

            String line;
            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }

            br.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```



##### OutputStreamWriter

Java IO 系统中用于将字符流转换为字节流的桥梁类

`OutputStreamWriter` 把 Java 中的字符（`char`）编码成字节，并输出到底层的 `OutputStream` 中。

```java
public class OutputStreamWriter extends Writer
```

###### 构造方法

1. `OutputStreamWriter(OutputStream out)`

   使用默认编码（⚠️不推荐）

2. ``OutputStreamWriter(OutputStream out, String charsetName)`

   字符集名称是字符串，如 `"UTF-8"`、`"GBK"`、`"ISO-8859-1"`

   ```java
   OutputStreamWriter writer = new OutputStreamWriter(new FileOutputStream("file.txt"), "UTF-8");
   writer.write("你好，世界！");
   writer.close();
   ```

3. `OutputStreamWriter(OutputStream out, Charset charset)`

   使用标准类 `StandardCharsets.UTF_8`，类型安全

   ```java
   OutputStreamWriter writer = new OutputStreamWriter(new FileOutputStream("file.txt"), StandardCharsets.UTF_8);
   ```

4. `OutputStreamWriter(OutputStream out, CharsetEncoder encoder)`



###### 常见方法

| 方法名                                 | 说明                   |
| -------------------------------------- | ---------------------- |
| `write(String str)`                    | 写入整个字符串         |
| `write(char[] cbuf, int off, int len)` | 写字符数组             |
| `flush()`                              | 刷新缓冲区（强制写入） |
| `close()`                              | 关闭流并刷新缓冲区     |







#### try-with-resources

`try (资源) {}`

小括号 `()` 里的内容**必须是资源声明**，而且这些资源对象**必须实现了 `AutoCloseable` 接口**。

在try块执行完毕后会自动调用`close()`方法释放资源

```java
try (
    BufferedReader reader = new BufferedReader(new FileReader("file.txt"));
    BufferedWriter writer = new BufferedWriter(new FileWriter("output.txt"))
) {
    writer.write(reader.readLine());
} catch (IOException e) {
    e.printStackTrace();
}
```





#### 测试

##### 复制文件夹内容

```java
public class FileTestDemo1 {
    public static void main(String[] args) {
        File src = new File(".\\src\\IODemo\\File1");
        File dest = new File(".\\src\\IODemo\\File2");
        try {
            copyDir(src, dest);
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    private static void copyDir(File src, File dest) throws IOException {
        if (!src.exists()) {
            throw new IOException("源目录不存在: " + src.getAbsolutePath());
        }
        if (!dest.exists()) {
            dest.mkdirs(); // 创建目标目录
        }
        File[] files = src.listFiles();
        if (files != null) {
            for (File file : files) {
                if (file.isFile()) {
                    try (FileInputStream fis = new FileInputStream(file); FileOutputStream fos = new FileOutputStream(new File(dest, file.getName()))) {
                        byte[] buffer = new byte[1024];
                        int len;
                        while ((len = fis.read(buffer)) != -1) {
                            fos.write(buffer, 0, len);
                        }
                    }
                } else if (file.isDirectory()) {
                    // 递归调用
                    copyDir(file, new File(dest, file.getName()));
                }
            }
        }
    }
}
```



##### 读取内容并按数字大小排序

```java
public static void main(String[] args) {
    File file = new File(".\\src\\IODemo\\TestDemo\\t1.txt");
    StringBuilder sb = new StringBuilder();
    // 使用 try-with-resources 自动关闭流
    try (FileInputStream fis = new FileInputStream(file)) {
        int len;
        byte[] buffer = new byte[1024];
        while ((len = fis.read(buffer)) != -1) {
            sb.append(new String(buffer, 0, len, StandardCharsets.UTF_8));
        }
    } catch (Exception e) {
        e.printStackTrace();
    }
    // 分割、转换成Integer、排序
    List<Integer> collect = Arrays.stream(sb.toString().split("-"))
            .map(Integer::parseInt) // 直接map成Integer
            .sorted()
            .collect(Collectors.toList());
    System.out.println(collect);
}
```







#### 换行

windows： `\r\n`  

在windows中，java对换行进行了优化  `\r`、 `\n`写其中一个就可以

Linux：`\n`

Mac：`\r`





## 缓冲流

### 字节缓冲流

**字节缓冲流**是对普通字节流（比如 `FileInputStream`、`FileOutputStream`）的增强，它通过一个**内部缓冲区**（通常是一个字节数组）来减少实际读写磁盘或网络的次数，从而提高 I/O 性能。

#### BufferedInputStream

字节缓冲输入流

- 当你调用 `read()` 方法时，它不会每次都直接从磁盘读取数据。
- 它会一次性从磁盘读取一大块数据到缓冲区（比如 8KB）。
- 之后的读取操作直接从缓冲区中取数据，只有当缓冲区用完后，才会再次从磁盘读数据。

| 方法签名                                        | 说明                                                         |
| :---------------------------------------------- | :----------------------------------------------------------- |
| `BufferedInputStream(InputStream in)`           | 创建一个默认缓冲区大小（8KB = 8192字节）的缓冲输入流，包装一个已有的输入流对象。 |
| `BufferedInputStream(InputStream in, int size)` | 创建一个指定缓冲区大小的缓冲输入流。                         |

#### BufferedOutputStream

字节缓冲输出流

- 当你调用 `write()` 方法时，它把数据先写入到内部缓冲区。
- 只有当缓冲区满了或者调用了 `flush()`/`close()` 方法时，才会将缓冲区中的数据一次性写到磁盘。

| 方法签名                                           | 说明                                                         |
| :------------------------------------------------- | :----------------------------------------------------------- |
| `BufferedOutputStream(OutputStream out)`           | 创建一个默认缓冲区大小的缓冲输出流，包装一个已有的输出流对象。 |
| `BufferedOutputStream(OutputStream out, int size)` | 创建一个指定缓冲区大小的缓冲输出流。                         |

#### 示例

```java
private static void copyDir1(File src, File dest) throws IOException {
    if (!src.exists()) {
        throw new IOException("源目录不存在: " + src.getAbsolutePath());
    }
    if (!dest.exists()) {
        dest.mkdirs(); // 创建目标目录
    }
    File[] files = src.listFiles();
    if (files != null) {
        for (File file : files) {
            if (file.isFile()) {
                File newFile = new File(dest, file.getName()); // 关键点：新建目标文件
                try (BufferedInputStream bis = new BufferedInputStream(new FileInputStream(file));
                     BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(newFile))) {
                    byte[] buffer = new byte[1024];
                    int len;
                    while ((len = bis.read(buffer)) != -1) {
                        bos.write(buffer, 0, len);
                    }
                    bos.flush(); // 刷新缓冲区，确保数据写完
                }
            } else if (file.isDirectory()) {
                // 递归调用
                copyDir1(file, new File(dest, file.getName()));
            }
        }
    }
}
```

实际上存在**两层缓冲区**：

| 层次   | 类型                                          | 作用                                                      |
| :----- | :-------------------------------------------- | :-------------------------------------------------------- |
| 第一层 | `BufferedInputStream` 内部自带的**8KB缓冲区** | 减少磁盘IO操作，批量从磁盘读入内存                        |
| 第二层 | 你自己定义的 `byte[] buffer`（比如1024字节）  | 从 `BufferedInputStream` 中批量读取数据，提高程序读写效率 |

```java
硬盘文件
   ↓（大块读取）
BufferedInputStream（内部8KB缓冲）
   ↓（按你的byte[] buffer大小读取）
程序
   ↓（按你的byte[] buffer大小写入）
BufferedOutputStream（内部8KB缓冲）
   ↓（大块写入）
硬盘文件
```

`BufferedInputStream`可以**减少和磁盘打交道的次数**。

`byte[] buffer`可以**减少程序和流之间打交道的次数**。





### 字符缓冲流

#### BufferedReader

字符缓冲输入流

##### 构造方法

```java
BufferedReader(Reader in)
BufferedReader(Reader in, int sz) // 可指定缓冲区大小
```

##### 常用方法

- `String readLine()`：读取一行文本（不包含换行符），返回 null 表示读完了。
- `int read()`：读取单个字符。
- `int read(char[] cbuf, int off, int len)`：读取字符数组。

##### 示例

```java
import java.io.*;

public class BufferedReaderExample {
    public static void main(String[] args) {
        try (BufferedReader reader = new BufferedReader(new FileReader("example.txt"))) {
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line); // 输出每一行
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

```java
// 本质上也是用 InputStreamReader 实现的，是现代写法的封装版，同样支持编码指定。
BufferedReader reader = Files.newBufferedReader(Paths.get("file.txt"),StandardCharsets.UTF_8);
```



#### BufferWriter

字符缓冲输出流

<span style="color:blue">输出流在关联文件时会清空文件</span>

##### 构造方法

```java
BufferedWriter(Writer out)
BufferedWriter(Writer out, int sz) // 可指定缓冲区大小
```

##### 常用方法

- `void write(String s)`：写入字符串。
- `void newLine()`：写入一个换行符（与平台无关）。
- `void flush()`：刷新缓冲区，把数据强制写入目标。
- `void close()`：关闭流并释放资源。

##### 示例

```java
import java.io.*;

public class BufferedWriterExample {
    public static void main(String[] args) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter("output.txt"))) {
            writer.write("Hello world!");
            writer.newLine(); // 换行
            writer.write("这是第二行文本。");
            writer.flush(); // 强制写入
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

```java
public class TestDemo3 {
    public static void main(String[] args) throws IOException {
        File file = new File(".\\src\\IODemo\\TestDemo3\\1.txt");
        File file1 = new File(".\\src\\IODemo\\TestDemo3\\2.txt");
        if (file.exists()) {
            if (!file1.exists()) {
                file1.createNewFile();
            }

            try (BufferedReader br = new BufferedReader(new FileReader(file));
                 BufferedWriter bw = new BufferedWriter(new FileWriter(file1))) {
                
                String line;
                // 逐行读取1.txt的内容并写入2.txt
                while ((line = br.readLine()) != null) {
                    bw.write(line);
                    bw.newLine(); // 写入换行符
                }
                bw.flush(); // 确保所有数据都写入文件
                
                System.out.println("文件复制完成！");
            }
        } else {
            System.out.println("源文件不存在！");
        }
    }
}
```

```java
public static void main(String[] args) {
    File file1 = new File(".\\src\\IODemo\\TestDemo2\\1.txt");
    File file2 = new File(".\\src\\IODemo\\TestDemo2\\2.txt");
    try (
        BufferedReader br1 = new BufferedReader(new FileReader(file1)); 
        BufferedWriter bw1 = new BufferedWriter(new FileWriter(file2))
    ) {
        String line;
        ArrayList<String> arr = new ArrayList<>();
        while ((line = br1.readLine()) != null) {
            arr.add(line);
        }
        // Comparator<T> 是一个函数式接口
        Collections.sort(arr, (o1, o2) -> {
            int i1 = Integer.parseInt(o1.split("\\.")[0]);
            int i2 = Integer.parseInt(o2.split("\\.")[0]);
            return i1 - i2;
        });
        for (String s : arr) {
            bw1.write(s);
            bw1.newLine();
        }
    } catch (Exception e) {
        e.printStackTrace();
    }
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



### `java.util.Arrays`

`java.util.Arrays` 是 Java 标准库（Java SE）中的一个 **工具类**。

它包含了用于 **操作数组** 的一系列静态方法，例如：排序、搜索、复制、填充、比较等。

由于是 **final类**（`public final class Arrays`），所以不能被继承。

#### 常用方法

`java.util.Arrays` 是 Java 提供的数组操作工具类，包含了一系列的静态方法，用于数组的排序、搜索、复制、比较、填充等操作。

1. `copyOf`

**作用**：复制整个数组到一个新数组，并可以指定新数组的长度。

```java
import java.util.Arrays;

int[] original = {1, 2, 3};
int[] copy = Arrays.copyOf(original, 5);

System.out.println(Arrays.toString(copy)); // [1, 2, 3, 0, 0]
```

- 如果新长度大于原数组，后面部分用默认值填充。

2. `copyOfRange`

**作用**：复制数组中指定范围的元素到新数组。

```java
int[] original = {1, 2, 3, 4, 5};
int[] rangeCopy = Arrays.copyOfRange(original, 1, 4);

System.out.println(Arrays.toString(rangeCopy)); // [2, 3, 4]
```

- `from`（包含），`to`（不包含）。

3. `sort`

**作用**：对数组进行升序排序。

```java
int[] arr = {5, 1, 4, 2, 3};
Arrays.sort(arr);

System.out.println(Arrays.toString(arr)); // [1, 2, 3, 4, 5]
```

- 支持基本数据类型数组和对象数组（对象数组可自定义比较器）。

4. `binarySearch`

**作用**：在**已排序数组**中使用二分查找法查找元素。

```java
int[] arr = {1, 2, 3, 4, 5};
int index = Arrays.binarySearch(arr, 3);

System.out.println(index); // 2
```

- 如果找到返回索引，否则返回负数（插入点的负值减一）。

5. `fill`

**作用**：将数组的所有元素填充为某个值。

```java
int[] arr = new int[5];
Arrays.fill(arr, 7);

System.out.println(Arrays.toString(arr)); // [7, 7, 7, 7, 7]
```

6. `equals`

**作用**：比较两个数组是否**元素顺序、内容相同**。

```java
int[] a = {1, 2, 3};
int[] b = {1, 2, 3};
boolean result = Arrays.equals(a, b);

System.out.println(result); // true
```

- 只对一维数组有效。

7. `deepEquals`

**作用**：深度比较两个**多维数组**是否内容相同。

```java
int[][] a = {{1, 2}, {3, 4}};
int[][] b = {{1, 2}, {3, 4}};
boolean result = Arrays.deepEquals(a, b);

System.out.println(result); // true
```

- 对嵌套数组也能正确比较。

8. `toString`

**作用**：返回一维数组的字符串表示。

```java
int[] arr = {1, 2, 3};
System.out.println(Arrays.toString(arr)); // [1, 2, 3]
```

- 适合打印调试使用。

9. `deepToString`

**作用**：返回多维数组的字符串表示。

```java
int[][] arr = {{1, 2}, {3, 4}};
System.out.println(Arrays.deepToString(arr)); // [[1, 2], [3, 4]]
```

- 可以正确打印嵌套数组。

10. `asList`

**作用**：将数组转换为 `List`（固定大小）。

```java
String[] array = {"a", "b", "c"};
List<String> list = Arrays.asList(array);

System.out.println(list); // [a, b, c]
```

- 注意：返回的 `List` **大小固定**，不能添加或删除元素。

11. `setAll`

**作用**：通过函数，给数组每个索引位置设置值。

```java
int[] arr = new int[5];
Arrays.setAll(arr, i -> i * i);

System.out.println(Arrays.toString(arr)); // [0, 1, 4, 9, 16]
```

12. `parallelSort`

**作用**：使用多线程并行排序，提高大数组的排序性能。

```java
int[] arr = {5, 3, 1, 2, 4};
Arrays.parallelSort(arr);

System.out.println(Arrays.toString(arr)); // [1, 2, 3, 4, 5]
```

- 对于非常大的数组，比 `sort` 更快。

13. `stream`

**作用**：将数组转为 `Stream` 流，方便使用 Stream API。

```java
int[] arr = {1, 2, 3, 4, 5};
int sum = Arrays.stream(arr).sum();

System.out.println(sum); // 15
```

14. `Spliterator`

**作用**：`Arrays.spliterator(arr)` 是用来**生成一个 `Spliterator`** 的。`Spliterator` 是 Java 8 引入的一个接口，全称是 **Splitable Iterator**，可以理解为**可分割的迭代器**，主要用于支持并行遍历，比如和 `Stream` API 配合使用。

```java
import java.util.Arrays;
import java.util.Spliterator;
import java.util.function.IntConsumer;

public class SpliteratorExample {
    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 5};

        // 创建一个 Spliterator 对象
        Spliterator.OfInt spliterator = Arrays.spliterator(arr);

        // 遍历元素
        spliterator.forEachRemaining((int value) -> {
            System.out.println("元素: " + value);
        });
    }
}
```











## Java正则

Java 正则表达式通过 `java.util.regex` 包下的 Pattern 类与 Matcher 类实现

### Pattern类

- `Pattern` 是一个正则表达式的编译表示。
- 它是不可变的（immutable），线程安全的。
- 使用 `Pattern.compile(String regex)` 来创建一个 `Pattern` 实例。



#### 常用方法

| 方法                               | 说明                               |
| ---------------------------------- | ---------------------------------- |
| `compile(String regex)`            | 编译正则表达式为 Pattern 对象      |
| `compile(String regex, int flags)` | 指定标志位（如忽略大小写）进行编译 |
| `matcher(CharSequence input)`      | 根据输入字符串创建 Matcher         |
| `split(CharSequence input)`        | 用正则表达式分割字符串             |
| `pattern()`                        | 返回原始正则表达式字符串           |

#### 示例

```java
import java.util.regex.*;

public class PatternExample {
    public static void main(String[] args) {
        String text = "hello123world456";
        String regex = "\\d+";

        Pattern pattern = Pattern.compile(regex);
        Matcher matcher = pattern.matcher(text);

        while (matcher.find()) {
            System.out.println("Found number: " + matcher.group());
        }
    }
}
```















































