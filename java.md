# 面向对象

## 什么是面向对象

面向对象编程本质 ： **以类方式组织代码，以对象的组织（封装） 数据**。



**三大特性** ：

 1. 封装

 2. 继承

 3. 多态

    

从认识角度考虑是先有对象后有类 。 

​	对象：具体事事物

​    类： 是抽象的，是对对象的抽象

从代码运行角度考虑先有类后有对象 。 类是对象的模板。



## 创建与初始化对象

使用new关键字创建对象

1. 使用new关键字创建的时候会，除了分配空间外，还会给 创建好的对象 进行默认的初始化，以及对类中构造的调用。

2. 类中的构造器也称为构造方法，实在进行创建对象的时候要调用的，并且构造器有以下两个特点

   1.  必须和类的名字相同
   2. 必须没有返回类型，也不能写void

   

使用构造器必须要掌握



## 构造器详解 alt + insert

~~~java
public class Person {
    // 一个类什么都不写，也会存在一个方法
    String name;

    // 实例化初始值
    // 1. 使用new 关键字，必须要有构造器
    // 无参
    public Person(){ 
//        this.name= "小明";     // 这里一般不写，用来初始化值
    }

    // 有参
    // 一旦定义有参构造，无参就必须显示未定义
    public Person(String name) {
        this.name = name;
    }
}
~~~

application

~~~java
package com.cop.demo02;

// 应用类(规范 ：一个项目只有一个入口)
public class Application {
    public static void main(String[] args) {
        // 构造器 (有参构造必须要传参数)
        Person person =new Person("xxx");
        System.out.println(person.name); //null

    }
}
~~~



**构造器 ： **

	1. 和类名相同
	2. 没有返回值



**作用**

 1. new本质在调用构造方法

 2.  初始化对象的值

     

**注意点 ：**

​	1. 定义有参构造之后，如果想使用无参构造，需要先定义 一个无参构造





## 内存讲解 （堆栈）

​		![3](D:\C盘移动过来的文件\Desktop\3.png)



## 小结

~~~
public class Pet {
    //   1. 类与对象
//    类是一个模板 ： 抽象 ，对象是一个具体的实例
   //    2. 方法 调用 !  
   //       对应的引用   引用类型 ： 基本类型 
    //   3.构造器 （有参 无参）
//       4.堆找
//       5.类：
//           静态的属性   属性
//           动态的行为   方法

//           封装 继承 多态
}
~~~





## 封装 （set/get）

**高内聚，低耦合**

**alt + insert **

**封装** ： 通常应禁止访问一个对象中数据的实际表示， 而应通过操作接口来访问，这称为信息隐藏。

1.  提高程序的安全性，保护数据
2.  隐藏代码的实现细节
3.  统一接口
4. 提高系统的可维护性

~~~java
package com.cop.demo04;

public class Student {
    // 私有 外部无法获得 ，
    private  String name;
    private  int id;
    private  char sex;

    // 提供一些可以操作这个属性的方法
    // get 获得这个数据
    public String getName(){
     return  this.name;
    }
    // set 获得这个数据
    public void  setName(String name){
        this.name = name;
    }

    // alt + insert
    public int getId() {
        return id;
    }

    public char getSex() {
        return sex;
    }

    public void setId(int id) {
        this.id = id;
    }

    public void setSex(char sex) {
        // 可做一些安全性的判断
        this.sex = sex;
    }
}
~~~

Application

~~~
package com.cop;
import com.cop.demo04.Student;

public class Application {
    public static void main(String[] args) {
       Student s1 = new Student();
       s1.setName("小明");  // 这里可以设置参数

        System.out.println(s1.getName());  // 小明
    }
}
~~~



## 继承

继承的本质是对某一批类的抽象，从而实现对现实世界更好的建模 。

使用 **extands** 关键字！

**extands**的意思“拓展”，子类是父类的拓展。

JAVA只有单继承，没有多继承！ （子类只有一个父类，父类有很多子类）

除了 继承 之外，类与类之间还有**依赖 组合 聚合**等

继承关系的两个类 ，子类（派生类），一个为父类（基类）。子类继承父类，使用extands表示。

~~~java
父类
package com.cop.demo05;

public class Person {
    private int money = 1000;
    public  void say(){
        System.out.println("say方法");
    }
}

子类
package com.cop.demo05;
// (Student)小类 继承 (Person)大类
public class Student extends Person{
    Person person; // 组合

}
~~~

application

~~~java
package com.cop;

import com.cop.demo05.Student;

public class Application {
    public static void main(String[] args) {
        
        Student student = new Student();
        student.say();  // 可以使用say方法
		//System.out.println(student.money);  报错
    }
}
~~~

【 **注** 】

**子类继承父类就会有父类的所有方法！**

**在java中 所有的类，都默认直接或者间接继承 Object**

~~~
public class Person /*extands Object*/ {
  
}
~~~

**private 不能直接 用  “ . ” ** 



## super 关键字

**super注意点**

  1. super调用父类的构造方法，必须在构造方法的第一个

  2. super 必须只能出现在子类方法或者构造方法中

  3. super 和this不能同时调用构造方法

     

**和this不同**

​	this : **本身**调用者这个对象

​	super: 代表**父类对象**的引用

前提

​	this:没有继承也可以使用

​    super: 只能在继承条件下可以使用

构造方法

​	this()； 本类构造

​	super(); 父类构造、

~~~java
子类
// (Student)小类 继承 (Person)大类
public class Student extends Person{
    public Student() {
        //先调用父类 supper() ,隐藏了父类的无参构造
        // supper(); 必须写在第一行
        System.out.println("Student无参执行");
    }

    private String name ="小明";
    public void print(){
        System.out.println("Student类");
    }
    public  void test1(){
        print();     // 当前类对象
        this.print();  //  输出当前类对象
        super.print();  // 输出父类
    }

    public  void  test(String name){
        System.out.println(name);  //  输出传入的参数
        System.out.println(this.name); // 输出 private
        System.out.println(super.name);  // 输出父类
    }
}


父类
public class Person {
// 父类无参必须要写，否则子类无法调用
    public Person() {
        System.out.println("person构造执行");
    }
    protected  String name ="小";
   // private 无法继承
   public void print(){
       System.out.println("person类");
   }
}
~~~

application

~~~java
import com.cop.demo05.Person;
import com.cop.demo05.Student;

public class Application {
    public static void main(String[] args) {
        Student student = new Student();
//      student.test("xiao");
    }
}
~~~



## 方法重写@override

**快捷键  alt + insert**

~~~java
子类
public class A extends B{
    // 重写 : 有功能的注释
    // 非静态方法才可以重写
    // 只能是 public才可以重写
    @Override
    public void test() {
//        super.test();
        System.out.println ("A的test方法");
    }
}

父类
public class B {
    public  void test(){
        System.out.println("B的test");
    }
}
~~~

application

~~~java
public class Application {
    public static void main(String[] args) {
        A a = new A();
        // 方法调用只和左边类型有关
        a.test();  // A

        // 父类引用指向子类
        B b =new A();
        b.test(); // B
    }
}
~~~

**注意**

1. 需要有继承关系，子类重写父类的方法

2. 方法名称必须相同

3.  参数列表必须相同

4. 修饰符可以扩大，不能缩小 public > protected > default > private

5. 抛出异常 ： 范围，可以被缩小，但不能扩大

   

重写，子类的方法和父类方法必须要一致，方法体不同！



**为什么需要重写？？？**

 1. 父类的功能子类不一定需要，或者不满足

    



## 多态

**动态编译** ： 类型可以扩展

**多态** ： 可以根据发送对象的不同而采用多种不同的行为方式

​            一个对象的实际类型是确定的，但可以指向对象的引用的类型有很多



**多态存在条件**

 1. 有继承关系

 2. 子类重写父类方法

 3. 父类引用指向子类对象

    

**【注意】**

 1. 多态是方法的多态 ，属性没有 多态

 2.  instanceof 关键字   引用类型转换

	3.  static,final,private  这些修饰的方法无法重写，也就无法实现多态！

    

~~~java
子类
public class Student extends Person {
    @Override
    public void run() {
        System.out.println("son");
    }
    public void eat(){
        System.out.println("eat");
    }
}
父类
public class Person {
    public void run(){
    }
}
~~~

application

~~~java
import com.cop.demo6.Person;
import com.cop.demo6.Student;

public class Application {
    public static void main(String[] args) {
        //一个对象的实际类型是确定的
//        new Student();
//        new Person();

        // 但可以指向的引用类型不确定
        // student 能调用的方法都是自己的或者继承父类的
        Student s1 = new Student();

        // perosn类 可以指向子类，但是不能调用子类方法
        Person s2 = new Student();  // 多态
        Object s3 = new Student();

        s2.run(); // 子类重写父类的方法，执行子类的方法
        s1.run();

        s1.eat();
//        s2.eat(); // 不能使用，引用使用的 Person类
    }
}
~~~





## instanceof和类型转换

















































































