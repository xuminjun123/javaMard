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



## 注解实现装配:information_source:



要是有注解须知 ： 

 1. 导入约束

 2. 配置注解的支持

    ~~~xml
    <context:annotation-config></context:annotation-config>
    ~~~



~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
         http://www.springframework.org/schema/context
         http://www.springframework.org/schema/context/spring-context.xsd
         http://www.springframework.org/schema/aop
         http://www.springframework.org/schema/tool/spring-aop.xsd">
    <context:annotation-config></context:annotation-config>

    <bean id="cat" class="com.kuang.pojo.Cat"></bean>
    <bean id="dog" class="com.kuang.pojo.Dog"></bean>

    <bean id="people" class="com.kuang.pojo.People"></bean>
</beans>
~~~



- @Autowaired

  直接在 属性上或者 setf方式上使用

  使用Autowired 我们可以不用编写Set方法，前提是你自动装配的属性IOC（Spring） 容器中存在，且符合byname!

  



【  科普  】：

~~~java
@Autowired(requierd =  false) 说明这个对象可以 为 null
    					      true 则不能为空
~~~

~~~java
@Nullable 字段标记这个注解 ,说明这个字段可以为 null
~~~

~~~java
@QuaQualifier("value=dog")  可以显示定义唯一的
~~~

如果@Autowired 自动装配 环境比较复杂 可以 使用@QuaQualifier(value="xxx")  ,指定唯一对象

~~~ java
@Resource 组合注解 
~~~



小结:

- @Resource 和@Autowrised 的区别
  - 都是用来 自动装配的,都可以放在属性上
  - @Autowrised 通过 byType 方式实现,而且必须要求这个对象村咋    **常用**
  - @Resource 默认通过 byname 方式是实现,如果找不到名字,就使用 byType  实现 ,如果两个都找不到的情况下,就报错 





## 使用注解开发

~~~java
package com.kuang.pojo;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

//  @Component 等价于 在xml 中创建bean
//  相当于在xml中 <bean id="user" class="com.kuang.pojo"/>
@Component
public class User {

//    @Value 相当于
//    <property name="name" value="小明"/>
//    @Value("小明")
//    public String name;


    public String name;

    @Value("小明")
    public void setName(String name) {
        this.name = name;
    }
}
~~~





步骤 :  

1. 导包 , 

2. 使用注解导入 context 约束 ,增加注解的支持

3. @ Component / @Value

4. 衍生注解 === @component

   -  dao  [ @Repository  ]

   - service   [  @Service  ] 

   - controller [  @Controller ]

     

   这4个注解 功能是一样的,都是代表,将某个类注册到Spring中,装配bean

5. 自动装配bean

~~~xmL
@Autowired(requierd =  false) 说明这个对象可以 为 null
    					      true 则不能为空
							自动装配通过类型名字
~~~



6. 作用域

`@Scope`("singleton")....

7. 小结

xml 与 注解 : 

-  XML更加万能 ,适用于人户场合,维护简单
- 注  解   不是自己类使用不了,维护难

最佳实践 : xml 用来管理bean,注解只负责完成属性的注入







## 使用JavaConfig 实现配置



实体类

~~~java
package com.kuang.pojo;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

//  这个 注解的意思 就是说明 这个类被Spring 接管，注册到容器中
@Component
public class User {
    private String name;

    public String getName() {
        return name;
    }

    @Value("小明") // 属性注入值
    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }
}
~~~



配置类

~~~java
package com.kuang.config;

import com.kuang.pojo.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

// 这个也会Spring 容器中，注册到容器中，因为他本来就是一个@Component
@Configuration
@ComponentScan("com.kuang.pojo") // 扫描包
@Import(KuangConfig.class)
public class KuangConfig {

   // 注册一个bean ,就相当于之前 的bean.xml
   // 这个方法的名字，=== bean.xml 中的id属性
   // 这个方法的返回值，相当于bean.xml标签中的class
  @Bean
  public User getUser(){
      return new User();  // 就是要返回注入的bean对象
  }
}
~~~



测试类

~~~java
import com.kuang.config.KuangConfig;
import com.kuang.pojo.User;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(KuangConfig.class);
        User getUser = context.getBean("getUser", User.class);
        System.out.println(getUser.getName());
    }
}
~~~



这种纯 Java的配置方式,在SpringBoot中随处可见!!!





## 代理模式:yellow_heart:

代理模式 是 SpringAOP的底层

代理模式 的分类 :

- 静态代理
- 动态代理



### 1. 静态代理 分析

- 抽象角色 : 一般会使用接口或者抽象类来解决

- 真实角色 : 被代理的角色

- 代理角色 : 代理真实角色,代理真实角色后,我们一般会做一些附属操作

- 客户 : 访问代理对象的人

  

代理模式的好处: 

- 可以使真实角色的操作跟家纯粹,不用去关重一些公共的业务

- 公共也就是交给 代理角色,实现业务的分工

- 公共业务发生扩展的时候,方便集中管理

  

缺点 : 

- 一个真实角色就会产生一个代理角色,代码量会翻倍,开发效率会变低



代码步骤  

1. 接口

~~~java
// 租房 方法
public interface Rent {
    public void rent();
}
~~~

2.  真实角色 I

~~~java
package com.kuang.demo01;

// 
public class Host implements Rent {
    public void rent(){
        System.out.println("房东要出租房子");
    }
}

~~~

3.  代理角色

~~~java
package com.kuang.demo01;

// 中介 代理角色
public class Proxy implements Rent {
    private Host host;

    public Proxy() {
    }

    public Proxy(Host host) {
        this.host = host;
    }

    @Override
    public void rent() { // 通过 中介 帮房东出租房子
        seeHouse();
        heTong();
        fare();
        host.rent();
    }

    // 看房
    public void seeHouse(){
        System.out.println("中介带 你看房子");
    }
    //收中介费
    public void heTong(){
        System.out.println("签租赁合同");
    }

    //收中介费
    public void fare(){
        System.out.println("收中介费");
    }
}
~~~

4. 客户端访问角色的人

~~~java
package com.kuang.demo01;

// 我 ，想要租房直接找房东
public class Client {
    public static void main(String[] args) {
        Host host = new Host();
//        host.rent();  // 我想找房子，当找不到只好找中介代理
        Proxy proxy = new Proxy(host);
        proxy.rent(); // 代理帮我找房子,代理一般会有附属操作
    }
}
~~~





### 2. 静态代理加深理解

 接口类

~~~java
public interface UserService {
    public void add();
    public void delete();
    public void update();
    public void query();

}
~~~



我 : 真实角色

~~~java
// 真实角色
public class UserServiceImpl  implements UserService{
    @Override
    public void add() {
        System.out.println("add");
    }

    @Override
    public void delete() {
        System.out.println("delete");
    }

    @Override
    public void update() {
        System.out.println("update");
    }

    @Override
    public void query() {
        System.out.println("query");
    }
}
~~~



代理 : 代理角色 可以添加新功能( 如:日志 )

~~~java
package com.kuang.demo02;

public class UserServiceProxy implements UserService {
    private UserService userService;

    public void setUserService(UserService userService) {
        this.userService = userService;
    }

    @Override
    public void add() {
        log("add");
        userService.add();
    }

    @Override
    public void delete() {
        log("delete");
        userService.delete();
    }

    @Override
    public void update() {
        log("update");
        userService.update();
    }

    @Override
    public void query() {
        log("query");
        userService.query();
    }

    // 添加日志方法
    public void log(String msg) {
        System.out.println("使用了日志" + msg + "信息");
    }
}

~~~



客户端访问

~~~java
public class Client {
    public static void main(String[] args) {
        UserServiceImpl userService = new UserServiceImpl();
//        userService.add();
        UserServiceProxy userServiceProxy = new UserServiceProxy();
        userServiceProxy.setUserService(userService);

        userServiceProxy.add();
    }

}
~~~



## 动态代理

- 动态代理和静态代理角色一样
- 动态代理的代理类是 动态生成的, 不是我们直接写好
- 一个动态代理类代理的是一个借口，一般对应的一类业务
- 动态代理分为 两大类 : 基于接口的动态代理 , 基于类的动态代理
  - 基于接口 ---　JDK　动态代理
  - 基于类　　cglib
  - java 字节码 实现 ： javasist

 

~~~java
// 真实角色
package com.kuang.demo02;

public interface UserService {
    public void add();
    public void delete();
    public void update();
    public void query();

}
~~~

~~~java
// 客户端

package com.kuang.demo04;

import com.kuang.demo02.UserService;
import com.kuang.demo02.UserServiceImpl;

public class Client {
    public static void main(String[] args) {

        // 真实角色
        UserServiceImpl userService = new UserServiceImpl();

        // 代理角色
        ProxyInvocationHander pih = new ProxyInvocationHander();

        // 设置要代理的对象
        pih.setTarget(userService);

        // 动态生成代理类
        UserService proxy = (UserService) pih.getProxy();

        proxy.query();
    }
}

~~~



~~~java
// 代理
package com.kuang.demo04;

import com.kuang.demo03.Rent;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

// 用这个类，自动生成代理类
public class ProxyInvocationHander implements InvocationHandler {

    // 被代理的接口
    private Object target;

    public void setTarget(Object target) {
        this.target = target;
    }

    // 生产得到代理类
    public Object getProxy(){
      return   Proxy.newProxyInstance(this.getClass().getClassLoader(),target.getClass().getInterfaces(),this);
    }

   // 处理代理实例，并返回结果
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {

        log(method.getName());
        // 动态代理机制的本质，就是使用反射机制实现
        Object result = method.invoke(target, args);
        return result;

    }
    public void  log(String msg){
        System.out.println("执行了"+ msg+ "方法");
    }
}

~~~







## AOP

### 1. 什么是AOP

`AOP`　：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。



AOP 是 oop 的延续，是软件开发的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。



利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各个部分之间的耦合度降低，提高程序的可重用性，同时提高开发效率。





















































































































、

































