# Redis  入门

## 概述

> Redis 是什么 ？

Redis  ( Remote  Dictionary  Server ) ， 即 远程字典服务！

是一个开源的使用ANSI  C语言 编写，支持网络，可基于内存亦可持久化的日志型  ， Key-Value 数据库 ，并提供多种语言的API ，免费和开源 ，是当下最热门的NoSQL 技术之一 ！

也被称为 结构化数据库。



>  Redis 能干嘛 ？ 

1. 内存存储 ，持久化 ，内存中断电即失 ，所以说持久化很重要
2. 效率该，可以用于告速缓存

3. 发布订阅系统
4. 地图信息分析
5. 计时器，计数器 （浏览量  ）

.....

> 特性

1. 多样化的数据类型

2. 持久化

3. 集群

4. 事务

5.  .....

   

## 安装

`推荐`linux 安装

### 1. window 下安装

- 下载 安装包 ：https://github.com/tporadowski/redis/releases

- 下载完毕解压压缩包 
- 运行 redis-server.exe 
- 使用Redis 客户端(  redis-cli.exe ) 链接 redis 

![redis](D:\typora\JAVA-MD\Redis\redis.png)

~~~shell
redis-cli.exe -h 127.0.0.1 -p 6379 # 链接命令
~~~



### 2. Linux 安装

基本 和 window 相同 不过是 linux 上

步骤见 **"菜鸟教程"** 

https://www.runoob.com/redis/redis-install.html

默认安装路径 在 `usr/local/bin`



新建一个 文件夹 在 usr/local/bin/ 名为xconfig(随意取) ， 把redis.conf 复制到 xconfig 下 ，

修改xconfig/redis.conf  在227行 no -> yes ，这个操作主要是为 修改 新的redis.conf 而且原redis.conf 不会弄坏。

如图：

![redisConf](D:\typora\JAVA-MD\Redis\redisConf.png)



启动redis  ，配置完成。

~~~sh
回到 user/local/bin
启动命令 redis-server xconfig/redis.conf
~~~

![redis 链接成功](D:\typora\JAVA-MD\Redis\redis 链接成功.png)

出现pong表示链接成功！







测试 set,get

![keys](D:\typora\JAVA-MD\Redis\keys.png)







# 基础知识

redis 一共有16 个数据库 ，默认使用的 第 0 个

可以使用`seclect` 进行切换数据库

~~~shell
[root@xuminjun bin]#  redis-server xconfig/redis.conf
[root@xuminjun bin]# redis-cli -p 6379
127.0.0.1:6379> select 3
OK
127.0.0.1:6379[3]> 
~~~



​    `flushdb` :**清空当前数据库**

   ` FLUSHALL`: **清空所有的数据库**

   

> Redis 是单线程的



- Redis是 基于内存操作，CPU不是Redis性能瓶颈，Redis 的瓶颈是根据机器的内存和网络带宽，既然可以使用单线程来实现，就使用单线程。
- Redis 是C语言编写， 官方的数据为 100000 + QPS ，完全不必同样是使用key-vale 的Memecache差！



## 五大基本数据类型



Redis 是一个开源的（BSD许可）的，内存中的数据结构存储系统，它可以用在[**数据库**]()，[**缓存**]()和**[消息中间件]()**

> 我们使用的SpringBoot ， Jedis 所有的方法就是这些命令

### Redis-key  

~~~shell
127.0.0.1:6379> clear
127.0.0.1:6379> keys *
(empty array)
127.0.0.1:6379> set name xu
OK
127.0.0.1:6379> keys *
1) "name"
127.0.0.1:6379> set age 18
OK
127.0.0.1:6379> keys *
1) "age"
2) "name"
127.0.0.1:6379> EXISTS name   # 判断当前Key是否存在
(integer) 1
127.0.0.1:6379> exists name1
(integer) 0
127.0.0.1:6379> move name 1 移除当前key
(integer) 1
127.0.0.1:6379> keys *
1) "age"
127.0.0.1:6379> set name xuminjun
OK
127.0.0.1:6379> keys *
1) "age"
2) "name"
127.0.0.1:6379> get name
"xuminjun"
127.0.0.1:6379> expire name 10    # 设置过期时间
(integer) 1
127.0.0.1:6379> ttl name  # 查看当前key 剩余时间
(integer) 2
127.0.0.1:6379> ttl name
(integer) -2
127.0.0.1:6379> get name
(nil)
127.0.0.1:6379> type name # 查看key 类型
~~~



### String ： 字符串

~~~shell
127.0.0.1:6379> set key1 v1
OK
127.0.0.1:6379> get key1
"v1"
127.0.0.1:6379> keys *
1) "key1"
2) "age"
127.0.0.1:6379> exists keys 
(integer) 0
127.0.0.1:6379> exists key1
(integer) 1
127.0.0.1:6379> get key1
"v1"
127.0.0.1:6379> strlen key1  # 获取字符串长度
(integer) 2
127.0.0.1:6379> append key1 "hello"  # 追加字符串，如果没有就相当于set key
(integer) 7
127.0.0.1:6379> get key1

############################################################
127.0.0.1:6379> set views 0
OK
127.0.0.1:6379> get views
"0"
127.0.0.1:6379> incr views  # 自增1  浏览量变为1
(integer) 1
127.0.0.1:6379> incr views
(integer) 2
127.0.0.1:6379> decr views   #自减1 浏览量-1
(integer) 1
127.0.0.1:6379> decr views
(integer) 0
127.0.0.1:6379> get views
"0"
127.0.0.1:6379> incrby views 10  # 设置步长，指定增量
(integer) 10
127.0.0.1:6379> incrby views 10
(integer) 20
127.0.0.1:6379> decrby views 5
(integer) 15
########################################################

# 字符串范围 range

127.0.0.1:6379> set key1 "hello,kuangshen" # 设置 key1 的值
OK
127.0.0.1:6379>  getrange key1 0 3 # 截取字符串 [0 3]
"hell"
127.0.0.1:6379> getrange key1 0 -1 # 获取全部的字符串 和 get key 是一样的
"hello,kuangshen"
127.0.0.1:6379> 

##############################################

#　替换！

127.0.0.1:6379> set key2 qwert
OK
127.0.0.1:6379> get key2
"qwert"
127.0.0.1:6379> setrange key2 1 xx  # 替换指定位置的字符串
(integer) 5
127.0.0.1:6379> get key2
"qxxrt"
127.0.0.1:6379> 

###############################################

# setex( 如果当前存在)  设置过期时间 
# setnx( 如果当前不存在 )  # 不存在设置
# 分布式（锁）经常用到
127.0.0.1:6379> set key3 30 "hello" 
(error) ERR syntax error
127.0.0.1:6379> setex keys 30 "hello"
OK
127.0.0.1:6379> ttl keys
(integer) 24
127.0.0.1:6379> ttl keys
(integer) 21
127.0.0.1:6379> get keys
"hello"
127.0.0.1:6379> setnx mykey "redis"  # 如果mykeys 不存在，创建mykey
(integer) 1
127.0.0.1:6379> keys *
1) "mykey"
2) "key1"
3) "key2"
127.0.0.1:6379> ttl keys3
(integer) -2
127.0.0.1:6379>  setnx mykey "mongodb" # 如果mykeys 存在，创建失败
(integer) 0
127.0.0.1:6379>

#####################################################
# mset
127.0.0.1:6379> mset k1 v1 k2 v2 k3 v3     # 同时设置多个值
OK
127.0.0.1:6379> keys *
1) "k3"
2) "k2"
3) "k1"
127.0.0.1:6379> msetnx k1 v1 k4 v4 # msetnx 是一个院子的操作，要么一起成功，要么一起失败
(integer) 0
127.0.0.1:6379> get k4
(nil)
127.0.0.1:6379> keys *
1) "k3"
2) "k2"
3) "k1"
127.0.0.1:6379> mget k1 k2 k3  # 同时获取多个值
1) "v1"
2) "v2"
3) "v3"


# 对象 设置
set user:1 {name:zhangsan,age:3} # 设置哟个user:1 对象，职位json字符串来保存一个对象

# 使用技巧
set user:{id}:{filed} 

127.0.0.1:6379> mset user:1:name xuminjun user:1:age 2
OK
127.0.0.1:6379> mget user:1:name user:1:age
1) "xuminjun"
2) "2"
127.0.0.1:6379> 

#############################################
#getset # 先get然后set

127.0.0.1:6379> getset db redis  # 如果不存在值，则返回 nil
(nil)
127.0.0.1:6379> get db 
"redis"
127.0.0.1:6379> getset db mongodb # 如果存在值，获取原来的值 并 设置新的值
"redis"
127.0.0.1:6379> get db
"mongodb"
127.0.0.1:6379> 

~~~







### List ：列表

在Redis 里面，我们可以把list 完成 ， 栈 ，队列，阻塞队列！

~~~bash
############################################
# push 
127.0.0.1:6379> lpush list one   # 将一个值或者多个值，插入到列表头部（左  ）
(integer) 1
127.0.0.1:6379> lpush list two
(integer) 2
127.0.0.1:6379> lpush list three
(integer) 3
127.0.0.1:6379> lrange list 0 -1 # 获取list 中的值
1) "three"
2) "two"
3) "one"
127.0.0.1:6379> lrange list 0 1
1) "three"
2) "two"


127.0.0.1:6379> rpush list rightr  # 将一个值或者多个值，插入到列表头部
(integer) 4
127.0.0.1:6379> lrange list 0 -1
1) "three"
2) "two"
3) "one"
4) "rightr"
127.0.0.1:6379>

##############################################
# pop
127.0.0.1:6379> lpop list    # 移除list 的第一个元素
"three"
127.0.0.1:6379> rpop list   # 移除list 的最后一个元素
"rightr"
127.0.0.1:6379> lrange list 0 -1
1) "two"
2) "one"
127.0.0.1:6379> 

###########################################################

# lindex  获取元素下标的值
127.0.0.1:6379> lindex list 1   # 通过下标获取list的值
"one"
127.0.0.1:6379> lindex list 0
"two"

##########################################################
# Llen 获取长度 
127.0.0.1:6379> lpush list one
(integer) 1
127.0.0.1:6379> lpush list two
(integer) 2
127.0.0.1:6379> lpush list three
(integer) 3
127.0.0.1:6379> llen list  # 返回列表的长度 
(integer) 3
127.0.0.1:6379> 

#############################################################
 #lrem 移除某一个key
 
127.0.0.1:6379> lrem list 1 one  # 移除list中指定个数的value，精确匹配
(integer) 1
127.0.0.1:6379> lrange list 0 -1
1) "three"
2) "two"
127.0.0.1:6379> lrange list 0 -1
1) "three"
2) "two"
127.0.0.1:6379> lpush list three
(integer) 3
127.0.0.1:6379> lrem list 2 three
(integer) 2
127.0.0.1:6379> lrange list 0 -1
1) "two"
127.0.0.1:6379> 

#######################################################
# ltrim 修剪
127.0.0.1:6379> rpush mylist "hello"
(integer) 1
127.0.0.1:6379> rpush mylist "hello1"
(integer) 2
127.0.0.1:6379> rpush mylist "hello2"
(integer) 3
127.0.0.1:6379> rpush mylist "hello3"
(integer) 4
127.0.0.1:6379> ltrim mylist 1 2   # 通过下标截取指定的长度
OK
127.0.0.1:6379> lrange mylist 0 -1 
1) "hello1"
2) "hello2"

####################################################
# rpoplpush  移除列表的最后一个元素 ， 将她移动到新的列表中

127.0.0.1:6379> rpush mylist "hello"
(integer) 1
127.0.0.1:6379> rpush mylist "hello1"
(integer) 2
127.0.0.1:6379> rpush mylist "hello2"
(integer) 3
127.0.0.1:6379> rpoplpush mylist myotherlist #移除列表的最后一个元素 ， 将她移动到新的列表中
"hello2"
127.0.0.1:6379> lrange mylist 0 -1 # 查看原来的列表
1) "hello"
2) "hello1"
127.0.0.1:6379> lrange myotherlist 0 -1
1) "hello2"
127.0.0.1:6379> 

 #########################################################################
# lset 将列表中指定下标的值替换为另外一个值  ，更新操作

 127.0.0.1:6379> exists list         #　判断这个列表是否存在
(integer) 0
127.0.0.1:6379> lset list 0 item　　　# 如果不存在列表，更新就会报错
(error) ERR no such key
127.0.0.1:6379> lpush list value1
(integer) 1
127.0.0.1:6379> lrange list 0 0
1) "value1"
127.0.0.1:6379> lset list 0 item # 如果存在，就会更新当前下标的值
OK
127.0.0.1:6379> lrange list 0 0
1) "item"
lset list 1 other
(error) ERR index out of range # 如果不存在，则会报错
127.0.0.1:6379> 

 ######################################################################
 
 # linsert  将某一具体的value插入某个原色前面或者后面
127.0.0.1:6379> rpush mylist "hello"
(integer) 1
127.0.0.1:6379> rpush mylist "world"
(integer) 2
127.0.0.1:6379> linsert mylist before "world" "other" # 插在world前面
(integer) 3
127.0.0.1:6379> lrange mylist 0 -1
1) "hello"
2) "other"
3) "world"
127.0.0.1:6379> linsert mylist after world new  # 插在new后面
(integer) 4
127.0.0.1:6379> lrange mylist 0 -1
1) "hello"
2) "other"
3) "world"
4) "new"
127.0.0.1:6379> 
 
~~~



> 小结 

- 实际上是一个 链表， `before`,  `after` ,`left`,`right` 都可以插入值

- 如果key 存在，创建新的链表
- 如果key 存在，新增内容
- 如果移除了所有值，空链表，也不代表不存在
- 在两边插入或者改动值，效率最高！中间元素，相对较低

消息队列！ （Lpush Rpop） 栈（Lpush Lpop）























 

















### set ：集合

set中的值是不能重复的

~~~bash
127.0.0.1:6379> sadd myset "hello"   # set集合中 添加元素
(integer) 1
127.0.0.1:6379> sadd myset "kuangshen"
(integer) 1
127.0.0.1:6379> sadd myset "xuminjun"
(integer) 1
127.0.0.1:6379> smembers myset # 查看指定set的所有值
1) "xuminjun"
2) "kuangshen"
3) "hello"
127.0.0.1:6379> sismember myset hello # 查看指定的所有值
(integer) 1
127.0.0.1:6379> sismember myset world
(integer) 0
127.0.0.1:6379> 
###########################################################################

# 获取set值个数
127.0.0.1:6379> scard myset # 获取set的值个数
(integer) 3

############################################################################

# 移除set 中某一个元素
# srem 
127.0.0.1:6379> srem myset hello # srem 移除某一个元素
(integer) 1
127.0.0.1:6379> scard myset
(integer) 2
127.0.0.1:6379> smembers myset
1) "xuminjun"
2) "kuangshen"

######################################################################

#set 随机抽取
127.0.0.1:6379> srandmember myset # 随机抽取一个元素
"kuangshen"
127.0.0.1:6379> srandmember myset
"xuminjun"
127.0.0.1:6379> srandmember myset
"kuangshen"
127.0.0.1:6379> srandmember myset 2 # 随机抽选出指定个数的元素
1) "xuminjun"
2) "kuangshen"
127.0.0.1:6379> 

##################################################
# spop ,随机删除指定key

127.0.0.1:6379> spop myset # 随机删除一个指定元素
"kuangshen"
127.0.0.1:6379> smembers myset
1) "nan"
2) "xuminjun"

################################################################

#将一个指定的值，移动到另外一个set集合中
# smove
127.0.0.1:6379> sadd myset "hello"
(integer) 1
127.0.0.1:6379> sadd myset "hworld"
(integer) 1
127.0.0.1:6379> sadd myset "kuangshen"
(integer) 1
127.0.0.1:6379> sadd myset2 "set2"
(integer) 1
127.0.0.1:6379> smove myset myset2 "kuangshen" # smove 把set1 的key1 移到 set2中
(integer) 1
127.0.0.1:6379> smen=mber myset
(error) ERR unknown command `smen=mber`, with args beginning with: `myset`, 
127.0.0.1:6379> smembers myset
1) "hworld"
2) "hello"
127.0.0.1:6379> smembers myset2
1) "kuangshen"
2) "set2"
127.0.0.1:6379> 

##########################################################################
# 微博，B站，共同关注（并集）
# 数字集合类 ：
- 差集
- 交集
- 并集
127.0.0.1:6379> sadd key1 a
(integer) 1
127.0.0.1:6379> sadd key1 b
(integer) 1
127.0.0.1:6379> sadd key1 c
(integer) 1
127.0.0.1:6379> sadd key2 c
(integer) 1
127.0.0.1:6379> sadd key2 d
(integer) 1
127.0.0.1:6379> sadd key2 e
(integer) 1
127.0.0.1:6379> sdiff key1 key2  # 差集
1) "a"
2) "b"
127.0.0.1:6379> sinter key1 key2 # 交集
1) "c"
127.0.0.1:6379> sunion key1 key2 # 并集
1) "e"
2) "b"
3) "c"
4) "d"
5) "a"

~~~

案例 ： 微博A用户将所有关注的人放在set集合中 ！将他的粉丝也放在一个集合中 ！ 共同关注。











### Hash ：哈希

map 集合，key-map 时候这个值是一个map集合

~~~bash
127.0.0.1:6379> hset myhash field1 kuangshen # set 一个具体的key-value
(integer) 1
127.0.0.1:6379> hget myhash field1 # 获取一个字段值
"kuangshen"
127.0.0.1:6379> hmset myhash field1 hello field2 world # set多个可以-value
OK
127.0.0.1:6379> hmget myhash field1 field2 # 获取多个字段 
1) "hello"
2) "world"
127.0.0.1:6379> hgetall myhash # 获取全部值
1) "field1"
2) "hello"
3) "field2"
4) "world"
127.0.0.1:6379> 
#######################################################################################

# 删除
127.0.0.1:6379> hdel myhash field1 # 删除hash指定的key字段
(integer) 1
127.0.0.1:6379> hgetall myhash
1) "field2"
2) "world"
127.0.0.1:6379> 

#########################################################################################

# 获取长度 hlen
127.0.0.1:6379> hlen myhash # 获取 key的长度
(integer) 1

##########################################

# 判断hash中的指定字段是否存在
127.0.0.1:6379>  hexists myhash field3
(integer) 0
127.0.0.1:6379> 

####################################################################

# 获取hash中的key和values
127.0.0.1:6379> hkeys myhash  # 获得hash指定的keys
1) "field2"
127.0.0.1:6379> hvals myhash  # 获得hash指定 values
1) "world"
127.0.0.1:6379> 

##################################################################

# incr 指定自增  desc自减 
127.0.0.1:6379> hset myhash field3 5  # 指定增量
(integer) 1
127.0.0.1:6379> hincrby myhash field3 1
(integer) 6
127.0.0.1:6379> hincrby myhash field3 1
(integer) 7
127.0.0.1:6379> hsetnx myhash field4 hello # 如果不存在则可以设置
(integer) 1
127.0.0.1:6379> hsetnx myhash field4 world # 如果存在值则不能设置
(integer) 0
127.0.0.1:6379> 

hash 变更的数据user name age ,尤其是用户信息之类的，经常变动的信息，hash更适用于对象 
的存储，String更适用于字符串存储




~~~





















### Zset ：有序集合

把set 排序

~~~bash
127.0.0.1:6379> zadd myset 1 one  # 添加一个只
(integer) 1
127.0.0.1:6379> zadd myset 2 two 3 three  # 添加多个值 
(integer) 2
127.0.0.1:6379> zrange myset 0 -1
1) "one"
2) "two"
3) "three"
127.0.0.1:6379> 

################################################################
# 实现排序
127.0.0.1:6379> zadd salary 100 xuminjun
(integer) 1
127.0.0.1:6379> zadd salary 200 xuminjun2
(integer) 1
127.0.0.1:6379> zadd salary 330 xuminjun3
(integer) 1
127.0.0.1:6379> zrangebyscore salary -inf +inf #从小到大排序
1) "xuminjun"
2) "xuminjun2"
3) "xuminjun3"
127.0.0.1:6379> zrangebyscore salary +inf -inf
(empty array)
127.0.0.1:6379> zrangebyscore salary -inf +inf withscores #  显示全部的 并且升序排序
1) "xuminjun"
2) "100"
3) "xuminjun2"
4) "200"
5) "xuminjun3"
6) "330"
127.0.0.1:6379> zrangebyscore salary -inf 200  withscores # 到一定区间排序
1) "xuminjun"
2) "100"
3) "xuminjun2"
4) "200"

127.0.0.1:6379> zrevrange salary 0 -1  # 降序排列
1) "xuminjun3"
2) "xuminjun2"
127.0.0.1:6379> 


################################################################
# 移除某个元素
127.0.0.1:6379> zrange salary 0 -1
1) "xuminjun"
2) "xuminjun2"
3) "xuminjun3"
127.0.0.1:6379> zrem salary xuminjun # 移除有序元素某一个元素
(integer) 1
127.0.0.1:6379> zrange salary 0 -1
1) "xuminjun2"
2) "xuminjun3"
127.0.0.1:6379> 
127.0.0.1:6379> zcard salary  # 获取有序集合的个数
(integer) 2

#############################################################
127.0.0.1:6379> zadd myset 1 hello
(integer) 1
127.0.0.1:6379> zadd myset 2 world
(integer) 1
127.0.0.1:6379> zadd myset 3 kuangshen
(integer) 1
127.0.0.1:6379> zcount myset 1 3 # 获取指定区间的成员数量
(integer) 3
127.0.0.1:6379> zcount myset 1 2
(integer) 2







~~~















## 三种特殊数据类型

### geospatial ： 地理位置

这个功能可以推算地理位置的信息，两地之间的距离，房源几公里的人！

朋友的定位 ，附近的人 ，打车距离计算 都会是用到 geospatial

只有 6个 命令，底层其实就是zset 

~~~bash
# 添加地理位置 geoadd
# 规则，量级无法直接添加，我们一般会下载城市数据，直接通过java导入
# key (纬度，经度，名称)
127.0.0.1:6379> GEOADD china:city 116.40 39.90 beijing
(integer) 1
127.0.0.1:6379> GEOADD china:city 121.47 31.23 shanghai
(integer) 1
127.0.0.1:6379> GEOADD china:city 106.50 29.53 chongqing
(integer) 1
127.0.0.1:6379> GEOADD china:city 114.04 22.52 shenzhen
(integer) 1
127.0.0.1:6379> GEOADD china:city 120.16 30.24 hangzhou
(integer) 1
127.0.0.1:6379> GEOADD china:city 108.96 34.26 xian
(integer) 1
127.0.0.1:6379>

###################################################
# geopos 获取指定的城市的经度和纬度

127.0.0.1:6379> geopos china:city beijing
1) 1) "116.39999896287918091"
   2) "39.90000009167092543"
127.0.0.1:6379> geopos china:city chongqing
1) 1) "106.49999767541885376"
   2) "29.52999957900659211"
127.0.0.1:6379> 

######################################################################
#geodsi 返回连个距离之间的距离
# 单位 ；m，km, mi(英里),ft(英尺)
127.0.0.1:6379> GEODIST china:city beijing shanghai # 查看直线距离
"1067378.7564"
127.0.0.1:6379> GEODIST china:city beijing shanghai km # 带单位
"1067.3788"
127.0.0.1:6379> 

###################################################

# 查看附近的人(获得附近的人地址，定位)通过班级来查询
# georadius
127.0.0.1:6379> GEORADIUS china:city 110 30 1000 km
1) "xian"
2) "shenzhen"
3) "hangzhou"
127.0.0.1:6379> GEORADIUS china:city 110 30 1000 km withdist # 经度纬度查询 
1) 1) "xian"
   2) "483.8340"
2) 1) "shenzhen"
   2) "924.2066"
3) 1) "hangzhou"
   2) "977.5143"
127.0.0.1:6379> GEORADIUS china:city 110 30 1000 km withcoord
1) 1) "xian"
   2) 1) "108.96000176668167114"
      2) "34.25999964418929977"
2) 1) "shenzhen"
   2) 1) "114.03999835252761841"
      2) "22.5200000879503861"
3) 1) "hangzhou"
   2) 1) "120.1600000262260437"
      2) "30.2400003229490224"
127.0.0.1:6379> 

###############################################
# 获得指定数量的人
127.0.0.1:6379> GEORADIUS china:city 110 30 1000 km withcoord count 1 # 限定 1个
1) 1) "xian"
   2) 1) "108.96000176668167114"
      2) "34.25999964418929977"

############################################
# GEORADIUSBYMEMBER 找出位于指定元素周围的其他元素
127.0.0.1:6379> GEORADIUSBYMEMBER china:city beijing 1000 km 
1) "beijing"
2) "xian"

##########################################
# geohash 命令 返回一个或者多个位置元素的geohash
# 将二维的经纬度转换为一维的字符串
# 两个字符串越接近，距离越短
127.0.0.1:6379> geohash china:city beijing shanghai
1) "wx4fbxxfke0"
2) (nil)
127.0.0.1:6379>  

############################################################
#移除某个地理位置
127.0.0.1:6379> zrange china:city 0 -1
1) "xian"
2) "shenzhen"
3) "hangzhou"
4) "beijing"
127.0.0.1:6379> zrem china:city beijing # 使用底层zset的zrem移除 key
(integer) 1
127.0.0.1:6379> zrange china:city 0 -1
1) "xian"
2) "shenzhen"
3) "hangzhou"

~~~



























### Hyperloglog基数统计

> 什么是基数？ -- 不重复的元素（如果数据特别大，可以接受误差）

简介： 

- Redis 2.8.9 推出 Hyperloglog数据结构
- Redis Hyperloglog 基数统计算法
- 0.81%错误率，统计UV任务，可以忽略不计

网页的UV（一个人访问一个网站多次，但还是算作一个人！）

优点 ：

- 占用的内存固定。2^64 不同的元素的技术，只需要12kb！ 如果从内存角度来比较的话Hyperloglog首选

~~~bash
127.0.0.1:6379> pfadd myset a b c d e f g h i # 创建第一组 key
(integer) 1
127.0.0.1:6379> pfadd myset2 i o k j h a s d f
(integer) 1
127.0.0.1:6379> pfcount myset2 # t统计key 元素数量
(integer) 9
127.0.0.1:6379> pfmerge myset3 myset myset2 # 把 key ，key2 => key3 (并集)
OK
127.0.0.1:6379> pfcount myset3 
(integer) 13
127.0.0.1:6379>
~~~

  【 注 】：

- 如果允許容错，name一定可是用Hyperloglog
- 如果不允许容错，就使用set自己的数据类型即可



### bitmaps 位图

> 位存储

登录/未登录 ; 活跃/不活跃 ；打卡/未打卡； 两个状态的都可以使用 Bitmaps !

Bitmaps位图，数据结构，都是操作二恶禁止来记录，就只有 0 ，1两个状态！



使用bitmap 来记录周一到周日的打卡！

~~~bash
##########################################################
#setbit
127.0.0.1:6379> setbit sign 0 1
(integer) 0
127.0.0.1:6379> setbit sign 1 0
(integer) 0
127.0.0.1:6379> setbit sign 2 1
(integer) 0
127.0.0.1:6379> setbit sign 3 0
(integer) 0
127.0.0.1:6379> setbit sign 4 1
(integer) 0
127.0.0.1:6379> setbit sign 5 0
(integer) 0
127.0.0.1:6379> setbit sign 6 1
(integer) 0
127.0.0.1:6379>
#######################################################
# getbit
127.0.0.1:6379> getbit sign 1
(integer) 0
127.0.0.1:6379> getbit sign 2
(integer) 1
127.0.0.1:6379> getbit sign 3
(integer) 0
127.0.0.1:6379>

########################################################

## 统计bitcount 为1 的数量
127.0.0.1:6379>bitcount sign 
(integer) 4
~~~





## Redis事务操作

> Redis 事务本质 ： 一组命令集合

Redis单条命令式保证原子性，但事务不保证原子性！

Redis事务，没有隔离级别的概念！

所有的命令在事务中，并没有直接执行！ 只有发起执行命令的时候才会执行！EXEC

Redis的事务 ：

- 开启事务（  multi  ）
- 命令入队（   ）

~~~bash
127.0.0.1:6379> multi # 开始事务
OK
127.0.0.1:6379> set k1 v1    
QUEUED
127.0.0.1:6379> set k2 v2 
QUEUED
127.0.0.1:6379> get k1   
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED 
127.0.0.1:6379> exec  # 执行事务
1) OK
2) OK
3) "v1"
4) OK
127.0.0.1:6379>
~~~



- 执行事务（ exec   ）

锁 ： 可以实现乐观锁



#**放弃事务**

~~~bash
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> set k4 v4
QUEUED
127.0.0.1:6379> discard # 取消事务
OK
127.0.0.1:6379> get k4 
(nil)
127.0.0.1:6379>
~~~



#**编译异常**（代码有问题，命令有错，）事务中所有命令都不会执行

~~~bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED
127.0.0.1:6379> getset k3  # 语法错误 没有getset命令
(error) ERR wrong number of arguments for 'getset' command
127.0.0.1:6379> set k4 v4
QUEUED
127.0.0.1:6379> get k4
QUEUED
127.0.0.1:6379> exec
(error) EXECABORT Transaction discarded because of previous errors.
127.0.0.1:6379> get k4
(nil)
127.0.0.1:6379>																		
~~~





#**运行异常**（ 如1除以0 ），如果事务队列中存在语法性 ，name执行命令的时候，其他命令是可以正常执行的，错误命令抛出异常

~~~bash
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> incr k1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED
127.0.0.1:6379> exec
1) OK
2) (error) ERR value is not an integer or out of range # 运行异常 仍会执行其他正确的操作
3) OK
4) OK
127.0.0.1:6379>
~~~



## Redis乐观锁

> 监控 ! watch-- 使用watch 可以充当乐观锁操作

**悲观锁**：

- 很悲观： 认为生命时候都会出问题，无论做什么都会加锁

**乐观锁**：

- 很乐观 ：认为生命时候都不会出问题，所以不会上锁，更新数据的时候去判断一下，在此期间是否有人修改这个数据 version!

更新的时候比较servion 

Redis测试监视 。



正常执行

~~~bash
127.0.0.1:6379> set money 100
OK
127.0.0.1:6379> set out 0
OK
127.0.0.1:6379> watch money  # 监视
OK
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379> decrby money 20
QUEUED
127.0.0.1:6379> incrby out 20
QUEUED
127.0.0.1:6379> exec
1) (integer) 80
2) (integer) 20
~~~



如果有另一个线程 也修改我们的值，这个时候就会导致 事务 执行失败

~~~bash

127.0.0.1:6379> watch money  # 监视
OK
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379> decrby money 10
QUEUED
127.0.0.1:6379> incrby out 10
QUEUED
127.0.0.1:6379> exec
1) (nil) 
~~~

解决办法 （unwatch）

~~~bash
127.0.0.1:6379> watch money  # 监视
OK
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379> decrby money 10
QUEUED
127.0.0.1:6379> incrby out 10
QUEUED
127.0.0.1:6379> exec
1) (nil) 

# 如果发现 事务执行失败 ，先解锁
127.0.0.1:6379> unwatch
OK
127.0.0.1:6379> watch money # 获取最新的一个值
OK
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379> decrby money 10
QUEUED
127.0.0.1:6379> incrby out 10
QUEUED
127.0.0.1:6379> exec # 会比对监视的值 是否发生变化，如果没有变化，就会执行成功
~~~







## 通过Jedis 操作事务



我们使用Java来操作Redis

> 什么是Jedis 是Redis官方推荐的Java连接工具，使用Java操作redis 中间件！如果你要使用Java操作redis,name一定对redis什么熟悉

1. 导入对应依赖

~~~xml
   <dependencies>
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>3.2.0</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.74</version>
        </dependency>
    </dependencies>
~~~



2. 编码测试：
   - 连接收据库
   - 操作命令
   - 断开

~~~java
  public static void main(String[] args) {
        // 1. 连接 打开 redis
        Jedis jedis = new Jedis();
        // 使用
        System.out.println(jedis.getClient().getPort());
        System.out.println("连接本地的Redis服务器成功");
        //查看服务是否运行
        System.out.println("服务正在运行：" + jedis.ping());    // pong ，则成功
   } 
~~~



## spring boot 整合redis

spring-Boot操作数据 ： spring-data ； jpa  ；jbdc ；mongodb ; redis

springData 也是和 springboot齐名的项目

说明： 在SpringBoot2.x之后，原来使用的jedis被替换为了lettuce 

jedis ：采用的是直连，多个线程操作，是不安全的。如果想要避免不安全使用jedis pool 连接池！ BIO

lettuce : 采用netty,实例可以在多个线程中进行共享，不存在线程不安全的情况！，可以减少线程数据了，更像NIO模式。



1. 导入依赖

~~~xml
		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
~~~



2. 配置连接

~~~yml
# 配置redis (默认localhost)
spring.redis.host=127.0.0.1
spring.redis.port=6379
~~~

3. 测试

~~~java

@SpringBootTest
class Redis02SpringbootApplicationTests {
    @Autowired
    private RedisTemplate redisTemplate;

    @Test
    void contextLoads() {
        // redisTemplate 可以操作不同的数据类型
        // opsForValue 操作string
        // opsForList  操作list
        // .... 数据类型

//        除了进本的操作，常用的方法都可以直接通过redisTemplate操作，比如:事务
//        redisTemplate.opForList()

        // 获取链接
//        RedisConnection redisConnection  = (RedisConnection) redisTemplate.getConnectionFactory();
//        redisConnection.flushDb();
//        redisConnection.flushAll();

        RedisConnection connection = redisTemplate.getConnectionFactory().getConnection();
        connection.flushAll();
        connection.flushDb();
    }

}
~~~



## 自定义redisTemplate

~~~java
package com.kuang.config;

import com.fasterxml.jackson.annotation.JsonAutoDetect;
import com.fasterxml.jackson.annotation.PropertyAccessor;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.jsontype.impl.LaissezFaireSubTypeValidator;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

import java.net.UnknownHostException;

@Configuration
public class RedisConfig {

    //    编写自己的redisTemplate
    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory)
            throws UnknownHostException {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(factory);
        //  Json序列化配置
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper om = new ObjectMapper();
        // 指定要序列化的域，field,get和set,以及修饰符范围，ANY是都有,包括private和public
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        // 指定序列化输入的类型，类必须是非final修饰的，final修饰的类，比如String,Integer等会抛出异常
        //om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);  //@Deprecated 已过时
        om.activateDefaultTyping(LaissezFaireSubTypeValidator.instance, ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);

        //配置具体的序列化方式
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();

        //key采用String的序列化方式
        template.setKeySerializer(stringRedisSerializer);

        //hash的key也采用String的序列化方式
        template.setHashKeySerializer(stringRedisSerializer);

        //value序列化采用jackson
        template.setValueSerializer(jackson2JsonRedisSerializer);
        template.setHashValueSerializer(jackson2JsonRedisSerializer);
        template.afterPropertiesSet();

        return template;
    }
}
~~~



编写RedisUtil 

https://blog.csdn.net/stonezry/article/details/106076303



测试 ：

~~~java
    @Test
    public void test1(){
        redisUtil.set("name","xuminjun");
        System.out.println(redisUtil.get("name"));
    }
~~~





## Redis的 配置文件

从上至下开始 ：



redis单位 ：

![单位](D:\typora\JAVA-MD\Redis\单位.png)



Redis对大小写不敏感

![对大小写不敏感](D:\typora\JAVA-MD\Redis\对大小写不敏感.png)



Redis可以多个conf组合在一起

![conf9](D:\typora\JAVA-MD\Redis\conf9.png)



Redis网络

~~~bash
bind 127.0.0.1  # 绑定IP

protected-mode yes # 保护模式

port 6379 # 端口
~~~



**通用配置**

~~~bash
daemonize yes # 以守护进程方式，默认是no,需要自己手动开启

pidfile /var/run/redis_6379.pid # 如果以后台方式运行，我们需要指定一个pidw文件

# 日志
# Specify the server verbosity level.
# This can be one of:
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably)  # 生产环境
# warning (only very important / critical messages are logged)
loglevel notice、

logfile ""  # 日志文件位置名

databases 16 # 数据库数量 默认16个

always-show-logo yes # 是否显示 logo
~~~



**快照** SNAPSHOTTING （持久化会用到）

持久化 : 在规定时间内，执行多少次操作，则会持久化到文件  .rdb  .aof

redis是内存数据库，如果没有持久化，那么数据断电及失

~~~bash
save 900 1     # 900s内，至少有一个 key进行了修改，我们立即进行持久化
save 300 10    # 300s内，如果至少10个 key进行了修改，我们立即持久化操作
save 60 10000  # 60s内，如果10000 key进行了修改，我们及进行持久化操作

stop-writes-on-bgsave-error yes # 持久化错误之后，是否进行工作

rdbcompression yes # 是否压缩rdb文件，需要消耗一下儿cpu资源

rdbchecksum yes # 报错rdb文件时候 进行错误校验

dir ./ # rdb文件保存目录
~~~



**复制 ** REPLICATION

   见 《主从复制》



**安全** SECURITY   

~~~bash
# requirepass foobared   790行 
requirepass 123456  # 设置密码 
~~~

一般使用 命令 设置密码

![密码](D:\typora\JAVA-MD\Redis\密码.png)



**客户端限制　ＣＬＩＥＮＴＳ**

大多不会使用到。

~~~bash
# maxclients 10000   # 834行，限制redis最大客户端数量

# maxmemory <bytes>  # 862 行，redis 配置最大内存容量

# maxmemory-policy noeviction # 892 行 # 内存达到上限之后的处理策略
                       			       # 移除一些过期的 key
                       			     
~~~



APPEND ONLY 模式 。aof 配置

~~~   bash
appendonly no # 默认不开启 aof 模式，认识使用rdb方式持久化

appendfilename "appendonly.aof" # 持久化文件的名字

# appendfsync always  # 每次修改 都会sync，消耗性能
appendfsync everysec  # 每秒执行一次 可能会丢失 l秒的数据
# appendfsync no      # 不执行sync,这个时候操作系统自己同步数据
~~~



## 持久化RDB操作:frowning:

持久化是重点！ 一般不需要修改

RDB性能比ＡＯＦ高

Redis是内存数据库，如果不讲内存中的数据库状态保存到磁盘，那么一旦服务器进程退出，服务器中的数据库状态也会消失，所以Redsi提供的持久化功能。

缺点:最后uic持久化后的数据可能丢失。我们默认就是RDB，一般情况下不需要修改这个配置！

rdb保存的文件是 dump.rdb

~~~bash
# The filename where to dump the DB
dbfilename dump.rdb 
~~~



## Redis发布订阅

Redis发布订阅（pub/sub）是一种消息通信模式；发送者（pub) 发送消息，订阅者（sub）接受消息。

Redis 客户段，可以订阅任意数量的频道。



订阅/发布 消息图 ：

![消息订阅](D:\typora\JAVA-MD\Redis\消息订阅.png)



 测试

订阅端 ： SUBSCRIBE

~~~bash
127.0.0.1:6379> SUBSCRIBE kuangshen
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "kuangshen"
3) (integer) 1   
1) "message"     # 消息
2) "kuangshen"   # 消息的频道
3) "hello,world"  # 消息的内容 接受发送端的信息为“hello，world”
~~~

发送端 ： PUBLISH

~~~bash
127.0.0.1:6379> PUBLISH kuangshen "hello,world" # 发送完 订阅段会接受到 “hello,wordl”
(integer) 1
~~~



使用场景 ：

1. 实时消息系统
2. 事实聊天！（频道当作聊天室，将消息回显给所有人）
3. 订阅，关注系统

稍微复杂的场景我们就会使用消息中间件 MQ







##  Redis主从复制:warning:



> 主从复制 ：j将一台服务器的数据，复制到其他的redis服务器。前者称为柱节点，后者称为从节点。 数据复制是单向的，只能由主节点到从节点。**Master**以写为主，**Slave**以读为主



一般来说，Redis 只用一台Redis 只用一台Redis 是不可能的（宕机），原因如下

1. 从结构上，单个redis 服务器会发生单点故障，并且 一台服务器需要处理所有的请求负载 ，压力较大。
2. 从容量上，单个Redis内存有限，就算一台Redis服务器内存容量为256 G，也不能将所有内存用作Redis存储内存，

电商网站上的商品，一般都是一次上传，无数次浏览，专业就是 “多读少写”

![集群](D:\typora\JAVA-MD\Redis\集群.png)



主从复制 ，读写分离！ 80%情况下都是进行读操作，减缓服务器压力。架构中经常使用，一主二从。

只从复制，**必须**要使用的。



### 1. 环境配置

~~~bash
# 查看当前 库 的信息
127.0.0.1:6379> info replication  # info  查看
# Replication
role:master          # 默认master            
connected_slaves:0
master_replid:fd9d40af014acaaa11cb51dc1c6fdb67a0313861
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
127.0.0.1:6379> 
~~~



开启多个 Redis ：如图 ：

第一个为 master主机  ,  第二/三个是从机

![](D:\typora\JAVA-MD\Redis\主从复制1.png)

主机 ，进入 redis.conf ，复制多个 redis.conf，修改 redis79、redis80、redeis81 的conf。

1. 首先先编辑redis79.conf  `vim redis79.conf`

~~~bash
logfile "6379.log"  # 260 行 修改 日志名称

dbfilename dump6379.rdb # 342 行 ，
~~~

修改redis80.conf 

~~~bash
port 6380 # 91行  修改端口

pidfile /var/run/redis_6380.pid # 247行 修改pid

logfile "6380.log" # 

dbfilename dump6380.rdb # 
~~~

修改 redis81.conf

~~~bash
port 6381 # 91行  修改端口

pidfile /var/run/redis_6381.pid # 247行 修改pid

logfile "6381.log" # 

dbfilename dump6381.rdb # 
~~~



修改完成 。重新启动 redis

![开启redis](D:\typora\JAVA-MD\Redis\开启redis.png)

环境搭建完成！ 启动 3个redis的服务，





### 2. 主从复制原理

环境搭建完成，此时 3个redis服务都是 master,可以输入`info replication` 命令

![repliaction](D:\typora\JAVA-MD\Redis\repliaction.png)



其余也是master。默认情况下3台都是主机。

配置主从复制。

一般情况下，只配置从机， 一主机（6379）。二从机（6380，6381）



在从机 6380上配置。6381上一样

~~~bash
127.0.0.1:6380> SLAVEOF 127.0.0. 1 6379  # slaveof  主机  端口 (认6379为master)
OK
127.0.0.1:6380> info replication
# Replication
role:slave # 此时6380 端口为从机
master_host:127.0.0.1  # 主机host
master_port:6379       # 主机端口
master_link_status:up
master_last_io_seconds_ago:1
master_sync_in_progress:0
slave_repl_offset:14
slave_priority:100
slave_read_only:1
connected_slaves:0
master_replid:bb2964677b2d1ef3d5342f6ee1a5358654e13897
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:14
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:14
127.0.0.1:6380> /usr/local/bin/xconfig

~~~



**此时在主机（6379端口）上可以看到2个从机****

![redis-2](D:\typora\JAVA-MD\Redis\redis-2.png)



一主二从搭建完成！

这是在命令中 配置的  是暂时的 ，真实情况应该在配置文件中配置，这样是永久的。



> 细节

- 主机可以写，丛集不能写，只能读。主机中所有的信息和数据，都会被从机保存

- 主机shoutdown后，没有配置哨兵模式下，从机不会自动变为主机







##　配置哨兵模式

> 概述

主从切换技术的方法 ：当主服务器宕机后，需要手动把一台服务器切花为主服务器，这就需要人工干预，费时费力，还会造成一段时间内服务不可用。 这不是一种推荐方式，更多时候，需要优先考虑**哨兵模式**。

哨兵模式 ：是一种的特殊的模式，首先Redis提供了哨兵的命令。哨兵是一个独立的进程，作为一个独立的进程，他会独立运行，其原理是哨兵通过发送命令，等待Redis服务器响应，从而监控运行多个redis实例。（   简单来说就是，自动将从机变为主机 ）

![哨兵](D:\typora\JAVA-MD\Redis\哨兵.png)

万一这种情况下，哨兵shoutdown了,也没有办法监控了，所以至少需要3个哨兵

便形成这种情况：

![哨兵3](D:\typora\JAVA-MD\Redis\哨兵3.png)

3个哨兵监控。

![哨兵子](D:\typora\JAVA-MD\Redis\哨兵子.png)

> 测试 

目前状态1主2从；

在xconfig下新建一个

~~~bash
vim sentinel.conf
~~~

~~~bash
#  sentinel monitor 被监控的名称 host port 1
sentinel monitor myredis 127.0.0.1 6379 1   # 1 表示主机down了，那就可以让从机投票，票数多的就会成为 主机
~~~

保存，目录结果如图

![目录](D:\typora\JAVA-MD\Redis\目录.png)

- 启动哨兵 redis-sentinel 文件

~~~bash
 redis-sentinel xconfig/sentinel.conf
~~~



![哨兵mode](D:\typora\JAVA-MD\Redis\哨兵mode.png)



完成哨兵模式，这只是1个哨兵。真实情况下应该至少3个哨兵









## 缓存穿透和雪崩



Redis缓存的使用 ，极大提成了应用程序的性能和效率，特别是数据查询方面。但同时，呀也带来一些问题，其中最要还得问题，也就是数据一致性问题，严格意义上，此问题无解，如果对数据一致性要求很高，那么就不能使用缓存。

另外的一些典型的问题减少，缓存穿透，缓存雪崩和缓存击穿。



**一. 缓存穿透**（查不到）

- 概念 ：用户想要查询一个数据，发现Redis内存数据没有，也就是缓存没有命中，于是象持久层数据库查询，发现也没有，余数本次查询失败。 当用户没有命中，于是都去请求了持久层的数据库。这会给持久层造成很大的压力，就时候出现了缓存穿透。

- 解决方案：

1. 布隆过滤器：布隆过滤器是一种数据结构。对所有可能查询的参数以hash形式存储，在控制层先进行校验，不符合则丢弃，从而避免对底层存储的循环压力。

![布隆](D:\typora\JAVA-MD\Redis\布隆.png)



- 缓存空对象：当存储层不命中后，即使返回的空对象也将其缓存起来，通知会设置一个过期时间，之后在访问这个数据将会从缓存中获取，保护了后端数据源

![缓存空对象2](D:\typora\JAVA-MD\Redis\缓存空对象2.png)









**二. 缓存击穿（量太大，缓存过期）**



缓存击穿 是指 一个key不停的扛着大并发，大并发集中在对这一个key进行访问，当这个key失效的瞬间，持续的大并发就会穿破缓存，直接请求数据库，就像在一个屏障上凿开一个洞



解决方案 :

- **设置热点数据永不过期**，不设置过期时间，所以不会出现热点key过期后产生的问题
- **加互斥锁** 分布式锁 ： 使用分布式锁，保证对于诶个key同时只有一个线程去查询后端服务，其他线程没有获得分布式锁的权限，因此只需要等待即可，这种方式高并发的压力转移到了分布式锁，因此对分布式锁考验很大。



**三 . 缓存雪崩**

![雪崩](D:\typora\JAVA-MD\Redis\雪崩.png)

![雪崩36](D:\typora\JAVA-MD\Redis\雪崩36.png)





















































































 





































































































































































