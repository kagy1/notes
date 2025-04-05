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

<span style="color:red">**将一个类所需要的对象（依赖）从外部传递（注入）进来，而不是让类自己去创建这些对象**</span>。

<span style="color:red">**DI 是 IoC 的一种具体实现方式**，但 IoC 不止可以用 DI 来实现。</span>

充分解耦，使用IoC容器管理bean(IoC)，在IoC容器内将有依赖关系的bean进行关系绑定(DI)

使用对象时不仅可以直接从IoC容器中获取，并且获取到的bean已经绑定了所有的依赖关系



### AOP

AOP（Aspect Oriented Programming）面向切面编程

- OOP（Object Oriented Programming）面向对象编程

作用：在不惊动原始设计的基础上为其进行功能增强

 无侵入式编程

- 连接点：程序执行过程中的任意位置，粒度为执行方法，抛出异常，设置变量等
- 切入点：匹配连接点的式子，切入点在连接点中
- 通知
- 通知类：定义通知的类
- 切面：描述通知与切入点的对应关系







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
  



#### bean实例化

1. 构造方法

2. 静态工厂

3. 实例工厂 

4. 使用FactoryBean实例化

   实现FactoryBean接口



### 注入bean

**setter注入**

- 在bean中定义引用类型属性并提供可访问的set方法

- 配置中使用property标签将ref属性注入引用类型对象

- 简单类型通过value注入

  ```java
  public class MyBean {
      private int xxx;
  
      // Setter 方法
      public void setXxx(int xxx) {
          this.xxx = xxx;
      }
  
      // Getter 方法（用于验证注入值）
      public int getXxx() {
          return xxx;
      }
  }
  ```

  ```xml
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
                             http://www.springframework.org/schema/beans/spring-beans.xsd">
  
      <!-- 定义 MyBean，并通过 property 注入值 -->
      <bean id="myBean" class="com.example.MyBean">
          <property name="xxx" value="10" />
      </bean>
  
  </beans>
  ```

- 引用类型通过ref注入



**构造器注入**

**xml自动装配**

使用`bean`标签内`autowire`属性



**`@Autowired`**

通过反射注入



**springboot使用 `@Value` 注入配置属性**

Spring Boot 提供了 `@Value` 注解，用于将配置文件（如 `application.properties` 或 `application.yml`）中的值注入到 Bean 的属性中。







## 常用注解

### `@Configuration`

**类级注解**：`@Configuration` 标注在类上，表明这个类可以包含一个或多个 `@Bean` 方法，并且 Spring 容器可以使用该类作为配置类来生成 bean 定义和自动装配。

**基本作用**：

1. **声明配置类**：明确告诉Spring这个类不是普通的组件，而是一个配置源
2. **支持@Bean方法定义**：配置类中可以使用@Bean注解定义Bean实例，用于取代bean.xml配置文件注册bean对象。
3. **启用CGLIB代理**：默认情况下，Spring会用CGLIB创建配置类的子类代理

### `@Bean`

**方法级注解**：用在方法上，声明这个方法返回一个 Spring 管理的 bean。该方法的返回值会被注册到 Spring IoC 容器中。

**单独使用时注意**：如果将 `@Bean` 定义在没有被 `@Configuration` 标记的类中（比如一个普通类或 `@Component` 类），Spring 仍然会注册这个 bean，但是这些方法之间直接调用时就不会走 Spring 容器的代理机制，因此可能会导致每次调用方法都会创建新的对象，无法保证单例性。

### `@RestController`

 Spring Boot 中提供的一个注解，用于简化开发 RESTful Web 服务

实际上结合了两个注解的功能：`@Controller` 和 `@ResponseBody`。

#### `@ResponseBody`

用于指示方法的返回值应该直接写入 HTTP 响应体（response body），而不是被视为视图名称（如 JSP 等）并由视图解析器解析。

当在类级别上添加 `@ResponseBody` 注解时，它会对该控制器类中的所有处理方法生效，而不需要在每个方法上单独添加。

如果开发的是纯 API 应用而不使用视图，几乎所有的请求处理方法都需要添加 `@ResponseBody` 注解。

包含在@RestController中

```java
@Controller
public class UserController {

    @GetMapping("/user/{id}")
    @ResponseBody
    public User getUser(@PathVariable Long id) {
        // 假设从服务层获取用户
        User user = userService.findById(id);
        return user; // 直接返回User对象，会被转换为JSON或XML
    }
}
```





### `@RequestBody`

是Spring框架内的一个注解，主要用于将**HTTP请求体**中的内容绑定到方法参数上，它通常和@PostMapping或其他需要接收请求体的HTTP方法注解一起使用，用于处理JSON、XML或其他格式的请求数据。

```java
@PostMapping("/users")
public User createUser(@RequestBody User user) {
    // 从HTTP请求体中提取JSON/XML并转换为User对象
    // 例如: {"name":"John","email":"john@example.com"}
}
```





### `@PathVariable`

`@PathVariable` 用于从 URL 路径中提取变量值。

```java
@GetMapping("/user/{id}")
public User getUser(@PathVariable Long id) {
    // 从URL路径中提取id值
    // 例如: /user/123 会将id赋值为123
}
```



### 全局异常处理

#### `@ControllerAdvice`

工作原理

- 被 `@ControllerAdvice` 注解的类会被 Spring 自动检测和注册
- 该类中的 `@ExceptionHandler` 方法将对所有控制器生效
- 可以通过属性限制其应用范围（特定包、特定控制器类型等）



#### `@ExceptionHandler`

`@ExceptionHandler` 用于定义异常处理方法，可以捕获并处理特定类型的异常。

1. 当控制器方法抛出异常时，Spring MVC 会寻找能处理该异常的 `@ExceptionHandler` 方法
2. 查找顺序：当前控制器 → 全局异常处理器（`@ControllerAdvice`）
3. 如果找到匹配的处理器，就使用它来处理异常并返回响应



```java
/**
 * 全局异常处理
 */
@ControllerAdvice(annotations = {RestController.class, Controller.class})
@ResponseBody
@Slf4j
public class GlobalExceptionHandler {
    // 指定这个方法专门处理SQLIntegrityConstraintViolationException异常
    // 这种异常通常在数据库操作违反完整性约束时抛出（如唯一键冲突、外键约束等）。
    @ExceptionHandler(SQLIntegrityConstraintViolationException.class)
    public R<String> exceptionHandler(SQLIntegrityConstraintViolationException ex) {
        log.error(ex.getMessage());
        return R.error("失败了");
    }
}
```





### Bean

#### `@Component`

配置bean

```java
@Component
@Component("bookDao")
```

如果没有为 `@Component` 提供参数，Spring 会默认使用类名（首字母小写）作为 Bean 的名称。

springboot提供@Component注解的三个衍生注解

- @Controller：表现层bean定义
- @Service：业务层Bean定义
- @Repository：数据层Bean定义



#### `@Autowired`

默认是根据**类型**来实现Bean的依赖注入

告诉 Spring 去查找一个符合要求的 Bean，并将其注入到目标位置。

```java
@Autowired
private MyService myService;
```

常用参数：

1. required
   - 说明：`required` 参数用来指定是否必须要注入该依赖。
   - 默认值：true

```java
@Autowired(required = false)
private Optional<MyService> myService;
```

如果有多个相同类型的bean，使用`@Qualifier`

- 自动装配基于反射设计创建对象并暴力反射对应属性为私有属性初始化数据，因此无需提供setter方法
- <span style="color:red">自动装配建议使用无参构造方法创建对象（默认）</span>，如果不提供对应构造方法，请提供唯一的构造方法



#### @Qualifier

在 Spring 中，`@Qualifier` 注解通常与 `@Autowired` 一起使用

默认情况下，Spring 容器中的 Bean 是按类型进行注入的。如果存在多个同类型的 Bean，Spring 就会抛出异常（`NoUniqueBeanDefinitionException`），因为它无法确定要注入哪个 Bean。

为了解决这种冲突，可以使用 `@Qualifier` 注解来指定具体的 Bean。

```java
@Component("beanA")
public class MyServiceA implements MyService {
    // 实现类 A
}

@Component("beanB")
public class MyServiceB implements MyService {
    // 实现类 B
}

@Autowired
@Qualifier("beanA")
private MyService myService; // 指定注入 beanA
```



#### `@Value`

使用`@Value`实现简单类型注入

```properties
app.name=MyApplication
app.version=1.0.0
```

```java
@Component
public class AppConfig {

    @Value("${app.name}")
    private String appName;

    @Value("${app.version}")
    private String appVersion;

    public void printConfig() {
        System.out.println("应用名称: " + appName);
        System.out.println("应用版本: " + appVersion);
    }
}
```



#### `@Bean`

使用`@Bean`管理第三方Bean











## SpringBoot注解

### `@SpringBootApplication`

这是一个组合注解，包括了`@Configuration`、`@EnableAutoConfiguration`和`@ComponentScan`三个注解。用于标识SpringBoot应用程序的入口类。

**@Configuration**：指示这个类是一个配置类，它定义了一个或多个@Bean方法，用于创建和配置Spring应用程序上下文中的Bean。

**@EnableAutoConfiguration**：启用Spring Boot的自动配置机制，它会自动添加所需的依赖项和配置，以使应用程序能够运行。

**@ComponentScan**：指示Spring Boot扫描当前包及其子包中的所有@Component、@Service、@Repository和@Controller注解的类，并将它们注册为Spring Bean。

@SpringBootApplication注解通常被用于Spring Boot应用程序的入口类上，用于启动Spring Boot应用程序。它可以简化Spring应用程序的配置和启动过程。


### `@EnableAutoConfiguration`



## SpringMVC

### 拦截器

拦截器(Interceptor)是一种动态拦截方法调用的机制，在SpringMVC中动态拦截控制器方法的执行

- 作用
  - 在指定的方法调用前后执行预先设定的代码
  - 阻止原始方法的执行

 在拦截器当中，开发人员可以在应用程序中做一些**通用性**的操作，比如通过拦截器来拦截前端发来的请求，判断Session中是否有登录用户的信息,进行拦截。



#### Spring使用

1. 定义拦截器类，实现`HandlerInterceptor`接口

2. 加上`@Component`注解

3. 重写 preHandle，postHandle，afterCompletion 三个方法

   ```java
   @Component
   public class ProjectInterceptor implements HandlerInterceptor {
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           System.out.println("preHandle");
           return true;
       }
   
       @Override
       public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
           System.out.println("postHandle");
       }
   
       @Override
       public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
           System.out.println("afterCompletion");
       }
   }
   ```

   

4. 定义辅助类，继承`WebMvcConfigurationSupport`类，或者实现`WebMvcConfigurer`接口。加上`@configuration`。（也可以写在config配置类里）

5. 注入拦截器对象，重写`addInterceptors`方法

   ```java
   @Configuration
   public class TestSupport extends WebMvcConfigurationSupport {
       @Autowired
       private ProjectInterceptor projectInterceptor;
   
       @Override
       protected void addInterceptors(InterceptorRegistry registry) {
           registry.addInterceptor(projectInterceptor).addPathPatterns("/test/**");
       }
   }
   ```

   结果

   ```java
   preHandle
   name: LZT
   postHandle
   afterCompletion
   ```

   

6. 拦截路径也可以配置在config类，使用`WebMvcConfiguration`



#### yml配置

不能直接通过 `application.yml` 文件来完成，因为拦截器是代码逻辑的一部分，路径的注册需要通过代码显式调用 `InterceptorRegistry` 来完成。

不过，你可以将路径相关的配置（比如拦截的路径和排除的路径）写入 `application.yml` 文件，然后在代码中读取这些配置。



1. 在 `application.yml` 中配置拦截路径

你可以在 `application.yml` 中定义一个自定义配置，来表示拦截的路径和排除的路径。例如：

```yaml
custom:
  interceptor:
    include-paths:
      - "/test/**"      # 配置需要拦截的路径
      - "/admin/**"
    exclude-paths:
      - "/test/login"   # 配置不需要拦截的路径
      - "/test/register"
```

2. 创建配置类读取 `yml` 配置

使用 Spring 的 `@ConfigurationProperties` 注解读取 `application.yml` 中的自定义配置。

自定义配置类

```java
@Component
@ConfigurationProperties(prefix = "custom.interceptor") // 绑定到配置前缀 custom.interceptor
public class InterceptorConfigProperties {

    private List<String> includePaths; // 拦截路径
    private List<String> excludePaths; // 排除路径

    public List<String> getIncludePaths() {
        return includePaths;
    }

    public void setIncludePaths(List<String> includePaths) {
        this.includePaths = includePaths;
    }

    public List<String> getExcludePaths() {
        return excludePaths;
    }

    public void setExcludePaths(List<String> excludePaths) {
        this.excludePaths = excludePaths;
    }
}
```

3. 在拦截器配置类中使用 `InterceptorConfigProperties`

通过注入 `InterceptorConfigProperties`，将 `application.yml` 中的路径配置应用到拦截器中。

配置类：

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Autowired
    private ProjectInterceptor projectInterceptor;

    @Autowired
    private InterceptorConfigProperties interceptorConfigProperties;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        // 从配置中获取路径
        registry.addInterceptor(projectInterceptor)
                .addPathPatterns(interceptorConfigProperties.getIncludePaths()) // 使用 yml 中的 include-paths
                .excludePathPatterns(interceptorConfigProperties.getExcludePaths()); // 使用 yml 中的 exclude-paths
    }
}
```

------

4. 运行结果

假设 `application.yml` 中配置如下：

```yaml
custom:
  interceptor:
    include-paths:
      - "/test/**"
      - "/admin/**"
    exclude-paths:
      - "/test/login"
      - "/test/register"
```

5. 总结

虽然不能直接在 `application.yml` 中配置拦截器，但可以通过以下方式实现路径的可配置化：

1. 在 `application.yml` 中定义自定义路径配置。
2. 创建一个类读取 `yml` 的配置（使用 `@ConfigurationProperties`）。
3. 在拦截器配置类中，动态使用这些路径配置。





**preHandle**

在处理器（Controller 的方法）执行之前进行拦截和处理

**目标方法执行前执行**。返回true：继续执行后续操作；返回false：中断后续操作。

当拦截器中出现对原始处理器的拦截，后面的拦截器均终止运行。

```java
@Override
public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception
```

参数

- request：请求对象
- response：响应对象
- handler：被调用的处理器对象，本质上是一个方法对象，对反射技术中的Method对象进行了再包装

**postHandle**

 **目标方法执行后执行**。

**afterCompletion**

**视图渲染完毕后执行，最后执行**

参数

- ex：如果处理器执行过程中出现异常情况，可以针对异常情况单独处理。





**addInterceptors**

addInterceptors方法可以添加多个参数

```java
@Override
protected void addInterceptors(InterceptorRegistry registry) {
    registry.addInterceptor(projectInterceptor).addPathPatterns("/test/**", "/test");
}
```



**运行顺序**

- preHandle：与配置顺序相同，必定运行
- postHandle：与配置顺序相反，可能不运行
- afterCompletion：与配制顺序相反，可能不运行

如果配置了多个拦截器，并且在其中一个拦截器的 `preHandle` 方法中返回了 `false`。

1. 后续的拦截器的 preHandle 不会执行
2. 所有拦截器的 postHandle 不会执行
3. `afterCompletion` 仍然会执行，包括为 true 的 `preHandle` 的拦截器，但未执行的拦截器不会调用其 `afterCompletion` 方法



#### 拦截器路径配置

`/*`：一级路径

`/**`：拦截所有请求

`/test/*`：拦截/test下的一级路径

`/test/**`：拦截/test下的任意级路径



### 过滤器 Filter

Spring 框架中的过滤器（Filter）是基于 Java Servlet 的过滤器规范实现的，可以用于拦截和处理 HTTP 请求和响应。它们在整个请求-响应生命周期中起着重要作用，通常用于实现认证、授权、日志记录、请求修改等功能。

#### 注解实现

首先先写一个过滤器Filter，在类的上方使用 @WebFilter 注解来创建Filter即可

```java
@WebFilter(filterName = "loginCheckFilter", urlPatterns = "/*")
@Slf4j
public class LoginCheckFilter implements Filter {
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest request = (HttpServletRequest) servletRequest;
        HttpServletResponse response = (HttpServletResponse) servletResponse;
        log.info("拦截到请求：{}", request.getRequestURI());
        filterChain.doFilter(request, response);
    }
}

```

在启动类上开启servlet组件扫描(`@ServletComponentScan`)

```java
@Slf4j
@SpringBootApplication
@ServletComponentScan
public class ReggieTestLApplication {

    public static void main(String[] args) {
        SpringApplication.run(ReggieTestLApplication.class, args);
    }

}
```







### 拦截器和过滤器的区别

1. 过滤器先执行，它是Servert规范的一部分更接近于底层。它会在Servlet请求之前和响应之后进行处理。

   拦截器后执行，它是SpringMVC的一部分更接近业务层，会在Controller请求之前和处理完毕之后进行处理




### Restful

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



### 静态资源

#### 默认路径

只要将静态资源放在类路径下: /static, /public, /resources, /META-INF/resources 就可以被直接访问-对应文件（**这是 Spring Boot 的默认设置好的** ）。

#### 修改路径

1. 编写配置类，继承WebMvcConfigurationSupport类，重写addResourceHandlers方法

   ```java
   @Slf4j
   @Configuration
   public class WebMvcConfig extends WebMvcConfigurationSupport {
       /**
        * 设置静态资源映射
        * @param registry
        */
       @Override
       protected void addResourceHandlers(ResourceHandlerRegistry registry) {
           log.info("开始进行静态资源映射...");
           registry.addResourceHandler("/backend/**").addResourceLocations("classpath:/backend/");
           registry.addResourceHandler("/front/**").addResourceLocations("classpath:/front/");
       }
   }
   ```

2. 配置yaml文件

   **spring.mvc.static-path-pattern**

   该配置用来定义**静态资源的访问路径模式**，即客户端通过什么 URL 路径来访问静态资源。

   默认值是 `/**`，表示所有静态资源都通过根路径（`/`）访问。

   例子：如果静态资源是 `classpath:/static/index.html`，则可以通过 `http://localhost:8080/index.html` 访问。

   ```yaml
   spring:
     mvc:
     	## 客户端访问 /backend/index.html，会去查找静态资源文件。
   	## 如果文件在 classpath:/static/index.html 中，Spring 会根据路径映射规则，找到并返回静态资源。
       static-path-pattern: /backend/**
   ```

   **spring.web.resources.static-locations**

   该配置用来定义静态资源的物理存储位置，即 Spring Boot 会从哪里加载静态资源文件。

   默认值为

   ```yaml
   spring:
     web:
       resources:
         static-locations: 
           - classpath:/META-INF/resources/
           - classpath:/resources/
           - classpath:/static/
           - classpath:/public/
   ```

   Spring Boot 会按顺序从这些默认路径中查找静态资源文件。

   



### WebMvcConfigurationSupport

`WebMvcConfigurationSupport` 是 `Spring Framework` 中的一个核心配置类，它为 Spring MVC 应用程序提供核心配置支持。这个类是 Spring MVC 配置的基础，提供了许多默认的配置选项。

#### **主要功能**

1. **提供Spring MVC的基础配置**：它创建并配置了Spring MVC所需的各种组件。
2. **作为@EnableWebMvc注解的内部实现**：当你在配置类上使用@EnableWebMvc注解时，实际上是引入了WebMvcConfigurationSupport作为配置基础。
3. **提供可扩展的配置点**：通过各种protected方法，允许子类覆盖默认行为。

#### **使用**

1. 通过@EnableWebMvc使用

   ```java
   @Configuration
   @EnableWebMvc
   public class WebConfig {
       // 配置内容
   }
   ```

2. 直接继承WebMvcConfigurationSupport

   ```java
   @Configuration
   public class WebConfig extends WebMvcConfigurationSupport {
       // 覆盖需要自定义的方法
   }
   ```

#### **常见的可覆盖方法**

1. **configureContentNegotiation**：配置内容协商策略
2. **configureMessageConverters**：添加或修改消息转换器
3. **extendMessageConverters**：扩展和定制HTTP消息转换器
4. **addFormatters**：添加类型转换器
5. **addResourceHandlers**：配置静态资源处理
6. **configureDefaultServletHandling**：配置默认Servlet处理
7. **addCorsMappings**：配置跨域请求处理
8. **configurePathMatch**：配置路径匹配
9. **addArgumentResolvers**：添加自定义参数解析器
10. **addReturnValueHandlers**：添加自定义返回值处理器
11. **configureHandlerExceptionResolvers**：配置异常处理器



##### extendMessageConverters

与configureMessageConverters的区别：

- `configureMessageConverters`：完全替换默认的消息转换器列表。如果此方法添加了任何转换器，Spring将不会注册默认的转换器。
- `extendMessageConverters`：在默认转换器已经配置之后被调用，允许您修改已存在的配置而不是完全替换它。

##### 消息转换器的双向作用

HttpMessageConverter接口在Spring MVC中负责两个方向的转换：

1. **请求体解析**：将HTTP请求体转换为Java对象（反序列化）
   - 用于`@RequestBody`注解标记的方法参数
   - 用于处理HTTP POST、PUT、PATCH等带有请求体的请求
2. **响应体生成**：将Java对象转换为HTTP响应体（序列化）
   - 用于`@ResponseBody`注解标记的方法返回值
   - 用于`@RestController`控制器中的所有方法返回值







# 数据序列化文件

## properties属性文件

### 特点

1. 只能是键值对
2. 键不能重复
3. 文件后缀一般是`.properties`结尾



- Properties是一个map集合，但是我们一般不会当做集合使用

- 核心作用：properties是用来代表属性文件的，通过properties可以读写属性文件里的内容



### 读取数据

在 Java 中，可以使用 `java.util.Properties` 类来读取和操作 `.properties` 文件。

```java
import java.io.FileInputStream;
import java.io.IOException;
import java.util.Properties;

public class PropertiesExample {
    public static void main(String[] args) {
        Properties properties = new Properties();
        try (FileInputStream fis = new FileInputStream("config.properties")) {
            // 加载 .properties 文件
            properties.load(fis);

            // 获取键对应的值
            String dbUrl = properties.getProperty("db.url");
            String dbUsername = properties.getProperty("db.username");
            String dbPassword = properties.getProperty("db.password");

            // 输出读取的值
            System.out.println("Database URL: " + dbUrl);
            System.out.println("Database Username: " + dbUsername);
            System.out.println("Database Password: " + dbPassword);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```





## XML

可扩展标记语言（EXtensible Markup Language）

### 特点

- 只能有一个根标签

- xml中的标签名可以自己定义（可扩展），但必须要正确的嵌套
- XML中的标签可以有属性

### 基本结构

**XML 声明**（可选）：

- 声明文件的版本、编码方式等信息。

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  ```

**根元素**

- 每个 XML 文件必须有且只有一个根元素，所有其他内容都必须包含在根元素中。

**属性**

- 元素可以包含属性，用于描述元素的附加信息。

  ```xml
  <person id="123" gender="male">
      <name>John</name>
  </person>
  ```



### 示例

```xml
<?xml version="1.0" encoding="UTF-8"?>
<company>
    <employee id="1">
        <name>John Doe</name>
        <position>Software Engineer</position>
        <salary>5000</salary>
    </employee>
    <employee id="2">
        <name>Jane Smith</name>
        <position>Data Scientist</position>
        <salary>7000</salary>
    </employee>
</company>
```



### 特殊符号

| **符号** | **描述**          | **转义实体** |
| -------- | ----------------- | ------------ |
| `<`      | 小于号            | `&lt;`       |
| `>`      | 大于号            | `&gt;`       |
| `&`      | 和号（Ampersand） | `&amp;`      |
| `'`      | 单引号            | `&apos;`     |
| `"`      | 双引号            | `&quot;` |



### CDATA

CDATA 块以 `<![CDATA[` 内容 `]]>` 结束。

```xml
<![CDATA[
这里是 CDATA 块中的内容，可以包含 <、>、& 等特殊符号。
]]>
```



### 解析XML

用IO流解析XML难度大

可以用开源框架，如`DOM4j`



### 约束文档

1. DTD文档
2. scheme文档



# 日志

可以将系统执行的信息，方便的记录到指定的位置

可以随时以开关的形式控制日志的启停

- 日志框架：`JUL(java.util.loggiing)`、`Log4j`、`Logback`

- 日志接口：设计日志框架的一套标准，日志框架需要实现这些接口

  `Commons Logging(JCL)`：`JUL(java.util.loggiing)`

  `SLF4J`：`Log4j`、`Logback`



## Logback

Logback是基于`slf4j`日志规范实现的框架

### 组成

**logback-core**（必须）

- Logback 的核心模块。
- 提供了基本的日志功能，是其他模块的基础。

**logback-classic**（必须）

- 是 Logback 的完整实现，兼容 SLF4J。
- 提供了对 SLF4J API（日志接口） 的支持，使 Logback 能无缝集成到任何使用 SLF4J 的应用中。

**logback-access**

- 提供对 Servlet 容器（如 Tomcat 和 Jetty）的日志支持。
- 主要用于记录 HTTP 请求和响应日志。

**SLF4J API**



### 配置文件

logback.xml



#### 变量

`<property>` 是 **Logback** 提供的一个配置标签，用来定义全局变量或者属性，这些属性可以在整个 Logback 配置文件中通过 `${}` 占位符的形式引用。

```xml
<property name="webRoot" value="./files"/>

<File>${webRoot}/api-view.log</File>
```





### 示例

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class LogbackExample {
    // 用于定义一个 日志记录器（Logger）对象
    private static final Logger logger = LoggerFactory.getLogger(LogbackExample.class);

    public static void main(String[] args) {
        logger.trace("This is a TRACE message");  // TRACE（最详细的日志级别）
        logger.debug("This is a DEBUG message");  // DEBUG 用于记录调试信息
        logger.info("This is an INFO message");  // INFO 用于记录重要的运行时信息，表示程序的正常操作。
        logger.warn("This is a WARN message");  // WARN 用于记录潜在的问题或异常情况
        logger.error("This is an ERROR message");  // ERROR 用于记录严重错误，这些错误通常会影响系统的正常运行。
    }
}
```

**Logger 是什么？**

- **`Logger`** 是日志框架中的核心组件，负责记录和输出日志信息。
- 每个 `Logger` 通常与程序中的一个类绑定（通常是当前类），用于记录该类的日志信息。
- 通过 `Logger` 对象，可以调用各种方法（如 `trace`、`debug`、`info`、`warn` 和 `error`），记录日志信息。



### 日志级别

日志信息的类型，日志都会分级别 低----高

- trace 追踪，指名程序运行轨迹
- debug 调试，实际应用中一般将其作为最低级别，而trace则很少使用
- info 输出重要的运行信息，数据连接，网络连接，IO操作等等
- warn 警告信息，可能会发生问题，使用较多



```xml
<root level="info">
	<appender-ref ref="CONSOLE"/>
    <appender-ref ref="FILE"/>
</root>
```

- 只有日志级别大于或等于核心配置文件配置的日志级别，才会被记录。



### SpringBoot集成

Spring Boot 默认集成了 Logback，无需额外配置即可使用。它通过 spring-boot-starter-logging 依赖自动引入 Logback 作为日志实现，并通过 spring-boot-starter 提供了开箱即用的日志功能。启动 Spring Boot 应用时，Logback 会默认加载并记录日志。

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
</dependency>
```





























