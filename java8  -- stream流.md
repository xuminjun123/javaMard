# Stream 流



https://blog.csdn.net/qq122516902/article/details/103975404/



~~~java

~~~

- 流的 一次消费

- 惰性求值
- 极早求值



collection(  )

## 流操作

- filter

- distinct()   // 去重



**切片**

- limit(int )  // 截断几条
- skip(int)    // 跳过几条
- sort（）    // 正序 （也可倒序）



**匹配**

- anyMatch()    // 判断是否所有元素都满足给定条件
- noneMatch()  // 判断是否所有元素都 不 满足给定 条件

 

**查找**

- findFirst()  // 查找第一个元素并返回
- findAny()   //  查找返回任意一个元素



**规约**

- reduce ()       // 将流中的元素约 成 一个值

- max()            // 最大值



**遍历**

- forEach()