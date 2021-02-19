# Shiro 框架 

## 1. shiro简介

- Apache Shrio 是一个Java 安全（权限） 框架

- shiro 可以非常容易的开发出足够好的应用，其不仅可以用在JavaSE，也可以用在JavaEE环境

- SHiro 可以完成认证，授权，加密，会话管理，Web集成，缓存等

- 下载地址 ： http://shiro.apache.org/

  



## 2. 快速体验

gitHub地址 ：https://github.com/apache/shiro

新建一个普通的Maven项目 shiro-study,在shiro-study目录下新建hello-shiro 的maven项目

如图：

<img src="D:\typora\JAVA-MD\srpingBoot-整合 shiro\shiro.png" alt="shiro" style="zoom:50%;" />

1. 导入包

~~~xml
 <dependencies>
        <dependency>
            <groupId>org.apache.shiro</groupId>
            <artifactId>shiro-core</artifactId>
            <version>1.4.1</version>
        </dependency>

        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.4.2</version>
        </dependency>

        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
     
     	
        <dependency>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
            <version>1.2</version>
        </dependency>
     
  </dependencies>
~~~

2. log4j的配置 ： resources下新建 lof4j.properties

~~~properties
# 以下来着 gitHub

log4j.rootLogger=INFO, stdout

log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d %p [%c] - %m %n

# General Apache libraries
log4j.logger.org.apache=WARN

# Spring
log4j.logger.org.springframework=WARN

# Default Shiro logging
log4j.logger.org.apache.shiro=INFO

# Disable verbose logging
log4j.logger.org.apache.shiro.util.ThreadContext=WARN
log4j.logger.org.apache.shiro.cache.ehcache.EhCache=WARN
~~~



3.  shiro.ini 是一个 shiro的测试文件，在resources 新建 shiro.ini

ini语法需要编辑器 下载插件 Ini。 **重启后，.ini文件没有语法效果，需要添加在setting里面添加.ini的文件格式。**

~~~ini
[users]
# user 'root' with password 'secret' and the 'admin' role
root = secret, admin
# user 'guest' with the password 'guest' and the 'guest' role
guest = guest, guest
# user 'presidentskroob' with password '12345' ("That's the same combination on
# my luggage!!!" ;)), and role 'president'
presidentskroob = 12345, president
# user 'darkhelmet' with password 'ludicrousspeed' and roles 'darklord' and 'schwartz'
darkhelmet = ludicrousspeed, darklord, schwartz
# user 'lonestarr' with password 'vespa' and roles 'goodguy' and 'schwartz'
lonestarr = vespa, goodguy, schwartz

# -----------------------------------------------------------------------------
# Roles with assigned permissions
# 
# Each line conforms to the format defined in the
# org.apache.shiro.realm.text.TextConfigurationRealm#setRoleDefinitions JavaDoc
# -----------------------------------------------------------------------------
[roles]
# 'admin' role has all permissions, indicated by the wildcard '*'
admin = *
# The 'schwartz' role can do anything (*) with any lightsaber:
schwartz = lightsaber:*
# The 'goodguy' role is allowed to 'drive' (action) the winnebago (type) with
# license plate 'eagle5' (instance specific id)
goodguy = winnebago:drive:eagle5
~~~



4. java目录下新建 QuickStart.java 

~~~java

import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.*;
import org.apache.shiro.config.IniSecurityManagerFactory;
import org.apache.shiro.mgt.SecurityManager;
import org.apache.shiro.session.Session;
import org.apache.shiro.subject.Subject;
import org.apache.shiro.util.Factory;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;


/**
 * Simple Quickstart application showing how to use Shiro's API.
 *
 * @since 0.9 RC2
 */
public class Quickstart {

    private static final transient Logger log = LoggerFactory.getLogger(Quickstart.class);


    public static void main(String[] args) {

        // The easiest way to create a Shiro SecurityManager with configured
        // realms, users, roles and permissions is to use the simple INI config.
        // We'll do that by using a factory that can ingest a .ini file and
        // return a SecurityManager instance:

        // Use the shiro.ini file at the root of the classpath
        // (file: and url: prefixes load from files and urls respectively):
        Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro.ini");
        SecurityManager securityManager = factory.getInstance();

        // for this simple example quickstart, make the SecurityManager
        // accessible as a JVM singleton.  Most applications wouldn't do this
        // and instead rely on their container configuration or web.xml for
        // webapps.  That is outside the scope of this simple quickstart, so
        // we'll just do the bare minimum so you can continue to get a feel
        // for things.
        SecurityUtils.setSecurityManager(securityManager);

        // Now that a simple Shiro environment is set up, let's see what you can do:

        // get the currently executing user:
        Subject currentUser = SecurityUtils.getSubject();

        // Do some stuff with a Session (no need for a web or EJB container!!!)
        Session session = currentUser.getSession();
        session.setAttribute("someKey", "aValue");
        String value = (String) session.getAttribute("someKey");
        if (value.equals("aValue")) {
            log.info("Retrieved the correct value! [" + value + "]");
        }

        // let's login the current user so we can check against roles and permissions:
        if (!currentUser.isAuthenticated()) {
            UsernamePasswordToken token = new UsernamePasswordToken("lonestarr", "vespa");
            token.setRememberMe(true);
            try {
                currentUser.login(token);
            } catch (UnknownAccountException uae) {
                log.info("There is no user with username of " + token.getPrincipal());
            } catch (IncorrectCredentialsException ice) {
                log.info("Password for account " + token.getPrincipal() + " was incorrect!");
            } catch (LockedAccountException lae) {
                log.info("The account for username " + token.getPrincipal() + " is locked.  " +
                        "Please contact your administrator to unlock it.");
            }
            // ... catch more exceptions here (maybe custom ones specific to your application?
            catch (AuthenticationException ae) {
                //unexpected condition?  error?
            }
        }

        //say who they are:
        //print their identifying principal (in this case, a username):
        log.info("User [" + currentUser.getPrincipal() + "] logged in successfully.");

        //test a role:
        if (currentUser.hasRole("schwartz")) {
            log.info("May the Schwartz be with you!");
        } else {
            log.info("Hello, mere mortal.");
        }

        //test a typed permission (not instance-level)
        if (currentUser.isPermitted("lightsaber:wield")) {
            log.info("You may use a lightsaber ring.  Use it wisely.");
        } else {
            log.info("Sorry, lightsaber rings are for schwartz masters only.");
        }

        //a (very powerful) Instance Level permission:
        if (currentUser.isPermitted("winnebago:drive:eagle5")) {
            log.info("You are permitted to 'drive' the winnebago with license plate (id) 'eagle5'.  " +
                    "Here are the keys - have fun!");
        } else {
            log.info("Sorry, you aren't allowed to drive the 'eagle5' winnebago!");
        }

        //all done - log out!
        currentUser.logout();

        System.exit(0);
    }
}
~~~





出现如下，表示成功：

![test](D:\typora\JAVA-MD\srpingBoot-整合 shiro\test.png)







## 3. shiro的subject



Quickstart.java 分析

~~~java
Subject currentUser = SecurityUtils.getSubject(); // 获得当前用户suject对象

Session session = currentUser.getSession();// 通过当前对象得到session

!currentUser.isAuthenticated(); // 判断用户是否认证

currentUser.getPrincipal() ;// 可以获得当前用户的认证

currentUser.hasRole("schwartz"); //判断用户是否有哪些权限

currentUser.logout();// 用户退出
~~~





## 4. springboot 集成

### 1. 搭建环境

- 新建一个web, 导入thymeleaf包

~~~xml
        <dependency>
            <groupId>org.thymeleaf</groupId>
            <artifactId>thymeleaf-spring5</artifactId>
            <version>3.0.12.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.thymeleaf.extras</groupId>
            <artifactId>thymeleaf-extras-java8time</artifactId>
            <version>3.0.4.RELEASE</version>
        </dependency>
~~~



- resources下template新建一个index.html

~~~html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>首页</h1>
<p th:text="${msg}"></p>
</body>
</html>
~~~

- java.com.kuang目录下新建controller的package,新建 MyController.java

~~~java
package com.kuang.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class MyController {
    @RequestMapping({"/","/index"})
    public String toIndex(Model model){
        model.addAttribute("msg","hello,Shiro");
        return "index";
    }
}
~~~

打开项目测试，localhost:8080

<img src="D:\typora\JAVA-MD\srpingBoot-整合 shiro\index.png" alt="index" style="zoom:50%;" />





- 导入Shiro整合spring包

~~~xml
      <dependency>
            <groupId>org.apache.shiro</groupId>
            <artifactId>shiro-spring</artifactId>
            <version>1.7.1</version>
        </dependency>
~~~

- 编写shiro的config ,在config目录下新建ShiroConfig.java

~~~java
package com.kuang.config;

import org.apache.shiro.spring.web.ShiroFilterFactoryBean;
import org.apache.shiro.web.mgt.DefaultWebSecurityManager;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class ShiroConfig {

    // ShiroFilterFactoryBean :3
    @Bean
    public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("securityManager") DefaultWebSecurityManager defaultWebSecurityManager ) {
        ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();

        // 设置安全管理器
        bean.setSecurityManager(defaultWebSecurityManager);
        return bean;

    }

    // DefaultWebSecurityManager  :2
    @Bean(name = "securityManager")
    public DefaultWebSecurityManager getDefaultWebSecurityManager(@Qualifier("userRealm") UserRealm userRealm) {
        DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
// 关联userRealm
        securityManager.setRealm(userRealm);
        return securityManager;
    }

    // 创建realm对象，需要自定义  :1
    @Bean
    public UserRealm userRealm() {
        return new UserRealm();
    }
}
~~~

- tempalte下新建 user目录,下新建 add.html和update.html

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>add</h1>
</body>
</html>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>update</h1>
</body>
</html>
~~~

- 编写控制器

~~~java
package com.kuang.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class MyController {
    @RequestMapping({"/","/index"})
    public String toIndex(Model model){
        model.addAttribute("msg","hello,Shiro");
        return "index";
    }

    @RequestMapping("/user/add")
    public String add(){
         return "user/add";
    }

    @RequestMapping("/user/update")
    public String update(){
        return "user/update";
    }
}
~~~



环境搭建完成

![环境搭建](D:\typora\JAVA-MD\srpingBoot-整合 shiro\环境搭建.png)

### 2.拦截

- 编写login.html

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>登录</h1>
<form action="">
    <p>
        用户名：<input type="text" name="username">
    </p>
    <p>
        密码：<input type="text" name="password">
    </p>
    <p>
        <input type="submit">
    </p>
</form>
</body>
</html>
~~~

-  编写控制器

~~~java
 @RequestMapping("/toLogin")
    public String toLogin(){
        return "login";
 }
~~~

- 拦截完整代码

~~~java
package com.kuang.config;

import org.apache.shiro.spring.web.ShiroFilterFactoryBean;
import org.apache.shiro.web.mgt.DefaultWebSecurityManager;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.LinkedHashMap;

@Configuration
public class ShiroConfig {

    // ShiroFilterFactoryBean :3
    @Bean
    public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("securityManager") DefaultWebSecurityManager defaultWebSecurityManager ) {
        ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();

        // 设置安全管理器
        bean.setSecurityManager(defaultWebSecurityManager);
        // 添加shiro的内置过滤器
         /**
          * anon: 无需认证，就可以访问
          * authc: 必须认证才能访问
          * user: 必须 拥有记住我功能才能访问
          * perms: 拥有对某个资源的权限才能访问
          * role: 拥有某个角色权限才能访问
          *
          * */

         // 拦截
        LinkedHashMap<String, String> filterMap = new LinkedHashMap<>();

//        filterMap.put("/user/add","authc");
//        filterMap.put("/user/update","authc");
        filterMap.put("/user/*","authc");

        bean.setFilterChainDefinitionMap(filterMap);

        bean.setLoginUrl("/toLogin");
        return bean;

    }

    // DefaultWebSecurityManager  :2
    @Bean(name = "securityManager")
    public DefaultWebSecurityManager getDefaultWebSecurityManager(@Qualifier("userRealm") UserRealm userRealm) {
        DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
// 关联userRealm
        securityManager.setRealm(userRealm);
        return securityManager;
    }

    // 创建realm对象，需要自定义  :1
    @Bean
    public UserRealm userRealm() {
        return new UserRealm();
    }
}
~~~

### 3. 认证

~~~java
 // 认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        System.out.println("执行了认证");

        // 用户名，密码 数据库中取
        String name = "root";
        String password = "123456";

        UsernamePasswordToken userToken = (UsernamePasswordToken) token;
        if(!userToken.getUsername().equals(name)){
            return  null; // 抛出异常
        }
        // 密码认证，shiro 会做
        return  new SimpleAuthenticationInfo("",password,"");
    }
~~~







## 5. shiro 整合mybatis



1. 导入包

~~~xml
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.16</version>
        </dependency>   
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.21</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.4</version>
        </dependency>
~~~



新建application.yml

~~~yaml
spring:
  datasource:
    username: root
    password: root
    driver-class-name: com.mysql.jdbc.Driver
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

- application.properties 

~~~properties
mybatis.type-aliases-package=com.kuang.pojo
mybatis.mapper-locations=classpath:mapper/*.xml
~~~

- 新建实体类

~~~java
package com.kuang.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private int id;
    private String name;
    private String pwd;
}
~~~



















































