---
title: spring学习
date: 2022-06-13 20:59:51
tags:
  - 学习笔记
categories:
  - 学习笔记
index_img: https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/spring%2F36fda1085cffacaebc7613ab8f227351.jpg
description: 去年专业实训没做笔记学完都忘了，这次要认真学补一下票
---

# Spring学习

{% note success %}

所有知识点都记下来太费时间，简单写点笔记大纲，遇到不会的再查。

{% endnote %}

## 1. Spring简介

### 1.1 Spring是什么

spring是分层的Java SE/EE应用full-stack轻量级开源框架，以**IoC**(Inverse Of Control:反转控制)和**AOP**(Aspect Oriented Programming:面向切面编程)为内核。

提供了**展现层SpringMVC**和**持久层Spring JDBCTemplate**以及**业务层事务管理**等众多的企业级应用技术，还能整合开源世界众多著名的第三方框架和类库，逐渐成为使用最多的Java EE企业应用开源框架

### 1.2 Spring的优势

1. 方便解耦，简化开发
   + 通过Spring提供的IoC容器，可以将对象间的依赖关系交由Spring进行控制，避免硬编码所造成的过度耦合。用户也不必再为单例模式类、属性文件解析等这些很底层的需求编写代码，可以更专注于上层的应用。

2. AOP变成的支持
   + 通过Spring的AOP功能，方便进行面向切面编程，许多不容易用传统OOP实现的功能可以通过AOP轻松实现

3. 声明式事物的支持
   + 可以将我们从单调烦闷的事务管理代码中解脱出来，通过声明式方式灵活的进行事务管理，提高开发效率和质量。

4. 方便程序的测试
   + 可以用非容器依赖的编程方式进行几乎所有的测试工作，测试不再是昂贵的操作，而是随手可做的事情。

5. 方便继承各种优秀框架
   + Spring对各种优秀框架(Struts、Hibernate、Hessian、Quartz等)的支持。

6. 降低JavaEE API的使用难度
   + Spring对JavaEE API(如JDBC、JavaMail、远程调用等)进行了薄薄的封装层，使这些API的使用难度大大降低。

7. Java源码是经典学习范例
   + Spring的源代码设计精妙、结构清晰、匠心独用，处处体现着大师对Java设计模式灵活运用以及对Java技术的高深造诣。它的源代码无意是Java技术的最佳实践的范例。

### 1.3 Spring的体系结构

![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/spring%2F116%20-%20dRRF0lY.png)

## 2. Spring快速入门

### 2.1 Spring程序开发步骤

![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/spring%2F117%20-%20ERsHZdA.png)

1. 导入Spring开发的基本包坐标
2. 编写Dao接口和实现类(Bean)
3. 创建Spring核心配置文件(xml文件)
4. 在Spring配置文件中配置UserDaoImpl(实现类)
5. 使用Spring的API获得Bean实例(创建ApplicationContext来getBean，用id号时要用到强制转换，或者可传`类名.class`)

## 3. Spring配置文件

### 3.1Bean标签基本配置

用于配置对象交由**Spring**来创建。

默认情况下它调用的使类中的**无参构造函数**，如果没有无参构造函数则不能创建成功

基本属性:

+ **id**:Bean实例在Spring容器中的唯一标识
+ **class**:Bean的全限定名称

`<bean id="id唯一" class="路径+类名"><\bean>`

### 3.2Bean标签范围配置

scope:指对象的作用范围，取值如下：

| 取值范围       | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| singleton      | 默认值，单例的   （加载配置文件时创建对象）                  |
| prototype      | 多例的      （getBean时创建对象）                            |
| request        | WEB项目中，Spring创建一个Bean的对象，将对象存入到request域中 |
| session        | WEB项目中，Spring创建一个Bean的对象，将对象存入到session域中 |
| global session | WEB项目中，应用在Portlet环境，如果没有Portlet环境那么globalSession相当于session |

1. 当scope的取值为**singleton**时
   + Bean的实例化个数:1个
   + Bean的实例化时机:当Spring核心文件被加载时，实例化配置的Bean实例
   + Bean的生命周期：
     + 对象创建:当应用加载，创建容器时，对象就被创建了
     + 对象运行:只要容器在，对象一直活着
     + 对象销毁:当应用卸载，销毁容器时，对象就被销毁了
2. 当scope的取值为prototype时
   + Bean的实例化个数:多个
   + Bean的实例化时机:当调用getBean()方法时实例化Bean
     + 对象创建:当使用对象时，创建新的对象实例
     + 对象运行:只要对象在使用中，就一直活着
     + 对象销毁:当对象长时间不用时，被Java的垃圾回收器回收了

### 3.3 Bean的生命周期配置

+ **init-method**:指定类中的初始化方法名称
+ **destroy-method**:指定类中销毁方法名称

### 3.4 Bean实例化三种方式

+ 无参**构造**方法实例化
+ 工厂**静态**方法实例化
  + 新建工厂类，设置**静态**方法返回bean类
  + class到工厂类，后面添加属性`factory-method="方法"`
  + 例如`<bean id="userDao" class="com.it.factory.StaticFactory" factory-method="getUserDao"></bean>`其中`getUserDao()`方法为工厂中返回UserDao类型的对象

+ 工厂**实例**方法实例化

  + 不再需要static方法

  + 举例:

    ```
    //spring帮忙创建工厂对象
    <bean id="factory" class="com.it.factory.DynamicFactory"></bean>
    //方法获得指定的UserDao类型对象
    <bean id="userDao" factory-bean="factory" factory-method="getUserDao"/>
    ```

### 3.5 Bean的依赖注入分析

![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/spring%2F118%20-%20h5W0SgN.png)

因为UserService和UserDao都在Spring容器中，而最终程序直接使用的时UserService，所以可以在Spring容器中，将UserDao设置到UserService内部。

![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/spring%2F119%20-%2050DlrRN.png)

### 3.6 Bean的依赖注入概念

依赖注入(Dependency Injection): 它是Spring框架核心IOC的具体实现

在编写程序时，通过控制反转，把对象的创建交给了Spring，但是代码中不可能出现没有以来的情况。

IOC解耦只是降低他们的依赖关系，但不会消除。例如:业务层仍会调用持久层的方法。

那这种业务层和持久层的依赖关系，在使用Spring之后，就让Spring来维护了。

简单地说，就是坐等框架把持久层对象传入业务层，而不用我们自己去获取。

### 3.7 Bean的依赖注入方式

+ 构造方法
  + `<constructer-arg>`标签 ref属性连接注入类
+ set方法
  + `<property>`标签 ref属性连接注入类

### 3.8 Bean的依赖注入的数据类型

上面的操作，都是注入的引用Bean，除了对象的引用可以注入，普通数据类型，集合等都可以在容器中进行注入。

注入数据的三种数据类型:

+ 普通数据类型 没有ref属性，用value传参
+ 引用数据类型
+ 集合数据类型 properties内部加子标签

### 3.9 引入其他配置文件(分模块开发)

实际开发中，Spring的配置内容非常多，这就导致Spring配置很繁杂且体积很大，所以，可以将部分配置拆解到其他配置文件中，而在Spring主配置文件通过import标签进行加载

`<import resource=".xml">`

### 3.10 知识总结

**Spring的重点配置**

+ `<bean>`标签
  + id属性:在容器中Bean实例的唯一标识，不允许重复
  + class属性:要实例化的Bean全限定名
  + scope属性:Bean的作用范围，常用是Singleton(默认)和prototype
  + `<property>`标签:属性注入
    + name属性:属性名称
    + value属性:注入的普通属性值
    + ref属性:注入的对象引用值
    + `<list>`标签
    + `<map>`标签
    + `<properties>`标签
  + `<constructor-arg>`标签
+ `<import>`标签:导入其他的Spring的分文件

## 4. SpringAPI

### 4.1 ApplicationContext的继承体系

applicationContext:接口类型，代表应用上下文，可以通过其实例获得Spring容器中的Bean对象

![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/spring%2F120%20-%203yIHrhF.png)

### 4.2 ApplicationContext的实现类

1. **ClassPathXmlApplicationContext** 传一个xml配置文件

   它是从类的根路径下加载配置文件 推荐使用这种

2. **FileSystemXmlApplicationContext**

​		它是从磁盘路径上加载配置文件，配置文件可以在磁盘的任意位置

3. **AnnotationConfigApplicationContext**

​		当使用注解配置容器对象时，需要使用此类来创建spring容器。用它来读取注解

### 4.3 getBean()方法使用

可传id和字节码对象

getBean(“id”)

getBean(class)

## 5. Spring配置数据源

### 5.1 数据源(连接池)的作用

+ 数据源(连接池)是提高程序性能出现的
+ 实现实例化数据源，初始化部分连接资源
+ 使用连接资源时从数据源中获取
+ 使用完毕后将连接资源归还给数据源

常见的数据源(连接池):DBCP、C3P0、BoneCP、Druid等

### 5.2 数据源开发步骤

1. 导入数据源的坐标和数据库驱动坐标
2. 创建数据源对象
3. 设置数据源的基本连接数据
4. 使用数据源获取连接资源和归还连接资源

### 5.3 数据源的手动创建

解耦合数据库参数到properties配置文件

采用ResourceBundle中的getBundle方法读取配置文件

### 5.4 Spring配置数据源

可以将DataSource的创建权交由Spring容器去完成

### 5.5 抽取jdbc配置文件

applicationContext.xml加载jdbc.properties配置文件获得连接信息

首先，需要引入context命名空间和约束路径:

+ 命名空间:`xmlns:context="http://www.springframework.org/schema/context"`
+ 约束路径:`http://www.springframework.org/schema/context`

​						`http://www.springframework.org/schema/context/spring-context.xsd`

![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/spring%2F121%20-%20NUti9u7.png)

### 5.6 知识要点

**Spring容器加载properties文件**

![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/spring%2F122%20-%20sMuZn1J.png)

## 6. Spring注解开发

### 6.1 Spring原始注解

spring是轻代码而重配置的框架，配置比较繁重，影响开发效率，所以注解开发是一种趋势，注解代替xml配置文件可以简化配置，提高开发效率。

Spring原始注解主要是替代`<Bean>`标签的配置

| 注解            | 说明                                           |
| --------------- | ---------------------------------------------- |
| **@Component**  | 使用在类上用于实例化Bean                       |
| **@Controller** | 使用在web层类上用于实例化Bean                  |
| **@Service**    | 使用在service层类上用于实例化Bean              |
| **@Repository** | 使用在dao层类上用于实例化Bean                  |
| **@Autowired**  | 使用在字段上用于根据类型依赖注入               |
| **@Qualifier**  | 结合@Autowired一起使用用于根据名称进行依赖注入 |
| **@Resource**   | 相当于@Autowired+@Qualifier,按照名称进行注入   |
| **@Value**      | 注入普通属性 ${}                               |
| **@Scope**      | 标注Bean的作用范围                             |
| @PostConstruct  | 使用在方法上标注该方法是Bean的初始化方法       |
| @PreDestroy     | 使用在方法上标注该方法是Bean的销毁方法         |

注意:使用注解进行开发时，需要在applicationContext.xml中配置组件扫描，作用是指定哪个包及其子包下的Bean需要进行扫描以便识别使用注解配置的类、字段和方法。

![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/spring%2F123%20-%20n7NA4Xf.png)

### 6.2 Spring新注解

使用上面的注解还不能全部替代xml配置文件，还需要使用注解替代的配置如下：

+ 非自定义的Bean的配置:`<bean>`
+ 加载properties文件的配置:`<context:property-placehoder>`
+ 组件扫描的配置:`<context:component-scan>`
+ 引入其他文件`<import>`

| 注解            | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| @Configuration  | 用于指定当前类是一个Spring配置类，当创建容器时会从该类上加载注解 |
| @ComponentScan  | 用于指定Spring在初始化容器时要扫描的包。<br />作用和在Spring的xml配置文件中的<br />`<contex:component-scan base-package="com.it"/>一样` |
| @Bean           | 使用在方法上，标注将该方法的返回值存储到Spring容器中         |
| @PropertySource | 用于加载.properties文件中的配置                              |
| @Import         | 用于导入其他配置类                                           |

## 7. Spring集成Junit

### 7.1 原始Junit测试Spring的问题

在测试类中，每个测试方法都有以下两行代码:

```java
ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
IAccountService as = ac.getBean("accountService",IAccountService.class);
```

这两行代码的作用是获取容器，每个测试代码里都得有，如果不写的话，直接会提示空指针异常，所以又不能轻易删掉。

### 7.2 上述问题解决思路

+ 让SpringJunit负责创建Spring容器，但是需要将配置文件的名称告诉它
+ 将需要进行测试Bean直接在测试类中进行注入

### 7.3 Spring集成Junit步骤

+ 导入spring集成Junit的坐标
+ 使用@Runwith注解替换原来的运行期
+ 使用@ContextConfiguration指定配置文件或配置类
+ 使用@Autowired注入需要测试的对象
+ 创建测试方法进行测试

## 8. Spring集成web环境

### 8.1 ApplicationContext应用上下文获取方式

应用上下文对象是通过**new ClasspathXmlApplicationContent(spring配置文件)**方式获取的，但是每次从容器中获得Bean时都要编写**new ClasspathXmlApplicationContext(spring配置文件)**，这样的弊端时配置文件加载多次，应用上下文对象创建多次。

在Web项目中，可以使用**ServletContentListener**监听Web应用的启动，我们可以在Web应用启动时，就加载Spring的配置文件，创建应用上下文对象**ApplicationContext**，在将其存储到最大的域**servletContext**域中，这样就可以在任意位置从域中获得应用上下文**ApplicationContext**对象了。

### 8.2 Spring提供获取应用上下文的工具

Spring提供了一个监听器**ContextLoaderListener**，该监听器内部加载Spring配置文件，创建应用上下文对象，并存储到**ServletContext**域中，提供了一个客户端工具**WebApplicationContextUtils**供使用者获得应用上下文对象。

需要做的只有两件事:

1. 在**web.xml**中配置**ContextLoaderListener**监听器(导入spring-web坐标)
2. 使用**WebApplicationContextUtils**供使用者获得应用上下文对象
