# Java巩固

## 一、线程相关

### 1. 进程与线程

> 概念

进程 ：相当于 一个应用程序 

线程 ：负责在单个程序里执行多任务。（其实就是一条执行路径）

**进程是所有线程的集合，每一个线程是线程中一条执行路径**



> 为什么需要使用到多线程？

- 提高程序运行效率。



> 多线程应用场景

- 多线程下载， 分批发送短信等。



> 多线程创建方式

- 第一种 继承Thread类，重写run 方法

  ~~~java
  package test;
  /*
  *  继承Thread,重写run
  * */
  public class CreatedThread  extends Thread{
  
      @Override
      public void run() {
          for (int i = 0; i < 20; i++) {
              System.out.println( "run() i:" +i);
          }
      }
  }
  
  
  =======================================================================
  
  package test;
  
  public class CreateThread2 {
      public static void main(String[] args) {
          CreatedThread createdThread = new CreatedThread();
         // 启动线程是调用start(),调用run()相当于主线程执行
          createdThread.start(); // 启动线程，不能使用run方法
      }
  }
  
  ~~~

  

- 第二种 实现Runnable 接口，重写run 方法（ 更好。）

  ~~~java
  package test;
  /*
  * 实现 runnable ,重写run
  * */
  public class CreateRunnable implements Runnable {
      @Override
      public void run() {
          for (int i = 0; i < 20; i++) {
              System.out.println( "run() i:" +i);
          }
      }
  }
  
  =======================================================================
  
  package test;
  
  public class CerateRunnable2 {
      public static void main(String[] args) {
          CreateRunnable createRunnable = new CreateRunnable();
          Thread thread = new Thread(createRunnable);
          thread.start();
      }
  }
  
  ~~~



- 匿名内部类方式

  ~~~java
  package test;
  
  public class Created {
      public static void main(String[] args) {
          Thread thread = new Thread(new Thread(new Runnable() {
              @Override
              public void run() {
                  for (int i = 0; i < 20; i++) {
                      System.out.println("run() i:" + i);
                  }
              }
          }));
          
          thread.start();
          
      }
      
  }
  
  ~~~



### 2. 获取线程对象以及名称

>  常用API 

|      start()       |          启动线程           |
| :----------------: | :-------------------------: |
|  currentThread()   |      获取当前线程对象       |
|      getID()       | 获取当前线程ID （从０开始） |
|     getName()      |      获取当前线程名称       |
| `sleep(long mill)` |          休眠线程           |
|      `stop()`      |          停止线程           |
|                    |                             |







## 二、JSON



> 常用 JSON解析框架

- fastjson(阿里)
- gson(谷歌)
- jackson(SpringMVC)  



### 1. fastjson 解析json



~~~xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.75</version>
</dependency>
~~~



> 组装 JSON



~~~json
package com.json;

import com.alibaba.fastjson.JSONArray;
import com.alibaba.fastjson.JSONObject;

import java.util.Arrays;

public class JsonTest {
    static String str = "{a:1,b:{c:2,d:3}}";

    public static void main(String[] args) {
 
        // 组装JSON
        JSONObject jsonData = new JSONObject();
        JSONObject jsonObject = new JSONObject();
        JSONObject jsonObject1 = new JSONObject();

        JSONArray jsonArray = new JSONArray();

        jsonObject.put("name","名字");
        jsonObject.put("age","年龄");
        jsonObject1.put("name1","名字1");
        jsonObject1.put("age1","年龄1");

        jsonArray.add(jsonObject);
        jsonArray.add(jsonObject1);

        jsonData.put("sites",jsonArray);

        System.out.println(jsonData.toJSONString());
// {"sites":[{"name":"名字","age":"年龄"},{"name1":"名字1","age1":"年龄1"}]}
    }

}
~~~









## 三、 反射

思考 ：

1. 如何不用new 创建对象
2. 累的私有成员，private 真的可以不被访问吗？ 



反射  ： `class.formName(包地址)`  

常用api :

- getDeclaredMethods();   获取该类的所有方法。
- getDeclareFields() ;        获取所有成员属性
-  ... ....



> 什么是反射 ？

就是正在运行，动态获取这个类的所有信息。



> 反射机制的作用

- 反编译
- 通过反射机制访问 java 对象的属性，方法构造方法等



> 反射机制的应用场景

- jbdc 加载驱动
- Spring ioc
- 框架







## 四、Spring   框架



>  什么是Spring？  

把每个bean 和 bean 之间的关系，都交给第三方容器（spring）管理。

原理就是使用 ： 反射 + dom4j 

spring 核心： ioc(控制) 、di(依赖) 、aop(面向切面编程)





> di 和 ioc 区别是什么 ？

- di  ： 依赖注入 

- ioc :   创建对象





















## 五、设计模式



> 单例模式

保证在 一个jvm 当中只能有一个实例

单例有7种写法 ，

- 懒汉式 ------- 不安全，所以需要加上同步`synchronized`

  - 懒汉式 : 当你需要的时候，才会被实例化，之后全是用同一个实例。

  ~~~java
  
  ~~~

  

- 饿汉时 ------- 天生线程安全，当class文件被加载的时候，被初始化了

  ~~~java
  
  ~~~

  

##  六、mysql锁

> 锁的类型

基于锁的属性分类 ： 共享锁 、排他锁



基于锁的粒度分类 :

- 行级锁（INNODB）
- 表级锁  ( INNODB 、MYISAM ) 
- 页级锁 (BDB 引擎 )

- 记录锁
- 间隙锁
- 临建锁



基于锁的状态分类 ; 意向共享锁，意向排他锁

- 共享锁 （ Share Lock）

  ~~~tex
  共享锁又称为读锁 ， 简称 S 锁 ;
  - 当一个事物为数据加上读锁之后，其他事务只能对该数据读锁，而不能对数据加 写锁，
  - 直到所有的读锁释放之后其他事务才能对其进行加持写锁 。 
  - 共享锁的特性主要是为了支持并发的读取数据，读取数据的时候不支持修改，避免出现重复读的问题 
  ~~~

- 排他锁 ( exclusuve Lock )

  ~~~java
  排他锁 又称为 写锁，简称 X 锁
  ~~~

- **表锁**

  ~~~tex
  表锁是指上锁的时候锁住的是整个表 , 当下一个事务访问该表的时候，必须等前一个事务释放了锁才能进行对表访问。
  
  特点 ： 粒度大，加锁简单 ，容易冲突
  ~~~

- **行锁**

  ~~~tex
  行锁 是指上锁的时候锁住的是表的某一行或多行记录 ，其他事务访问同一张表时，只有被锁住的记录不能被访问，其他的记录可正常访问 ： 
  
  特点 : 力度小，加锁比表锁麻烦，不容易冲突 ，相比表锁支持的并发高
  ~~~

- 记录锁（Record Lock）

  ~~~tex
  记录锁 也是行锁的一种，只不过记录锁的范围只是表中的某一条记录 ， 记录锁是事务在 加锁后锁住的只是表的某一条记录。
  精准条件命中， 并且命中的条件字段是唯一索引
  加了记录锁之后数据可以避免数据字啊查询的时候被修改的重复读问题，也避免了字啊修改事务未提交前被其他是我读取的脏读问题。
  ~~~

- 页锁





































































































































 















 




















































































