# JWt



组成 ：-- 用点拼接

- Header 
- payload 
- signature  签名算法

~~~java
var encodedString = base64UrlEncode(header) + '.' +
base74UrlEncode(payload)
    
var signature = HMACSHA256(encodedString,'secret')    
~~~



## 生成

pom.xml

- JWT依赖

~~~java
         <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt</artifactId>
            <version>0.9.1</version>
        </dependency>

<!--        jdk 1.8 以上需要导入-->
        <dependency>
            <groupId>javax.xml.bind</groupId>
            <artifactId>jaxb-api</artifactId>
            <version>2.3.0</version>
        </dependency>
        <dependency>
            <groupId>com.sun.xml.bind</groupId>
            <artifactId>jaxb-impl</artifactId>
            <version>2.3.0</version>
        </dependency>
        <dependency>
            <groupId>com.sun.xml.bind</groupId>
            <artifactId>jaxb-core</artifactId>
            <version>2.3.0</version>
        </dependency>
        <dependency>
            <groupId>javax.activation</groupId>
            <artifactId>activation</artifactId>
            <version>1.1.1</version>
        </dependency>

~~~



-- 生成ＪＷＴ

```java
 @Test
    public void jwt(){
        JwtBuilder jwtBuilder = Jwts.builder();
        String jwtToken = jwtBuilder
                // header
                .setHeaderParam("typ", "JWT")
                .setHeaderParam("alg","HS256")
                // payload
                .claim("username","tom")
                .claim("role","admin")
                .setSubject("admin-jwtTest")
                // 有效期 当前系统的24小时
                .setExpiration(new Date(System.currentTimeMillis()*time))
                // 设置id （这里uuid）
                .setId(UUID.randomUUID().toString())
                // signature
                .signWith(SignatureAlgorithm.HS256,signature)
                .compact();// 拼接
        System.out.println(jwtToken);
 }
```



生成结果：

~~~java
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6InRvbSIsInJvbGUiOiJhZG1pbiIsInN1YiI6ImFkbWluLWp3dFRlc3QiLCJleHAiOjU4NDQ2NjE3Mjk4ODQwMDAsImp0aSI6IjczNDM0NWJkLWNjZDUtNDVmYi05NjRlLTA2ZjgxYmU4YzI1MyJ9.g6su8wdjjiIm35VO9G03V6zjboGUTMul_Pc4KIQYPHA
~~~



## 解密



~~~java
    @Test
    public void pare(){
        String token = "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6InRvbSIsInJvbGUiOiJhZG1pbiIsInN1YiI6ImFkbWluLWp3dFRlc3QiLCJleHAiOjU4NDQ2NjE3Mjk4ODQwMDAsImp0aSI6IjczNDM0NWJkLWNjZDUtNDVmYi05NjRlLTA2ZjgxYmU4YzI1MyJ9.g6su8wdjjiIm35VO9G03V6zjboGUTMul_Pc4KIQYPHA";
        JwtParser jwtParser = Jwts.parser();
        Jws<Claims> claimsJws = jwtParser.setSigningKey(signature).parseClaimsJws(token);//对token 解析
        System.out.println("claimsJws======>>");
        System.out.println(claimsJws);

        Claims claims = claimsJws.getBody();
        System.out.println(claims.get("username"));
        System.out.println(claims.get("role"));
        System.out.println(claims.getId());
        System.out.println(claims.getSubject());
        System.out.println(claims.getExpiration()); // 有效期
    }

/** 

claimsJws======>>
header={typ=JWT, alg=HS256},body={username=tom, role=admin, sub=admin-jwtTest, exp=5844661729884000, jti=734345bd-ccd5-45fb-964e-06f81be8c253},signature=g6su8wdjjiIm35VO9G03V6zjboGUTMul_Pc4KIQYPHA
tom
admin
734345bd-ccd5-45fb-964e-06f81be8c253
admin-jwtTest
Tue Mar 22 22:00:00 CST 185211927

*/
~~~





























