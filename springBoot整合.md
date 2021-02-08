# 资源整合篇




## 整合Redis





## 整合mybatis







## 整合 Druid









## 整合PageHelper





## 整合Shiro



## 整合Swagger









# 常用注解



## Spring Bean 管理相关



  ### @Bean



### @Scope



### @Component



### @Autowired



### @Configuration



### @RestController

 

## 传值相关

### @PathVariable









### @RequestParam





### @RequestBody





## 读取配置信息

###  @value








### @ConfigurationPropeoties





##  Http请求



### Get







### Psot



### Put





### Delete













## 参数校验



### 常用字段验证注解

~~~bash
@NotEmpty             	 # 被注释的字符串的不能为 null 也不能为空
@NotBlank 				 # 被注释的字符串非 null，并且必须包含一个非空白字符
@Null 					 # 被注释的元素必须为 null
@NotNull 				 # 被注释的元素必须不为 null
@AssertTrue 			 # 被注释的元素必须为 true
@AssertFalse 			 # 被注释的元素必须为 false
@Pattern(regex=,flag=)	 # 被注释的元素必须符合指定的正则表达式
@Email 					 # 被注释的元素必须是 Email 格式。
@Min(value)				 # 被注释的元素必须是一个数字，其值必须大于等于指定的最小值
@Max(value)				 # 被注释的元素必须是一个数字，其值必须小于等于指定的最大值
@DecimalMin(value)		 # 被注释的元素必须是一个数字，其值必须大于等于指定的最小值
@DecimalMax(value) 		 # 被注释的元素必须是一个数字，其值必须小于等于指定的最大值
@Size(max=, min=)		 # 被注释的元素的大小必须在指定的范围内
@Digits (integer, fraction) # 被注释的元素必须是一个数字，其值必须在可接受的范围内
@Past					 # 被注释的元素必须是一个过去的日期
@Future 				 # 被注释的元素必须是一个将来的日期
    
    .... ....
~~~







### 验证请求体

~~~java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Person {

    @NotNull(message = "classId 不能为空")
    private String classId;

    @Size(max = 33)
    @NotNull(message = "name 不能为空")
    private String name;

    @Pattern(regexp = "((^Man$|^Woman$|^UGM$))", message = "sex 值不在可选范围")
    @NotNull(message = "sex 不能为空")
    private String sex;

    @Email(message = "email 格式不正确")
    @NotNull(message = "email 不能为空")
    private String email;

}
~~~





### 验证请求参数







































