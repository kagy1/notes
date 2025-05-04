# 线程

并发：同一时刻，有多个指令在单个CPU上交替执行

并行：同一时刻，有多个指令在多个CPU上同时执行

## 多线程实现方式

### 继承Thread类

**实现步骤**

1. 创建一个类并继承自 `Thread` 类。
2. 重写 `Thread` 类的 `run()` 方法，将线程的任务逻辑写在 `run()` 方法中。
3. 创建该类的实例。
4. 调用 `start()` 方法启动线程。

#### 普通写法

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

#### 匿名内部类写法

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

#### lambda写法

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



#### start方法

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

#### run方法

`t.run()` 只是普通方法调用。

它会在**当前线程**（比如 main 线程）中执行，不会创建新线程。

所以它不会实现并发或多线程的效果。





### 实现Runnable接口

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



### Callable接口和Future接口

可以获取到多线程的结果

#### 使用`FutureTask`

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



## 查看杀死进程

### windows

tasklist 查看进程

taskkill 杀死进程

```bash
tasklist | findstr java
taskkill /F /PID 28060
```



### Linux

`ps fe`、`ps aux`



## 守护线程

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



## 线程状态

| 状态名          | 含义                                                       |
| --------------- | ---------------------------------------------------------- |
| `NEW`           | 新创建，还没启动（调用了 `new Thread()` 但还没 `start()`） |
| `RUNNABLE`      | 可运行状态，正在运行或准备运行（由 JVM 管理调度）          |
| `BLOCKED`       | 阻塞状态，等待别的线程释放 **同步锁（synchronized）**      |
| `WAITING`       | 无限期等待（例如 `Object.wait()`、`Thread.join()`）        |
| `TIMED_WAITING` | 有限时间等待（例如 `Thread.sleep(x)`、`join(x)`）          |
| `TERMINATED`    | 线程执行完毕或异常终止                                     |



## 类

### Thread

#### 常用方法

##### 线程标识相关方法

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

##### 线程控制方法

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

##### 线程堆栈相关方法

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

#### 类方法

`currentThread()`

```java
Thread currentThread = Thread.currentThread();
System.out.println("当前线程名称: " + currentThread.getName());
System.out.println("当前线程ID: " + currentThread.getId());
```



### ThreadLocal

ThreadLocal并不是一个Thread，而是Thread的局部变量。当使用ThreadLocal维护变量时，ThreadLocal为每个使用该变量的线程提供独立的变量副本，所以每一个线程都可以独立地改变自己的副本，而不会影响其他线程所对应的副本。

`ThreadLocal` 是 Java 提供的一种机制，用于 **为每个线程提供独立的变量副本**。也就是说，即使多个线程访问同一个 `ThreadLocal` 变量，它们看到的却是彼此**隔离**的副本。

`ThreadLocal` 是 Java 中用于创建**线程局部变量**的类，位于 `java.lang` 包中。它为每个线程提供独立的变量副本，线程之间的变量互不干扰。这在需要线程隔离的数据存储时非常有用，比如避免多线程环境中共享变量带来的数据一致性问题。

#### 基本概念

当我们使用普通的成员变量时，多个线程共享这些变量，可能会导致线程安全问题。而通过 `ThreadLocal`，每个线程都能拥有自己的变量副本，避免了线程之间的干扰。

- **核心原理**：`ThreadLocal` 为每个线程提供一个独立的变量副本。这些变量副本实际上存储在每个线程自己的 `ThreadLocalMap` 中。
- **作用场景**：当每个线程需要独立的变量，且线程之间的变量不需要共享时，适合使用 `ThreadLocal`。

#### 使用方法

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

#### 常用方法

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



## 同步与异步

需要等待结果返回，才能继续运行是同步

不需要等待结果返回，就能继续运行就是异步





## 线程安全

### 示例

```java
public class ThreadDemo3 {
    static int counter = 0;

    public static void main(String[] args) {
        Thread t1 = new Thread(new Runnable() {
            public void run() {
                for (int i = 0; i < 5000; i++) {
                    counter++;
                }
            }
        });

        Thread t2 = new Thread(new Runnable() {
            public void run() {
                for (int i = 0; i < 5000; i++) {
                    counter--;
                }
            }
        });
        t1.start();
        t2.start();
        try {
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        System.out.println("counter = " + counter);
    }
}
```

结果counter不为0

**为什么会出现这个情况：**

如果两个线程在执行这些步骤时交错执行，就会出现所谓的 **竞态条件（Race Condition）**。

假设初始 `counter = 0`。两个线程同时执行如下操作：

**线程 1 执行** `counter++`

- 读取 `counter` 的值（值为 0）
- 加 1，变成 1（此时还没写回）
- **线程被中断**

**线程 2 执行** `counter--`

- 读取 `counter` 的值（值仍为 0）
- 减 1，变成 -1
- 写回 `counter = -1`

**线程 1 恢复执行**

- 把之前计算好的 1 写回 `counter`，变成 `counter = 1`

结果是 `counter = 1`，但我们加了一次、减了一次，**按理说应该是 0！**



**解决**

使用`synchronized`

同步方法或代码块，保证互斥访问

```java
public class ThreadDemo3 {
    static int counter = 0;

    public static synchronized void increment() {
        counter++;
    }

    public static synchronized void decrement() {
        counter--;
    }

    public static void main(String[] args) {
        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 5000; i++) {
                increment();
            }
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 5000; i++) {
                decrement();
            }
        });

        t1.start();
        t2.start();
        try {
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        System.out.println("counter = " + counter);
    }
}
```

```java
public class ThreadDemo3 {
    static int counter = 0;
    static Object lock = new Object();

    public static void main(String[] args) {
        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 50000; i++) {
                synchronized (lock) {
                    counter++;
                }
            }
        });
        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 50000; i++) {
                synchronized (lock) {
                    counter--;
                }
            }
        });
        t1.start();
        t2.start();
        try {
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("Final value of counter: " + counter);
    }
}
```

```java
public class ThreadDemo4 {
    public static void main(String[] args) {
        Room room = new Room();
        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 50000; i++) {
                room.increment();
            }
        });
        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 50000; i++) {
                room.decrement();
            }
        });
        t1.start();
        t2.start();
        try {
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("Counter: " + room.getCounter());
    }
}

class Room {
    private int counter = 0;

    public void increment() {
        synchronized (this) {
            counter++;
        }
    }

    public void decrement() {
        synchronized (this) {
            counter--;
        }
    }

    public int getCounter() {
        synchronized (this) {
            return counter;
        }
    }
}
```

使用 `AtomicInteger`

```java
import java.util.concurrent.atomic.AtomicInteger;

public class ThreadDemo3 {
    static AtomicInteger counter = new AtomicInteger(0);

    public static void main(String[] args) {
        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 5000; i++) {
                counter.incrementAndGet();
            }
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 5000; i++) {
                counter.decrementAndGet();
            }
        });

        t1.start();
        t2.start();
        try {
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        System.out.println("counter = " + counter.get());
    }
}
```



### 临界区

问题出在多个线程访问共享资源

多个线程共享资源时读写操作时发生指令交错，就会出现问题

一段代码内如果存在对共享资源的多线程读写操作，称这段代码块称为**临界区**



### 竞态条件

多个线程在临界区内执行，由于代码的执行序列不同而导致结果无法预测，称之发生了竞态条件。

### 原子性

原子操作指的是 **不可中断的操作**，要么全部完成，要么全部不做。

### synchronized

Java 中 `synchronized` 是一种**内置锁机制**，用于防止**多个线程同时访问共享资源**，从而避免**线程安全问题**。 

它的作用是：**让同一时刻只有一个线程可以执行被 `synchronized` 修饰的方法或代码块**。

悲观锁,排他锁

**同步实例方法**：锁定当前对象实例

```java
public synchronized void method() {
    // 同步代码
}
```

```java
class Test{
    public synchronized void test(){
        
    }
}
// 等同于
class Test{
    public void test(){
        synchronized(this){
            
        }
    }
}
```

**同步静态方法**：锁定当前类的Class对象

```java
public static synchronized void staticMethod() {
    // 同步代码
}
```

```java
class Test{
    public synchronized static void test(){
        
    }
}
// 等同于
class Test{
    public static void test(){
        synchronized(Test.class){
            
        }
    }
}
```

**同步代码块**：可以指定要锁定的对象

```java
public void method() {
    synchronized(this) {  // 锁定this对象
        // 同步代码
    }
    
    synchronized(object) {  // 锁定指定对象
        // 同步代码
    }
    
    synchronized(ClassName.class) {  // 锁定类对象
        // 同步代码
    }
}
```





























