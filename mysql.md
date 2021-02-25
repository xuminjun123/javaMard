# Mysql

## where条件

| 操作符                 | 含义         | 范围 | 结果  |
| ---------------------- | ------------ | ---- | ----- |
| =                      | 等于         | -    | false |
| <> 或 !=               | 不等于       |      | true  |
| >                      |              |      |       |
| <                      |              |      |       |
| <=                     |              |      |       |
| >=                     |              |      |       |
| between  ...   and ... | 在某个范围内 |      |       |
| and                    | &&           |      |       |
| or                     | \|\|         |      |       |
| like                   |              |      |       |
| in                     |              |      |       |
| is null                |              |      |       |





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






~~~





## 去重

**distinct*

~~~mysql
select distinct `字段` from 表 
~~~





## 表关联







