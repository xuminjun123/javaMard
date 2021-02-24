# SpringIOC 核心

1.  使用Spring 框架

2. 反射机制

IOC 控制反转 Inverse of Control ，创建对象的权限 ，Java程序中需要用到的对象不再由程序员自己创建 ， 而是由给IOC 容器来创建 。



## 1. servlet

- 新建 普通的Maven 项目 。导入包

~~~xml
<dependencies>
        <!--sevlet 依赖-->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>4.0.1</version>
        </dependency>
    </dependencies>
    <!--设置Maven 的JDK版本，默认是5，手动修改到8-->
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
        </plugins>
    </build>

	<packaging>war</packaging>
~~~

- 创建Servlet

~~~java
package com.southwind;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/hello")
public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       resp.getWriter().write("Spring");
    }
}
~~~

- 部署到Tomcat

- 启动Tomcat 浏览器访问  [localhost:8080/hello]()

浏览器出现  Spring 

- servlet , Service , Dao

Controller ---> Service ---> Dao

- 当需求发生变更时候，肯需要频繁修改Java代码 ，效率很低 ，如何解决

使用静态工厂来创建特定的 实现类 ， 不在把代码写在 service中了

~~~java
package com.southwind.factory;

import com.southwind.dao.HelloDao;
import com.southwind.dao.impl.HelloDaoImpl;

public class BeanFactory {
    public static HelloDao getDao(){
        return new HelloDaoImpl();
    }
}
~~~

上述方式  并不能解决问题 ，因为需求发生改变人需要改变代码

不改变Java 代码 ，就可以实现 实现类的 切换。

使用 外部表配置文件的方式，将具体的实现类写道 配置文件中，Java程序只需要读取配置文件即可。



反射 

- 在resources 下新建 factory.properties

定义外部配置文件

~~~properties
helloDao=com.southwind.dao.impl.HelloDaoImpl
~~~

- Java读取这个配置文件

~~~java
package com.southwind.factory;

import com.southwind.dao.HelloDao;
import com.southwind.dao.impl.HelloDaoImpl;

import java.io.IOException;
import java.lang.reflect.InvocationTargetException;
import java.util.Properties;

public class BeanFactory {

    private static Properties properties;

    static {
        properties = new Properties();
        try {
            properties.load(BeanFactory.class.getClassLoader().getResourceAsStream("factory.properties"));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    public static Object getDao(){
        String value = properties.getProperty("helloDao");
        System.out.println(value);

        // 反射机制创建对象
        try {
            Class clazz = Class.forName(value);
            Object object = clazz.getConstructor(null).newInstance(null);
            return object;
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        } catch (NoSuchMethodException e) {
            e.printStackTrace();
        }

        return null;
    }
}
~~~

- 修改Service

~~~java
private HelloDao helloDao = (HelloDao) BeanFactory.getDao();
~~~



Spring IOC 中的bean 是单例的。

~~~java
package com.southwind.factory;

import com.southwind.dao.HelloDao;
import com.southwind.dao.impl.HelloDaoImpl;

import java.io.IOException;
import java.lang.reflect.InvocationTargetException;
import java.util.HashMap;
import java.util.Map;
import java.util.Properties;

public class BeanFactory {

    private static Properties properties;
    private static Map<String,Object> cache = new HashMap<>();

    static {
        properties = new Properties();
        try {
            properties.load(BeanFactory.class.getClassLoader().getResourceAsStream("factory.properties"));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    public static Object getDao(String beanName){

        // 先判断缓存中是否存在 bean
        if(!cache.containsKey(beanName)){
            synchronized (BeanFactory.class){
                if(!cache.containsKey(beanName)){

                    String value = properties.getProperty(beanName);
                    System.out.println(value);

                    // 反射机制创建对象
                    try {
                        Class clazz = Class.forName(value);
                        Object object = clazz.getConstructor(null).newInstance(null);
                        return object;
                    } catch (ClassNotFoundException e) {
                        e.printStackTrace();
                    } catch (InstantiationException e) {
                        e.printStackTrace();
                    } catch (IllegalAccessException e) {
                        e.printStackTrace();
                    } catch (InvocationTargetException e) {
                        e.printStackTrace();
                    } catch (NoSuchMethodException e) {
                        e.printStackTrace();
                    }

                }
            }
        }
        return cache.get(beanName);
    }
}
~~~



自己放弃了创建对象的权限，将创建对象的权限 交给BeanFactory , 这种控制权交给另一个的思想 ，角色控制反转IOC 。







## 2. spring IOC 注解使用

XML 和 注解 ，XML已经被淘汰了， 目前主流是注解 。

~~~java
package com.southwind.spring.test;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {
    public static void main(String[] args) {
        // 加载IOC　容器
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext("com.southwind.spring.entity");

        System.out.println(applicationContext.getBean("account"));

        String[] beanDefinitionNames = applicationContext.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            System.out.println(beanDefinitionName);
        }
    }
}
~~~



~~~java
package com.southwind.spring.entity;

import lombok.Data;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Data
@Component
public class Order {
    @Value("xxx123")
    private String orderId;

    @Value("1000.0")
    private Float price;

}
~~~

~~~java
package com.southwind.spring.entity;

import lombok.Data;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Data
@Component
public class Account {
    @Value("1")
    private Integer id;
    @Value("张三")
    private String name;
    @Value("22")
    private Integer age;

    @Autowired
    @Qualifier("order") 
    private Order myOrder;
}
~~~



## 3. IOC基于注解　的执行原理















