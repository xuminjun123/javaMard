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
            <artifactId>spring-jdbc</artifactId>
            <version>5.1.9.RELEASE</version>
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



1.