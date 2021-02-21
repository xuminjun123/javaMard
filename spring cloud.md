# Spring Cloud

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







##  Netfilx 第一代



> Netfilx 是一家美国公司，在美国，加拿大提供互联网随选流媒体播放 ，定制DVD，蓝光光碟在线出租 业务。



针对 多种Netfilx  组件提供的开发工具包，其中包括Eureka , Hystrix ，Ribbon， zuul , Archaius等。

- `Netfilx Eureka`: 一个基于 Rest 服务的服务治理组件 ， 包括服务注册中心 ，服务注册与服务发现机制的实现，实现了 云端负载均衡和中间层服务的故障转移。
- `Netfilx Hystrix`: 容错管理工具 ，实现断路器模式 ，通过控制服务的节点 ，从而对延迟和故障提供更加强大的容错能力。
- `Newfilx Ribbon`:客户端负载均衡的服务调用组件
- `Netfilx Feign`: 基于Ribbon 和 Hystrix 的生命是服务调用组件
- `Netfilx Zuul`: 微服务网关 ， 提供动态路由 ， 访问过滤等服务
- `Netfilx Archaius`: 配置管理API ， 包含 一系列配置管理 API ，提供动态类型化属性 , 线程安全配置操作 ， 轮询框架 ，回调机制等功能。



## Alibaba

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
|  分布式消息   |          -          |      SCS RabbitMQ      |           -           |         -          |           -            |     `SCS RoketMQ`     |
|   负载均衡    |       Ribbon        |           -            |           -           |         -          |           -            |      `Dubbo LB`       |
|  分布式事务   |          -          |           -            |           -           |         -          |           -            |        `Seata`        |







































































