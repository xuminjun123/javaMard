# Spring Cloud（上）

## 概念定义

- Spring Cloud 是一个服务治理平台，提供了一些服务框架。

  包含了：

  -  服务注册与发现
  - 配置中心
  - 消息中心
  - 负载均衡
  - 数据监控

- Spring cloud 是一个微服务框架，相比Dubbo等RPC 框架，Spring Cloud提供全套的分布式系统解决方案

- Spring cloud 对微服务基础框架 Netfilx 的多个开源组件进行封装，同时有实现了和云端平台以及Spring Boot框架的集成。

- Spring Cloud 是一个基于Spring Boot 实现的云应用开发工具，它为开发中的配置管理 ，服务发现 ， 断路器 ，智能路由 ， 微代理， 控制总线 ，全局锁 ， 决策竞选 ，分布式会话和集群状态管理操作提供了一种简单的开发方式。





## 子项目



###  Netfilx 第一代



> Netfilx 是一家美国公司，在美国，加拿大提供互联网随选流媒体播放 ，定制DVD，蓝光光碟在线出租 业务。



针对 多种Netfilx  组件提供的开发工具包，其中包括Eureka , Hystrix ，Ribbon， zuul , Archaius等。

- `Netfilx Eureka`: 一个基于 Rest 服务的服务治理组件 ， 包括服务注册中心 ，服务注册与服务发现机制的实现，实现了 云端负载均衡和中间层服务的故障转移。
- `Netfilx Hystrix`: 容错管理工具 ，实现断路器模式 ，通过控制服务的节点 ，从而对延迟和故障提供更加强大的容错能力。
- `Newfilx Ribbon`:客户端负载均衡的服务调用组件
- `Netfilx Feign`: 基于Ribbon 和 Hystrix 的生命是服务调用组件
- `Netfilx Zuul`: 微服务网关 ， 提供动态路由 ， 访问过滤等服务
- `Netfilx Archaius`: 配置管理API ， 包含 一系列配置管理 API ，提供动态类型化属性 , 线程安全配置操作 ， 轮询框架 ，回调机制等功能。



### Alibaba 第二代

​      SpringCloud Alibaba 致力于提供为服务开发的一站式解决方案 ， 此项目包含开发分布式应用微服务的必须组件，方便开发者通过 SpringCloud 编程模型轻松使用这些组件来开发分布式应用服务。

​		依托 Spring Cloud Alibaba ,只需要添加一些注解和少量配置，就可以将Spring Cloud 应用接入阿里微服务解决方案 ，通过阿里中间件来迅速搭建分布式应用系统。

- `Nacos`: 阿里巴巴开源产品 ， 一个更易于郭建云原生应用的动态服务发现 ， 配置管理和服务管理平台
- `Sentinel`：面向分布式服务架构的轻量级流浪控制产品，把流量作为切入点，从流量控制 ， 熔断降级 ，系统负载保护等多个维度保护服务的稳定性。
- `RocketMQ`: 一个开源的分布式消息系统 ，基于高可用分布式集群技术 ，提供低延时 ，高可靠的消息发布与订阅服务 。
- `Dubbo`: Apache Dubbo 是一款高性能Java RPC 框架。
- `Seata`: 阿里巴巴开源产品 ， 一个易于使用的高性能微服务分布式事务解决方案。
- `Alibaba Cloud ACM`: 一款分布式架构环境中对应用配置进行集中管理和推送的应用配置中心产品。
- `Alibaba Cloud OSS`：阿里云对象存储服务 ，是阿里云提供的海量 ，安全， 低成本 ，高可靠的云存储服务，可以在任何应用 ，任何时间 ，任何地点存储和访问任意类型的数据。
- `Alibaba Cloud SchulerX`: 阿里中间件团队开发的一款分布式任务调度产品 ， 提供秒级 ，精准 ，高可靠， 高可用的定时 （基于Cron表达式）任务调度服务。
- `Alibaba CLoud SMS`: 覆盖全球的短信服务 ，友好 ，高效，智能的互联化通话能力，帮助企业迅速搭建客户融达通道。


|               | SpringCloud Netfilx |    SpringCloud 官方    | SpringCloud Zookeeper | SpringCloud Consul | SpringCloud Kubernetes | `SpringCloud Alibaba` |
| :-----------: | :-----------------: | :--------------------: | :-------------------: | :----------------: | :--------------------: | :-------------------: |
|  分布式配置   |      Archaius       |  Spring Cloud Config   |       Zookeeper       |       Consul       |       ConfigMap        |        `Nacos`        |
| 服务注册/发现 |       Eureka        |           -            |       Zookeeper       |       Consu        |       Api Server       |        `Nacos`        |
|   服务熔断    |       Hystrix       |           -            |           -           |         -          |           -            |      `Sentinel`       |
|   服务调用    |        Feign        | OpenFeign RestTemplate |           -           |         -          |           -            |      `Dobbo RPC`      |
|   服务网关    |        Zuul         |  SpringCloud Gateway   |           -           |         -          |           -            |     `Dubbo PROXY`     |
|  分布式消息   |          -          |      SCS RabbitMQ      |           -           |         -          |           -            |     `SCS RoketMQ`     |
|   负载均衡    |       Ribbon        |           -            |           -           |         -          |           -            |      `Dubbo LB`       |
|  分布式事务   |          -          |           -            |           -           |         -          |           -            |        `Seata`        |





### 常用组件

- `Spring Cloud Netfilx Eureka`: 服务注册中心

- `Spring Cloud NetFilx Ribbon`: 客户端负载均衡

- `Spring Cloud Netfilx Hystrix`:服务容错保护

- `Spring Cloud Netfilx Feign `: 声明式服务调用

- `Spring Cloud openFeign(可替代 Feign)`: OpenFeign 是 Spring Cloud 在Feign 的基础上支持了SpringMVC的注解，如@Request Mapping 等 。OpenFengn的@FeignClient 可以解析SpringMVC的＠RequestMapping 注解下的接口，并通过动态代理的方式产生实现类，实现类中做负载均衡并调用其他服务。

- `Spring Cloud Netfix Zuul`:ApI 网关服务 ，过滤，安全，监控 ，限流 ，路由

- `Spring Cloud Gateway(可替代zuul)`：Spring Cloud Gateway 是Spring 官方基于Spring5.0 ， Spring Boot 2.0 和Project Reactor 等技术开发的网关 ，Spring Cloud Gateway 皆在微服务架构提供一种简单而有效的统一的API 路由管理方式 。 Spring Cloud Gateway 作为SpringCloud 生态中的网关 ，目标是替代 NetFlix Zuul ,其不仅提供统一的路由方式 ， 并且基于Filter 链的方式提供了网关基本的功能 ，例如 ： 安全 ，监控/埋点 ，和限流等。

- `Spring Cloud Config`: 分布式配置中心 。配置管理工具 ，支持使用Git存储内容 ，支持配置的外部化存储 ，支持客户端配置信息刷新 ， 加解密配置内容等。

- `Spring Cloud Config Bus`:事件 ， 消息总线 ，用于在集群（例如 ，配置变化事件）中传播状态变化 ，可与Spring Cloud Config 联合实现热部署 。
- `Spring Cloud Stream`: 消息驱动微服务
- `Spring Cloud Sleuth`:分布式服务跟踪
- `Spring Cloud Alibaba`: 阿里巴巴结合自身微服务实践，开源的微服务全家桶 。在Spring Cloud 项目中孵化，很有可能称为 Spring Cloud 第二代标准实现



### 总结

|                | Spring Cloud 第一代          | Spring Cloud 第二代                            |
| -------------- | ---------------------------- | ---------------------------------------------- |
| 网关           | Spring Cloud Zuul            | Spring Cloud Gateway                           |
| 注册中心       | Eureka ， Consul , Zookeeper | 阿里Nacos ,拍拍贷Radar等可选                   |
| 配置中心       | Spring Cloud Config          | 阿里Nacos , 携程Apollo ,随行付Config Keeper    |
| 客户端负载均衡 | Ribbon                       | spring-cloud-commons的SpringCloud LoadBalancer |
| 熔断器         | Hystrix                      | spring-cloud-r4j(Resilence4J) , 阿里Sentinel   |



## 版本定义规则

Spring Cloud 采用伦敦的地铁站名称来作为版本号的命名 ，根据首字母排序， 字母顺序靠后的版本号越大。

Spring 官方详细的版本查看接口 ： https://start.spring.io/actuator/info



### 发布计划

| 版本号    | 版本       | 说明                                                         |
| --------- | ---------- | ------------------------------------------------------------ |
| BUILD-XXX | 开发版     | 开发团队内部使用                                             |
| M         | 里程碑版   | MileStone , M1 表示第1个里程碑版本，一般同时标注PRE，表示预览版 |
| RC        | 候选发布版 | Release Candidate ,正式发布版的前一个观察期，不添加新功能，主要注重于除错 |
| SR        | 正式发布版 | Service Release ,SR1 表示第一个正式版本，一般同时标注GA ， 表示稳定版 |
| GA        | 稳定版     | 经过全面测试并可对外发行称之为 GA（General Availability）    |



## Eureka 服务注册中心



​			服务注册中心 是 服务实现服务化管理的核心组件，类似于目录服务的作用 ，主要用来**存储服务信息** ，譬如 提供着Url 串， 路由信息等 。 服务注册中心是微服务架构中最基础的设施之一。



![学习目标](D:\typora\JAVA-MD\srpingCloudImages\学习目标.png)





### 1. 什么是注册中心

注册中心 可以说是 微服务架构中的 **“ 通讯录 ”**，  它记录了服务和服务地址的映射关系 。 

在分布式架构中服务注册到这里 ，当服务需要调用其他服务时 ，就到这里找到服务的地址 ，进行调用 。



举例 ： 

当我想给张三打电话时， 那我需要在通许录中按照名字找到张三 ，然后可以拨打他的电话。   ---   **==服务发现==**

李四办了手机号 ，并把手机号告诉我，我办手机号的号码存进通讯录 。 ---  ==服务注册==

通讯录 --- ==注册中心==

**总结**： 服务注册中心的作用就是服务的注册和服务的发





### 2. 常见的注册中心

常见的注册中心 ： 

- Netflix Eureka
- Alilbaba Nacos
- hashiCorp Consul
- Apache Zookeeper
- CoreOS Etcd
- CNCF CoreDNS





![常见的注册中心](D:\typora\JAVA-MD\srpingCloudImages\常见的注册中心.png)





### 3. 为什么需要注册中心

​		在分布式系统中 ，我们不仅仅是需要在注册中心找到服务和服务地址的映射关系这么简单 ，我们还需要考虑更多更复杂的问题：

- 服务注册后，如何被及时发现
- 服务宕机后，如何及时下线
- 服务如何有效的水平扩展
- 服务发现时 ，如何进行路由
- 服务异常时，如何进行降级
- 注册中心如何实现自身的高可用



​			这些问题的解决都依赖着**注册中心**，简单看， 注册中心的功能有点类似于ＤＮＳ服务器或者负载均衡器　，而实际上，　**注册中心**作为微服务的基础组件　，可能更加复杂，也需要更多的灵活性和时效性。所以我们还需要学习更多Spring Cloud 微服务组件协同完成应用开发 。

 





### 4. 注册中心解决了什么问题

 







　　

### 5. 什么是Eureka 注册中心

​    Eureka  是Netflix 开发的服务发现组件 ，本身是一耳光基于REST 的服务 ，Spring Cloud 将它集成 在子项目SpringCloud Netflix 中，实现Spring Cloud 的服务注册与发现 ，同时提供了负载均衡，故障转移等能力 。





### 6. Eureka 注册中心的三种角色



<img src="D:\typora\JAVA-MD\srpingCloudImages\三种角色.png" alt="三种角色" style="zoom:50%;" />

- **Eureka Server** ： 通过Register , Get , Renew 等接口提供服务的注册和发现
- **Application Service(Service Provider)**:服务提供方，把自身的服务实例注册到Eureka Server中。
- **Application Client (Service Consumer )**: 服务调用方 ，通过Eureka Server 获取服务列表 ，消费服务

具体如图所示： 

<img src="D:\typora\JAVA-MD\srpingCloudImages\注册中心.png" alt="注册中心" style="zoom:50%;" />

**步骤：**

- 0:  实例化服务

- 1：将服务注册到注册中心

- 2：注册中心收录服务 

     user-service               127.0.0.1:9091

     goods-service            127.0.0.1:9092

     sso-service                 127.0.0.1:9093

     order-service             127.0.0.1:9094

​          ... ....

- 3:   从注册中心获取服务列表
- 4：基于负载均衡算法从地址列表选择一个服务地址进行调用
- 5：定期发送心跳





### 7. Eureka 入门案例



- 新建一个普通的Maven 项目 （Eureka-demo）
- pom 添加依赖

~~~xml
    <!--集成 Spring-boot-start-parent 依赖-->
    <!--继承方式，实现复用 -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.4.RELEASE</version>
    </parent>

    <!--
        集中定义依赖组件版本号 ，但不引入
        在子工程中用到声明的依赖时，可以不加依赖的版本号
        这样可以统一管理工程中用到的依赖版本
     -->
    <properties>
        <!--  spring-cloud Hoxton.SR1 依赖-->
        <spring-cloud.version>Hoxton.SR1</spring-cloud.version>
    </properties>

    <!--项目依赖管理 父项目只是声明依赖 ，子项目需要写明需要的依赖关系(可以省略版本信息)-->
    <dependencyManagement>
        <dependencies>
            <!--spring cloud依赖 -->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>


    <build>
        <finalName>eureka</finalName>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
~~~

- 新建一个子项目 eureka-server，勾选 quickstart ，pom配置注册中心的环境

~~~xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <artifactId>eureka-server</artifactId>
    <groupId>org.example</groupId>
    <version>1.0-SNAPSHOT</version>

    <!--继承父依赖 -->
    <parent>
        <groupId>org.example</groupId>
        <artifactId>eureka-demo</artifactId>
        <version>1.0-SNAPSHOT</version>
    </parent>

    <!--项目依赖-->
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
            <version>3.0.1</version>
        </dependency>

        <!-- spring Boot web 依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.4.2</version>
        </dependency>

        <!--spring Boot test 依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <version>2.4.2</version>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>juint-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>
</project>
~~~

- 新建一个启动类EurekaServerApplication(org.example下)

~~~java
package org.example;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;


@EnableEurekaServer  
@SpringBootApplication
public class EurekaServerApplication {

    public static void main(String[] args) {
        SpringApplication.run(EurekaServerApplication.class);
    }

}
~~~

- 新建application.yml

~~~yaml
server:
  port: 8761 # 端口

Spring:
  application:
    name: eureka-server # 应用名称

# 配置Eureka Server 注册中心
eureka:
  instance:
    hostname: localhost                 # 主机名 ，不配置时候默认根据操作系统的主机名来获取
  client:
    register-with-eureka: false         # 是否将自己注册到注册中心 ，默认为true(集群环境)
    fetch-registry: false               # 是否从注册中心获取服务注册中心 ，默认为true(集群环境)
    service-url:                        # 注册中心对外暴露的注册地址
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
~~~

​		此时如果直接启动项目是会报错的，错误信息 ：`com.sun.jersey.api.client.ClientHandlerException: java.net.ConnectException: Connection refused: connect`

​		这是因为Eureka 默认开启了将自己注册至 **注册中心**和**注册中心获取服务注册**的配置 ，如果该应用的角色是祖册中心并且是单节点的话，要关闭这两个配置项。

- 启动 -- 成功如图 浏览器访问 ：  [localhost:8761]()



<img src="D:\typora\JAVA-MD\srpingCloudImages\Eureka启动成功.png" alt="Eureka启动成功" style="zoom:50%;" />





### 8. 高可用Eureka 注册中心



#### 8.1 注册中心eureka-server

​		在刚才的父工程下再创建 一个`eureka-server02`注册中心的项目 ，如果是多机器部署不用修改端口，通过IP区分服务，如果在一台机器上演示需要修改端口区分服务。

- 新建子项目`eureka-server02`  的普通maven，勾选quickstart 
- 在`eureka-server02`中pom配置

~~~xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <artifactId>eureka-server02</artifactId>
    <groupId>org.example</groupId>
    <version>1.0-SNAPSHOT</version>

    <!--继承父依赖 -->
    <parent>
        <groupId>org.example</groupId>
        <artifactId>eureka-demo</artifactId>
        <version>1.0-SNAPSHOT</version>
    </parent>

    <!--项目依赖-->
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
            <version>3.0.1</version>
        </dependency>

        <!-- spring Boot web 依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.4.2</version>
        </dependency>

        <!--spring Boot test 依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <version>2.4.2</version>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>juint-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>
</project>
~~~

- 新建application.yml 

~~~yaml
server:
  port: 8002 # 端口

Spring:
  application:
    name: eureka-server # 应用名称

# 配置Eureka Server 注册中心
eureka:
  instance:
    hostname: eureka02                 # 主机名 ，不配置时候默认根据操作系统的主机名来获取
  client:
    service-url:                        # 注册中心对外暴露的注册地址
      defaultZone:  http://localhost:8003/eureka/ # 指向8003端口
~~~

- 继续建立 ``eureka-server03` 操作如同 上述

- `eureka-server03` 中的application.yaml修改

~~~yaml
server:
  port: 8003 # 端口

Spring:
  application:
    name: eureka-server # 应用名称

# 配置Eureka Server 注册中心
eureka:
  instance:
    hostname: localhost                 # 主机名 ，不配置时候默认根据操作系统的主机名来获取
  client:
    service-url:                        # 注册中心对外暴露的注册地址  
      defaultZone: http://localhost:8002/eureka/ # 指向8002端口

~~~

使其互相指定

成功如图：

浏览器 访问 ：[localhost:8002 ]()    指向了 8003端口

![8002](D:\typora\JAVA-MD\srpingCloudImages\8002.png)

浏览器 访问 ：[localhost:8003]()   指向了 8003端口

![8003](D:\typora\JAVA-MD\srpingCloudImages\8003.png)



 8002和8003 端口互相呼应，3个端口 同样如此 。 集群搭建完成!

3个端口 如：

~~~yaml
server:
  port: 8003 # 端口

Spring:
  application:
    name: eureka-server # 应用名称

# 配置Eureka Server 注册中心
eureka:
  instance:
    hostname: localhost                 # 主机名 ，不配置时候默认根据操作系统的主机名来获取
  client:
    service-url:                        # 注册中心对外暴露的注册地址  
      defaultZone: http://localhost:8002/eureka/,http://localhost:8004/eureka/
~~~





#### 8.2 显示IP + 端口

​        一个普通的Netflix Eureka 实例注册ID 等于主机名 （即 没每个主机近提供一项服务），Spring Cloud Eureka提供合理的默认值 。我们可以进行修改 。

~~~yaml
eureka:
  instance:
    hostname: localhost                 # 主机名 ，不配置时候默认根据操作系统的主机名来获取
    prefer-ip-address: true             # 是否使用ip 地址注册
    instance-id: ${spring.cloud.client.ip-address}:${server.port}
~~~



如图：



![ip](D:\typora\JAVA-MD\srpingCloudImages\ip.png)





#### 8.3 服务提供者搭建



==service-provider==

1. 创建子项目`service-provider` 服务提供者的项目，勾选 quickstart

2. 新建application.yml

~~~yaml
server:
  port: 7070 # 端口

Spring:
  application:
    name: service-product # 应用名称

# 配置Eureka Server 注册中心
eureka:
  instance:                # 主机名 ，不配置时候默认根据操作系统的主机名来获取
    prefer-ip-address: true             # 是否使用ip 地址注册
    instance-id: ${spring.cloud.client.ip-address}:${server.port}
  client:
    service-url:                        # 注册中心对外暴露的注册地址
      defaultZone: http://localhost:8002/eureka/,http://localhost:8003/eureka/
~~~
3. pom 依赖

~~~xml
 <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
            <version>3.0.1</version>
        </dependency>

        <!-- spring Boot web 依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.4.2</version>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>

        <!--spring Boot test 依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <version>2.4.2</version>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>juint-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>

~~~



3. 配置controller,pojo,service

- pojo层

~~~java
package org.example.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.io.Serializable;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class Product implements Serializable {
// 一个对象序列化的接口，一个类只有实现了Serializable接口，它的对象才能被序列化。
    private Integer id;
    private String productName;
    private Integer productNum;
    private Double productPrice;
}
~~~

- service层

~~~java
package org.example.service;

import org.example.pojo.Product;

import java.util.List;

public interface ProductService {
//    商品服务的接口
    List<Product> selectProductList();
}
~~~

- controller

~~~java
package org.example.controller;

import org.example.pojo.Product;
import org.example.service.ProductService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;

import java.util.List;

public class ProductController {

    @Autowired
    private ProductService productService;

    /*
    * 查询商品列表
    * */
    @GetMapping("/list")
    public List<Product> selectProductList(){
        return productService.selectProductList();
    }
}
~~~



- 启动类

~~~java
package org.example;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication // 开启 EurekaClient 注解。默认会开启该注解
public class ServiceProviderApplication {

    public static void main(String[] args) {
        SpringApplication.run(ServiceProviderApplication.class,args);
    }
}
~~~

- 运行 [localhost:8002]() 如图

  

![provider](D:\typora\JAVA-MD\srpingCloudImages\provider.png)







#### 8.5服务消费者搭建

> 服务消费者搭建 service-consumer 第一种方式 ：DiscoveryClient



- 新建子项目 `service-consurmer`勾选quickstart

- pom添加

~~~xml

    <!--项目依赖-->
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
            <version>3.0.1</version>
        </dependency>

        <!-- spring Boot web 依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.4.2</version>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>

        <!--spring Boot test 依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <version>2.4.2</version>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>juint-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>

~~~

- application.yml 

~~~yaml
server:
  port: 9090 # 端口

Spring:
  application:
    name: service-consumer # 应用名称

# 配置Eureka Server 注册中心
eureka:
  client:
    register-with-eureka: false
    registry-fetch-interval-seconds: 10
    service-url:                        # 注册中心对外暴露的注册地址
      defaultZone: http://localhost:8002/eureka/,http://localhost:8003/eureka/
~~~

- 搭建pojo,  controller , service

pojo  -- Order实体类

~~~java
package org.example.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.io.Serializable;
import java.util.List;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class Order implements Serializable {
    private Integer id;
    private String orderNo;
    private String orderAddress;
    private Double totalPrice;
    private List<Product> productList;
}

~~~

pojo -- Product

~~~java
package org.example.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.io.Serializable;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class Product implements Serializable {
// 一个对象序列化的接口，一个类只有实现了Serializable接口，它的对象才能被序列化。
    private Integer id;
    private String productName;
    private Integer productNum;
    private Double productPrice;
}
~~~

service -- OrderService

~~~java
package org.example.service;

import org.example.pojo.Order;

public interface OrderService {
    /*
    * 根据主键查询订单
    * */
    Order selectOrderById(Integer id);
}
~~~



**对于服务消费我们这里讲三种实现方式**

1. DiscoveryClient : 通过元数据获取服务信息
2. LoadBalancerClient : Ribbon 的负载均衡器
3. @LoadBalanced : 通过注解开启Ribbon的负载均衡器



> 服务消费者搭建 service-consumer 第二种方式 LoadBalancerClient





> 服务消费者搭建 service-consumer 第三种方式 @LoadBalanced 







### 9. Eureka 架构原理



### 10. ACP 原则



### 11. Eureka 自我保护



### 12. Eureka 优雅停服



### 13. Eureka 安全认证

























