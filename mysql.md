# Mysql

## where条件

| 操作符                 | 含义         | 范围 | 结果                                                     |
| ---------------------- | ------------ | ---- | -------------------------------------------------------- |
| =                      | 等于         | -    | false                                                    |
| <> 或 !=               | 不等于       |      | true                                                     |
| >                      |              |      |                                                          |
| <                      |              |      |                                                          |
| <=                     |              |      |                                                          |
| >=                     |              |      |                                                          |
| between  ...   and ... | 在某个范围内 |      |                                                          |
| and                    | &&           |      | 逻辑与，两个都为真，结果为真                             |
| or                     | \|\|         |      | 逻辑或，其中一个为真，则结果为真                         |
| not                    |              |      |                                                          |
| `like`                 |              |      | A like B ,如果a匹配b,则结果为真                          |
| `in`                   |              |      | a in (a1,a2...)  假设a 在a1...其中的某一个值中，结果为真 |
| is null                |              |      | 如果 操作符号为null ,结果为真                            |
| is not null            |              |      | 如果操作付不为null,结果为真                              |

【注】 ：

**like**  

- %    表示结合任意字符
- _     表示结合只有一个字的
- __    表示结合2个字的

- %字% 表示匹配当中有“字”的



**in** : 具体的一个值或者 多个值





## CURD 命令



**insert**

~~~mysql
insert into 表名([字段名]) values('值1')
~~~

 【 注意事项】 ：

- 字段和字段之间使用 英文逗号 隔开

- 字段是可以省略的，但是后面的值必须要一一对应

  



**delete** 删除

~~~mysql
delete from `表名` where 条件
~~~



**truncate** 清空

~~~mysql
truncate `表`
~~~



 delete 和 truncate 区别 ：

- 相同点 : 都能删除数据 ，都不会删除表结构
- 不同 ： 
  - truncate 重新设置 自增列， 计数器会归零
  - truncate 不会影响事务



**update**

~~~mysql
update `表` set `字段1`=新值1,`字段2`=新值2 where 条件       # 不指定where条件 会把所有的都改掉
~~~





**select**

~~~mysql
select * from `表`              # 查看 表数据全部内容

select `字段1`,`字段2` from `表` # 查看表的 字段1,字段2

# AS 别名（可以省略）
select `字段` AS 别名  from `表` AS 表别名

select version()   # 查询版本

select @@auto_increment_increment # 查询自增的步长

select 100*3   # 用来计算

select `字段` +1 AS '加1' from `表`

~~~





## 去重

**distinct*

~~~mysql
select distinct `字段` from 表 
~~~





## 联表查询



![联表查询](D:\typora\JAVA-MD\mysqlImages\联表查询.png)







**Inner- join**

~~~mysql
select 别名.字段1,字段2... 
from 表1 AS 别名1 
Inner join 表2 AS 别名2
on 别名1.id = 别名2.id
~~~



| 操作       | 描述                                       |
| ---------- | ------------------------------------------ |
| Inner join | 如果表中至少有一个匹配，就返回行           |
| left join  | 会从左表中返回所有的值，即使游标中没有匹配 |
| right join | 会从右表中返回所有的值，即使左表中没有匹配 |





## 自连接



自连接：**一张表拆为两张一样的表**



![自连接png](D:\typora\JAVA-MD\mysqlImages\自连接png.png)



执行

![自连接ok](D:\typora\JAVA-MD\mysqlImages\自连接ok.png)



## 分页和排序



**分页：**limit

~~~mysql
limit(查询起始下标,pagesize)
~~~





**排序：**

~~~mysql
-- 排序:   升序 ASC  , 降序 DESC 
-- order by 通过哪一个字段排序 ，怎么排
select s.`studentId`,`studentName`,`studentResult`
from student s
inner join `result` r
on s.studentId = r.studentId
inner join `subject`sub
on r.`subjectNo` = sub.`subjectNo`
where sujectName = "数据结构-1"
order by `studentResult` ASC
~~~



## 子查询和嵌套查询



**子查询：**在where语句中嵌套一个子查询语句

where(select * from 表... )

~~~mysql
select `字段` from `表` where `字段`=(select `字段` from `表` where 条件)
~~~





**嵌套查询:**

in(select * from 表)



## mysql 函数





数学运算

select :

~~~mysql
select count(*) from table where 条件 
SELECT ABS(-8)         # 绝对值
select ceiling(9.4)    # 向上去整
select floor(9.4)      # 向下取整
select rand()          # 返回一个 0~1 之间的随机数
select sign(-10)       # 判断一个数字的符号 ，负数返回-1 ，正数返回1

# 字符串函数
select char_length('string') # 返回长度
select concat('strng')       # 拼接字符串
select insert('string',1,2)  # 从某一个位置开始替换某一个长度
select lower('string')       # 转换为小写
select upper('String')       # 转换为大写
......

# 时间和日期函数
select current_date()      # 获取当前日期
select curdate()           # 获取当前日期
select now()               # 获取当前时间
select localtime           # 本地时间
select sysdate()           # 系统时间

select year(now())
select month(now())
select day(now())
select hour(now())
select second(now())


# 系统
select system_user()
select user()
select version()

~~~





## 聚合函数



| 函数名称 | 描述   |
| -------- | ------ |
| count()  | 计数   |
| sum()    | 求和   |
| avg()    | 平均值 |
| max()    | 最大值 |
| min()    | 最小值 |
| ....     | ...    |



~~~mysql
select count(`字段`) from 表  -- count(字段), 会忽略所有的null 值
select count(*) from 表  -- 不会忽略null值
select count(1) from 表  -- 会忽略null值
~~~







==分组==

group by   字段 having 条件





## 数据库MD5加密（拓展）

加密： MD５（）　

~~~mysql
update testmd5 set pwd=DM5(pwd) where id=1   # 加密pwd
~~~





## 索引

索引 ： 索引（index） 是帮助Mysql 高效获取数据的**数据结构** 。（排好序的快速查找数据结构）

 实际上 索引也是一张表，该表保存了 逐渐与索引字段，并且只想实体表的记录，所以 索引列也是占空间的。

### 索引分类

- 主键索引  PRIMARY KEY  （ 唯一标识，不可重复 ）

- 唯一索引  unique key （ 避免重复的列出现，可以重复 ）
- 常规索引 KEY  （ 默认index,key 来设置 ）
- 全文索引  fullText （ 特定数据库下才有 ，MyISAM ）





**索引的使用**

- 在创建表的时候给字段增加索引
- 创建完毕 ，增加索引



显示所有的索引信息 

~~~mysql
show index from  `表`
~~~

看一条 SQL 语句的性能，可以使用 `explain` 关键字查看语句性能，这里说一下其中的 `type` 字段的部分含义，



### 什么时候建立索引

索引优势 ：虽然索引大大提高查询速度名单同时却会降低更新表的速度，如对表进行 **增删改**

​						 因为更新表时mysql 不仅仅要保存数据 ，还要 保存一下索引文件每次更新添加索引列的字						 段。

​	

那些情况需要建立索引 ？

- 主键自动建立唯一索引
- 频繁作为查询条件的字段应该创建索引
- 查询中与其他表关联的字段，外键关系建立索引
- 单键/组合索引的选择问题，组合索引性价比高
- 查询中排序的字段，排序字段若通过索引去访问将大大提交排序速度
- 查询中统计或者 分组字段



那些情况 不需要建立索引。

- 表记录少
- 经常增删改的表或者字段
- where条件里用不到的字段不创建索引
- 过滤性不好的不适合建立索引



















































































































