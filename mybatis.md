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











