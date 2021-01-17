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
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.2.12.RELEASE</version>
</dependency>


        <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.2.12.RELEASE</version>
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

bean 的xml配置

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans   xmlns="http://www.springframework.org/schema/beans" 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans 
         http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
   
   //  使用spring 来创建对象,在spring这些都称为Bean 
   // 使用Spring来创建对象 ,在spring这些都称为Bean
   // id = 变量名 
   // class = new 的对象
   // property 相当于给对象中的属性设置一个值 
    
   
    <bean id="mysqlImpl" class="com.kuang.dao.UserDaoMysqlImpl"/>
    <bean id="oracleImpl" class="com.kuang.dao.UserOracleImpl"/>

    <bean id="UserServiceImpl" class="com.kuang.service.UserServiceImpl">
        <property name="userDao" ref="mysqlImpl"/>
    </bean>
</beans>
~~~

- ref : 引用spring容器中创建好的对象

- value : 具体的值,基本数据类型

  

~~~java
// 创建上下文对象
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");
~~~

现在 可以彻底不在程序中去改动了,要实现不同的操作,只需要在xml配置文件中进行修改,所谓就是IOC  对象由Spring 创建 ,管理 ,装配,







## spring 创建对象的方式



1. 默认使用无参构造 ,构建对象

2. 如果想要使用有参构造

   第一种 ,下标赋值

   ~~~java
   <bean id="user" class="com.kuang.pojo.User">
           <constructor-arg index="0" value="狂神说Java"></constructor-arg>
    </bean>
   ~~~

   第二种不建议使用:通过类型创建

   ~~~java
       <bean id="user" class="com.kuang.pojo.User">
          <constructor-arg type="java.lang.String" value="小明"></constructor-arg>
       </bean>
   ~~~

   第三种 : 直接通过参数名 来设置

   ~~~java
       <bean id="user" class="com.kuang.pojo.User">
           <constructor-arg name="name"  value="小明"></constructor-arg>
       </bean>
   ~~~

   

总结: 在配置文件加载时候,容器中管理的对象就已经初始化了,



## spring的配置说明

### 1. 别名

~~~xml
// 没啥用 ,因为有 bean 设置name
    <alias name="user" alias="userAs"></alias>

~~~

### 2. bean的配置

- id :  bean的唯一标识符,也就是我们学的对象名

- class : bean 对象的包名 + 类型

- name : 也就是 别名 ,还可以去多个名字

  

### 3. 导入 import

import ,一啊不能用于团队使用,它可将多个配置文件,导入合并为一个

~~~xml
    <import resource="beans2.xml"></import>
~~~



## DI 依赖注入:zap:

###   1. 构造器注入

   ... (  前面有 )

### 2. Set 方式注入[重点]

依赖注入 : Set注入

- 依赖 : bean 对象的常见依赖于容器
- 注入 : bean 对象中的所有属性,有容器来注入



【 环境搭建 】

1. 复杂类型

2. 真实测试对

   

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

    <bean id="address" class="com.kuang.pojo.Address"></bean>

    <bean id="student" class="com.kuang.pojo.Student">
        <!--        第一种 : 普通值注入-->
        <property name="name" value="小明"></property>
        <!--        第二种 ： bean 注入 引用类型 ref -->
        <property name="address" ref="address"></property>
        <!--        第三种： 数组注入-->
        <property name="books">
            <array>
                <value>红楼梦</value>
                <value>西游记</value>
                <value>水浒传</value>
                <value>三国演义</value>
            </array>
        </property>

        <!--        list 注入-->
        <property name="hobbys">
            <list>
                <value>听歌1</value>
                <value>听歌2</value>
                <value>听歌3</value>
            </list>
        </property>
        <!--    map注入-->
        <property name="card">
            <map>
                <entry key="身份证" value="123"></entry>
                <entry key="银行卡" value="1234"></entry>
            </map>
        </property>

<!--        set-->
        <property name="games">
            <set>
                <value>LOL1</value>
                <value>LOL2</value>
                <value>LOL3</value>
            </set>
        </property>

<!--        空值注入-->
        <property name="wife">
            <null></null>
        </property>

        <property name="info">
            <props>
                <prop key="driver">2020</prop>
                <prop key="sex">男</prop>
            </props>
        </property>
    </bean>
</beans>
~~~





### 3. 拓展方式注入

~~~xml
 xmlns:p="http://www.springframework.org/schema/p"
 xmlns:c="http://www.springframework.org/schema/c"
~~~



**1. p命名空间注入**

~~~xml
<!--    p 命名空间注入 可以直接注入属性的值 p == property-->
    <bean id="user" class="com.kuang.pojo.User" p:name="小明" p:age="18">
    </bean>
~~~

**2. c命名空间注入**

~~~xml
<!--    c 命名空间注入 ，通过构造器注入 c == constructs  -->
    <bean id="user2" class="com.kuang.pojo.User"  c:name="小红" c:age="18">
    </bean>
~~~



测试:

~~~java
    @Test
    public void test2(){
        ApplicationContext context = new ClassPathXmlApplicationContext("userbeans.xml");
        User user = context.getBean("user", User.class);
        System.out.println(user);
    }
~~~





### bean 的作用域

- scope =" singleton" 单例模式   （ 所有的共享一个对象 ）

~~~xml
<bean scope="singleton" />
~~~

- scope =" prototype"  原型模式   （ 每个都是单独对象啊 ）

- 其余的 request  , session  , application 必须在web 开发中使用到 。







## 自动装配Bean

-  自动装配是 Spring 满足 bean 依赖的一种方式
- Spring 会在 上下文中自动寻找 ，并且自动给bean 装配属性



在Spring 有三种装配方式 

1. 在xml中显示的配置
2. 在java中显示配置
3. **隐式的自动装配** [ 重点  ]



环境搭建： 

1. cat，dog，people 实体类 

2. beans 配置

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans   xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

    <bean id="cat" class="com.kuang.pojo.Cat"></bean>
    <bean id="dog" class="com.kuang.pojo.Dog"></bean>
    <bean id="people" class="com.kuang.pojo.People">
        <property name="name" value="小明"></property>
        <property name="dog" ref="dog"></property>
        <property name="cat" ref="cat"></property>
     </bean>
</beans>
~~~

3. 测试环境

~~~java
import com.kuang.pojo.People;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyTest {

    @Test
    public void test1() {
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        People people = context.getBean("people", People.class);
        people.getDog().shout();
        people.getCat().shout();
    }
}
~~~



第一种：  byname

~~~xml
   <bean id="cat" class="com.kuang.pojo.Cat"></bean>
    <bean id="dog" class="com.kuang.pojo.Dog"></bean>
    <bean id="people" class="com.kuang.pojo.People" autowire="byName">
        <property name="name" value="小明"></property>
    </bean>
~~~

```tex
byname ： 会自动创建容器上下文查找，和自己对象set方法后面的值对应的beanid
```



第二种：byType

~~~xml
autowire="byType"
~~~

~~~tex 
byType ： 会自动创建容器上下文查找，和自己对象属性类型相同的bean
~~~



## 注解实现装配



































































































































































































、

































