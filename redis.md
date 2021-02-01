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

出现表示链接成功！







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





















### set ：集合















### Hash ：哈希

















### Zset ：有序集合







## 三种特殊数据类型

### geospatial ： 地理位置



### hyperloglog ： 基数统计



### bitmaps ：位图场景



























































































