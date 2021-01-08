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



## 实现 Runnale 接口

**实现接口Runnale接口**

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



~~~java

~~~























