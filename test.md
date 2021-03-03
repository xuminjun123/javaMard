## 双亲委派



![双亲委派](D:%5Ctypora%5CjavaMard-master%5CJVMImages%5C%E5%8F%8C%E4%BA%B2%E5%A7%94%E6%B4%BE.png)



​		以上就是Java类加载的双亲委派过程，总结就是：

1. 加载类的时候优先加载jdk的核心类库（lib目录）   --  BootStrapClassLoader

2.  再去加载jdk的扩展类库（lib/ext目录）                    -- ExtClassLoader

3. 最后再加载应该程序的类。                                         -- ApplicationLoader

   

    好处：

   1. 这样做的好处是防止有人写了冒充jdk的类，比如说你自己写了一个Integer类冒充jdk的Integer类，根据双亲委派机制jdk加载的时候肯定是优先加载lib目录下的Integer类而不是你这个冒充的类。
   2. 同时避免类的重复加载 ，英文JVM 中区分不同类 ，不仅仅是根据类名，相同的class文件被不同的ClassLoader加载就是不同的两个类





## Java 异常体系



- java 中所有异常都来自顶级父类 Throwable
- Throwable 下有两个子类 Exception 和 Error
- Error 是程序无法处理的错误 ， 一旦出现这个错误 ，则程序将被迫停止运行
- Exception不会导致程序停止 ，又分为两个部分RunTimeException 运行异常和 CheckedException 检查异常。
- RunTimeException 常常发生在程序运行过程中，导致当前线程执行失败  。 CheckedException 常常发生在程序编译过程中，会导致程序编译不通过









## GC 如何判断对象可以被回收



- 引用计数法 ： 每个对象有一个引用计数属性 ，新增一个引用时 计数加 1 ，引用释放时计数 减1 ，计数为 0 时可以回收（ Java 并没有采用这种机制 ）

- 可达性分析法 ： 从 GC Roots 开始向下搜索，搜索所走过的路径称为引用链。 当一个对象GC Roots 没有任何引用链相连时 ，则证明此对象是不可用的，那么虚拟机就判断是 可回收对象 。



GC Roots 对象有：

- 虚拟机栈（栈帧中的本地变量表）中引用的对象
- 方法区中静态类属性引用的对象、
- 方法区中常量引用的对象
- 本地方法栈中JNI（即一般说的native ）引用的对象





要宣告一个对象死亡，至少要经过两次标记过程：

1、经过可达性分析后，一个对象并没有与GC Root关联的引用链，将会被第一次标记和筛选。筛选条件是此对象有没有必要执行finalize()方法。如果对象没有覆盖finalize()方法，或者已经执行过了。那就认为他可以回收了。如果有必要执行finalize()方法，那么将会把这个对象放置到F-Queue的队列中，等待执行。



2、虚拟机会建立一个低优先级的Finalizer线程执行F-Queue里面的对象的finalize()方法。如果对象在finalize()方法中可以『拯救』自己，那么将不会被回收，否则，他将被移入一个即将被回收的对象集合。





## 什么是Bean的 自动装配

开启 自动装配，只需要在xml 配置文件 <bean>中定义“autowire”属性 

~~~xml
<bean id="user" class="com.xxx.xxx.user" autowire=""  />    
~~~





autowire 属性 有5种

- no 手动装配 ，需要使用Value 或者ref的方式明确指定属性是手动装配

- byName 根据属性名称进行自动装配
- byType   根据bean类型进行自动装配
- constructor 
- autodetect



## Spring Boot自动装配原理



@Import + @Configuration + Spring spi



![springBoot 原理](D:%5Ctypora%5CjavaMard-master%5CJVMImages%5CspringBoot%20%E5%8E%9F%E7%90%86.png)





## 为什么要使用嵌入式服务器



- 节省下载tomcat ,应用也不需要再打包 ，然后放到webapp目录下在运行

- 只需要安装一个Java 虚拟机，就可以直接在上面部署应用程序了

- spring Boot已近内置 tomcat.jar ,运行main方法时会自动启动tomcat,并利用tomcat的spi机制加载springMVC





## mybatis的优缺点

![mybatis](D:%5Ctypora%5CjavaMard-master%5CJVMImages%5Cmybatis.png)