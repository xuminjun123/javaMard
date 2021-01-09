# 多线程

## 基本概述

>多任务，多线程，多进程区别。

**多任务**： 同一时间依旧只做了一家事情。如图

![](D:\typora\JAVA-MD\微信截图_20210107215530.png)



**多线程**，如图所示：

![](D:\typora\JAVA-MD\微信截图_20210107215849.png)



**多进程**

![](D:\typora\JAVA-MD\微信截图_20210107220301.png)



**核心概念**

1. 线程就是独立的执行路径；
2. 在程序运行时，及时没有自己创建线程，后台也会有多个线程，如主线程，gc线程
3. main()称之为，系统的入口，用于执行整个程序
4. 在一个进程中，如果开辟了多个线程，线程的运行有调度器安排调度，调度器是与操作系统紧密相关的，先后顺序是不能认为干预的。
5. 对同一份资源操作是，会存在资源抢夺问题，需要加入并发控制；
6. 线程会带来额外的开销，如cpu调度时间，并发控制开销。
7. 每个线程在自己的工作内存交互，内存控制不当会造成数据不一致。



## 继承 Thread类

>线程创建 ： Thread(重点),Runnable（重点）,Callable（3-5年之后核心）

Thread :

 1. 自定义线程类继承 Thread类

 2. 重写 run 方法，编写线程执行体

 3. 创建线程对象，调用 start()方法启动线程

    

~~~java
// 创建线程 继承 Thread类，
// 重写 run()
// start开启线程
public class TestThread extends Thread{
    @Override
    public void run(){
        // run 方法线程体
        for(int i=0;i<20;i++){
            System.out.println("看代码");
        }
    }

    public static void main(String[] args) {
        // main 线程，主线程
        // 创建一个线程对象
        TestThread testThread = new TestThread();
        testThread.start();

        for(int i=0;i<2000;i++){
            System.out.println("学习Java代码");
        }
    }
}

~~~

**总结**

​	线程开启不一定立即执行，有ＣＰＵ调度安排



## 多线程网图下载

~~~java
package com.kuang.demo01;

import org.apache.commons.io.FileUtils;

import java.io.File;
import java.io.IOException;
import java.net.URL;

// 实现多线程下载图片
public class TestThread2 extends  Thread{
    private String url; // 地址
    private String name; // 文件名

    public TestThread2(String url,String name){
        this.url = url;
        this.name = name;
    }

    // 线程执行体
    @Override
    public void  run(){
        WebWDownloader webWDownloader =new WebWDownloader();
        webWDownloader.downloader(url,name);
        System.out.println("下载文件名"+name);
    }

    public static void main(String[] args) {
        TestThread2 t1 =new TestThread2("https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=1819216937,2118754409&fm=26&gp=0.jpg","1.jpg");
        TestThread2 t2 =new TestThread2("https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=1819216937,2118754409&fm=26&gp=0.jpg","2.jpg");
        TestThread2 t3 =new TestThread2("https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=1819216937,2118754409&fm=26&gp=0.jpg","3.jpg");

        t1.start();
        t2.start();
        t3.start();

    }
}

// 下载器
class WebWDownloader{
    // 下载方法
    public void  downloader(String url,String name){
        try {
            FileUtils.copyURLToFile(new URL(url),new File(name));
        } catch (IOException e) {
            e.printStackTrace();
            System.out.println("IO异常，下载出现异常");
        }
    }
}
~~~

**总结**

	1. 子类继承 Thread类举办多线程能力
	2. 启动线程：子类对象.start()
	3. 不建议使用：避免OOP单继承局限性



## 实现 `Runnale` 接口

**实现接口`Runnale`接口**

 	1. 实现接口Runnable 具有多线程能力
 	2. 启动线程： 传入目标对象 + Thread对象.start()
 	3. 推荐使用：避免单继承局限性，灵活方便，方便同一个对象被多个线程使用

~~~java
package com.kuang.demo01;

// 创建线程方式2 ： 实现 Runnale接口
// 重写 run()方法，执行线程需要丢人runnale
public class TestThread3 implements Runnable{

    @Override
    public void run() {
        // run 方法线程体
        for (int i = 0; i < 200; i++) {
            System.out.println("看代码---" + i);
        }
    }

    public static void main(String[] args) {
        // main 线程，主线程
        // 创建一个线程对象
        TestThread3 testThread3 = new TestThread3();

        Thread thread  = new Thread(testThread3);
        thread.start();
        new Thread(testThread3).start();
        for(int i=0;i<2000;i++){
            System.out.println("学习Java代码---"  + i);
        }
    }
}
~~~



## 初始并发问题

>并发问题：多个线程操作同一个资源对象，线程不安全，数据紊乱。

~~~java
// 多个线程同时操作同一个对象
public class TestThread4 implements Runnable {
    // 票数
    private int ticketNums =10;

    @Override
    public void run() {
        while(true){
            if(ticketNums<=0){
                break;
            }

            // 模拟延时
            try {
                Thread.sleep(200);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            System.out.println(Thread.currentThread().getName() + "拿到了" + ticketNums--+"票");
        }
    }

    public static void main(String[] args) {
        TestThread4 ticket = new TestThread4();
        new Thread(ticket,"小红").start();
        new Thread(ticket,"小米").start();
        new Thread(ticket,"黄牛").start();
    }
}
~~~



## 并发案例

 **龟兔赛跑**

案例：

~~~java
package com.kuang.demo01;
// 模拟龟兔赛跑
public class Race implements Runnable {

    // 胜利者
    private static  String winner;

    @Override
    public void run() {
        for(int i=0;i<=100;i++){

            // 模拟兔子休息
            if(Thread.currentThread().getName().equals("兔子")){
                try {
                    Thread.sleep(200);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }

            // 判断比赛是否结束
            boolean flag = gameOver(i);
            if(flag){
                break;
            }
            System.out.println(Thread.currentThread().getName() + "跑了"+ i +"步");
        }
    }
    // 判断是否完成比赛
    private boolean gameOver(int steps){
        // 判断是否有胜利者
        if(winner != null){
            return true;
        }{
            if(steps >= 100){
                winner = Thread.currentThread().getName();
                System.out.println("winner is " + winner);
            }
        }
        return  false;
    }

    public static void main(String[] args) {
        Race race =new Race();

        new Thread(race,"兔子").start();
        new Thread(race,"乌龟").start();

    }
}
~~~





## 实现Callable接口(了解即可)

> callable 接口下载图片

**步骤**

	1. 实现Callable 接口，需要返回值类型
 	2. 重写call 方法，需要抛出异常
 	3. 创建目标对象
 	4. 创建执行服务
 	5. 提交执行
 	6. 获取效果
 	7. 关闭服务

~~~java
public class TestCallable implements Callable<Boolean> {
    private String url; // 地址
    private String name; // 文件名

    public TestCallable(String url,String name){
        this.url = url;
        this.name = name;
    }

    // 线程执行体
    @Override
    public Boolean  call(){
         WebWDownloader webWDownloader =new WebWDownloader();
        webWDownloader.downloader(url,name);
        System.out.println("下载文件名"+name);
        return true;
    }

    public static void main(String[] args) throws ExecutionException, InterruptedException {
        TestCallable t1 =new TestCallable("https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=1819216937,2118754409&fm=26&gp=0.jpg","1.jpg");
        TestCallable t2 =new TestCallable("https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=1819216937,2118754409&fm=26&gp=0.jpg","2.jpg");
        TestCallable t3 =new TestCallable("https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=1819216937,2118754409&fm=26&gp=0.jpg","3.jpg");

        // 创建执行服务
        ExecutorService ser = Executors.newFixedThreadPool(3);
        // 提交之行
        Future<Boolean> r1 = ser.submit(t1);
        Future<Boolean> r2 = ser.submit(t2);
        Future<Boolean> r3 = ser.submit(t3);
        // 获取结果
        boolean rs1 = r1.get();
        boolean rs2 = r2.get();
        boolean rs3 = r3.get();
        // 关闭服务
        ser.shutdownNow();
    }
}

// 下载器
class WebWDownloader{
    // 下载方法
    public void  downloader(String url,String name){
        try {
            FileUtils.copyURLToFile(new URL(url),new File(name));
        } catch (IOException e) {
            e.printStackTrace();
            System.out.println("IO异常，下载出现异常");
        }
    }
}
~~~

​	

**总结**

	1. callable 的好处
 	2. 可以定义返回值
 	3. 可以抛出异常



## 静态代理模式

~~~java
package com.kuang.demo03;

public class StaticProxy {
    public static void main(String[] args) {
        WeddingCompany weddingCompany = new WeddingCompany(new You());
        weddingCompany.HappyMarry();
    }
}

interface  Marry{

    void HappyMarry();
}
// 真实角色
class You implements Marry{
    @Override
    public void HappyMarry() {
        System.out.println("输出内容");
    }
}

// 代理角色
class WeddingCompany implements Marry{
    
    private Marry target;

    public WeddingCompany(Marry target){
        this.target = target;
    }

    @Override
    public void HappyMarry() {
        before();
        this.target.HappyMarry(); // 真实角色
        after();
    }

    private void after() {
        System.out.println("之后");
    }

    private void before() {
        System.out.println("之前");
    }
}
~~~

​	

**总结**

 1. 真是对象 和 目标对象 实现同一个接口

 2. 代理真实角色

    



## `Lamda` 表达式

 ~~~sh
b= (a)->{    // ()可输入参数
 System.out.pritIn("打印内容" + a)
};
 ~~~

**好处**

1. 避免匿名内部类定义过多

2. 可以让代码更加简洁

3. 3，去掉无意义代码，只留下核心代码

   

函数式接口 的定义：

 1. 任何接口，如果只包含唯一一个抽象方法 ，那么它就是函数式接口

    ~~~java
    public interface Runnale {
        public abstract void run();
    }
    ~~~

 2. 对于函数式接口，我妈可以通过`Lamda`表达式来创建该接口对象

~~~java
package com.kuang.lambda;
/*
* 推导Lambda 表达式
* */
public class TestLambda1 {

    // 3.静态内部类
    static class Like2 implements ILike{
        @Override
        public void lambda() {
            System.out.println("Lambda-study2");
        }
    }


    public static void main(String[] args) {
        ILike like = new Like();
        like.lambda();

        like = new Like2();
        like.lambda();

        // 4.局部内部类
        class Like3 implements ILike{
            @Override
            public void lambda() {
                System.out.println("Lambda-study3");
            }
        };

        like = new Like3();
        like.lambda();
        
        // 匿名内部类
        // 没有名称，必须借助接口或者父类
        like = new ILike() {
            @Override
            public void lambda() {
                System.out.println("Lambda-study4");
            }
        };

        like.lambda();

        // 6. Lambda 简化
        like = ()->{
            System.out.println("Lambda - study6");
        };

        like.lambda();
    }
}

// 定义一个函数式接口
interface ILike{
    void lambda();
}
// 实现类
class Like implements ILike{
    @Override
    public void lambda() {
        System.out.println("Lambda-study1");
    }
}
~~~



## 线程停止

> 线程有5大状态

1. 创建状态

2. 就绪状态

3. 阻塞状态

4. 死亡状态

   **如图所示**：

![微信截图_20210109120636](D:\typora\JAVA-MD\微信截图_20210109120636.png)







































































