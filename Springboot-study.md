# Springboot



## 配置文件映射到属性和实体类

1. 方式1

- Controller 层上面加上配置 

~~~java 
@PropertySource({"classpath:application.properties"})   //只能在controller 上使用
~~~

- 增加属性

~~~java
@Value("{test.name}")
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



## SpringBootTest 单元测试

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



## mockMVC 

https://blog.csdn.net/wo541075754/article/details/88983708

1. @AutoConfigureMockMvc

   @SpringBootTest(class={main函数的入口})

2. 相关API 

   ​	perform ： 执行一个RequestBuilder 请求

   ​	andExpect : 添加ResultMatcher ---》 MockMvcResultMatchers 验证规则

   ​	andReturn : 最后返回相应的 MvcResult



##  banner设置

springboot 个性化 banner设置

- application.properties  添加 spring.banner.location=banner.txt

- banner.txt 自定义banner

.......（不重要）:wolf:



## 配置全局异常



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



## 自定义异常

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





## springboot 启动

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

~~~java
@Autowired
private JmsMessageImplate jmsTemplate
~~~

如图



















































































































































 











































































