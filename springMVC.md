# SpringMVC

## ＭＶＣ简介 

Controller ： 控制器

1. 取得表单数据
2. 调用业务逻辑
3. 转向指定的页面



Model : 模型

1. 业务逻辑
2. 保存数据的状态

View： 视图

1. 显示页面



SpringMVC　特点　：

　1. 轻量级，简单易学

2. 高效，基于请求响应的ＭＶＣ框架
3. 约定大于配置
4. 与spring 谦容性好，无缝结合
5. RESTful,数据验证，格式化，本地化，主题等
6. 简单灵活



原理 ...

包 

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>SpringMVC</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>springmvc-01-serlvert</module>
        <module>springmvc-02-hellomvc</module>
    </modules>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
    </dependencies>

</project>
~~~



resources ---> spring -serlet

~~~xml

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

    
    // 处理器映射器
    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
    // 处理器适配器
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
    // 视图解析器  ： 模板引擎 
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="InternalResourceViewResolver">
     // 前缀   
        <property name="prefix" value="/WEB-INF/jsp/"/>
     // 后缀   
        <property name="suffix" value=".jsp"/>
    </bean>

    <bean id="/hello" class="com.kuang.controller.HelloController"/>
</beans>
~~~



~~~xml
   // web.xml
// 1, 注册servlet
     <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
       // 通过舒适化参数指定springMVC配置文件的位置，进行关联
       <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
// 启动顺序，数字越小 ，启动越早
        <load-on-startup>1</load-on-startup>
    </servlet>
    

// 所有请求都会被SpringＭＶＣ拦截
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
~~~



## 注解开发 SpringMVC





## SpringMVC整合mybatis

目录结构

<img src="D:\typora\JAVA-MD\springMVC\整合1.png" alt="整合1" style="zoom:50%;" />

1. 新建一个普通的 maven项目
2. pom.xml 导入依赖

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>ssmbuild</artifactId>
    <version>1.0-SNAPSHOT</version>
    <!--    依赖：junit,数据库驱动，连接池，servlet,jsp,mybatis,mybatis-spring,spring-->
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.46</version>
        </dependency>
        <dependency>
            <groupId>com.mchange</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.5.4</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.2</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.2</version>
        </dependency>
         <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.4.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.2.4.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.16</version>
        </dependency>
    </dependencies>

    <!--Maven静态资源过滤问题-->
    <build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
        </resources>
    </build>
</project>
~~~



2 .建立数据库 ssmbuild

~~~mysql
CREATE DATABASE ssmbuild;
USE ssmbuild;
CREATE TABLE `books`(
`bookID` INT NOT NULL AUTO_INCREMENT COMMENT '书id',
`bookName` VARCHAR(100) NOT NULL COMMENT '书名',
`bookCounts` INT NOT NULL COMMENT '数量',
`detail` VARCHAR(200) NOT NULL COMMENT '描述',
KEY `bookID`(`bookID`)
)ENGINE=INNODB DEFAULT CHARSET=utf8;

INSERT INTO `books`(`bookID`,`bookName`,`bookCounts`,`detail`)VALUES
(1,'Java',1,'从入门到放弃'),
(2,'MySQL',10,'从删库到跑路'),
(3,'Linux',5,'从进门到进牢')
~~~

3.  idea 链接数据库 ssmbuild

4. resources 新建一个 database.properties

   ~~~sh
   jbdc.driver=com.mysql.Driver
   # 如果使用的是 mysql8.0+ ,增加一个mysql时区 &serverTimezone=Asia/Shanghai
   jbdc.url=jdbc:mysql://localhost:3306/ssmbuild?useSSL=true&useUnicode=true&characterEncoding=utf8
   jbdc.username=root
   jbdc.password=root
   ~~~

5. java包新建 dao,controller,pojo,service 的包

6. 新建一个 实体类

   ~~~java
   package com.kuang.pojo;
   
   import lombok.AllArgsConstructor;
   import lombok.Data;
   import lombok.NoArgsConstructor;
   
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class Books {
       private int bookID;
       private String bookName;
       private int bookCounts;
       private String detail;
   }
   ~~~

7. dao层 BookMapper的接口

   ~~~java
   package com.kuang.dao;
   
   import com.kuang.pojo.Books;
   import org.apache.ibatis.annotations.Param;
   
   import java.util.List;
   
   public interface BookMapper {
       // 增加一本书
       int addBook(Books books);
   
       // 删除一本书
       int deleteById(@Param("bookId") int id);
   
       // 更新一本书
       int updateBook(Books books);
   
       // 查询一本书
       Books queryBookById(@Param("bookId") int id);
   
       // 查询全部的书
       List<Books> queryAllBook();
   }
   ~~~

8. BookMapper.xml

   ~~~xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.kuang.dao.BookMapper">
       <insert id="addBook" parameterType="Books">
           insert into books (bookName,bookCount,detail)
           values (#{bookName},#{bookCounts},#{detail});
       </insert>
   
       <delete id="deleteBookById" parameterType="int">
           delete from books where bookID = #{bookId}
       </delete>
   
       <update id="updateBook" parameterType="Books">
           update books
           set bookName=#{bookName},bookCounts=#{booCounts},detail={detail}
           where bookID=#{bookID};
       </update>
   
       <select id="queryBookById" resultType="Books">
           select * from books
       </select>
   
       <select id="queryAllBook" resultType="Books">
           select * from books
       </select>
   
   </mapper>
   ~~~

9. service层  BookService接口类

   ~~~java
   package com.kuang.service;
   
   import com.kuang.pojo.Books;
   import org.apache.ibatis.annotations.Param;
   
   import java.util.List;
   
   public interface BookService {
       // 增加一本书
       int addBook(Books books);
   
       // 删除一本书
       int deleteById(int id);
   
       // 更新一本书
       int updateBook(Books books);
   
       // 查询一本书
       Books queryBookById(int id);
   
       // 查询全部的书
       List<Books> queryAllBook();
   }
   ~~~

   

10. service层  BookService实现类

~~~java
package com.kuang.service;

import com.kuang.dao.BookMapper;
import com.kuang.pojo.Books;

import java.util.List;

public class BookServiceImpl implements BookService{
    // 业务层调用dao层 ： 组合Dao
    private BookMapper bookMapper;

    public void setBookMapper(BookMapper bookMapper) {
        this.bookMapper = bookMapper;
    }

    @Override
    public int addBook(Books books) {
        return bookMapper.addBook(books);
    }

    @Override
    public int deleteById(int id) {
        return bookMapper.deleteById(id);
    }

    @Override
    public int updateBook(Books books) {
        return bookMapper.updateBook(books);
    }

    @Override
    public Books queryBookById(int id) {
        return bookMapper.queryBookById(id);
    }

    @Override
    public List<Books> queryAllBook() {
        return bookMapper.queryAllBook();
    }
}
~~~





## SpringMVC整合spring



1. spring-dao.xml

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
   
       <!--1. 关联数据文件-->
       <context:property-placeholder location="classpath:database.properties"/>
   
       <!--2. 连接池
           dbcp: 半自动化操作 ，不能自动链接
           c3p0: 自动化操作 （自动化的加载配置文件,并且可以自动链接）
           druid:
           hikari:
       -->
       <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
           <property name="driverClass" value="${jbdc.driver}"/>
           <property name="jdbcUrl" value="${jbdc.url}"/>
           <property name="user" value="${jbdc.username}"/>
           <property name="password" value="${jbdc.password}"/>
   
           <!--c3p0 连接池的私有属性   -->
           <property name="maxPoolSize" value="30"/>
           <property name="minPoolSize" value="10"/>
           <!--关闭连接后不自动commit-->
           <property name="autoCommitOnClose" value="false"/>
           <!--  获取链接超时时间-->
           <property name="checkoutTimeout" value="10000"/>
           <!-- 当获取链接失败重试次数 -->
           <property name="acquireRetryAttempts" value="2"/>
       </bean>
       <!--3.　sqlSessionFactory -->
       <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
           <property name="dataSource" ref="dataSource"/>
           <!--绑定Mybatis的配置文件 -->
           <property name="configLocation" value="classpath:mybatis-config.xml"/>
       </bean>
   
       <!--4. 配置dao接口扫描包，动态的实现了Dao接口可以注入到Spring容器中-->
       <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
           <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
           <!-- 要扫描的包 -->
           <property name="basePackage" value="com.kuang.dao"/>
       </bean>
   </beans>
   ~~~

2. spring-service.xml

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
   
       <!--1. 扫描service下的包-->
       <context:component-scan base-package="com.kuang.service"/>
   
       <!--2. 将我们的所有的业务类，注入到Spring ， 可以通过配置，或者注解实现-->
       <bean id="BookServiceImpl" class="com.kuang.service.BookServiceImpl">
           <property name="bookMapper" ref="bookMapper"/>
       </bean>
   
       <!--3. 声明式事务-->
       <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
           <!--注入数据源 -->
           <property name="dataSource" ref="dataSource"/>
       </bean>
       <!--4. aop事务支持 -->
       
   </beans>
   ~~~



## ssm框架 整合springMVC



1. 

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/cache
http://www.springframework.org/schema/cache/spring-cache.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!--1. 注解驱动-->
    <mvc:annotation-driven/>
    <!--2. 静态资源过滤-->
    <mvc:default-servlet-handler/>
    <!--3. 扫描包 controller -->
    <context:component-scan base-package="com.kuang.controller"/>
    <!--4. 视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
~~~





---

以上 SpringＭＶＣ整合完毕！

## CURD 实战



1. 查询书籍功能 controller 层

~~~java

~~~





### application.yml

~~~yml
server:
  # 端口
  port: 8081

spring:
  # 数据源配置
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/yeb?useUnicode=true&characterEncoding=UTF-8&?serverTimezone=Asia/Shanghai
    username: root
    password: root
    hikari:
      # 连接池名
      pool-name: DataHikariCP
      # 最小空闲链接数
      minimum-idle: 5
      # 空闲连接存活最大时间，默认 600000（10分钟）
      idle-timeout: 180000
      # 最大链接数 默认10
      maximum-pool-size: 10
      # 链接最大存活时间 0:永久默认1800000（30分钟）
      max-lifetime: 1800000
      # 链接超时时间 默认30000 （30秒）
      connection-timeout: 30000
      # 测试链接是否可用的查询语句
      connection-test-query: SELECT 1


# Mybatis-plus 配置
  # 配置Mapper映射文件
  mapper-locations: classpath*:/mapper/*Mappper.xml
  # 配置Mybatis数据返回类型别名 （默认别名是类名）
  type-aliases-package: com.xxxx.server.pojo
  configuration:
    # 自动驼峰命名
    map-underscore-to-camel-case: false

## Mybatis Sql 打印（ 方法接口所在的包，不是Mapper.xml所在的包 ）
logging:
  level:
    com.xxxx.server.mapper: debug
~~~



log4j

























