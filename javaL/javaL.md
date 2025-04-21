# JAVA

## 类

### 定义类

一个java文件中最多只能有一个`public`类，并且这个`public`类的名字必须和文件名相同





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

**使用抽象类而不是接口的情况抽象方法**

- 需要在类中定义成员变量，抽象类可以有非静态、非常量的成员变量，而接口中的变量默认都是`public static final`（常量）。
- 需要提供方法的默认实现
- 需要使用非public访问修饰符控制某些方法
- 想要实现代码重用，共享一些公共方法的实现
- 需要定义构造函数
- 类之间有明确的"is-a"关系（继承关系）





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
- **静态方法**是可以通过类的实例来调用的，但 **不推荐** 这样做。

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

- **必须实现或继承**：<span style="color:red">匿名内部类必须继承一个类或者实现一个接口。</span>

- **直接创建对象**：匿名内部类会在定义的同时直接实例化对象。

- **只能有一个实例**：因为没有类名，无法重复实例化。

- **可以重写多个方法**

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







### `@FunctionalInterface`

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

##### 普通写法

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

##### 匿名内部类写法

```java
// 等同于
public class ThreadDemo3 {
    public static void main(String[] args) {
        Thread t = new Thread() {
            @Override
            public void run() {
                for (int i = 0; i < 10000; i++) {
                    System.out.println(getName() + "hello world");
                }
            }
        };

        Thread t2 = new Thread() {
            @Override
            public void run() {
                for (int i = 0; i < 10000; i++) {
                    System.out.println(getName() + "hello world");
                }
            }
        };

        t.start();
        t2.start();
    }
}
```

##### lambda写法

```java
public class ThreadDemo3 {
    public static void main(String[] args) {
        Thread t = new Thread(() -> {
            for (int i = 0; i < 10000; i++) {
                System.out.println(Thread.currentThread().getName() + " hello world");
            }
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 10000; i++) {
                System.out.println(Thread.currentThread().getName() + " hello world");
            }
        });

        t.start();
        t2.start();
    }
}
```





**优点**

1. **简单直接**：通过继承 `Thread` 类，可以直接调用 `start()` 方法启动线程，代码较为直观。
2. **易于实现**：适合快速创建单一功能的线程。

**缺点**

1. **单继承限制**：Java 是单继承语言，继承了 `Thread` 类后就无法再继承其他类，限制了类的扩展性。
2. **线程与任务耦合**：继承 `Thread` 类后，线程的任务逻辑与线程本身耦合在一起，难以分离任务逻辑和线程控制。



##### start方法

```
t.start()
   ↓
JVM 创建新线程
   ↓
新线程内部自动调用 t.run()
   ↓
t.run() 检查是否有 target
   ↓
有的话执行 target.run()（你传入的 Runnable 实现）
```

##### run方法

`t.run()` 只是普通方法调用。

它会在**当前线程**（比如 main 线程）中执行，不会创建新线程。

所以它不会实现并发或多线程的效果。





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

```java
public class ThreadDemo3 {
    public static void main(String[] args) {
        Thread t1 = new Thread(new Runnable() {
            public void run() {
                for (int i = 0; i < 10; i++) {
                    System.out.println("Thread 1: " + i);
                }
            }
        });

        t1.start(); // ✅ 正确启动新线程
    }
}
```

```bash
public class ThreadDemo3 {
    public static void main(String[] args) {
        Runnable runnable = new Runnable() {
            public void run() {
                for (int i = 0; i < 10; i++) {
                    System.out.println(Thread.currentThread().getName() + ":" + i);
                }
            }
        };

        Thread thread = new Thread(runnable); // 创建线程对象
        thread.start(); // 启动新线程，run() 会在子线程中执行
    }
}
```

```java
public class ThreadDemo3 {
    public static void main(String[] args) {
        Runnable task = () -> {
            for (int i = 0; i < 10; i++) {
                System.out.println(Thread.currentThread().getName() + " → " + i);
            }
        };

        Thread t1 = new Thread(task, "CustomThread-1");
        t1.start();
    }
}
```

```java
Runnable runnable = () -> System.out.println("Run in thread");
Thread t1 = new Thread(runnable);
```



**传入 `Runnable` 但又重写了 `run()` 会怎样？**

```java
Thread t = new Thread(() -> System.out.println("Runnable running")) {
    @Override
    public void run() {
        System.out.println("Thread.run() running");
    }
};

t.start(); // 会打印哪个？
```

会打印 `Thread.run() running`

因为你重写了 `Thread` 的 `run()` 方法，`target.run()` 就不会被调用了。



#### Callable接口和Future接口

可以获取到多线程的结果

##### 使用`FutureTask`

FutureTask 实现了 Runnable 和 Future

```java
public class ThreadDemo3 {
    public static void main(String[] args) {
        Callable<Integer> callable = () -> {
            int result = 100;
            Thread.sleep(5000);
            return result;
        };
        FutureTask<Integer> task = new FutureTask<>(callable);
        Thread thread = new Thread(task);
        thread.start();
        try {
            int i = task.get();
            System.out.println("Result: " + i);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```



### 查看杀死进程

#### windows

tasklist 查看进程

taskkill 杀死进程

```bash
tasklist | findstr java
taskkill /F /PID 28060
```



#### Linux

`ps fe`、`ps aux`



### 守护线程

 java进程需要等待所有线程，有一种特殊的线程叫做守护线程，**只要其他非守护线程运行结束了，即使守护线程的代码没有执行完，也会强制结束。**

```java
public class ThreadDemo3 {
    public static void main(String[] args) throws InterruptedException {
        Thread t = new Thread(new Runnable() {
            @Override
            public void run() {
                try {
                    TimeUnit.SECONDS.sleep(100000);
                    System.out.println("Hello World");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println("Hello World");
            }
        });
        t.setDaemon(true);  // 设置t为守护线程，其他非守护线程结束t自己结束
        t.start();

        Thread.sleep(1000);
        System.out.println("结束");
    }
}
```



### 线程状态

| 状态名          | 含义                                                       |
| --------------- | ---------------------------------------------------------- |
| `NEW`           | 新创建，还没启动（调用了 `new Thread()` 但还没 `start()`） |
| `RUNNABLE`      | 可运行状态，正在运行或准备运行（由 JVM 管理调度）          |
| `BLOCKED`       | 阻塞状态，等待别的线程释放 **同步锁（synchronized）**      |
| `WAITING`       | 无限期等待（例如 `Object.wait()`、`Thread.join()`）        |
| `TIMED_WAITING` | 有限时间等待（例如 `Thread.sleep(x)`、`join(x)`）          |
| `TERMINATED`    | 线程执行完毕或异常终止                                     |



### 类

#### Thread

##### 常用方法

###### 线程标识相关方法

`getId()`：获取线程的唯一标识符。

```java
Thread t = new Thread();
long id = t.getId();
System.out.println("线程ID: " + id);
```

`getName()/setName()`：获取或设置线程名称。

```java
Thread t = new Thread();
t.setName("MyCustomThread");
System.out.println("线程名称: " + t.getName());
```

`currentThread()`

静态方法，获取当前正在执行的线程引用。

```java
Thread current = Thread.currentThread();
System.out.println("当前线程: " + current.getName());
```

###### 线程控制方法

`start()`

启动线程，使线程进入就绪状态。

```java
Thread t = new Thread(() -> {
    System.out.println("线程开始运行");
});
t.start(); // 正确的启动方式
```

`run()`

包含线程执行的代码，通常由JVM调用，不应直接调用。

```java
Thread t = new Thread(() -> {
    System.out.println("这是线程的任务");
});
// t.run(); // 错误用法，这只是普通方法调用
t.start(); // 正确用法
```

`sleep(long millis)`

使当前线程暂停执行指定的毫秒数。

让线程从`running`状态进入`timed waiting`状态

```java
try {
    System.out.println("线程将休眠2秒");
    Thread.sleep(2000); // 休眠2秒
    System.out.println("线程继续执行");
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

`join()`

等待该线程终止。阻塞当前线程

<span style="color:blue">`join()` 是主线程**主动阻塞自己**去等别人，而不是控制别人何时运行。</span>

```java
Thread t = new Thread(() -> {
    try {
        Thread.sleep(3000);
        System.out.println("子线程执行完毕");
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
});
t.start();

try {
    System.out.println("等待子线程完成");
    t.join(); // 主线程等待t线程结束
    System.out.println("主线程继续执行");
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

为什么需要join

```java
public class ThreadDemo3 {
    static int r = 0;

    public static void main(String[] args) throws InterruptedException {
        test1();
    }

    public static void test1() throws InterruptedException {
        System.out.println("开始");
        Thread t = new Thread(() -> {
            try {
                TimeUnit.SECONDS.sleep(2);
                r = 100;
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        t.start();
        System.out.println("r结果：" + r);
    }
}
```

因为主线程和线程t是并行执行的，t线程需要2秒才赋值r=10

而主线程一开始就要打印r的结果

```java
public class ThreadDemo3 {
    static int r = 0;

    public static void main(String[] args) throws InterruptedException {
        test1();
    }

    public static void test1() throws InterruptedException {
        System.out.println("开始");
        Thread t = new Thread(() -> {
            try {
                TimeUnit.SECONDS.sleep(2);
                r = 100;
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        t.start();
        t.join();  // 等待t线程执行完毕
        System.out.println("r结果：" + r);

    }
}
```



`join(long millis)`

等待该线程终止的时间最长为指定的毫秒数。

- 如果线程在这段时间内执行完，`join` 提前返回。

- 如果线程还没执行完，主线程将不再等，继续执行。

```java
Thread t = new Thread(() -> {
    /* 耗时任务 */
});
t.start();

try {
    t.join(1000); // 最多等待1秒
    // 无论线程t是否结束，1秒后都会继续执行
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

`yield()` 

提示线程调度器当前线程愿意让出CPU使用权。

让当前线程从`Running`状态进入`Runnable`状态

```java
Thread.yield(); // 当前线程让步，但不保证让步成功
```

`interrupt()`

中断线程。

```java
Thread t = new Thread(() -> {
    try {
        for (int i = 0; i < 10; i++) {
            if (Thread.currentThread().isInterrupted()) {
                System.out.println("线程被中断，退出");
                return;
            }
            System.out.println("计数: " + i);
            Thread.sleep(1000);
        }
    } catch (InterruptedException e) {
        System.out.println("线程在sleep时被中断");
    }
});
t.start();

// 3秒后中断线程
try {
    Thread.sleep(3000);
    t.interrupt();
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

`isInterrupted()`

检测线程是否已被中断。

```java
Thread t = new Thread(() -> {
    while (!Thread.currentThread().isInterrupted()) {
        // 执行任务直到被中断
    }
    System.out.println("线程检测到中断信号，结束执行");
});
```

`interrupted()`

静态方法，检测当前线程是否被中断，并清除中断状态。

其他线程可以调用interrupted方法打断正在睡眠的线程，此时sleep方法抛出InterruptedException异常

```java
if (Thread.interrupted()) {
    System.out.println("线程被中断，中断状态被清除");
}
```

线程状态和属性方法

`isAlive()`

判断线程是否还在运行。

```java
Thread t = new Thread(() -> {
    try {
        Thread.sleep(2000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
});
t.start();

System.out.println("线程启动后是否存活: " + t.isAlive());
try {
    Thread.sleep(3000);
} catch (InterruptedException e) {
    e.printStackTrace();
}
System.out.println("线程执行后是否存活: " + t.isAlive());
```

`getState()`

获取线程状态。

```java
Thread t = new Thread();
System.out.println("新建线程状态: " + t.getState()); // NEW
t.start();
System.out.println("启动后状态: " + t.getState()); // 可能是RUNNABLE
```

`getPriority()/setPriority(int)`

获取或设置线程优先级。

```java
Thread t = new Thread();
t.setPriority(Thread.MAX_PRIORITY); // 设置为最高优先级(10)
System.out.println("线程优先级: " + t.getPriority());
```

`isDaemon()/setDaemon(boolean)`

检查线程是否为守护线程或将线程设置为守护线程。

```java
Thread t = new Thread();
t.setDaemon(true); // 设置为守护线程，必须在start()前调用
System.out.println("是否为守护线程: " + t.isDaemon());
```

`getThreadGroup()`

获取线程所属的线程组。

```java
Thread t = new Thread();
ThreadGroup group = t.getThreadGroup();
System.out.println("线程组: " + group.getName());
```

`getContextClassLoader()/setContextClassLoader()`

获取或设置线程上下文类加载器。

```java
Thread t = Thread.currentThread();
ClassLoader cl = t.getContextClassLoader();
System.out.println("上下文类加载器: " + cl);
```

###### 线程堆栈相关方法

`getStackTrace()`

获取线程的堆栈跟踪信息。

```java
Thread t = Thread.currentThread();
StackTraceElement[] stackTrace = t.getStackTrace();
for (StackTraceElement element : stackTrace) {
    System.out.println(element);
}
```

`dumpStack()`

打印当前线程的堆栈跟踪信息到标准错误流。

```java
Thread.dumpStack(); // 打印当前线程的堆栈跟踪
```

##### 类方法

`currentThread()`

```java
Thread currentThread = Thread.currentThread();
System.out.println("当前线程名称: " + currentThread.getName());
System.out.println("当前线程ID: " + currentThread.getId());
```



#### ThreadLocal

ThreadLocal并不是一个Thread，而是Thread的局部变量。当使用ThreadLocal维护变量时，ThreadLocal为每个使用该变量的线程提供独立的变量副本，所以每一个线程都可以独立地改变自己的副本，而不会影响其他线程所对应的副本。

`ThreadLocal` 是 Java 提供的一种机制，用于 **为每个线程提供独立的变量副本**。也就是说，即使多个线程访问同一个 `ThreadLocal` 变量，它们看到的却是彼此**隔离**的副本。

`ThreadLocal` 是 Java 中用于创建**线程局部变量**的类，位于 `java.lang` 包中。它为每个线程提供独立的变量副本，线程之间的变量互不干扰。这在需要线程隔离的数据存储时非常有用，比如避免多线程环境中共享变量带来的数据一致性问题。

##### 基本概念

当我们使用普通的成员变量时，多个线程共享这些变量，可能会导致线程安全问题。而通过 `ThreadLocal`，每个线程都能拥有自己的变量副本，避免了线程之间的干扰。

- **核心原理**：`ThreadLocal` 为每个线程提供一个独立的变量副本。这些变量副本实际上存储在每个线程自己的 `ThreadLocalMap` 中。
- **作用场景**：当每个线程需要独立的变量，且线程之间的变量不需要共享时，适合使用 `ThreadLocal`。

##### 使用方法

创建一个 `ThreadLocal` 实例时，可以通过 `set()` 方法设置当前线程的值，通过 `get()` 方法获取当前线程的值。

```java
public class ThreadLocalExample {
    // 创建一个 ThreadLocal 对象
    private static ThreadLocal<Integer> threadLocal = new ThreadLocal<>();

    public static void main(String[] args) {
        Thread thread1 = new Thread(() -> {
            threadLocal.set(1);
            System.out.println("Thread 1: " + threadLocal.get());  // Thread 1: 1
        });

        Thread thread2 = new Thread(() -> {
            threadLocal.set(2);
            System.out.println("Thread 2: " + threadLocal.get());  // Thread 2: 2
        });

        thread1.start();
        thread2.start();
    }
}
```

使用 `initialValue()` 提供初始值

```java
public class ThreadLocalInitialValueExample {
    private static ThreadLocal<Integer> threadLocal = ThreadLocal.withInitial(() -> 0);

    public static void main(String[] args) {
        Thread thread1 = new Thread(() -> {
            System.out.println("Thread 1 initial value: " + threadLocal.get());  // Thread 1 initial value: 0
            threadLocal.set(100);
            System.out.println("Thread 1 updated value: " + threadLocal.get());  // Thread 1 updated value: 100
        });

        thread1.start();
    }
}
```

##### 常用方法

`get()`

获取当前线程关联的变量值。

```java
ThreadLocal<String> threadLocal = new ThreadLocal<>();
System.out.println(threadLocal.get()); // 输出 null

ThreadLocal<String> threadLocalWithInitial = ThreadLocal.withInitial(() -> "Default Value");
System.out.println(threadLocalWithInitial.get()); // 输出 "Default Value"
```

`set(T value)`

为当前线程设置线程局部变量的值。

`remove()`

移除当前线程的值，防止内存泄漏。

```java
ThreadLocal<String> threadLocal = new ThreadLocal<>();
threadLocal.set("ThreadLocal Value");
System.out.println(threadLocal.get()); // 输出 "ThreadLocal Value"

threadLocal.remove();
System.out.println(threadLocal.get()); // 输出 null
```



### 同步与异步

需要等待结果返回，才能继续运行是同步

不需要等待结果返回，就能继续运行就是异步



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



### 方法引用





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







### Collectors

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

1. 编译异常（Exception）：程序在编译过程中发现的异常，受检异常。

   在编译时必须被处理的异常，否则程序无法通过编译。

   <span style="color:red">java文件通过javac生成字节码文件时，编译阶段进行处理的异常。</span>

   在编译阶段java不会运行代码，只会检查语法是否错误，或者做一些性能优化

   需要显式地捕获或抛出。

   如`SQLException`、`IOException`、`ClassNotFoundException`

2. 运行时异常（RuntimeException）：又称为非受检异常。

   在编译时不强制处理的异常，在运行时出现。

   <span style="color:red">jvm通过java命令执行字节码文件</span>

   程序员可以选择处理，也可以不处理。

   如`NullPointerException`、`ArrayIndexOutOfBoundsException`、`ArithmeticException`、`ClassCastException`

   `IllegalArgumentException`、`IndexOutOfBoundsException`

3. 错误（Error）：由java虚拟机生成并抛出的异常，程序对其不作处理。

   表示 JVM 本身无法恢复的严重问题。

   通常不需要程序处理，程序员也无法控制。
   
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

- 泛型只支持引用数据类型，不能写基本数据类型

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

指定注解的生命周期，也就是注解保留的阶段。

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
3. 支持的属性类型：
   - 基本类型：`int`、`float`、`double`、`boolean` 等。
   - `String`。
   - 枚举类型。
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



### 获取class对象

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

1. 获取所有字段

   ```java
   Field[] publicFields = clazz.getFields(); // 公有字段
   Field[] allFields = clazz.getDeclaredFields(); // 所有字段
   ```

2. 获取单个字段

   ```java
   Field field = clazz.getField("publicFieldName"); // 公有字段
   Field privateField = clazz.getDeclaredField("privateFieldName"); // 私有字段
   ```

3. 获取字段值

   ```java
   Field field = clazz.getDeclaredField("name");
   field.setAccessible(true); // 绕过访问限制
   Object value = field.get(obj); // 获取字段值
   ```

4. 设置字段值

   ```java
   Field field = clazz.getDeclaredField("name");
   field.setAccessible(true);
   field.set(obj, "New Value"); // 设置字段值
   ```

#### Field

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

`field.set(Object obj, Object value)`：设置指定对象的此字段的值为 `value`。

```java
field.set(obj, "New Value");
```

##### 控制字段的访问权限

`field.setAccessible(boolean flag)`：设置是否允许通过反射访问私有字段。

```java
field.setAccessible(true); // 绕过私有字段的访问限制
```



### 获取成员方法







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



## Spring工具类

### DigestUtils

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



### UUID

```java
String FileName = UUID.randomUUID().toString();
```



### BeanUtils





## 第三方库

### Hutool

Hutool是一个小而全的Java工具类库，通过静态方法封装



### Lombok

#### java17 record 类

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

#### @Data

包含`@Getter`,`@Setter`,`@RequiredArgsConstructor`,`@ToString`,`@EqualsAndHashCode`

#### @Getter

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



#### @Setter

#### @Tolerate

忽略

#### 构造方法

##### @AllArgsConstructor

##### @NoArgsConstructor

##### @RequiredArgsConstructor

#### @ToString

#### @EqualsAndHashCode

#### var & val

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



#### 日志框架

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













