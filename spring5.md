# spring5

##  简介

- spring : 2002 首次推出雏形 ，interface21 框架
- ＳＳＨ：SpringMvc + Spring + Mybatis
- 官网 ： https://spring.io/projects/spring-framework
- 中文文档：https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference
- 下载地址 ： https://repo.spring.io/release/org/springframework/spring
- GitHub ：https://github.com/spring-projects/spring-framework

导包 ：

~~~xml
 <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.2.12.RELEASE</version>
        </dependency>

<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.3.3</version>
</dependency>
~~~

优点 ： 

- spring 是一个开源的免费的框架

- Spring是以恶搞轻量级的，非入侵式的框架

- 控制饭庄（ioc）， 面向切面编程（AOP）

- 支持事务的处理，堆框架整合的支持

  

## spring 组成级拓展

- Spring Boot  :

  -  一个 快速开发的脚手架

  - 基于SpringBoot 可以快速的开发单个微服务

  - 约定大于配置

  

- spring Cloud
  - SpringCloud 是基于Spring Boot 实现的



## IOC理论推导:waning_crescent_moon:

之前步骤 ：

1. UserDao 接口

2. UserDaoImpl 实现了

3. UserServIce 业务接口

4. UserServiceImpl 业务实现类

   

之前代码中，用户的需求可能会影响我们原来的代码，我们需要根据用户的需求去修改源代码！ 如果程序代码量十分大，修改异常的成本十分昂贵！

> 使用set 注入后，程序不在具有主动性，而变成被动的接受对象

使用set接口

~~~java
public class UserServiceImpl implements UserService{
    private UserDao userDao;

    // 利用set进行动态实现值的注入
    public  void  setUserDao(UserDao userDao){
        this.userDao = userDao;
    }

    public void getUser(){
        userDao.getUser();
    }

}
~~~



## IOC本质

控制反转ＩＯＣ，是一种思想，DI（依赖注入）是实现IOC 的一种方法。

IOC是Spring框架的核心内容，使用多种方式完美的实现了IOC ,可以使用XML配置，也可以使用注解，新版本的Spring也可以零配置ＩＯＣ。

... .... 理论



## Hello Spring :yum:































































































、

































