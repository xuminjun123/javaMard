# String 常用方法

- 1. int length() 

  - 获取长度 ；返回int 类型

- 2. char charAt(值)

  - 从字符 中 取出 指定的字符 ；返回 char 类型

- 3. char toCharArray()

  - 将字符串变成一个字符数组 ； 返回 char 数组类型

- 4. int indexOf("字符")

  - 查找一个指定的字符串是否存在；不存在返回 -1

    返回字符串出现的位置

- 5. int lastIndexOf() 

  - 从末尾查找一个指定的字符串是否存在；不存在返回 -1

    返回字符串出现的位置

- 6. toUpperCase() ;toLowerCase()

  - 字符串大小写转换

- 7. String[] split（“字符  ”）

  - 分割字符串； 返回字符串数组

- 8. boolean equals(Object obj)

  -  比较两个字符串是否相等， 返回布尔值

- 9. boolean equalsnoreCase(String)

  - 忽略大小写比较两个字符串是否相等 ，返回布尔值

- 10. String substring(int beginIndex , int endIndex)

  - 截取字符串，包含一个参数和两个参数的方法，返回字符串

- 11. boolean contains(String)

  - 判断一个字符串是否包含指定内容，返回一个布尔值

- 12. replace(char oldChar, char new Char) 

  - 新字符替换旧字符， 也可以达到去除空格的效果一种