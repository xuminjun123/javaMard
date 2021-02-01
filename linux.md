# linux

## 安装

1. 购买服务器（  或者安装虚拟机 ）

2. 安装Xshell 和Xftp 远程连接linx系统

`用户名` : root

`密码`: xuminjun@123

`主机名`: 101.132.155.62



成功如图：

![linux](D:\typora\JAVA-MD\Linux\linux.png)



xptp : 使用

![xftp](D:\typora\JAVA-MD\Linux\xftp.png)



# 基本命令使用

1. 关机: shutdown
2. 关机并重启 :  shutdown -r now
3. 重启 ： reboot
4. 关闭系统 : halt
5. sync ： 将数据有内存同步到硬盘中

总结 ： 不管是重启还是关闭系统，首先要运行sync命令 把内存中的数据写到磁盘中



> 系统目录结构

 ```sh
查看 :  ls /
 ```

 如图 ：

![目录结构](D:\typora\JAVA-MD\Linux\目录结构.png)



##  常用命令



`cd` : 切换命令



`./`  : 当前目录

`ls` : 列出目录

-  -a : 查看全部文件，包括隐藏文件
- -l : 列出所有文件，包含文件的属性和权限，没有隐藏文件
- 所有Linux 可以组合使用

`pwd`： 显示当前用户所在目录

 `mkdir` : 创建目录

`mkdir -p`: 创建多级目录

![cd](D:\typora\JAVA-MD\Linux\cd.png)



`rmdir` ： 删除目录

`rmdir -p` : 强制以下递归 删除 文件



`cp`:  复制文件

-  cp 原目录 新目录  

  如果名字重复 -y ： 覆盖  ；-n : 放弃;

  

`rm` ： 一出文件或者目录

- -f  忽略不存在的文件 ，不会出现警告，强制删除

- -r 递归删除目录

- -i 互动删除询问是否删除

  ~~~shell
  rm -rf /      系统中所有的文件删除 （ 删除跑了 ）
  ~~~

  

`mv`: 移动文件 或者 目录 （ 重命名文件）

- -f : 强制

- -u 只替换 已经更新过的文件

  





## 文件内容查看





Linux 系统使用以下命令来查看 文件的内容

- cat  由第一行开始显示文件内容
- tac  从最后一行开始显示，可以看出tac是 cat倒写
- more 一页一页 的显示文件内容
- nl 显示的时候，顺道输出行号
- less 与 more 类似 ，但是比 more 更好，可以王前翻页
- head 只看头几行
- tail 只看尾巴几行

可以使用 man[命令] 查看命令使用文档  如 ：man cp



`ifconfig` : 查看ip 地址信息



> 拓展 linux链接

1. 硬连接 ： A--B，假设 B是 A 的硬连接，那么他们连个指向同一个文件，允许一个文佳拥有多个路径，用户可以通过这种机制建立硬连接到一些重要文件上，防止误删！ 
2. 软连接 ：类似Window下的快捷方式 ，删除的源文件 ，快捷方式也访问不了

`ln`  ：创建链接

`touch`： 创建文件

`echo`: 输入字符串





# Vim 编辑器



基本上 vi/vim  共有3种 模式

1. **命令模式**

用户 启动 vim ,便进入命令模式

此状态下敲击键盘会被Vim 识别为 命令，而非输入字符。 比如此时按下i ,并不会输入一个 字符， i 被当作 一个命令。

以下是常用命令 ：

-  `i`   切换到输出模式 ，以 输入字符
- `x`    删除当前光标所在的字符
- `:`   切换到底线命令模式 ， 已在最后一行输入命令

若想要编辑文本 ; 启动Vim ，进入 命令模式 ，按下 **i** ， 切换到输入模式

命令模式 只有一些基本的命令 ，因此仍要 靠底线命令模式，输入更多命令

~~~shell
vim kuangstudy.txt
~~~

![命令模式](D:\typora\JAVA-MD\Linux\命令模式.png)





2. **输入模式**

   在命令模式下按下 i 进入 输入模式

   在 输入模式中，可以使用以下按键 ：

   -  `字符按键以及shift`  , 输入字符
   - `ENTER` 回车键，换行
   -  `BACK SPACE`  退格键 ，删除光标 前一个字符
   - `DEL` 删除键 删除 光标后一个字符
   - `HOME/END`　移动光标到首／行　尾
   - `Page UP/Page Down`  上下翻页
   - `Insert `  切换光标为输入/替换模式 ，光标便问竖线/下划线
   - `ESC`  推出模式，进入命令模式

![输入模式](D:\typora\JAVA-MD\Linux\输入模式.png)



**3.  底线命令模式**

在命令模式下按下 : ( 英文冒号 ) 进入底线命令模式。 光标 就移动到最底下

![底线命令模式](D:\typora\JAVA-MD\Linux\底线命令模式.png)



在底线 命令模式 中可以输入单个 或者多个字符的命令 ，可用的命令非常多。

在底线命令模式中，基本的命令有 （ 已经省略了冒号 ） 

- `:q`   推出程序
- `:w`   保存文件

按下ESC 键 可以随时退出点命令模式 。



#  账号管理

![账号管理参数](D:\typora\JAVA-MD\Linux\账号管理参数.png)



`useradd ` 创建 用户

- -m   : 自动创建这用户的 主目录  

`userdel`: 删除用户

- -r   删除用户的时候将他的目录页一并删除

`usermod`: 修改用户

`su 用户名`  : 切换用户 普通用户

`sudo su` ： 切换root用户

![linux用户](D:\typora\JAVA-MD\Linux\linux用户.png)

`hostname`:  查看用户名

`hostname 用户名` ： 修改用户名

`passwd`: 设置密码  （ linux 上输入密码不会显示 ）

`passwd -l xxx` ： 冻结xxx 用户





# 磁盘管理

`df`: 列出文件目录

![磁盘管理](D:\typora\JAVA-MD\Linux\磁盘管理.png)





# 进程管理

**PS** 查看当前系统中正在执行的各种进程的信息

`ps` :

- -a 显示当前终端运行的所有的进程信息

- -u 以用户的信息显示进程

- -x 显示后台运行进程的参数

- -aux 查看后台运行进程的参数

  ~~~shell
  ps -aux|grep mysql   查看mysql 的进程
  
  # | 在 Linux 叫做 管道符 A|B
  # grep 查找文件中符合条件的字符串
  ~~~

  

  `ps -ef` : 可以查看父进程的信息

  `pstree `： 进程树

  - -p 显示父id
  - -u 显示用户组
  - -pu 

  `kill 进程id `   : 结束进程 

  

# java 环境安装

安装软件一般有三种方式： rpm (JDK) ,解压缩（tomcat）,yum在线安装（docker）

## 1.  JDK 安装

 我们在开发java 程序必须要的环境！

> rpm上 **不需要** 配置环境变量

~~~sh
# 检测 当前系统是否存在java环境 ！ java -version
# 如果有的话就卸载
rpm -qa|grep jdk     # 检测JDK版本信息
rpm -e --nodeps jdk_ 

# 卸载完毕后即可安装jdk
# rpm -ivh java的rpm包名

# java -version 检查 java版本
~~~

~~~sh
# 安装环境变量
vim /etc/profile    # linux 配置环境变量的目录
~~~



在文件 最后面 增加

~~~shell
JAVA_HOME=/usr/java/jdk1.8.0_221-amd64
CLASSPATH=%JAVA_HOME%/lib;%JAVA_HOME%/jre/lib
PATH=$JAVA_HOME/bin;$JAVA_HOME/jre/bin
export PATH　CLASSPATH JAVA_HOME
~~~



发布 一个项目



## 2. tomact安装

1. 解压 tomcat

~~~shell
tar -zxvf apache-tomcat-9.0.41.tar.gz 
~~~

2. 启动tomcat 测试 ！cd进入tomcat目录  ./xxx.sh 脚本

~~~bash
# 执行  ./startup.sh 
# 停止  ./shotdown.sh
~~~



## 3. Dockers （yum安装）

官网地址： 

https://docs.docker.com/engine/install/centos/





































  # 防火墙状态

~~~bash
# 查看firewall 服务状态
systemctl status firewalld

# 开启重启，关闭 fireealld.service 服务
## 开启
service firelld start

## 重启
service firewalld restart

## 关闭
service firewalld stop

#查看防火墙规则
firewall-cmd --list-all    # 查看全部信息
firewall-cmd --list-ports  # 只看端口信息

# 开启端口
开启端口命令： firewall-cmd --zone=public --addd-port=80/tcp --permanent
重启防火墙：   systemctl restart firewalld.service

#命令含义
--zone  #作用域
--add-port=80/tcp
--permanent # 永久生效，没有此参数重启后失效
~~~

















































































































































































 































































































