# 集合

> 概念 ：  对象的容器，定义了 多个对象进行操作 的常用方法。
>
> 可实现数组的功能。

- 集合 与 数组区别 ：
  - 1. 数组长度固定，集合长度不固定
  - 2. 数组可以存储基本类型和引用类型 ， 集合只能存储 引用类型





## Collection 体系集合

<img src="D:\typora\JAVA-MD\集合\collection.png" alt="collection" style="zoom:50%;" />



> 特点 ： 代表任意一组类型 的对象 ，无序 、无下标、不能重复



方法 ：

- boolean add(Object obj )            //  添加一个对象
- boolean addAll(Conection c)      //  将一个集合中所有的对象添加到此集合中 

- void clear()                                    // 清空此集合和中所有对象
- boolean contains(Object 0)          //  检查集合中是否包含0 对象
- boolean equals(Object 0)              //  比较此集合是否指定对象 相等
- boolean isEmpty()                         //  判断此集合是否为空
- boolean remove(Object 0)            //  在刺激和中移除 o 对象
- int size()                                          //  返回此集合中元素的个数
- Object[] toArray()                            //  将此集合转换成数组



`Collection collection = new ArrayList()`

遍历集合  ：

- 使用增强for

 ~~~java
for(Object obj:collection) {
    System.out.print(object)
}
 ~~~

- 使用迭代器

  - hasNext()   有没有下一个元素
  - next()          获取下一个元素
  - remove()    删除下一个元素

  ~~~java
  Interator it = collection.iterator();
  while(it.hasNext()){
      String s = (String)it.next();
      System.out.printIn(s)
          
      it.remove(); // 遍历完也删除完    
  }
  ~~~

  





## list 接口

特点 ： 有序 、有下标、元素可以重复

方法 ：

- void  add(int index , Object o)                   // 在index 位置插入对象 o
- boolean addAll(int index , Collection c)    // 将一个集合中的元素添加到此集合中的index 位置
- Object get(int index)                                    // 返回集合中指定位置的元素
- List subList(int fromIndex, int toIndex)      // 返回fromIndex 和 toTndex 之间的集合元素 

- remove（）

- set（） // 替换

`List list = new ArrayList<>()` 

遍历集合 ：

- 使用for 遍历 

~~~java
for(int i=0;i<list.size();i++){
 	System.out.printIn(list.get(i))   
}
~~~

- 增强for

~~~java
for(Object obj : list){
    System.out.printIn(obj) 
}
~~~

- 迭代器

~~~java
Interator it = list.iterator();
while(it.hasNext()){
    System.out.printIn(it.next())
}
~~~

- 使用列表迭代器 ， 和Iterator的区别 ， ListIterator 可以向前遍历，添加、删除、修改元素

~~~java
ListIterator list = list.listIterator();
// 从前往后遍历
while(lit.hasNext()){
    System.out.printIn(lit.nextIndex() + ":" + lit,next())
}

// 从后往前遍历
while(lit.hasPrevious()){
    System.out.printIn(lit.previousIndex() + ":" + lit.previous())
}
~~~

 

**判断：**

- list.contains("apple")    // 判断是否存在apple
- list.isEmpty()                  // 判断是否为空



**获取位置 ：**

- list.indexOf("华为")       // 返回下标



## list 实现类 

- ArrayList   【  ！ 重点  】
  - 数组结构实现 、查询快 、增删慢 
  - 运行效率块 、线程不安全
- Vector
  - 数组结构实现， 查询块、 增删慢
  - 运行效率慢  、 线程安全
- LinkedList :
  - 链表结构实现 ，增删快、查询慢



## ArrayList 使用

数组结构 ，查询快 、增删慢 

`ArrayList arrayList = new ArrayList<>();`

- 添加元素

  ~~~java
  arrayList.add(Object);
  ~~~

- 删除元素

  ~~~java
  arrayList.remove(Object);
  ~~~

- 遍历元素  【重点】

  ~~~java
  - 1. 增强for
   
  - 2. 迭代器     
      
  ~~~

- 判断

  ~~~java
  arrayList.contains(Object) // 是否包含
  arrayList.isEmpty()         // 是否为空  
  ~~~

- 查找

  ~~~java
  arrayList.indexOf(obj)     
  ~~~

  





## LinkedList 集合

- 链表结构 ，查询慢，增删快

  

`LinkedList linkedList = new LinkedList<>();`

1. 添加元素 add()

~~~java 
linkedList.add(obj);
~~~

2. 删除元素remove()

~~~java
linkedList.remove(obj)
~~~

3. 遍历

~~~java
-  for
 for(int i=0;i<linkedList.size();i++){
     System.out.printIn(linkedList.get(i))
 }
    
- 增强for
 for(Object obj : linkedList){
     Student s = (Student)object;
     System.out.printIn(s.toString())
 }       
~~~

~~~java
- 迭代器
itreator it = linkedList.iterator();   
while(it.hasNext()){
	Student s = (Student)it.next();
    System.out.printIn(s.toString());
} 

- 列表迭代器 
ListInterator lit = linkedList.listIterator();  
while(it.hasNext()){
	Student s = (Student)lit.next();
    System.out.printIn(s.toString());
} 
~~~



4. 判断
   - contains( obj )
   - isEmpty()

5. 获取

linkedList.indexOf()







# 泛型



- Java 泛型 是jdk 中引入的 一个新特性 ，其本质是参数化类型 ，把类型作为参数传递



- 常见形式有  泛型类 、泛型接口 、泛型方法



- 语法 
  - <T,....>  T 称为 类型占位 符 ，表示一种引用类型 

- 好处：
  - 1. 提高代码重用性
  - 2. 防止类型转换异常，提高代码安全性



## 泛型类

- 语法 ： 类名<T>
  - T 是类型占位符 ，表示一种引用类型，如果编写多个使用 逗号 隔开

~~~java
public class Mygeneric<T>{
    // 使用泛型 T
    // 创建变量
    T t;
    
    // 泛型作为方法的参数
    public void show(T t){
        System.out.printIn(t);
    }
    
    // 泛型作为方法的返回值
    public T getT(){
        return t;
    }
}
~~~

- 使用  这里<> 必须引用类型

~~~java
mygeneric<String> myGeneric = new myGeneric<>()
~~~





## 泛型接口

- 语法 ： 接口名<T>
  - 注意 ，不能泛型静态常量

~~~java
public interface MyInterface<T>{
 	String name = "张三"    
	T server(T t);
}
~~~



- 实现类

~~~java
public class MyInterfaceImpl implements MyInterface<String>{
    @Override
    public String server(String t){
        return null;
        
    }
}
~~~





## 泛型方法





# set 接口

- 特点 ： 无序 、无下标、元素不可重复
- 方法全部 继承自 collection 方法 

`Set<String> set = new HashSet<>()`

- 添加 

  ~~~java
  set.add("xx")
  ~~~

- 删除

  ~~~java
  set.remove("xxx")
  ~~~

- 遍历

~~~java
- 增强for
~~~

~~~java
- 迭代器
~~~

## hashSet   【 重点 】

- 存储结构 ：哈希表（数组 + 链表 + 红黑树）

























## TreeSet 





















































































​	

















