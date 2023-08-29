---
title: JavaWeb学习
date: 2022-06-14 14:34:18
tags:
  - 学习笔记
categories:
  - 学习笔记
description: 学习JavaWeb的笔记
index_img: https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/javaweb%2Fv2-27e661e0e92ad69e1f63543c58eb28b0_1440w.jpg
---

# JavaWeb学习笔记

## 1. JavaWeb介绍

+ Web：World Wide Web(www),即全球广域网，也成为万维网。
+ JavaWeb: 是用Java技术来解决web互联网领域的**技术栈**。

![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/javaweb%2F124%20-%20uuSQfrP.png)

## 2. SQL

### 2.1 SQL分类

+ DDL(Data Definition Language)数据定义语言，用来定义数据库对象：数据库，表，列等
+ DML(Data Manipulation Language)数据操作语言，用来对数据库中的表的数据进行增删改
+ DQL(Data Query Language)数据查询语言，用来查询数据库中表的记录(数据)
+ DCL(Data Control Language)数据控制语言，用来定义数据库的访问权限和安全级别，及创建用户。

### 2.* 事务

#### 事务简介

+ 数据库的**事务**(Transaction)是一种机制、一个操作序列，包含了**一组数据库操作命令**
+ 事务把所有的命令作为一个整体一起向系统提交或撤销操作请求，即这一组数据库命令**要么同时成功，要么同时失败**
+ 事务是一个不可分割的工作逻辑单元

```sql
-- 开启事务
START TRANSACTION;
或者	BEGIN;
-- 提交事务
COMMIT;
-- 回滚事务
ROLLBACK;
```

#### 事物四大特征

+ 原子性(**A**tomicity):事务是不可分割的最小操作单位，要么同时成功，要么同时失败
+ 一致性(**C**onsistency):事务完成时，必须使所有的数据都保持一致状态
+ 隔离性(**I**solation):多个事务之间，操作的可见性
+ 持久性(**D**urability):事务一旦提交或回滚，他对数据库中的数据的改变就是永久的

查询事务的默认提交方式

`select @@autocommit`值为1自动提交，值为0手动提交

## 3. JDBC

### 3.1 JDBC概念:

+ JDBC就是使用Java语言操作关系型数据库的一套API
+ 全称:(Java DataBase Connectivity) Java数据库连接

![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/javaweb%2F125%20-%20FSOECYa.png)

### 3.2JDBC本质

+ 官方(sun公司)定义的一套操作所有关系型数据库的规则，即接口
+ 各个数据库厂商去实现这套接口，提供数据库驱动jar包
+ 我们可以使用这套接口(JDBC)编程，真正执行的代码是驱动jar包中的实现类

### 3.3 JDBC好处

+ 各数据库厂商使用相同的接口，Java代码不需要针对不同数据库分别开发
+ 可随时替换底层数据库，访问数据库的Java代码基本不变

### 3.4 步骤

0. 创建工程，导入驱动jar包
1. 注册驱动

​		`Class.forName("com.mysql.jdbc.Driver");`

 2. 获取连接

    `Connection conn = DriverManager.getConnection(url,username,password)`;

 3. 定义SQL语句

    `String sql = "update..."`

 4. 获取执行SQL对象

    `Statement stmt = conn.createStatement();`

 5. 执行SQL

    `stmt.executeUpdate(sql);`

 6. 处理返回结果

 7. 释放资源

### 3.5 JDBC API 详解

#### DriverManager

+ DriverManager(驱动管理类)作用：

  1. 注册驱动

     Driver类源码调用`DriverManager.registerDriver`注册驱动

  2. 获取数据库连接

     + 参数

       1. url：连接路径

       ```
       语法:jdbc:mysql://ip地址(域名):端口号/数据库名称？参数键值对1&参数键值对2...
       实例:jdbc:mysql://127.0.0.1:3306/db1
       细节:
       	1.如果连接的是本机mysql服务器，并且mysql服务默认端口是3306，则url可以简写为:jdbc:mysql:///数据库名称？参数键值对
       	2.配置useSSL=false参数，禁用安全连接方式，解决警告提示
       ```

       2. user：用户名

       3. password：密码

**提示：**

+ MySQL 5之后的驱动包，可以省略注册驱动的步骤
+ 自动加载jar包中META-INF/service/java.sql.Driver文件中的驱动类

#### Connection

+ Connection(数据库连接对象)作用：
  1. 获取执行SQL的对象
  2. 管理事务

1. 获取执行SQL的对象

   + 普通执行SQL对象

     `Statement createStatement()`

   + 预编译SQL的执行SQL对象：防止SQL注入

     `PreparedStatement prepareStatement(sql)`

   + 执行存储过程的对象

     `CallableStatement prepareCall(sql)`

2. 事务管理

   + MySQL事务管理

   ```
   开启事务:BEGIN;/START TRANSCATION;
   提交事务:COMMIT;
   回滚事务:ROLLBACK;
   MySQL默认自动提交事务
   ```

   + JDBC事务管理:Connection接口中定义了3各对应的方法

   ```
   开启事务:setAutoCommit(boolean autoCommit):true为自动提交事务;false为手动提交事务，即为开启事务
   提交事务:commit()
   回滚事务：rollback()
   ```

   

#### Statement

+ Statement作用

  执行SQL语句

  ```
  int executeUpdate(sql):执行DML、DDL语句
  返回值:(1)DML语句影响的行数(2)DDL语句执行后，执行成功也可能返回0
  ```
  
  ``` 
  ResultSet executeQuery(sql)：执行DQL语句
  返回值:ResultSet结果集对象
  ```

#### ResultSet

+ ResultSet(结果集对象)作用

  1. 封装了DQL查询语句的结果

     `ResultSet stmt.executeQuery:执行DQL语句，返回ResultSet对象`

+ 获取查询结果

  ```
  boolean next():(1)将光标从当前位置向前移动一行 (2)判断当前行是否为有效行
  返回值:
  	true:有效行，当前行有数据
  	false:无效行，当前行没有数据
  ```

  ```
  xxx getXxx(参数):获取数据
  xxx:数据类型；如：int getInt(参数);String getString(参数)
  参数:
  	int:列的编号，从1开始
  	String:列的名称
  ```

+ 使用步骤:

  1. 游标向下移动一行，并判断改行是否有数据:next()
  2. 获取数据:getXxx(参数)

  ```
  //循环判断游标是否是最后一行末尾
  while(rs.next()){
  	//获取数据
  	rs.getXxx(参数);
  }
  ```

#### PreparedStatement

+ PreparedStatement作用:

  1. 预编译SQL语句并执行:预防SQL注入问题

+ 步骤

  1. 获取PreparedStatement对象

     ```java
     //SQL语句中的参数值，使用?占位符替代
     String sql="select * from user where username = ? and password = ?""
     //通过Connection对象获取，并传入对应的sql语句
     PreparedStatement pstmt = conn.prepareStatement(sql);
     ```

  2. 设置参数值

     ```
     PreparedStatement对象:setXxx(参数1，参数2):给？赋值
     Xxx:数据类型;如setInt(参数1，参数2)
     参数:
     	参数1: ?的位置编号，从1开始
     	参数2: ?的值 
     ```

  3. 执行SQL

     ```
     executeUpdate();/executeQuery; ： 不需要再传递sql
     ```

     

+ SQL注入

  + SQL注入是通过操作输入来修改事先定义好的SQL语句，用以达到执行代码对服务器进行**攻击**的方法。

+ PreparedStatement好处:
  1. 预编译SQL，性能更高
  2. 防止SQL注入：**将敏感字符进行转义**

1. PreparedStatement预编译功能开启:`useServerPrepStmts=true`加到url后面
2. 配置MySQL执行日志(重启mysql服务后生效)

```
log-output=FILE
general-log=1
general_log_file="D:\mysql.log"
slow-query-log=1
slow_query_log_file="D:\mysql_slow.log"
long_query_time=2
```

+ PreparedStatement原理:
  1. 在获取PreparedStatement对象时，将sql语句发送给mysql服务器进行检查，编译(这些步骤很耗时)
  2. 执行时就不用再进行这些步骤了，速度更快
  3. 如果sql模板一样，则只需要进行一次检查、编译

![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/javaweb%2F126%20-%20xoXlsJW.png)

### 3.6 数据库连接池

#### 数据库连接池简介

+ 数据库连接池是个容器，负责分配、管理数据库连接(Connection)
+ 它允许应用程序重复使用一个现有的数据库连接，而不是再重新建立一个
+ 释放空闲时间超过最大空闲时间的数据库连接来避免因为没有释放数据库连接而引起的数据库连接遗漏
+ 好处:
  + 资源重用
  + 提升系统响应速度
  + 避免数据库连接遗漏

#### 数据库连接池实现

+ 标准接口:DatasSource

  + 官方(SUN)提供的数据库连接池标准接口，由第三方组织实现此接口

  + 功能:获取连接

    `Connection getConnection()`

+ 常见的数据库连接池:

  + DBCP
  + C3P0
  + Druid

+ Druid(德鲁伊)

  + Druid连接池是阿里巴巴开源的数据库连接池项目
  + 功能强大，性能优秀，是Java语言最好的数据库连接池之一

#### Druid使用步骤

1. 导入jar包 druid-1.1.12.jar
2. 定义配置文件
3. 加载配置文件
4. 获取数据库连接池对象
5. 获取连接

### 3.7 JDBC练习

#### 环境准备

+ 数据库表
+ 实体类pojo
+ 测试用例

#### 操作七个步骤

1. 获取Connection
2. **定义SQL**
3. 获取PreparedStatement对象
4. **设置参数**
5. 执行SQL
6. **处理结果**
7. 释放资源

## 4. Maven

+ Maven是专门用于管理和构建Java项目的工具，它的主要功能有:
  + 提供了一套标准化的项目结构
  + 提供了一套标准化的构建流程(编译，测试，打包，发布)
  + 提供了一套依赖管理机制

不同IDE标准化的项目结构不一样，不通用

Maven提供了一套标准化的项目结构，所有IDE使用Maven构建的项目结构完全一样，所有IDE创建的Maven项目可以通用

### Maven简介

+ Apache Maven是一个项目管理和构建**工具**，它基于项目对象模型(POM)的概念，通过一小段描述信息来管理项目的构建、报告和文档

### Maven基本使用

+ Maven常用命令
  + compile:编译
  + clean:清理
  + test:测试
  + package:打包
  + install:安装

### Maven生命周期

+ Maven构建项目生命周期描述的是一次构建过程经历了多少个实践

+ Maven对项目构建的生命周期划分为3套

  + clean:清理工作
  + default:核心工作
  + site:产生报告，发布站点等

  同一生命周期内，执行后边的命令，前边的所有命令会自动执行

### Maven坐标详解

+ 什么是坐标？
  + Maven中的坐标是**资源的唯一标识**
  + 使用坐标来定义项目或引入项目中需要的依赖
+ Maven坐标主要组成
  + groupId:定义当前Maven项目隶属组织名称(通常是域名反写)
  + artifactId:定义当前Maven项目名称(通常是模块名称)
  + version:定义当前项目版本号

#### 依赖范围

+ 通过设置坐标的依赖范围(scope),可以设置对应jar包的作用范围：编译环境、测试环境、运行环境

| 依赖范围        | 编译classpath | 测试classpath | 运行classpath | 例子              |
| --------------- | :-----------: | :-----------: | :-----------: | ----------------- |
| compile(默认值) |       Y       |       Y       |       Y       | logback           |
| test            |       -       |       Y       |       -       | Junit             |
| provided        |       Y       |       Y       |       -       | servlet-api       |
| runtime         |       -       |       Y       |       Y       | jdbc驱动          |
| system          |       Y       |       Y       |       -       | 存储在本地的jar包 |

依赖范围:import 引入DependencyManagement

## 5. MyBatis

### 5.1 MyBatis简介

+ MyBatis是一款优秀的持久层框架，用于简化JDBC开发
+ MyBatis本是Apache的一个开源项目iBatis，2010年这个项目由apache software foundation迁移到了google code，并且改名为MyBatis。2013年11月迁移到Github

**持久层**

+ 负责将数据保存到数据库的那一层代码
+ JavaEE三层架构：表现层、业务层、持久层

**框架**

+ 框架就是一个半成品软件，是一套可重用的、通用的、软件基础代码模型
+ 在框架的基础之上构建软件编写更加高效、规范、通用、可扩展

**JDBC缺点**

1. 硬编码
   + 注册驱动，获取连接
   + SQL语句
2. 操作繁琐
   + 手动设置参数
   + 手动封装结果集

![](https://dognew-1312375098.cos.ap-nanjing.myqcloud.com/javaweb%2F127%20-%20PCmpW47.png)

+ MyBatis免除了几乎所有的JDBC代码以及设置参数和获取结果集的工作

### 5.2 MyBatis快速入门

举例：查询user表中所有数据

1. 创建user表，添加数据

2. 创建模块，导入坐标

3. 编写MyBatis核心配置文件 -- >替换连接信息 解决硬编码问题

4. 编写SQL映射文件 -- > 统一管理sql语句，解决硬编码问题

5. 编码

   1. 定义POJO类
   2. 加载核心配置文件,获取SqlSessionFactory对象

   ```java
   String resource = "mybatis核心配置文件名";
   InputStream inputStream = Resources.getResourceAsStream(resource);
   SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
   ```

   3. 获取SqlSession对象，执行SQL语句

   `SqlSession sqlSession = sqlSessionFactory.openSession();`

   `sqlSession.selectList("命名空间.id");`

   4. 释放资源

### 5.3 解决SQL映射文件的警告提示

+ 产生原因:Idea和数据库没有建立连接，不识别表信息
+ 解决方式:在dea中配置MySQL数据库连接

### 5.4 Mapper代理开发

+ 目的
  + 解决原生方式中的硬编码
  + 简化后期执行SQL

+ 步骤

  1. 定义与SQL映射文件同名的Mapper接口，并且将Mapper接口和SQL映射文件放置在同一目录下
  2. 设置SQL映射文件的namespace属性为Mapper接口全限定名
  3. 在Mapper接口中定义方法，方法名就是SQL映射文件中sql语句的id，并保持参数类型和返回值类型一致
  4. 编码
     1. 通过SqlSession的getMapper方法获取Mapper接口的代理对象
     2. 调用对应方法完成sql的执行

  + 细节：如果Mapper接口名称和SQL映射文件名称相同，并在同一目录下，则可以使用包扫描的方式简化SQL映射文件的加载`<package name="com.it.map"/>`

### 5.5 Mybatis核心配置文件

+ 去官网查

细节：配置各个标签时，需要遵守前后顺序

### 5.6 关于映射别名

实体类属性名和数据库表列明不一致，不能自动封装数据

1. 起别名：在SQL语句中，对不一样的列名起别名，别名和实体类属性名一样可以定义`<sql>`片段，提升复用性
2. resultMap：定义`<resultMap>`完成不一致的属性名和列名的映射
