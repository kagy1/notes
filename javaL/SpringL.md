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

### IOC



## 常用注解

### `@Configuration`

**类级注解**：`@Configuration` 标注在类上，表明这个类可以包含一个或多个 `@Bean` 方法，并且 Spring 容器可以使用该类作为配置类来生成 bean 定义和自动装配。

### `@Bean`

**方法级注解**：用在方法上，声明这个方法返回一个 Spring 管理的 bean。该方法的返回值会被注册到 Spring IoC 容器中。

**单独使用时注意**：如果将 `@Bean` 定义在没有被 `@Configuration` 标记的类中（比如一个普通类或 `@Component` 类），Spring 仍然会注册这个 bean，但是这些方法之间直接调用时就不会走 Spring 容器的代理机制，因此可能会导致每次调用方法都会创建新的对象，无法保证单例性。