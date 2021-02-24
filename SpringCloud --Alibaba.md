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




## 服务治理


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













## 服务调用负载均衡











## Ribbon负载均衡











## 什么是Sentinel













## Sentinel 流控规则







## Sentinel 热点规则 &授权规则











## RocketMQ 服务搭建













## Java 实现消息的生产和消费







## Spring Cloud Alibaba 整合 RocketMQ





## Gateway 路由映射









## Gateway 路由限流









## Gateway API  分组限流











## 生命是分布式事务





## Seata 环境搭建







## Seata 实现分布式事务











































