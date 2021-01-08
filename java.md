# 面向对象

## 什么是面向对象

> 面向对象编程本质 ： **以类方式组织代码，以对象的组织（封装） 数据**。



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

~~~java
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

> 封装 ： 通常应禁止访问一个对象中数据的实际表示， 而应通过操作接口来访问，这称为信息隐藏。

1.  提高程序的安全性，保护数据
2.  隐藏代码的实现细节
3.  统一接口
4. 提高系统的可维护性

~~~java
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

~~~java
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

​    

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

​	instanceof （类型转换） 引用类型 ，可以用来判断一个对象是什么类型

```java
System.out.println(x instanceof y); // 有关系为true
```

~~~java
public class Application {
    public static void main(String[] args) {
        // object > string
        // object > perosn > teacher
        // object > person > student
       Object object = new Student();
       System.out.println(object instanceof Student); // true
       System.out.println(object instanceof Person); // true
       System.out.println(object instanceof Object); // true
       System.out.println(object instanceof Teacher); //false
       System.out.println(object instanceof String); // false
System.out.println("=============");
       Person person = new Student();
        System.out.println(person instanceof Student); // true
        System.out.println(person instanceof Person); // true
        System.out.println(person instanceof Object); // true
//        System.out.println(person instanceof Teacher); // 报错
//        System.out.println(person instanceof String); // 报错
System.out.println("=============");
        Student student = new Student();
        System.out.println(student instanceof Student); // true
        System.out.println(student instanceof Person); // true
        System.out.println(student instanceof Object); // true
//        System.out.println(student instanceof Teacher); // 报错
//        System.out.println(student instanceof String); // 报错
    }
}
~~~



类型转化

~~~java
// student 类
public class Student extends Person {
    public void go(){
    }
}

// teacher 类
public class Teacher {
}

// person 类
public class Person {
    public void run(){
    }
}

public class Application {
    public static void main(String[] args) {
        // 类型转换 ： 父  子
      // 高                  低
      Person obj =  new Student();
      // student 这个对象转化为Student类型,我们就可以使用Student类型的方法
        Student student = (Student) obj;
        student.go();

        // 子类 转换为 父类，可能丢失本来的一些方法
        Person person = student;
		// person.go() 报错
    }
}
~~~



**小总结**

	1. 父类引用指向子类的对象
	2. 把子类转换为父类，向上转移
	3. 把父类转换为子类，向下转移，强制转换
	4. 方便方法的调用，减少重复的代码！ 



## static关键字详解

~~~java
package com.cop.demo07;

public class Person {
    // 匿名代码快
    // 第二步执行
    {
        // 代码块
        System.out.println("匿名代码快");
    }

    // 静态代码块，用于初始化
    // 最早执行，只执行一次
    static{
        System.out.println("静态代码块");
    }

    // 第三步执行
    public Person() {
        System.out.println("构造方法");
    }

    public static void main(String[] args) {
        Person person = new Person();
        System.out.println("=======");
        Person person2 = new Person();
    }
}

~~~

 



## 抽象类 abstract

```java
1. abstract 修饰符 可以用来修饰方法（抽象方法）也可以用来修饰（抽象类）。
2. 抽象类中**没有抽象方法** ，但是抽象方法的类一定要声明为抽象类。
3. 抽象类 **不能使用new关键字** 来创建对象，他是用来让子类继承。
4. 抽象方法 只有方法的声明，没有方法的实现，他是用来让子类实现的。
5. 子类继承抽象类，那么就必须要实现抽象类没有的抽象方法，否则该子类也要声明为抽象类
```

~~~java
子类
package com.cop.demo08;

public abstract class A extends Action {
    // 抽象类的所有方法，继承他的子类，都必须要重写他的方法
    // 除非 他本身也是抽象类
//    @Override
//    public void doSomething() {
//
//    }

}

父类
package com.cop.demo08;

// abstract
public abstract class Action {
    // 约束 有人帮我们实现
    // abstract 抽象方法，只有方法名字，没有方法的实现！
    public abstract void  doSomething();
} 
~~~

application

~~~java
import com.cop.demo08.Action;

public class Application {
    public static void main(String[] args) {
//      new Action(); // 报错  抽象类 不能 new
        // 抽象类中 可以写普通方法
        // 抽象类方法必须在抽象类中

    }
}
~~~



## 接口的定义与实现

关键字：**interface **



**普通类：只有具体实现**

**抽象类：具体实现和规范（抽象方法）都有**

**接口：只有规范**



> 接口就是规范，本质是契约，，比如说人间的法律



**接口作用**

 1. 约束

 2. 定义一些方法，让不同的人实现

 3. 方法都是 abstract

 4. 属性 都是 public static final

 5. 接口不能被 new实例化

 6. 接口没有 构造方法

 7.  implements 可以实现多个

 8. 必须重写接口中的方法

    

~~~java
// 接口
// interface 关键字
// 接口都需要实现类
public interface UserService {
    // 接口里面定义 属性都是常量
    // 默认 public static final 修饰
    int AGE = 99;

    // 接口中的所有定义其实都是抽象的 public abstract
      void add(String name);
      void delete(String name);
      void update(String name);
      void query(String name);

}


public interface TimeService {
    void timer();
}

// 实现类
package com.cop.demo09;

// interface 关键字
// 接口都需要实现类
public interface UserService {
    // 接口里面定义 属性都是常量
    // 默认 public static final 修饰
    int AGE = 99;

    // 接口中的所有定义其实都是抽象的 public abstract
      void add(String name);
      void delete(String name);
      void update(String name);
      void query(String name);

}
~~~





## 内部类

内部类就是在一个类的内部在定义一个类。 如 ， A类中 定义一个 类B ，那么B类相对A类来说就是内部类，而A类相对于B类及时外部类了。

	1. 成员内部类
	2. 静态内部类
	3. 局部内部类
	4. 匿名内部类



1. 成员内部类

~~~java
public class Outer {
    private int id;
    public void  out(){
        System.out.println("这是外部类的方法");
    }
    public  class  Inner {
        public void in() {
            System.out.println("这是一个内部类方法");
        }

         public void getId(){
            System.out.println(id);
        }
    }
}

// application
public class Application {
    public static void main(String[] args) {
        Outer outer = new Outer();

        // 通过外部类来实例化内部类
//        outer.new Inner();
        Outer.Inner innner = outer.new Inner();
        innner.getId();

    }
}

~~~

 

2. 静态内部 类 static

~~~java
public class Outer {
    private static int id;
    public void  out(){
        System.out.println("这是外部类的方法");
    }
    public  class  Inner {
        public void in() {
            System.out.println("这是一个内部类方法");
        }

         public static void getId(){
            System.out.println(id);
        }
    }
}

// application
public class Application {
    public static void main(String[] args) {
        Outer outer = new Outer();

        // 通过外部类来实例化内部类
//        outer.new Inner();
        Outer.Inner innner = outer.new Inner();
        innner.getId();

    }
}

~~~

3.局部内部类

~~~java
public class Outer {
    // 局部内部类
    public void method(){
        class Inner{

        }
    }
}
// 一个java类中 可以有多个 class类，但是只能有一个 public class
class A{

}

~~~



4. 匿名内部类

   ~~~java
   package com.cop.demo10;
   
   public class Test {
       public static void main(String[] args) {
           // 匿名内部类
           // 没有名字初始化类
           new Apple().eat();
       }
   }
   class Apple{
       public void eat(){
           System.out.println("1");
       }
   }
   
   ~~~

   



# 异常机制

## 处理机制

1. 抛出异常
2. 捕获异常


> 异常处理5个关键字

try,catch,finally,throw,throws

try,catch，finally

~~~java
public class Test {
    public static void main(String[] args) {
        int a = 1;
        int b = 0;

         // Ctrl + Alt + T
        try{ // try 监控区
            System.out.println(a/b);
        }catch(ArithmeticException e){  // catch(错误类型)
            System.out.println("程序出现异常");
        }finally {  // 都会执行 可以不写
            System.out.println("finally");
        }
    }
}
~~~



throw,throws

~~~java
public class Test {
    public static void main(String[] args) {
        new Test().test(1,0);
    }

    // 假设这个方法中，处理不了异常，方法抛出异常
    public void test(int a,int b){
        if(b==0){  // throw
            throw new ArithmeticException(); // 主动抛出异常,一般用在方法中
        }
    }
}

~~~

























































































































