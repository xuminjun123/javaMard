# SQ L 问题

**开头为'小'字的内容**：           where `字段名` like '小%'

**开头不是为'小'字的内容**       where `字段名` not like '小%'

**包含'小'字的所有内容**           where `字段名` LIKE '%小%'



**score 为98,60,92的所有内容**   where score IN (98,60,92);



**合并查询表 user 和表 user_ext 中 id 相同的所有数据**    select * from user, user_ext WHERE user.id = user_ext.id;

**获取表 user 中字段 score 的平均值 ** select avg(score) from user

**按照字段 weight 进行倒序排序**  ORDER BY weight DESC;



**通过左连接 获取表 user(别名t1) 和表 user_ext(别名t2) 中字段 id 相同的数据，其中字段 age 大于9，并仅返回 id、students、age、weight 这几个字段的数据**

~~~mysql
 SELECT t1.id, t1.students, t2.age, t2.weight FROM user AS t1 LEFT JOIN user_ext AS t2 ON t2.id = t1.id WHERE t2.age > 9;
~~~

