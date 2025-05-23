# 华为|字节|腾讯|京东|网易|滴滴面经分享（6个offer）

本文是一位读者的面经分享。希望这篇文章的内容可以对小伙伴们有帮助！



**每个人成功的经历都不可复制， 我们可以借鉴吸收别人的经验为己所用。**



另外，把自己上岸的经历分享出来是一件非常棒的事情，我在这里实名为这位读者点个赞👍



### 个人介绍


目前大三，本科就读于电子科技大学。



我在大一进入学校实验室学习，负责数据收集、日常开发、NLP。用到的技术包括：



+ 语言：Java、Python
+ 技术： 
    - 爬虫：协程、异步OI、正则表达式
    - 后端：SpringBoot、MyBatis、MySQL
    - 前端：HTML、CSS、JavaScript、BootStrap
    - 深度学习：Pytorch、Keras



在实验室接触的比较广泛，不过感觉不够深入，于是在大二下开始深入后端技术。



我在大二下开始做了些开源项目并深入Java相关技术，深入学习了： Java核心技术、Java虚拟机、Java并发编程、设计模式、MySQL、Spring、SpringBoot、Mybatis。



在大三上期，11月开始准备Java实习相关事务：



一个月的面试后，陆续拿到了字节，网易、京东、滴滴、腾讯和某区块链公司的6个实习offer。



### 复习经历


因为之前就深入学习过，所以总的复习时间也不长，大概是一周左右，后面是通过边面试边查漏补缺的方式来补短板。



> 前两天的复习内容：
>



#### Java基础


+ 面向对象特性：封装，多态（动态绑定，向上转型），继承
+ 泛型，类型擦除
+ 反射，原理，优缺点
+ `static`，`final` 关键字
+ `String`，`StringBuffer`，`StringBuilder`底层区别
+ BIO、NIO、AIO
+ `Object` 类的方法
+ 自动拆箱和自动装箱



#### Java集合框架


+ List ：`ArrayList`、`LinkedList`、`Vector`、`CopyOnWriteArrayList`
+ Set：`HashSet`、`TreeSet`、`LinkedHashSet`
+ Queue：`PriorityQueue`
+ Map：`HashMap`，`TreeMap`，`LinkedHashMap`
+ fast-fail，fast-safe机制
+ 源码分析（底层数据结构，插入、扩容过程）、线程安全。



#### Java虚拟机


+ 类加载机制、双亲委派模式、3种类加载器（`BootStrapClassLoader`，`ExtensionClassLoader`，`ApplicationClassLoader`）
+ 运行时内存分区（PC，Java虚拟机栈，本地方法栈，堆，方法区（永久代，元空间））
+ JMM：Java内存模型
+ 引用计数、可达性分析
+ 垃圾回收算法：标记-清除，标记-整理，复制
+ 垃圾回收器：比较，区别（Serial，ParNew，Parallel Scavenge ，CMS，G1）Stop The World
+ 强、软、弱、虚引用
+ 内存溢出、内存泄漏排查
+ JVM调优，常用命令



#### Java并发


+ 三种线程初始化方法（`Thread`、`Callable`，`Runnable`）区别
+ 线程池（`ThreadPoolExecutor`，7大参数，原理，四种拒绝策略，四个变型：Fixed，Single，Cached，Scheduled） 
    - 有界、无界任务队列，手写`BlockingQueue`。
    - 乐观锁：CAS（优缺点，ABA问题，DCAS）
    - 悲观锁： 
        * `Synchronized`： 
            + 使用：方法（静态，一般方法），代码块（this，`ClassName.class`）
            + 1.6优化：锁粗化，锁消除，自适应自旋锁，偏向锁，轻量级锁
            + 锁升级的过程和细节：无锁->偏向锁->轻量级锁->重量级锁（不可逆）
            + 重量级锁的原理（`monitor`对象，`monitorenter`,`monitorexit`）
            + `ReentrantLock`：和`Synchronized`区别？（公平锁、非公平锁、可中断锁....）、原理、用法
    - `ThreadLocal` ：底层数据结构：`ThreadLocalMap`、原理、应用场景。
    - `Atomic` 类（原理，应用场景）
    - AQS：原理、`Semaphore`、`CountDownLatch`、`CyclicBarrier`
    - `Volatile`：原理：有序性，可见性



> 第三天的复习内容：
>



#### MySQL


+ 架构：Server层，引擎层（缓存，连接器，分析器，优化器，处理器）
+ 引擎：InnoDB，MyISAM，Memory区别
+ 聚簇索引，非聚簇索引区别（从二叉平衡搜索树复习（AVL，红黑树）到B树，最后B+树）
+ MySQL、SQL优化方法
+ 覆盖索引，最左前缀匹配
+ 当前读，快照读
+ MVCC原理（事务ID，隐藏字段，Undo，ReadView）
+ Gap Lock、Next-Key Lock、Record Lock
+ 三大范式



#### SQL


+ 常用SQL
+ 连接：自连接，内连接（等值，非等值，自然连接），外连接（左，右，全）
+ Group BY 和 Having
+ Explain



> 第四天的复习内容：
>



#### Spring


+ AOP原理（JDK动态代理，CGLIB动态代理）和 IOC原理
+ Spring Bean生命周期
+ SpringMVC 原理
+ SpringBoot常用注解



#### 设计模式


+ 三种类型：创建、结构、行为
+ 单例模式：饿汉，懒汉，DCL
+ 简单工厂，工厂方法，抽象工厂
+ 代理模式
+ 装饰器模式
+ 观察者模式
+ 策略模式
+ 迭代器模式
+ ....



> 第五天的复习内容：
>



#### 计算机网络


+ OSI模型、TCP/IP模型
+ TCP和UDP区别
+ TCP可靠性传输原理：重传、流量控制、拥塞控制、序列号与确认应达号、校验和
+ 三次握手、四次挥手过程、原理
+ timewait、closewait
+ HTTP 
    - 报文格式
    - 1.0 1.1 2.0
    - 状态码
    - 无状态解决（Cookie Session原理）
+ HTTPS 
    - CA证书
    - 对称加密
    - 非对称加密
+ DNS解析过程，原理
+ IP协议、ICMP协议（Ping、Tracert）、ARP协议、路由协议
+ 攻击手段与防范：XSS、CSRF、SQL注入、DOS、DDOS



> 第六天的复习内容：
>



#### 操作系统


+ 进程、线程和协程区别
+ 进程通信方式（管道，消息队列，共享内存，信号，信号量，socket）
+ 进程调度算法（先来先服务，短作业优先，时间片轮换，多级反馈队列，优先级调度）
+ 内存管理：分页（页面置换算法：手写LRU）、分段、虚拟内存



> 第七天和以后的复习内容：
>



每天做点刷算法题(剑指offer、LeetCode 面试Hot题) +查漏补缺。



### 字节跳动


#### 第一面


1.  自我介绍，介绍项目 
2.  协程、线程、进程区别 
3.  手写LRU（要求用泛型写）、手写DCL 
4.  DNS解析过程 
5.  输入一个URL到浏览器，整体流程 
6.  谈谈Java虚拟机你的认识？垃圾回收算法？垃圾回收器 
7.  知道哪些Java的锁？CAS的缺点？ 



#### 第二面


1. 自我介绍、介绍项目
2. 手写最大堆
3. 设计模式了解吗？几大类型？谈谈工厂模式？
4. 谈一下Java集合框架？HashMap线程安全的吗？会出现什么问题？
5. 说说MySQL的架构？
6. InnoDB和MyISAM区别？
7. 知道聚簇索引和非聚簇索引吗？B树和B+树区别？
8. 一道LeetCode难问题：接雨水（动态规划解决）



#### 第三面


1. 自我介绍、介绍开源项目
2. 线程池了解吗？原理？可以写个BlockingQueue吗？
3. 说说fast-fail和fast-safe？
4. 了解死锁吗？怎么解决？
5. 进程间通信方式？哪种最高效？
6. 说说MYSQL优化策略？
7. 说了一下部门介绍，主要业务，说可能会转GO等等



#### 第四面（HR）


1. 介绍自己
2. 团队怎么协作？有没有矛盾？怎么解决的？
3. 入职时间？实习多久？



### 华为


#### 第一面


1. 自我介绍
2. 谈项目（谈了很久）
3. HTTP 的无状态怎么解决？（Cookie Session）
4. TCP如何保证可靠性传输？（校验和，序列号和确认应答号，重传，流量控制，拥塞控制）
5. ARP过程？
6. 进程调度算法？
7. 一道动态规划题目：不同路径



#### 第二面


1. 自我介绍
2. 谈项目（你觉得收获最大的项目）
3. 谈谈Spring AOP 和 IOC
4. 谈谈你知道的MySQL所有内容
5. 手写个归并排序
6. 谈谈你对分布式系统的认识？
7. 谈谈你对华为的认识？华为的文化和价值观？



#### HR


技术面试都通过了，问HR怎么样，说应该没问题，等了一星期offer，最后发offer的时候，HR说我的性格测试没通过，Offer审批不下来，人傻了。因为华为在成都，字节在北京，而且技术官的意向是很稳能进华为，我想着在家近的地方实习，在等待的一周中就把字节拒了，最后华为没发到offer，直接架空，崩溃！第一次找实习没太多经验，策略不对，心里很难受，不过调整了一下，继续了新的面试



### 网易


#### 第一面


1. 自我介绍
2. 介绍一个对自己影响深刻的项目
3. 说说进程间调度的算法
4. 说说匿名函数
5. 说说协程、线程、进程。
6. 你对游戏引擎了解多少？
7. 手写地杰斯特拉算法？
8. 了解A*算法吗？
9. 说说Python和Java的区别？
10. Java是怎么进行垃圾回收的？
11. 然后聊了很多生活上的问题，非技术问题。



#### 第二面


1. 自我介绍
2. 介绍项目
3. 说说深度优先搜索算法、回溯算法
4. 一道算法题：一个走迷宫问题，DFS+回溯解决。
5. 你对C熟悉吗？Lua使用过吗？
6. 介绍业务，主要工作内容。



#### HR面


1. 自我介绍
2. 介绍一个项目中遇到的问题，怎么解决的？
3. 介绍一下博客？开源项目？为什么花时间做这些？
4. 大学最成功的一件事？



### 滴滴


#### 第一面


1. 自我介绍、介绍项目
2. Java面向对象的三大特性？
3. 了解Java哪些锁？Synchronized优化内容？锁升级过程？
4. 谈谈Java虚拟机？类加载机制？
5. 知道双亲委派模式吗？有什么好处？
6. Java运行时内存分区？
7. 死锁了解吗？如何解决？
8. 哪些对象可以作为GC ROOTS？
9. 了解的设计模式？手写一下DCL吧



#### 第二面


1. 自我介绍
2. 介绍项目（难点以及怎么解决的？）
3. 谈谈MySQL的各种引擎？
4. 覆盖索引和非覆盖索引区别？
5. MYSQL优化方法有哪些？
6. 讲讲HashMap的原理，put过程？resize过程？线程安全吗？死循环问题？
7. 了解什么中间件吗？
8. 讲讲Java里面的锁？
9. 一道算法题：最长公共子串



#### HR面


1. 自我介绍
2. 到岗时间
3. 自己的优势
4. 大学最失败的一件事
5. 对加班的看法



### 京东


#### 第一面


1. 自我介绍
2. 谈项目
3. TCP如何保证可靠传输？拥塞控制算法？
4. 讲讲Spring的AOP？
5. SpringBoot常用哪些注解？
6. 谈谈Java虚拟机？
7. 垃圾回收算法有哪些？
8. 了解哪些垃圾回收器？讲一下CMS垃圾回收过程
9. 算法题： 
    1. 两个栈实现队列
    2. 最近公共祖先节点



#### 第二面


1. 自我介绍
2. 讲讲Java集合框架，HashMap原理。
3. 知道哪些锁？
4. 谈谈公平锁和非公平锁？
5. Synchronized和ReentrantLock区别
6. MySQL的索引为什么快？有哪些索引？原理数据结构？
7. MySQL有哪些优化的策略？
8. 死锁了解吗？
9. ThreadLocal了解吗？原理？
10. 手写一个堆排序。
11. 一道算法题：完全平方数（动态规划）



#### HR面


1. 自我介绍
2. 多久可以到岗？实习时间？
3. 对加班看法？
4. 如何团队分工的？



### 腾讯


#### 第一面


1. 自我介绍
2. 介绍项目
3. 说说协程和线程区别？
4. Java虚拟机的作用？垃圾回收的过程？
5. 了解的垃圾回收器？
6. 手写快排
7. 算法题：按K位反转链表
8. 一百亿个数，n个机器，怎么排序？（桶排序）



#### 第二面


1. 自我介绍
2. 介绍项目
3. TCP和UDP区别？如何保证可靠性？
4. HTTP的状态码记得哪些？
5. ICMP是哪层的？有什么用？
6. 会哪些框架？
7. Spring的AOP认识？
8. MySQL InnoDB和MyISAM区别？
9. 谈谈各种索引？为什么用B+树不用B树？
10. 死锁的条件？如何解决？
11. OOM怎么排查？
12. 介绍业务



#### HR面


1. 自我介绍
2. 多久能来实习？实习多久？
3. 加班看法？
4. 看你掌握技术挺多，如何快速学习一个技术的？



### 总结


因为之前学的也比较深入，复习时间也没用太多，主要就是写点算法题保持手感。



面试中遇到的问题，9成都已经复习了，而且也比较基础，也都在掌握之中。



像中间件、微服务这些我没写在简历上，不是很会，面试官也不会刻意刁难你，实习的话，感觉大厂可能更注重基础和对知识的深入度，面试了一个月收货还是挺多的，希望总结一下面经，帮到更多的人~



准备大厂面试的话，注重基础，多练算法题，基本上就没问题了！加油！



> 更新: 2022-05-29 15:24:39  
> 原文: <https://www.yuque.com/snailclimb/mf2z3k/ht7ocg>