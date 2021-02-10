

# 资源整合篇




## 整合Redis

详情见 Redis 篇《SpringBoot整合redis》



## 整合JBDC

1. 新建项目。链接数据库 Database;

   Resources 目录下 新建application.yml

~~~yml
spring:
  datasource:
    username: root
    password: root
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&useUnicode=true&characterEncoding=utf8&useSSL=false
~~~

2. test测试

~~~java
package com.kuang;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import javax.sql.DataSource;

@SpringBootTest
class SpringbootDataApplicationTests {

    @Autowired
    DataSource dataSource;

    @Test
    void contextLoads() {
        // 查看默认的数据源
        System.out.println(dataSource.getClass());
    }

}
~~~



测试结果如图：

![jbdc](D:\typora\JAVA-MD\springBoot整合\jbdc.png)



3. 使用 ，信息一个JBDCController

~~~java
@RestController
public class JBDCController {
    @Autowired
    JdbcTemplate jdbcTemplate;

    //  查询数据库的所有信息 ,这里没有使用mybatis
    // 没有实体类，数据库的东西，使用Map获取
    @GetMapping("/userList")
    public List<Map<String,Object>> usrList(){
        String sql="select * from user";
        List<Map<String, Object>> maps = jdbcTemplate.queryForList(sql);
        return maps;
    }
}
~~~

浏览器显示如图：

![浏览器打印](D:\typora\JAVA-MD\springBoot整合\浏览器打印.png)











## 整合 Druid

### 1. 简介

Druid 是阿里巴巴 开源平台上一个数据库连接池实现，结合C3P0、DBCP、PROXOOL等DB池的优点，同时加入日志监控。

Druid可以很好的监控 DB 池链接和SQL的执行情况。天生就是针对监控而生的DB链接池



### 2.使用

引入数据源

~~~xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.21</version>
</dependency>

<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.8.12</version>
</dependency>
~~~



要想使用指定的数据源 ，只需要早application.yml 配置 type

~~~yaml
spring:
  datasource:
    username: root
    password: root
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&useUnicode=true&characterEncoding=utf8&useSSL=false
    type: com.alibaba.druid.pool.DruidDataSource
    # 下面为连接池的补充设置，应用到上面所有数据源中

    # 初始化大小，最小，最大

    initialSize: 5
    minIdle: 5
    maxActive: 20

    # 配置获取连接等待超时的时间
    maxWait: 60000

    # 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒
    timeBetweenEvictionRunsMillis: 60000

    # 配置一个连接在池中最小生存的时间，单位是毫秒
    minEvictableIdleTimeMillis: 300000
    validationQuer: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false

    # 打开PSCache，并且指定每个连接上PSCache的大小
    poolPreparedStatements: true
    maxPoolPreparedStatementPerConnectionSize: 20

    # 配置监控统计拦截的filters，去掉后监控界面sql无法统计，'wall'用于防火墙
    filters: stat,wall,log4j

    # 通过connectProperties属性来打开mergeSql功能；慢SQL记录
    connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=5000

    # 合并多个DruidDataSource的监控数据

    #spring.datasource.useGlobalDataSourceStat=true
~~~





3. 测试使用

~~~java
package com.kuang.config;

import com.alibaba.druid.pool.DruidDataSource;
import com.alibaba.druid.support.http.StatViewServlet;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.web.servlet.ServletRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.servlet.ServletRegistration;
import javax.sql.DataSource;
import java.util.HashMap;

@Configuration
public class DruidConfig {

    @ConfigurationProperties(prefix = "spring.datasource")
    @Bean
    public DataSource druidDataSource(){
        return new DruidDataSource();
    }

    // 后台监控功能
    @Bean
    public ServletRegistrationBean  a(){
        ServletRegistrationBean<StatViewServlet> bean = new ServletRegistrationBean<>(new StatViewServlet(), "/druid/*");

        //后台需要有人登录，账户密码登录
        HashMap<String, String> initParamters = new HashMap<>();
        // 增加配置
        initParamters.put("loginUsername","admin"); // 登录Key名固定的
        initParamters.put("loginPassword","123456");

        // 运行谁可访问
        initParamters.put("allow",""); // 不写谁都可以访问

        // 禁止谁可以访问
//        initParamters.put("xuminjun","localhost")

        bean.setInitParameters(initParamters);// 初始化参数

        return bean;

    }

}
~~~



如图所示 ：

![druid9](D:\typora\JAVA-MD\springBoot整合\druid9.png)



输入设置的用户名，密码即可登录；如图

![](D:\typora\JAVA-MD\springBoot整合\Druid完.png)



可以过滤一些请求

~~~java
    @Bean
    public FilterRegistrationBean webSatFilter(){

        FilterRegistrationBean bean = new FilterRegistrationBean();

        bean.setFilter(new WebStatFilter());
        // 可以过滤哪些请求呢？
        HashMap<String, String> initparamters = new HashMap<>();
        //过滤，这些不会统计
        initparamters.put("exclusions","*.js,*.css,/druid/*");
        bean.setInitParameters(initparamters);
        return bean
    }
~~~









## 整合Mybatis

1. 导入包

~~~xml
<!-- https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.4</version>
</dependency>
~~~



2. 配置

properties / yml 

~~~properties
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&useUnicode=true&characterEncoding=utf8&useSSL=false
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
~~~



3. 测试链接

```java
package com.kuange;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import javax.sql.DataSource;
import java.sql.SQLException;

@SpringBootTest
class MybatisDemoApplicationTests {
    @Autowired
    DataSource dataSource;

    @Test
    void contextLoads() throws SQLException {
        System.out.println(dataSource.getClass());
        System.out.println(dataSource.getConnection());
    }

}
```

结果如图所示：

![测试mybatis](D:\typora\JAVA-MD\springBoot整合\测试mybatis.png)





4. 使用

- 编写 pojo 实体类 ，如：User

~~~java
package com.kuange.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class User {
    private int id;
    private String name;
    private String pwd;
}
~~~

- 接口

~~~java
package com.kuange.mapper;

import com.kuange.pojo.User;
import org.apache.ibatis.annotations.Mapper;
import org.springframework.stereotype.Repository;

import java.util.List;

@Mapper  // 表示这个是被mybatis的mapper 类Dao
@Repository
public interface UserMapper {

    List<User> queryUserList();
    User queryUserById(int id);

    int addUser(User user);
    int updateUser(User user);
    int deleteUser(int id);
}
~~~

- properties整合mybatis

~~~properties
# 整合mybatis
mybatis.type-aliases-package=com.kuang.pojo
mybatis.mapper-locations=classpath:mybatis/mapper/*.xml
~~~



- resources 编写UserMapper.xml 

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.kuange.mapper.UserMapper">
    <select id="queryUserList" resultType="User">
        select * from user 
    </select>
</mapper>

.....
~~~

- controller

~~~java
package com.kuange.controller;

import com.kuange.mapper.UserMapper;
import com.kuange.pojo.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;

import java.util.List;
@RestController
public class UserController {

    @Autowired
    private UserMapper userMapper;
    @GetMapping("/queryUserList")
    public List<User> queryUserList(){
        List<User> userList = userMapper.queryUserList();
        for (User user : userList) {
            System.out.println(user);
        }
        return userList;
    }

}
~~~























## 整合PageHelper





## 整合Shiro





见SpringBoot整合 shrio 篇





## 整合Swagger

### 1. 简介

- 最流行的API的框架，RestFul API 文档在线自动生成工具（接口文档）
- API文档与API定义同步更新
- 直接运行，可以直接测试API接口
- 支持多种语言如java、php



### 2. 使用

- 导入jar包（swagger2  ; swaggerUI  ）,新建springBoot - web项目

~~~xml
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>

<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
~~~

- 配置Swagger ==》Config

~~~java

@Configuration
@EnableSwagger2    // 开启swagger2 
public class SwaggerConfig {
}

~~~

- 测试 ：[localhost:8080/swagger-ui.html]()

![swagger](D:\typora\JAVA-MD\springBoot整合\swagger.png)









### 3. 配置Swagger

```java
package com.kuang.swagger.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.service.Contact;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

import java.util.ArrayList;

import static springfox.documentation.service.ApiInfo.DEFAULT_CONTACT;

@Configuration
@EnableSwagger2    // 开启swagger2
public class SwaggerConfig {
    // 配置swagger的Bean实例
    @Bean
    public Docket docket() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                ;
    }

    // apiInfo : swagger 配置信息
    private ApiInfo apiInfo() {

        // 作者信息
        Contact DEFAULT_CONTACT = new Contact("xu", " https://xuminjun666.gitee.io/xuboke", "1786143676@qq.com");

        return new ApiInfo("Api Documentation",
                "Api Documentation",
                "1.0", " https://xuminjun666.gitee.io/xuboke",
                DEFAULT_CONTACT, "Apache 2.0",
                "http://www.apache.org/licenses/LICENSE-2.0",
                 new ArrayList());
    }
}
```



![配置信息](D:\typora\JAVA-MD\springBoot整合\配置信息.png)



### 4. 配置扫描接口

 `Docket.select()`

~~~java
@Configuration
@EnableSwagger2    // 开启swagger2
public class SwaggerConfig {
    // 配置swagger的Bean实例
    @Bean
    public Docket docket() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                // RequestHandlerSelectors 配置扫描接口的方式
                //  .basePackage 扫描包
                //  .any 全部扫描
                //  .none 都不扫描
                //  .withClassAnnotation 扫描类注解
                //  .withMethodAnnotation 扫描方法上的注解
                .apis(RequestHandlerSelectors.basePackage("com.kuang.swagger.controller"))
                // 过滤路径 ,过滤出/XU 下的接口
                .paths(PathSelectors.ant("/XU/**"))
                .build()
                ;
    }

}
~~~

是否启用swagger

~~~java
@Bean
    public Docket docket() {
        return new Docket(DocumentationType.SWAGGER_2)
                .enable(true) // 是否启用 ，生产环境关闭，开发环境开启
                ;
    }
~~~





### 5. 配置API分组

```java
@Bean
public Docket docket1() {
    return new Docket(DocumentationType.SWAGGER_2)
        .groupName("小组1")
            ;
}


@Bean
public Docket docket2() {
    return new Docket(DocumentationType.SWAGGER_2)
            .groupName("小组2")
            ;
}

@Bean
public Docket docket3() {
    return new Docket(DocumentationType.SWAGGER_2)
            .groupName("小组3")
            ;
}

... ....
```



**实体类配置：**

新建一个实体类，如User类

~~~java
@ApiModel("用户实体类") // swagger给实体类加注释
public class User {

    // swagger给自动加注释
    @ApiModelProperty("用户名")
    public String username;
    @ApiModelProperty("密码")
    public String password;

}
~~~

为了 和 swagger建立联系，接口中返回值存在实体类

~~~java

@RestController
public class HelloController {
   
    // 只要我们的接口中返回值存在实体类，就会被swagger扫描
    @PostMapping(value="/user")
    public User User(){
        return new User();
    }

}
~~~

如图：

![实体类配置](D:\typora\JAVA-MD\springBoot整合\实体类配置.png)

 **在方法和参数加注释**

~~~java
@RestController
public class HelloController {

    @ApiOperation("Hello 控制类") // 给方法加注释
    @GetMapping(value="/hello2")
    public String hello2(@ApiParam("用户名") String username){
        return "hello";
    }

}
~~~



如图：

![注释](D:\typora\JAVA-MD\springBoot整合\注释.png)

















# Restful 风格

- URL 的命名 `必须` 全部小写

- URL 中资源（`resource`）的命名 `必须` 是名词，并且 `必须` 是复数形式

- `必须` 优先使用 `Restful` 类型的 URL

- URL 中不能出现 `-`，`必须` 用下划线 `_` 代替

- URL `必须` 是易读的

- URL `一定不可` 暴露服务器架构

~~~sh
1. 删除资源 `必须` 用 `DELETE` 方法 

2. 创建新的资源 `必须` 使用 `POST` 方法

3. 更新资源 `应该` 使用 `PUT` 方法 

4 获取资源信息 `必须` 使用 `GET` 方法
~~~

![restful](D:\typora\JAVA-MD\springBoot整合\restful.png)

![restful2](D:\typora\JAVA-MD\springBoot整合\restful2.png)











# 常用注解



## Spring Bean 管理相关



### @Configuration

表示 ： Spring的一个配置类；=== @Component



  ### @Bean

@Bean是一个方法级别上的注解，主要用在@Configuration注解的类里，也可以用在@Component注解的类里。



~~~java
@Configuration
public class AppConfig {
 
    @Bean
    public TransferService transferService() {
        return new TransferServiceImpl();
    }
 
}
~~~

@Bean不仅可以作用在方法上，也可以作用在注解类型上，在运行时提供注册。

**value**： name属性的别名，在不需要其他属性时使用，也就是说value 就是默认值

**name**： 此bean 的名称，或多个名称，主要的bean的名称加别名。如果未指定，则bean的名称是带注解方法的名称。如果指定了，方法的名称就会忽略，如果没有其他属性声明的话，bean的名称和别名可能通过value属性配置

**autowire** ： 此注解的方法表示自动装配的类型，返回一个`Autowire`类型的枚举



~~~java

~~~











### @Scope

声明 Spring Bean 的作用域，使用方法:

```text
@Bean
@Scope("singleton")
public Person personSingleton() {
    return new Person();
}
```



**四种常见的 Spring Bean 的作用域：**

- singleton : 唯一 bean 实例，Spring 中的 bean 默认都是单例的。
- prototype : 每次请求都会创建一个新的 bean 实例。
- request : 每一次 HTTP 请求都会产生一个新的 bean，该 bean 仅在当前 HTTP request 内有效。
- session : 每一次 HTTP 请求都会产生一个新的 bean，该 bean 仅在当前 HTTP session 内有效。





### @Component

只需要在对应的类上加上一个**@Component**注解，就将该类定义为一个Bean了，



把普通pojo实例化到spring容器中

**`在自动扫描此类的时候，类名转化为spring bean，即相当<bean id="" class="" />中的id。id由类名而来，id遵从：`不指定bean的名称，默认为类名首字母小写其它不变，之后赋给id**

一旦使用关于Spring的注解出现在类里，例如我在实现类中用到了@Autowired注解，被注解的这个类是从Spring容器中取出来的，那调用的实现类也需要被Spring容器管理，加上@Component



@Component 可以细分 ：

- **@controller** 控制器（注入服务）
  用于标注控制层，相当于struts中的action层

- **@service** 服务（注入dao）
  用于标注服务层，主要用来进行业务的逻辑处理

- **@repository**（实现dao访问）
  用于标注数据访问层，也可以说用于标注数据访问组件，即DAO组件











### @Autowired

自动导入 对象类中，被注入的类同样要被Spring容器管理

比如 ： 

~~~java
public class Cat {
  ......
}

@RestController
@RequestMapping("/cats")
public class CatController {
   @Autowired
   private Cat cat;
   ......
}
~~~

源码 ：

~~~java
@Target({ElementType.CONSTRUCTOR, 
         ElementType.METHOD, 
         ElementType.PARAMETER, 
         ElementType.FIELD, 
         ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Autowired {
    boolean required() default true;
}
~~~

从源码中，我们可以看到以下几点：

1. 该注解可以用在 **构造函数、方法、方法内的参数、类属性和注解上**；
2. 该注解在程序运行时有效；
3. 有一个属性 required，默认true，表示该装配是否是必须的，若是，则找不到对象的类时抛出异常；







[]()











### @RestController

 

Restful 风格

~~~java
@RestController
public class DemoApplication {
  @GetMapping("/helloworld")
   public String hello() {
   return "Hello World!";
   }
}
~~~











## 传值相关

### @PathVariable

- `@PathVariable`  : 获取路劲变量的值

~~~java
@Getmapping("/{id}")
public User getUser(@PathVariable("id") Long userId){    // 可以获取路径变量id的值
        return userService.selectUserById(userId);
 }
~~~



- `@RequestHeader` ： 获取请请求头

@RequestHeader 带key，获取key的值，不带key,就获取header所有的参数

~~~java
    //@RequestHeader:获取浏览器头信息：限制请求头中必须有User-Agent
    //User-Agent:封装了浏览器的信息
    @RequestMapping("show22")
    public String show22(@RequestHeader("User-Agent")String userAgent,
                        @Requestheader Map<String,String> header){
        System.out.println("hello"+browser);
        return "/index.jsp";
    }
~~~



-  `@RequestParam `: 获取post请求体参数



- `@CookieValue`：获取Cookie

声明Cookie可以获取所有Cookie的参数类型

~~~java
  @RequestMapping("show25")
    public String test25(@CookieValue("JSESSIONID")String jsessionid,
                        @CookieValue("_ga") Cookie cookie
                        ){
        model.addAttribute("msg", "获取cookie,jsessionid:" + jsessionid);
        
        System.out.printIn(cookie.getName() + " => "  + cookie.getValue )
        return "hello2";
    }
~~~



- `@RequestAttribute`:获取request域属性

- `@RequestBody`:获取请求体

- `@MatrixVariable`: 矩阵变量







### @RequestParam

@RequestParam：将请求参数绑定到你控制器的方法参数上,将前端传入的参数复制给定义的参数。



语法 ： @RequestParam(value="参数名"，required=”true/false”,defaultValue=“”)

~~~java
@RequestMapping("show16")
    public ModelAndView test16(@RequestParam("name")String name){
        ModelAndView mv = new ModelAndView();
        mv.setViewName("hello2");
        mv.addObject("msg", "接收普通的请求参数：" + name);
        return mv;
 }
~~~



### @RequestBody

- 获取请求体参数【post】
- 前端只能post提交JSON数据，@Request 最多只能有一个/@Requestparam可以有多个

~~~java
@PostMapping("/user")
public Map postMethod(@RequestBody String content){
    
    Map<string,Object> map = new HashMap<>();
    map.put("content");
    return map
        
}
~~~











## 读取配置信息

###  @value

一般情况可以给属性赋值 ；

~~~java
@Component
public class Cat{
    
    @Value("猫")
    private String name;
    
}
~~~



配合yaml可以读取配置文件。

`@Value("${property}")` 读取比较简单的配置信息：



<img src="D:\typora\JAVA-MD\springBoot整合\yamlValue.png" alt="yamlValue" style="zoom:80%;" />
















### @ConfigurationPropeoties

配合 yaml使用可以读取配置文件。

只有组件是容器中的组件，才能提供@ConfigurationPropeoties功能

将配置文件中的每一个属性的值，映射到这个组件中！

参数``prefix` 将配置文件中的person下面的所有属性 一一对应。

![configurationproerties](D:\typora\JAVA-MD\springBoot整合\configurationproerties.png)











##  Http请求



### Get

- @GetMapping是一个组合注解，是@RequestMapping(method = RequestMethod.GET)

- 只能Get请求

~~~java
//只接受get方式的请求
	@GetMapping("/testGetMapping")
	public String testGetMapping(Model model) {
		model.addAttribute("msg","测试@GetMapping注解");
		return "success";
	}
~~~







### Post

- ```java
  @PostMapping("users")` 等价于`@RequestMapping(value="/users",method=RequestMethod.POST)
  ```

  

~~~java
	//只接受post方式的请求
	@PostMapping("/testPostMapping")
	public String testPostMapping(Model model) {
		model.addAttribute("msg","测试@PostMapping注解");
		return "success";
	}
~~~



### Put

- ```java
  @PutMapping("/users/{userId}")` 等价于`@RequestMapping(value="/users/{userId}",method=RequestMethod.PUT)
  ```

  

~~~java
	//只接受put方式的请求
@PutMapping("/testPutMapping")
~~~

### Delete

- `@DeleteMapping("/users/{userId}")`等价于   `@RequestMapping(value="/users/{userId}",method=RequestMethod.DELETE)`

	//只接受delete方式的请求
	@DeleteMapping("/testDeleteMapping")





## 参数校验@Validated

JSR303 校验

### 常用字段验证注解

~~~bash
@NotEmpty             	 # 被注释的字符串的不能为 null 也不能为空
@NotBlank 				 # 被注释的字符串非 null，并且必须包含一个非空白字符
@Null 					 # 被注释的元素必须为 null
@NotNull 				 # 被注释的元素必须不为 null
@AssertTrue 			 # 被注释的元素必须为 true
@AssertFalse 			 # 被注释的元素必须为 false
@Pattern(regex=,flag=)	 # 被注释的元素必须符合指定的正则表达式
@Email 					 # 被注释的元素必须是 Email 格式。
@Min(value)				 # 被注释的元素必须是一个数字，其值必须大于等于指定的最小值
@Max(value)				 # 被注释的元素必须是一个数字，其值必须小于等于指定的最大值
@DecimalMin(value)		 # 被注释的元素必须是一个数字，其值必须大于等于指定的最小值
@DecimalMax(value) 		 # 被注释的元素必须是一个数字，其值必须小于等于指定的最大值
@Size(max=, min=)		 # 被注释的元素的大小必须在指定的范围内
@Digits (integer, fraction) # 被注释的元素必须是一个数字，其值必须在可接受的范围内
@Past					 # 被注释的元素必须是一个过去的日期
@Future 				 # 被注释的元素必须是一个将来的日期
    
    .... ....
~~~







### 验证请求体

~~~java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Person {

    @NotNull(message = "classId 不能为空")
    private String classId;

    @Size(max = 33)
    @NotNull(message = "name 不能为空")
    private String name;

    @Pattern(regexp = "((^Man$|^Woman$|^UGM$))", message = "sex 值不在可选范围")
    @NotNull(message = "sex 不能为空")
    private String sex;

    @Email(message = "email 格式不正确")
    @NotNull(message = "email 不能为空")
    private String email;

}
~~~





### 验证请求参数

**一定不要忘记在类上加上** **`Validated`** **注解了，这个参数可以告诉 Spring 去校验方法参数。**

~~~java
@RestController
@RequestMapping("/api")
@Validated
public class PersonController {

    @GetMapping("/person/{id}")
    public ResponseEntity<Integer> getPersonByID(@Valid @PathVariable("id") @Max(value = 5,message = "超过 id 的范围了") Integer id) {
        return ResponseEntity.ok().body(id);
    }
}
~~~



































