#  SpringCloud Alibaba

创建 父工程   

~~~xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>Hoxton.SR3</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-clou-alibaba-dependencies</artifactId>
            <version>2.2.1.RELEASE</version>
        </dependency>
    </dependencies>
</dependencyManagement>
~~~




## Nacos服务治理


服务治理 = 服务注册 + 服务发现



### Nacos 服务注册

 Nacos 不需要写代码，只需要 拷贝下来 运行就行了

[nacos-server-1.2.1.zip](https://pan.baidu.com/s/1kDelloNK23RIbFjj6efmqg)
下载地址 ： https://pan.baidu.com/s/1kDelloNK23RIbFjj6efmqg
提取码: 6d8b



<img src="D:\typora\JAVA-MD\srpingCloudImages\nacos.png" alt="nacos" style="zoom:50%;" />



window 启动startup.cmd

linux   启动 startup.sh



启动完成

![NacosCmd](D:\typora\JAVA-MD\srpingCloudImages\NacosCmd.png)



运行成功 ！

浏览器输入 : http://localhost:8848/nacos/index.html 

账号 : nacos

密码 : nacos

![nacosOK](D:\typora\JAVA-MD\srpingCloudImages\nacosOK.png)

Nacos 运行成功！



==创建nacos-provider==

创建一个子项目 provider 

pom

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
      <!--与父工程关联-->
    <parent>
        <groupId>com.southwind</groupId>
        <artifactId>springclouddemo</artifactId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>
    <groupId>com.southwind</groupId>
    <artifactId>provider</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>provider</name>
    <description>Demo project for Spring Boot</description>
    <properties>
        <java.version>1.8</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
            <version>2.2.1.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
~~~



application.yml

~~~yml
 spring:
  cloud:
    nacos:
      discovery:
        server-addr:  localhost:8848
  application:
    name: provider   
~~~



启动运行。

浏览刷新 locahst: 8080/nacos/index.html



![provider-start](D:\typora\JAVA-MD\srpingCloudImages\provider-start.png)







### Nacos 服务发现

Nacos-consumber

新建 consumber 子项目

pom  导入包

~~~xml
<parent>
    <groupId>com.southwind</groupId>
    <artifactId>provider</artifactId>
    <version>0.0.1-SNAPSHOT</version>
</parent>

<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
    <version>2.2.1.RELEASE</version>
</dependency>
~~~



建立一个controller层

~~~java
package com.southwind.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.cloud.client.discovery.DiscoveryClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

@RestController
public class ConsumberController {

    @Autowired
    private DiscoveryClient discoveryClient;

    @GetMapping("/instances")
    public List<ServiceInstance> instances(){
        List<ServiceInstance> provider = discoveryClient.getInstances("provider");
        return provider;
    }
}
~~~



application.yml

~~~yml
server:
  port: 8180
~~~



浏览器 运行 localhost:8181/instances



<img src="D:\typora\JAVA-MD\srpingCloudImages\consumber.png" alt="consumber" style="zoom:50%;" />



## Gateway



srping cloud  gateway ： 服务网关

1. 为什么 需要使用服务网关

- 单体应用 ： 浏览器发起请求到单体应用所在的机器，应用从数据库查询原路返回给浏览器 ，对于单体应用来说是不需要网关的 。
- 微服务 ： 微服务的应用可能部署在不同的机房 ，不同地区，不同域名下，此时客户端（浏览器/手机/软件工具）想要请求对应的服务。都需要知道机器的Ip地址或者域名，当微服务实例众多是，这是非常难以记忆的，对于客户端说也太复杂难以维护。此时就有了网关，客户端相关的请求直接发送到网关，由网关根据请求标识解析出具体的微服务地址，再把请求转发到微服务实例，这其中的记忆功能就由 网关来操作。



2. 网关具有身份认证 与 安全，审查与监控，动态路由，负载均衡，缓存，请求分片与管理， 静态响应处理等功能。

功能 ：

- 性能 ： API 高可用。负载均衡，容错机制
- 安全：权限身份认证 ，托名，流量清洗，后端签名（保证全链路可信调用） ，黑名单（非法调用的限制）

- 日志 ：日志记录 ，一旦涉及分布式，全链路跟踪必不可少

- 缓存 ： 数据缓存

- 监控 ：记录请求响应数据，API 耗时分析，性能监控

- 限流 ： 流量控制，错峰流控，可以定义多种限流规则

- 灰度 ：线上灰度部署，可以减少分险

- 路由 ： 动态路由规则

  



###　常用的网关解决方案

1. Nginx ＋Lua

Nginx 适合做门户网关，是作为整个全局的网关。gateway 时候做业务网关，主要对应不同的客户提供服务，用于聚合业务。（  gateway 可以实现熔断，重试，这是Nginx 不具备的 ）

2. Kong

本身基于Nginx + Lua ，提供大量的插件 （ 付费 ）

3. Treafix

4. Spring Cloud Netfilx Zuul
5. Spring Cloud Gateway










