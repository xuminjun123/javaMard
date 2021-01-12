# serverlet 框架

## meaven 讲解





![meaven](D:\typora\JAVA-MD\serverletImages\meaven.png)



![meaven2](D:\typora\JAVA-MD\serverletImages\meaven2.png)









## tomcat 讲解

文件夹信息 :

![微信截图_20210112125801](D:\typora\JAVA-MD\serverletImages\微信截图_20210112125801.png)



启动关闭 :

 bin目录 下面 的 

 1. `shutdown.bat `关闭

 2. `shartup.bat`开启

    

tomcat的conf目录下面的server.xml 

1. 可以配置启动的端口号 :

```java
 <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```

2.  可以配置主机名称 :

   ~~~java
    <Host name="localhost"  appBase="webapps"
               unpackWARs="true" autoDeploy="true">
   ~~~

   

访问测试 :  http://localhost:8080/

论 : 网站是如何访问的?

1. 输入一个域名 : 回车

2. 检查本机 C:\Windows\System32\drivers\etc\hosts配置文件有没有这个域名映射

   ~~~java
   # localhost name resolution is handled within DNS itself.
   #	127.0.0.1       localhost
   #	::1             localhost
   ~~~

   - 有 :  直接返回对应的IP地址,在这个地址中,有我们需要访问的程序,可以直接访问
   - 没有 : 去ＤＮＳ服务器（全世界的域名都在这里）上找

3. 可以配置环境变量(可选)

   



## 发布一个网站

1. 自己写的网站(html)放到Tomcat上,`D:\tomcat\webapps`下,就可以直接访问localhost:8080/....

     

   

























