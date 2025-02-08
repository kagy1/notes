# Spring

## spring介绍

**Spring** 是一个泛指的术语，通常用来描述由 Spring 社区开发的整个生态系统。它涵盖了多个子项目和工具，包括：

- **Spring Framework**（核心框架）
- **Spring Boot**
- **Spring Data**
- **Spring Security**
- **Spring Cloud**
- **Spring Batch**
- 等等

### Spring Framework

**Spring Framework** 是 Spring 生态系统中的核心项目，也是 Spring 的基础。它是一个开源的 Java 应用开发框架，最初由 Rod Johnson 于 2003 年发布。通常称为`Spring框架`

Spring Framework 的主要功能包括：

- **依赖注入（Dependency Injection，DI）：** 提供灵活的 IOC 容器，简化组件之间的依赖管理。
- **控制反转：**IoC，反转资源获取方向；把自己创建的资源、向环境索取资源变为环境将资源准备好，我们享受资源注入。
- **面向切面编程（Aspect-Oriented Programming，AOP）：** 用于实现横切关注点（如日志记录、事务管理等）。
- **声明式事务管理：** 简化数据库事务的处理。
- **强大的集成能力：** 可以与各种第三方库和框架（如 Hibernate、JDBC、JPA 等）无缝集成。
- **Web 开发支持：** 提供了 Spring MVC 模块，用于构建基于 Servlet 的 Web 应用。
- **灵活性和可扩展性：** 支持多种配置方式（XML、Java 配置、注解）。



### SpringBoot

**Spring Boot** 是一个基于 Spring Framework 的子项目，旨在简化 Spring 应用程序的开发和部署。

它提供了开箱即用的功能，减少了繁琐的配置工作，并通过自动化配置（Auto Configuration）和嵌入式服务器（如 Tomcat、Jetty）实现快速开发和运行。

Spring Boot 的核心特性

Spring Boot 的主要特性包括：

**1）自动化配置**

- Spring Boot 提供了 

  自动化配置（Auto Configuration）

  ，根据类路径中的依赖和配置文件自动配置 Bean。

  - 例如，Spring Boot 自动检测是否存在某些类（如 `DataSource`），并自动配置数据源。

**2）嵌入式服务器**

- 内置了 **Tomcat、Jetty 和 Undertow** 等轻量级 Web 容器，无需手动部署到外部服务器。
- 开发者只需要运行一个 JAR 文件即可启动应用。

**3）Starter 依赖**

- Spring Boot 提供了一系列 

  Starter 依赖

  ，用于快速引入常用模块：

  - `spring-boot-starter-web`：引入 Spring MVC 和嵌入式 Tomcat。
  - `spring-boot-starter-data-jpa`：引入 JPA 和 Hibernate。
  - `spring-boot-starter-security`：引入 Spring Security。

- 这些 Starter 依赖简化了依赖管理。

**4）生产级特性**

- 提供了许多开箱即用的生产级功能：
  - **健康检查**（Actuator）。
  - **应用监控**。
  - **外部化配置**（通过 `application.properties` 或 `application.yml`）。

**5）命令行支持**

- Spring Boot 提供了命令行工具，用于运行和测试应用程序。

**6）简化配置**

- 通过约定优于配置（Convention over Configuration）的理念，避免繁琐的手动配置。







## 核心概念

### IOC DI

`IoC( Inversion of Control )`控制反转

使用对象时，在程序中不要主动使用new产生对象，转换为由外部提供对象。

对象的创建控制权由程序转移到外部，这种思想称之为控制反转。

<span style="color:red">不要自己去主动创建或管理依赖的对象，而是把这种控制权交给外部系统</span>

Spring技术对IoC思想进行了实现

- Spring提供了一个容器称为`IoC`容器，用来充当IoC思想中的"外部"
- IoC容器负责对象的创建、初始化等一系列工作，被创建或被管理的对象在IoC容器中统称为`Bean`

`DI(Dependency Injection)`依赖注入

在容器中建立bean与bean之间的依赖关系的整个过程，称为依赖注入。

**将一个类所需要的对象（依赖）从外部传递（注入）进来，而不是让类自己去创建这些对象**。

<span style="color:red">**DI 是 IoC 的一种具体实现方式**，但 IoC 不止可以用 DI 来实现。</span>

充分解耦，使用IoC容器管理bean(IoC)，在IoC容器内将有依赖关系的bean进行关系绑定(DI)

使用对象时不仅可以直接从IoC容器中获取，并且获取到的bean已经绑定了所有的依赖关系



## Spring配置

### 配置bean

- 传统Spring配置

  在传统的 Spring 应用中，你需要手动创建 `applicationContext.xml` 或 Java 配置类来定义 Bean，例如：

  ```xml
  <!-- applicationContext.xml -->
  
  <beans>
      <!--bean标签标示配置bean
      id属性标示给bean起名字
      class属性表示给bean定义类型-->
      <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>
  
      <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
          <!--7.配置server与dao的关系-->
          <!--property标签表示配置当前bean的属性
          name属性表示配置哪一个具体的属性
          ref属性表示这个属性的值由 ID 为 bookDao 的 Bean 提供-->
          <property name="bookDao" ref="bookDao"/>  <!--依赖注入-->
      </bean>
  </beans>
  ```

  ```java
  public class App2 {
      public static void main(String[] args) {
          //3.获取IoC容器
          ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
          //4.获取bean（根据bean配置id获取）
          BookDao bookDao = (BookDao) ctx.getBean("bookDao");
          bookDao.save();
  
          BookService bookService = (BookService) ctx.getBean("bookService");
          bookService.save();
  
      }
  }
  ```

  或者使用java配置

  ```java
  @Configuration
  public class AppConfig {
      @Bean
      public MyBean myBean() {
          return new MyBean();
      }
  }
  ```





## 常用注解

### `@Configuration`

**类级注解**：`@Configuration` 标注在类上，表明这个类可以包含一个或多个 `@Bean` 方法，并且 Spring 容器可以使用该类作为配置类来生成 bean 定义和自动装配。

### `@Bean`

**方法级注解**：用在方法上，声明这个方法返回一个 Spring 管理的 bean。该方法的返回值会被注册到 Spring IoC 容器中。

**单独使用时注意**：如果将 `@Bean` 定义在没有被 `@Configuration` 标记的类中（比如一个普通类或 `@Component` 类），Spring 仍然会注册这个 bean，但是这些方法之间直接调用时就不会走 Spring 容器的代理机制，因此可能会导致每次调用方法都会创建新的对象，无法保证单例性。

### `@RestController`

 Spring Boot 中提供的一个注解，用于简化开发 RESTful Web 服务

实际上结合了两个注解的功能：`@Controller` 和 `@ResponseBody`。





### `@RequestBody`

是Spring框架内的一个注解，主要用于将**HTTP请求体**中的内容绑定到方法参数上，它通常和@PostMapping或其他需要接收请求体的HTTP方法注解一起使用，用于处理JSON、XML或其他格式的请求数据。



## SpringBoot注解

### `@SpringBootApplication`

这是一个组合注解，包括了`@Configuration`、`@EnableAutoConfiguration`和`@ComponentScan`三个注解。用于标识SpringBoot应用程序的入口类。

**@Configuration**：指示这个类是一个配置类，它定义了一个或多个@Bean方法，用于创建和配置Spring应用程序上下文中的Bean。

**@EnableAutoConfiguration**：启用Spring Boot的自动配置机制，它会自动添加所需的依赖项和配置，以使应用程序能够运行。

**@ComponentScan**：指示Spring Boot扫描当前包及其子包中的所有@Component、@Service、@Repository和@Controller注解的类，并将它们注册为Spring Bean。

@SpringBootApplication注解通常被用于Spring Boot应用程序的入口类上，用于启动Spring Boot应用程序。它可以简化Spring应用程序的配置和启动过程。


### `@EnableAutoConfiguration`





## Restful

示例

```java
package com.kagy.springboot_his_test.controller;


import com.kagy.springboot_his_test.dto.PatInpatOrderCostDto;
import com.kagy.springboot_his_test.service.TestService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.ArrayList;
import java.util.List;
import java.util.Map;

@RestController
@RequestMapping("/test")
public class TestController {

    @Autowired
    private TestService testService;

    @GetMapping("/say")
    public void say() {
        testService.sayHello();
    }

    @PostMapping("/selectTest")
    public List<Map<String, Object>> selectTest(@RequestBody PatInpatOrderCostDto dto) {
        String patId = dto.getPatId();
        if (patId == null && patId.isEmpty()) {
            return new ArrayList<>();
        }
        System.out.println("patId: " + patId);
        List<Map<String, Object>> result = testService.selectTest(patId);
        return result;
    }
}

```

