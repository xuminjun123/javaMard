# Springboot



## 1. 配置文件映射到属性和实体类

1. 方式1

- Controller 层上面加上配置 

~~~java 
@PropertySource({"classpath:application.properties"})   //只能在controller 上使用
~~~

- 增加属性

~~~java
@Value("${test.name}")
private String name;
~~~

2. 方式2

- 添加 @Component 注解

- 使用@PropertySource

- 使用@ConfigurationProperties 注解 设置相关属性

- 必须通过 注入 IOC 对象Resource 进来 ，才能在类中使用获取的配置文件值。

  @Autowired

  `private ServerSettings serverSettings`

例如

~~~java
@Configuration
@ConfigurationProperties(prefix="test")
@PropertySource(value="classpath:resource.properties")
public class  test {
    ....
}

~~~



## 2.SpringBootTest 单元测试

1. 引入相关依赖

springboot 程序测试依赖，如果是自动创建项目默认添加

~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <version>test</version>
</dependency>
~~~



2. 使用

~~~java
@RunWith(SpringRunner.class)        // 底层用的juint
@SpringBootTest(classes={main函数的入口})       // 需要指定应用
public class test{
    ....
}
~~~



## 3. mockMVC 

https://blog.csdn.net/wo541075754/article/details/88983708

1. @AutoConfigureMockMvc

   @SpringBootTest(class={main函数的入口})

2. 相关API 

   ​	perform ： 执行一个RequestBuilder 请求

   ​	andExpect : 添加ResultMatcher ---》 MockMvcResultMatchers 验证规则

   ​	andReturn : 最后返回相应的 MvcResult



##  4. banner设置

springboot 个性化 banner设置

- application.properties  添加 spring.banner.location=banner.txt

- banner.txt 自定义banner

.......（不重要）:wolf:



## 5. json框架 和 jackson

简介 ：介绍常用的json 框架和注解

1、常用框架 阿里fastjson，谷歌gson等



2、 jackson 处理相关实体类 （加 注解有不同效果 ）

- 指定字段不返回 ：@ JsonIgnore
- 指定日期格式 ：@JsonFormat(pattern = "yyyy-MM-dd hh:mm:ss",locale="zh",  timezone = "GMT+08:00")

- 空子段不返回 ： @JsonInclude(Include.NON_NULL)
- 指定别名： @JsonProperty





## 6. tomcat 方式文件上传(一) 





单个 文件：  `MultipartFile file`

多個文件 ：  `MultipartFile[] files`

上传文件 ： `file.transferTo(tempFile)`    //transferTo() 用于文件保存 ，效率比																												fileSteam更高

~~~java
private static final string filePath = "/test/pictures/"

@RequestMapping(value = "upload")
@ResponseBody
public String upload(@RequestParam("head_img") MultipartFile file,HttpServletRequest request){
    
    // file.isEmpty(); 判断图片是否为空
    // file.getSize(); 图片大小进行判断
    
    String name = request.getParameter("name")
    System.out.prinIn("用戶名" + name)
    // 获取文件名    
    String fileName =  file.getOriginalFilename();//name 
    System.out.prinIn("上传的文件名" + filename);
    
    // 获取文件的后缀名
    String suffixName = fileName.substring(fileName.lastIndexOf("."));
    System.out.prinIn("上传的文件后缀名" + suffixName);
    
    // 文件上传后的路径
    fileName = ＵＵＩＤ．randomUUID() + suffixName;
    System.out.prinIn("转换后的名称" + fileName);
    
    file tempFile = new File(filePath +fileName); // filePath 是文件上传的路径
      try {
               files.transferTo(tempFile);//生成临时文件
               return fileName;  
        	}catch(Exception e){
               e.printStackTrace();
          }
}
~~~

## 6.2 jar包文件上传(二)

**jar包方式运行web项目的文件上传和访问处理**

1. 文件大小配置，启动类里面配置

~~~java
@Bean
public MultipartConfigElement multipartConfigElement(){
    MultipartConfigFactory factory = new MultipartConfigFactory();
    
    // 单个文件最大
    factory.setMaxFileSize("1024KB"); //KB,MB
    
    // 设置总文件上传数据总大小
    factory.setMaxRequestSize("1024000KB");
    return factory.createMultipartConfig()
}
~~~

2. 打成jar包需要依赖

`spring-boot-maven-plugin`

3. 文件上传和访问需要指定的磁盘路径

   application.properties 中正佳下面配置

   1）web.images-path=/User/images

   2）spring.resources.static-locations = classpath:/META_INF/resources,claseepath:/resources/,classpath:/static/,${web.upload-path}

   























## 7.配置全局异常



1. 异常注解介绍

   @ControllerAdvice 如果是返回 json数据 ，则用 RestControllerAdvice ,就可以不加 @ResponBody

   捕获全局异常，处理所有不可知的异常

   @ExceptionHandler(value=Exception.class)

例如：

~~~java
@RestControllerAdvice     // 这个注解会返还字符串，而不是跳转视图
public class ErrTest {
    // 捕获全局一场，处理所有不可知的异常
    @ExceptionHandler(value = Exception.class)
    Object handleException(Exception e, HttpServletRequest request){
        Map<String,Object> map = new HashMap<>();
        
        map.put("code",100);
        map.put("msg",e.getMessage());
        map.put("url",request.getRequestURL());
        return map;
    }

}

~~~



## 8.自定义异常

1. 配置一个异常类 继承 RuntimeException

~~~java
package com.study;
@Getter
@Setter
public class ErrClass extends RuntimeException {
    private String code;
    private String msg;

   
    public ErrClass(String code,String msg){
        this.code=code;
        this.msg =msg;
    }

}
~~~



2. 使用

~~~java
@RestControllerAdvice
public class GlobalException{
	@ExceptionHandler(value=ErrClass.class)
	public AjaxResult myError(ErrClass e,HttpServletRequest request){
		... ...
    }      
}
~~~





## 9. springboot 启动

### SpringBoot启动方式  

1. jar 包方式启动   、 war 包方式<packaging>war</packaging>(一般不用，如果用需要下载tomcat，修改										启动类)，

~~~xml
// maven 插件，把程序打包成jar 包
<build>
   <plugins>
      <plugin>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-maven-plugin</artifactId>
         <configuration>
            <fork>true</fork> <!-- 如果没有该配置，devtools不会生效 -->
         </configuration>
      </plugin>
   </plugins>
</build>
~~~



- 如果没有加，会报 `java -jar spring-boot-demo-0.01.SNAPSHOT.jar no main mainfest attribute,in spring-boot-demo-0.0.1SNAPSHOT.jar` 

- 如果没有安装maven ，用 `mvn spring-boot:run`





### springboot启动原理



。。。。 这里听不太懂？？？











## SpringBoot过滤器

---- 这里 过滤器和监听器 ，尚且还不懂

1. SpringBoot启动默认加载的Filter
   - characterEncodingFilter
   - hiddenHttpMethodFilter
   - HttpPutFormContentFilter
   - requestContextFilter



2. Filter 优先级

   - Ordered.HIGHST_PRECEDENCE
   - Ordered.LOWESR_PRESCDENCE

   低位值意味更加的优先级

3. 自定义FIlter
   - 使用Servlet3.0 的注解进行配置
   - 启动类里面增加，@ServletComponentScan
   - 新建一个Filter类，implements Filter,并实现对应的接口









## Springboot 持久层数据库

**JPA框架**

- jpa 在复杂查询的时候性能不是很好，开发便捷

**Hiberante**

- 解释ORM框架　，对象关系映射

**mybatis 框架**

- 最常用的框架,半 ORM





### 整合mybatis

- 加入依赖

  ~~~xml
  <!-- SpringBoot集成mybatis框架 -->
  <dependency>
     <groupId>org.mybatis.spring.boot</groupId>
     <artifactId>mybatis-spring-boot-starter</artifactId>
     <version>1.3.2</version>
  </dependency>
  <!-- Mysql JDBC 驱动 -->
  <dependency>
     <groupId>mysql</groupId>
     <artifactId>mysql-connector-java</artifactId>
     <version></version>
  </dependency>
  <!--  durid 第三方数据源 -->
  <dependency>
     <groupId>com.alibab</groupId>
     <artifactId>druid</artifactId>
     <version>1.1.6</version>
  </dependency>
  ~~~

- 加入配置文件

~~~properties
spring.datasource.url=
spring.datasource.username=
spring.datasource.password=
# 如果不是有默认的数据源 ( com.zaxxer.hikari.hikariDataSource )
spring.datasource.type = com.alibaba.druid.pool.DruidDataSource

# 开启控制台打印SQL
mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
~~~

加入配置 ，注入到sqlSessionFaction 等都是SpringBoot帮我们完成



- 启动类增加mapper 扫描

  @MapperScan("xxx.xxx.mapper")

  用于注入的数据源

  ~~~java
  @Bean
  @ConfigurationProperties(prefix = "spring.data.source")
  public DataSource druidDataSource(){
      
  }
  ~~~

  

- 开发mapper ---> @Service --- >@Controller 

推荐使用# 而不是$ ,因为mybatis会先预编译，存在sql问题

~~~java
@Insert("INSERT INTO user(name,pwd) VALUES(#{name},{#pws})")
@Options(userGeneratedKeys=true,keyProperty="id",keyColumn="id") // keyProperty java对象主键
int insert(User user)
~~~

Userservice --> UserServiceImpl

~~~java
@Service
public class UserServiceImpl implements Userservice{
	@Autowired
	private Usermapper userMapper;
	
	@Override
	public int add(User user){
		userMapper.insert(user);
		int id = user.getId();
		return id;
	}
}
~~~

@Controller

~~~java
@RestController
@RequestMapping("/api")
public class userController{
    @Autowired
    private UserService userService;
    
    @GettMapping("add")
    public Object add(String msg){
        User user = new user();
        user.setName("xx");
        user.setPwd(12);
        
        int id= userService.add(user);
        return JsonData.buildSuccess(id)
    }
}
~~~







### mybatis事务

- 在UserServiceImpl，开始事务，会好性能（有很多锁）

  ~~~java
  @Override
  @Transactional(propagetion=Propagetion.REQUIRED)     // 默认隔离级别
  public int add(User user){
      userMapper.insert(user);
      int id = user.getId();
      return id;
  }
  ~~~





## 整合 Redis

### 简单使用



- 导包  `spring-boot-starter-data-redis`

- application.properties配置

  ~~~yaml
  spring.redis.host=127.0.0.1
  spring.redis.port=6379
  ... ...
  # redis 线程池配置
  spring.redis.pool.max-idle=200   # 最大线程池
  spring.redis.pool.min-idle=200   # 最小线程池
  # 如果赋值为-1, 则表示不限制 ，pool 已经分配了maxActive个jedis实例，则比pool的状态为 exhausted(耗尽)
  spring.redis.pool.max-active=2000 
  
  # 等待可用链接的最大时间，单位毫秒，默认值-1 ，表示永不超时
  spring.redis.pool.max-wait=1000
  ~~~

- 测试

  ~~~java
  // Controller层
  
  @Autowired 
  private RedisTemplate redisTemplate;
  
  redisTemplate.ops....
  ~~~

  ~~~java
  // 单元测试
  @Autowired
  private redisClient redis; 
  ~~~

  





### redis 可视化

可视化工具 下载地址 ：https://redisdesktop.com/download

 免费 windows下载地址 ： https://blog.csdn.net/lizhiyuan_eagle/article/details/88742993





## 定时任务



### 同步定时任务



1. 常见定时任务 Java自带的java.util.Timer类（配置麻烦，时间延后问题）
2. Quartz框架（配制简单，xml或者注解）
3. springboot 使用注解方式
   - @EnableScheduling 开启定时任务，自动扫描
   - @Component 被容器扫描



使用

- 1. 在main上加@EnableScheduling 

- 2. 定时业务类 加@Scheduled(fixedRate=2000)

  ~~~@java
  @Component
  public class Test{
  	@Scheduled(fixedRate=2000)     // 每过2秒执行下面方法
  	public void sum(){
  		System.out.printIn("当前时间" + new Date()
  	}
  }
  ~~~









### 异步任务



异步任务 和使用场景 ： log 、发送邮件 、短信 .... 等

**使用**

1. 启动类使用 @EnableAsync 注解功能   (开启异步任务注解)
2. 在类上 @Async 表示 下面方法异步执行

注意点 ：

- 1. 要把异步任务类并使用 @Component 标记组件被容器扫描，不能直接写到Controller
  2. 增加Future<String> 返回结果 AsyncResult<String>("执行完成"）
  3. 如果需要拿到结果，需要判断全部的 task.isDone()
- 通过注入方式，注入到Controller 里面，如果测试前后别改为同步则把Async注释掉



### 常用定时任务配置实战

1. 常用定时任务表达式配置和在线生成器
   - cron 定时任务表达式 `@Scheduled(cron="*/1*****")` 表示每秒
   - fixedRate : 多久执行一次
   - fixedDelay : 上一次执行结束时间后  xx 秒后再次执行
   - fixedDelayString : 字符串形式，可以通过配置文件指定









## LogBack 

新一代日志框架，在 log4j 基础上大量改良，不能单独使用，推荐配合日志框架SLF4J 来使用







## elasticseacher 搜索框架

（   我暂时用不上，不想搞这个 。以后有时间来  ）



elasticseacher 主要特点 ：

1. 全文检索，结构化检索，分析，接近实时处理，。分布式搜索（可部署百台服务器），处理PB 级别的数据搜索纠错，自动完成

使用场景 ： 日志搜索，数据聚合，数据监控，报表统计分析









## 消息队列JMS介绍



讲解JMS 的基础知识

1. 什么时JMS : Java 消息服务(Java message Service), Java 平台中关于面向消息中间件的接口
2. JMS 是一种与厂商无关API，用来访问消息收发系统消息,他类似于 JDBC (  Java Database connectivity) 。这里， JDBC　是可以用来访问许多不同关系数据库，API



使用场景 :

- 跨平台
- 多语言
- 多项目
- 解耦
- 分布式事务
- 流量控制
- 最终一致性
- RPC调用
  - 上下游对接,数据源变动 -> 通知下属





概念 :

- JMS提供者 : Apache ActiveMQ  、RabbitMQ 、Kafka 、Notify 、MetaQ 、 RocketMQ

- JMS 生产者
- JMS  消费者
- JMS 队列
- JMS 主题





## ActiveMQ5.x 消息队列



特点 :

- 支持多语言Java,C,C++PHP ...
- 支持需要高级功能,如消息组,虚拟目标,通配符和符合目标
- 完全支持JMS　1.1和J2EE 1.4支持瞬态，持久，事务和ＸＡ消息
- spring 支持，ActiveMQ 可以轻松嵌入到Spring 应用程序中 ，并使用Spring和 XML 配置机制进行配置。
- 支持流行的Ｊ２ＥＥ服务器
- 使用JDBC 和高性能日志，支持非常快速的持久化



下载地址 ： http://activemq.apache.org/activemq-5153-release.html

快速开始 ： http://activemq.apache.org/getting-started.html

- 64位 机器则 打开win64目录下的 activemq.bat

- bin目录里面启动，选择对应的系统版本和位数，activeMQ start 启动
- 启动后访问 路径 http://127.0.0.1:8681/

- 用户名和密码 默认admin

- 官方案例集合 https://github.com/spring-projects/spring-boot/tree/master/spring-boot-samples



![ActiveMQ](D:\typora\JAVA-MD\springBoot整合\ActiveMQ.png)

面板 ：

- Name : 队列名称
- Number Of Pending Messages :等待消息的个数
- Number Of Consumers : 当前链接消费着数目
- Messages Enqueued : 进入队列的消息总个数 ，包括出对联和代消费的，这个数量只增不减。
- Message Dequeued： 已经消费的消息数量





### springboot整合 ActiveMQ

- POM依赖

  整合消息队列  activemq 和 线程池

  ~~~xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-activemq</artifactId>
      <version></version>
  </dependency>
      
  <dependency>
      <groupId>org.apache.activemq</groupId>
      <artifactId>activemq-pool</artifactId>
      <version></version>
  </dependency>
  ~~~

- application.properties 配置文件配置

~~~properties
# 整合jms 测试，安装在别的机器，防火墙和端口号要记得开放
Spring.activemq.boke-url = tcp://127.0.0.1:61616 

# 集群配置
# spring.activemq.broker-url=failover:(tcp://localhost:61616,tcp://localhost:61617)

spring.activemq.user=admin
spring.active.password=admin

# 下列配置要增加依赖
spring.activemq.pool.enabled=true
spring.activemq.pool.max-connections=100

~~~

- springboot 启动类 @EnableJms ,开启支持jms



实例：

在启动类 中 添加：

~~~java
@SpringbootApplication
@EnableJms  // 开启支持jms
public class application{
    @Bean  
    public Queue queue(){
        return new ActiveMQQueue("common.queue");
    }
    
    public static void main(String[] args){
       springApplication.run(application.class,args)
    }
    
}
~~~

具体业务类

如图

<img src="D:\typora\JAVA-MD\springBoot整合\ActiveMQ案例.png" alt="ActiveMQ案例" style="zoom: 80%;" />

 

controller层

<img src="D:\typora\JAVA-MD\springBoot整合\activemq案例controller.png" alt="activemq案例controller" style="zoom: 67%;" />



还没完，以后再弄









## SpringBoot 多环境配置



1. 不同的环境使用不同配置

   例如数据库配置 ，在开发的时候，我们一般用开发数据库，而在生产环境的时候，使用正式的数据

2. 配置文件存放路径

   classpath根目录的“/config” 包下

   classpath的根目录下

3. springboot 允许通过命名约定按照一定的格式（application-{profile}.properties）来定义多个配置文件



案例：

application.porperties （指定那个环境） 、application-dev.porperties ( 开发环境 )  、application-test.porperties  ( 测试环境 )、



application.properties 读取不同的test.url就能指定那个环境

~~~properties
# SpringBoot多环境配置

# 默认local本地环境
test.url=local 
# 指定test环境，改动这里可以改变环境
spring.profiles.active=test 
~~~



application-test.porperties(测试环境 )

~~~properties
test.url=test.com
~~~

application-dev.porperties ( 开发环境 ) 

~~~properties
test.url=dev.com
~~~



controller层

~~~java
@RestController
@RequestMapping("/api")
public class OrderController{
    @Value("${test.url}")      // 这里使用@Value()
    private String dimain;
    
    ...
}
~~~





## SpringBoot响应式编程



1. 基础理解

   - 依赖时间驱动（Event-driven）

   - 一系列事件称为流

   - 异步
   - 非阻塞
   - 观察者模式

2. 网上的一个例子

   int a = b+c       //命令式编程后续b 和 c 变化 ，都不影响 a

   int a= b+c        // 响应式编程，a 的变化，会和 b、c 的变化而变化（时间驱动）

3. SpringBoot2 底层是用 Spring5 ，开始支持响应式编程，Spring又是基于Reactor试下响应式。





### webflux 响应式

一般是用来做 推送 使用的。

- 数据发送给前端是一条一条发送的，不是一下子是json格式所有数据。



1. webFlux中，请求和响应不再是webMVC 中的ServletRequest 和 ServletResponse, 而是 ServerRqquest 和ServerResponse

2. 加入依赖，如果同时依赖spring-boot-starter-web, 则会优先用Spring-boot-starter-web

   ~~~xml
   <!-- SpringBoot集成mybatis框架 -->
   <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-webflux</artifactId>
   </dependency>
   ~~~

3. 启动方式是 Netty ,默认8080

4. 参考官网





## Springboot 主动推送

使用场景 ： 如聊天机器人，sse轮询，股票，行情之类

1. 第一种  ：ajax定时拉取

2. 第一种  ： 服务端主动推送：WebSocket

   全双工的 ：本质上是一个额外的tcp链接，建立和关闭时握手使用http 协议，其他数据传输不使用http协议更加复杂一些，使用与需要进行复杂数据通讯的场景

3. 第三种  :  SSE 

   - html5 新标准 ： 用力爱从服务端实时推送数据导浏览器端

   - 直接建立在当前http 连接上 ，本质上是保持一个http长链接，轻量协议

   - 简直的服务器数据推送的场景，使用服务器推送

     

## 部署到服务器

步骤：

1. 去除相关生产环境没用的jar(如热部署dev-tool)

2. 本地maven 打包为jar

3. 服务器安装jdk ，上传jar ，上传工具 winscp（windows系统）  、securtyCRT （windows系统）、 filezilla（mas 系统）  

    在jar包目录下 ，启动 java -jar xxxxx..xx.jar

4. 访问路径。。。

   防火墙和开放端口









































































































































































































 











































































