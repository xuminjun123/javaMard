# Java基础题



##   == 和equals 比较



 == ：  对比的是**栈中的值** ， 基本数据类型是变量值，引用类型是堆内存对象的地址。



equals : Object 默认也是采用 == 比较 ，通常会重写 。

equals方法比较**字符串**是否相等时。

~~~java
public boolean equals(Object anObject) {
    // 判断是否是对象类型
    if (this == anObject) {
        return true;
    }     
    //判断是否是String类型的实例
    if (anObject instanceof String) {
        String anotherString = (String)anObject; //强转为String类型
        int n = value.length;
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
}
~~~



被重写 的equals() 方法其实是**比较两个字符串中的内容**。

~~~java
public class interview {

    public static void main(String[] args) {
        String str1 = "hello";                // 堆内存
        String str2 = new String("hello");    // 栈内存
        String str3 = str2;                   // 栈内存

        System.out.println(str1 == str2);  // false   
        System.out.println(str1 == str3);  // false
        System.out.println(str2 == str3);  // true
        System.out.println(str1.equals(str2)); // true
        System.out.println(str1.equals(str3)); // true
        System.out.println(str2.equals(str3)); // true

    }
}
~~~







## hashCode 和equals

**hashCode 的 作用 :** 获取哈希码 ，也称为散列码 ，返回一个 int 整数。

这个哈希码的作用是确定该对象在哈希表中的索引位置。



Java对于eqauls方法和hashCode方法是这样规定的： 

1、如果两个对象相同，那么它们的hashCode值一定要相同；

2、如果两个对象的hashCode相同，它们并不一定相同   





## final 作用

final 最终 

- 修饰类 ：表示类不可被继承
- 修饰方法 ：表示方法不可被子类 重写 , 但是 可以重载
- 修饰变量 ：表示变量一旦被赋值 ，就不可以更改



（1） 修饰成员变量 ：在使用final 修饰的成员变量  指定初始值

（2）修饰局部变量 ： 在使用final 修饰的局部变量  使用之前要一定要先赋值





 ## List 和 Set 区别

 

List : 有序  。按对象 进入的顺序保存对象 ，可重复 。 允许多个 Null ,可以通过迭代器逐一遍历， 还可以使用get(int index) 获取指定下标的元素。

set : 无序  。不可重复 ，最多只能 一个Null 元素，取元素时只能用迭代器 取得所有元素，在逐一遍历各个元素









# Spring

 ## IOC 和 AOP 理解

IOC ：控制反转



AOP :   将程序的交叉业务逻辑（比如安全，日志，事务等），封装成为一个切面，然后注入到目标对象（具体业务）中去 。ＡＯＰ可以对某个对象或某些对象的功能进行增强。





# JSON框架



## 常用框架

阿里的Fastjson，谷歌的gson等

JavaBean 序列化为Json ，性能Jackson  >  FastJson > Gson > Jaon-lib ....



`Jackson` 最快 ，处理相关自动

- 指定字段不返回 @JsonIgnore   放在字段上，不会返回给前端
- 指定日期格式     @JsonFormat（pattern=”yyyy-MM-dd hh:mm:ss“ local="zh",timezone="GMT+8"） 

- 空子段不返回     @JsonInclude(Include.NoN_Null)

- 指定别名             @JsonProperty

  



## 文件上传

1. 文件上传 MultipartFile file ,源自SpringMVC 



前端：

~~~html
<form enctype="multipart/form-data" method="post" action="/upload">
    文件:<input type="file" name="head_img">
    姓名:<input type="text" name="name"/>
    <input type="submit" value="上传">
</form>
~~~



这里是不完整的，缺少　判断图片是否为空（file.isEmpty）、判断图片大小(file.getSize) .... 

~~~java
private static final String filePath = "/user/..... 文件存放地址/"  // 后面要有 /
 
@Requet(value = "upload")
@ResponBody
public String upload(@Requestparam("head_img") MultipartFile file,HttpServletRequest request){
    String name = request.getParamter("name");
    System.out.printIn("用户名" + name);
    
    // 获取文件名
    String filename = file.getOriginalFilename();
    System.out.printIn("上传文件名" + filename);
    
    // 获取文件的后缀名,比如图片的(.png / .jpg)
    String suffixName = fileName.substring(fileName.lastIndexOf("."));
    System.out.printIn("上传文件名" + filename);
    
    // 文件上传的路径,这里是 ＵＵＩＤ，最好是时期　
    fileName = UUID.randomUUID() + suffixName;
    System.out.printIn("转换后的名称" + filename);
    
    File dest = new File(filePath + fileName); 
    
  	try {   // 这里返回 错误码, 以下　没有将图片存入数据库
        file.transferTo(dest);
        return fileName;
    }  catch (IllegalStateException e){
        e.printStackTrace();
    }catch {
        e.printStackTrace();
    }
     return "上传失败"
} 
~~~













# Springboot



## 1. 注解 配置

@RestController 、@Requestmapping 是SpringMVC注解

@RestController = @Controller + @ResponseBody  (  可以返回Json字符串 )

@SpringBootApplication = @Configuration + @EnableAutoConfiguration + @ComponentScan 



@GetMapping（value=""）    get 请求 相当于 @RequestBody + @RequestMapping 

@pathVariable   

@RequestParam(default=" " ， name =" "，required =true/false)  默认值

@RequestBody  获取JSON ，post 提交，这里需要指定 httpt头为 "content-type":"application/json"

 @RequestHeader（"access_token"）  获取请求头信息 



## 热部署

spring-boot-dev-tool

























