# mybatis框架学习

​	 

##  第一个mybatis程序

### 1.搭建环境



1.  新建一个meaven项目
2. pom.xml 文件导入相关依赖

~~~java
   <!--  导入依赖-->
    <dependencies>
        <!--    mysql驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.5.40</version>
        </dependency>
        <!--    mybaties-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.2</version>
        </dependency>
        <!--    juint-->
        <dependency>
            <groupId>juint</groupId>
            <artifactId>juint</artifactId>
            <version>4.12</version>
        </dependency>
    </dependencies>
~~~

3. 删除src 目录

4. 创建一个模块

   菜单栏 new --->>> mdule----->>>next----->>>输入名称--->finish



### 2.  编写mybatis工具类

1. 在mybats-01(刚刚新建的模块) -目录下 新建 mybatis-config.xml ,粘贴下面内容

~~~java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--核心配置文件-->
<configuration>
<!--    配置多套环境-->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306?serverTimezone=GMT%2B8/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="org/mybatis/example/BlogMapper.xml"/>
    </mappers>
</configuration>

~~~

2. 链接数据库

3. 编写工具类

   
   
   ```java
package com.kuang.utils;
   
   import org.apache.ibatis.io.Resources;
   import org.apache.ibatis.session.SqlSession;
   import org.apache.ibatis.session.SqlSessionFactory;
   import org.apache.ibatis.session.SqlSessionFactoryBuilder;
   
   import java.io.IOException;
   import java.io.InputStream;
   
   public class MybatisUtils {
       private static SqlSessionFactory sqlSessionFactory;
   
       static {
           try {
               // 使用Mybatis第一步 ： 获取 SqlSessionFactory对象
               String resource = "mybatis-config.xml";
               InputStream inputStream = Resources.getResourceAsStream(resource);
               sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
           } catch (IOException e) {
               e.printStackTrace();
           }
   
       }
   
       // 既然有了 SqlSessionFactory，顾名思义，我们可以从中获得 SqlSession 的实例。SqlSession 提供了在数据库执行 SQL 命令所需的所有方法。
       // 你可以通过 SqlSession 实例来直接执行已映射的 SQL 语句。
       public static SqlSession getSqlSession() {
           return sqlSessionFactory.openSession();
       }
   }
   
   ```



### 3. 编写代码

- 实体类   

   User

  ~~~java
  package com.kuang.pojo;
  
  public class User {
      private int id;
      private String name;
      private String pwd;
      // 无参构造
      public User(){
  
      }
      // 有参构造
      public User(int id,String name, String pwd){
          this.id = id;
          this.name = name;
          this.pwd = pwd;
      }
      public int getId(){
          return id;
      }
      public void setId(int id){
          this.id = id;
      }
      public String getName(){
          return name;
      }
      public void setName(String name){
          this.name = name;
      }
      public String getPwd(){
          return pwd;
      }
      public void setPwd(String pwd){
          this.pwd = pwd;
      }
  
      @Override
      public String toString() {
          return "User{" +
                  "id=" + id +
                  ", name='" + name + '\'' +
                  ", pwd='" + pwd + '\'' +
                  '}';
      }
  }
  ~~~

- 接口Dao

  UserDao 

  ~~~java
  package com.kuang.dao;
  
  import com.kuang.pojo.User;
  
  import java.util.List;
  
  public interface UserDao{
      List<User> getUserList();
  }
  ~~~

  

- 接口实现类  === >>>>mapper(后面都是mapper文件) :exclamation:

    UserMapper.xml 

  ~~~java
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  
  <!--  namespace 绑定一个对应的Dao/Mapper 接口 -->
  <mapper namespace="com.kuang.dao.UserDao">
  
      <!--  
      id 对应  UserDao中的 list<User> getUserList();
      resultType : 相当于实现 User接口，返回的是list<User>中的User
      -->
      <select id="getUserList" resultType="com.kuang.pojo.User">
      select * from mybaits.user
    </select>
  </mapper>
  ~~~

  

### 4.测试

1. juint 测试代码编写

~~~java
package com.kuang.dao;

import com.kuang.pojo.User;
import com.kuang.utils.MybatisUtils;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;

import java.util.List;

public class UserDaoTest {

    @Test
    public void test() {
        // 第一步获得Sqlsession 对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();

        // 方式1 ： 执行Sql getMapper()
        UserDao userDao = sqlSession.getMapper(UserDao.class);
        List<User> userList = userDao.getUserList();

        for (User user : userList) {
            System.out.println(user);
        }

//         关闭Sqlsession
        sqlSession.close();
    }
}
~~~



测试遇到的问题

1. 配置文件没有注册
2. 绑定接口错误
3. 方法名称错误
4. 返回类型错误
5. meaven导出资源



## 增删改查:female_detective:

>  CURD 需要提交事务

 **以后文件需要改动的地方 **

1. mapper.xml 中的 namespace( namespace绑定的包名 要和 Dao和mapper一样 )

2. **select**  查询语句

   -  `id` ： 对应的namespace中的方法名称

   - `resultType` : sql语句执行的返回值 ！

   - `parameterType` : 参数类型

     ~~~java
     // mapper.java
     public interface UserMapper{
         /*根据id查询用户*/
         User getUserById(int id);
     }
     ~~~

     ~~~java
     // mapper.xml
         <select id="getUserById" parameterType="int" 			       resultType="com.kuang.pojo.User">
         select * from user where id = #{id}
       </select>
     ~~~

     ~~~java
     // test
     @Test
         public void getUserById() {
             // 第一步获得Sqlsession 对象
             SqlSession sqlSession = MybatisUtils.getSqlSession();
     
             UserMapper mapper = sqlSession.getMapper(UserMapper.class);
             User user = mapper.getUserById(1);
             System.out.println(user);
     
             // 关闭Sqlsession
             sqlSession.close();
     }
     ~~~

     

3. **insert** 添加语句

   ~~~java
   <insert id="addUser" parameterType="com.kuang.pojo.User">
        insert into user (id,name,pwd) values (#{id},#{name},#{pwd});
   </insert>
   ~~~

   

4. **update** 更新语句

   ~~~java
       <update id="updateUser" parameterType="com.kuang.pojo.User">
           update user set name=#{name},pwd=#{pwd} where id=#{id} ;
       </update>
   ~~~

   

5. **delete** 删除语句

   ~~~java
       <delete id="deleteUser" parameterType="int">
           delete from user where id=#{id};
       </delete>
   ~~~





## Map和模糊查询拓展

> 如果，实体类或者数据库中的表，字段或者参数过多，应当考虑使用Map！

~~~java
// mapper.java
 /**
    * 添加一个用户 使用map传
 */
int addUser2(Map<String,Object> map);

// mapper.xml
 <insert id="addUser2" parameterType="">
        insert into user (id,name,pwd) values (#{userid},#{userName},# {passwprd});
</insert>
    
    // test
    @Test
    public void addUser2() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();

        UserMapper mapper = sqlSession.getMapper(UserMapper.class);

//        万能map
        Map<String, Object> map = new HashMap<String, Object>();
        map.put("userid", 5);
        map.put("userName", "hello");
        map.put("passwprd", 222333);
        mapper.addUser2(map);

        // 提交事务
        sqlSession.commit();
        sqlSession.close();
    }
~~~



总结 ：

Map 传递参数，直接在sql中去除key即可！ 【 parameterType = "map" 】

对象传递参数，直接诶在sql中去出对象的属性即可 【 parameterType="Object" 】

只有一个基本类型参数下，可以直接在sql中取到！

多个参数只有Map,或者注解。



**模糊查询拓展**

1. java代码执行的时候，传递通配符%  %

   ~~~java
   select * from user where name like #{value}
   ~~~

   

2. 在sql中拼接通配符

   ~~~java
   select * from user where name like "%"#{value}"%"
   ~~~

   

~~~java
// mapper.java
 /**
 * 模糊查询
 */
  List<User> getUserLike(String value);
~~~

~~~java
// mapper.xml       
<select id="getUserLike" resultType="com.kuang.pojo.User">                  select * from user where name like #{value}
</select>
~~~

~~~java
   // test
   // 模糊查询
    @Test
    public void getUserLike() {
        // 第一步获得Sqlsession 对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();

        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        List<User> userList = mapper.getUserLike("%李%");
        for (User user : userList) {
            System.out.println(user);
        }

    // 关闭Sqlsession
        sqlSession.close();
    }
~~~





## 配置解析

1. 核心配置文件

   - mybatis-config.xml(  proerties: 属性，settings: 设置 ，typeAliases ： 类型别名  ，environments: 环境配置 ，mappers：映射器 )

     

2. 配置环境（environments）
   - mybatis 可以配置多套环境
   - 尽管可以配置多个环境，但每个sqlSessionFactory 实例只能选择一种环境
   - mybatis默认的事务管理器： JDBC ， 连接池 ： POOLED

3. 属性 （properties）： 

   - 我们可以通过properties属性来实现引用配置文件

   - 这些属性都是可外部配置且可动态替换的，即可以在典型的Java属性文件中配置，也可以通过properties元素的子元素来传递。

编写一个配置文件

~~~properties
driver=com.mysql.Driver
url=jdbc:mysql://localhost:3306/mybatis?useUnicode=true&characterEncoding=utf8
uername=root
password=root
~~~

- 可以直接引入文件
- 可以在其中增加一些属性配置
- 如果两个文件有同一个字段，优先使用外部配置文件





## 配置别名优化

~~~java
  // 可以给实体类取别名 
    <typeAliases>
        <typeAlias type="com.kuang.pojo.User" alias="User"></typeAlias>
    </typeAliases>
        
   // 可以通过包名取别名，默认别名为首字母大小写
    <typeAliases>
        <package name="com.kuang.pojo"/>
    </typeAliases>  
~~~

mapper.xml即可以优化了。

在实体类较少，使用第一种。 （ 可以IDY别名 ）

实体类多，使用第二种。    （ 无法DIY别名，但是通过注解可以 ）

~~~java
@Alias("name") 
public class User{
}
~~~





## 配置之映射说明

映射器 ( mappers )

方式一: 使用resource文件绑定注册

~~~xml
    // 每一个Mapper.xml都需要在mubatis核心文件中注册
	<mappers>
        <mapper resource="com/kuang/dao/UserMapper.xml"/>
    </mappers>
~~~



方式二:  使用class文件绑定注册

~~~xml
    // 每一个Mapper.xml都需要在mubatis核心文件中注册
	<mappers>
        <mapper class="com/kuang/dao/UserMapper"/>
    </mappers>
~~~

- 接口和他的Mapper配置文件必须同名

- 接口和他的Mapper配置必须在他的同一个包下

  

方式三:  使用扫描包绑定注册

~~~xml
    // 每一个Mapper.xml都需要在mubatis核心文件中注册
	<mappers>
        <mapper class="com/kuang/dao/UserMapper"/>
    </mappers>
~~~

- 接口和他的Mapper配置文件必须同名
- 接口和他的Mapper配置必须在他的同一个包下



## 生命周期和作用域

**生命周期和作用域,是至关重要的,因为错误的适用于会导致非常严重的并发问题**

![微信截图_20210113184516](D:\typora\JAVA-MD\mybatisImages\微信截图_20210113184516.png)



1. `SqlSessionFactoryBuilder: `  **用一次**

- 一旦创建了`SqlSessionFactory`,就不需要在使用它了

- 一般放在局部变量中

2. `SqlSessionFactory`:一旦创建就必须**一直存在**,(当成数据连接池)

   - `SqlSessionFactory`的最佳作用域是应用作用域

   - 最简单使用单例模式,或者静态单例模式

3. `SqlSession`: **用完关闭**
   - 连接到连接池的一个请求
   - SqlSession 的实例不是线程安全的,因此是不能被共享的,所以他的最佳作用域是请求或者方法作用域.
   - 用完之后需要赶紧关闭,否则资源被占用

![微信截图_20210113184625](D:\typora\JAVA-MD\mybatisImages\微信截图_20210113184625.png)

每一个Mapper代表一个业务。





## ResultMap结果集映射







