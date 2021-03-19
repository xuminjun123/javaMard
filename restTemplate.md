# RestTemplate  -  Study

## 1. getForObject 

样例程序 ：

~~~java
@GetMapping("/getData")
public Object getForObject(){
    // 远程访问Url UserDTo
    String url = "http://xxxx"
	// 请求入参
    Map<String, Long> paramMap = new hashMap<>();
    
    // 第一个参数url ,第二个是 返回类型 ，第三个是入参
    UserDTO result = restTemplate.getForObject(url,UserDTO.class,paramMap);
    // Map<String,Object> result = retTemplate.getForObject(url,Map.class,paramMap);
    return result;
}
~~~



## 2. getForEntity

样例 程序 ：

~~~java
@GetMapping("/getData")
public Map<String,Object> getForEntity(){
    
    // 远程访问Url 
    String url = "http://...."
        
 	// 空的入参
    Map<String, Long> paramMap = new hashMap<>();    
        
 	ResponseEntity<HashMap> responseEntity = 
        resTemplate.getForEntity(url.HashMap.class,paramMap);
    
    // 状态码包装类
    HttpStatus statusCode = responseEntity.getStatusCode();
   	
    // 返回状态码
    int StatusCodeValue = responseEntity.getStatusCodeValue();
     
    // http 返回头 
    HttpHeaders headers = responseEntity.getHeaders();
    
    // 
    return responseEntity.getBody()
}
~~~









## 3. postForObject

restTemplate 的 Post 方法 与 Get 方法的区别 是post方法传参 必须是 `MultiValueMap`

~~~java
@RestController
public class UserController {
    // 实体传参
    @RquestMapping(value = "/adduser")
    public UserDTO addUser(UserDTO userDTO) {
		return userDTO;    
    }
    
    
    // 基本类型传参
    @RequestMapping(value = "/adduser2")
     public UserDTO addUser2(Long userId , String userName) {
		return userDTO;    
        UserDTO userDTO = new UserDTO();
        userDTO.setUserId(userId);
        userDTO.setUserName(userName); 
        return userDTO; 
    }
    
}


~~~



**基本类型传参和实体类型传参**

~~~java
@GetMapping("/postData")
public UserDTO postObject(){
    // 远程访问 URL
    String url = "http://xxx"
    
    // post 方法必须使用 MultiValueMap 传参  、 UserDTO 也可以    
    MultiValueMap<String,Object> paramMap = new LinkedMultiValueMap<>(); 
    
    paramMap.add("userId",1000L);
    paramMap.add("userName","xxx");
    
    // 返回对象 ,第一个参数 url ,第二个参数 入参， 第三个参数 返回值 
    userDTO userDTO = restTemplate.postForObject(url,paramMap,UserDTO.class);
    
    return userDTO;
       
}
~~~



**@RequestBody传参，需要是要HttpEntity形式传参**

~~~java
@GetMapping("/postData")
public UserDTO postForObject2(){
    // 申明请求头
	HttpHeaders headrs = new HttpHeaders();
	// application/json
    headers.setContentType(MediaType.APPLICATION_JSON);
    
    String url = "http://xxx"
	
    /**
    * 此处不能使用 MultiValueMap 会报错
    MultiValueMap<String,Object> paramMap = new LinkedMultiValueMap<>();
    paramMap.add("userId",100L);
    paramMap.add("userName","xuminjun");
     */
    
     // 此处可以使用HashMap 替代，但是会有警告
    UserDTO userDTO = new UserDTO(); 
 	userDTO.setUserId(1000L);       
    userDTO.setUserName("名字xxx");
    
    
    // 使用HttpEntity 形式包装传参
    HttpEntity<UserDTO> entityParam = 
        new HttpEntity<UserDTO>(userDTO,headers);
	
    UserDTO result = restTemplate.postForObject(url,entityParam , UserDTO.class);

	return result;
}
~~~































## 4. postForEntity

postForEntity 和 postForObject 的唯一区别 时返回的时候 使用 `ResponseEntity<UserDTO>` ,其他一样

~~~java
@GetMapping("/postData")
public UserDTO postForEntity(){
    
    // 远程访问 URL userDTO
    String url = "http://xxxx"
    
   MultiValueMap<String,Object> paramMap = new LinkedMultiValueMap<>();
    
   paramMap.add("userId",100L);
   paramMap.add("usrName", "xuminjun");
    
   ResponseEntity<UserDTO> userDTOResponseEntity = 
       restTemplate.postForEntity(url,paramMap,userDTO.class);
    
   HttpStatus statusCode = userDTOResponseEntity.getStatusCode();
   int statusCodeValue = userDTOResponseEntity.getstatusCodeValue();
    
   HttpHeader headers = userDTOResponseEntity.getHeaders();
   return userDTOResponseEntity.getBody();
        
}
~~~



## 5. exchage



Exchange方法既可以发送 Get 请求 ，也可以发送 Post 请求

Exchage方法既可以 做 getForEntity ，也可以postForEntity

~~~java
@GetMapping("/exchange")
public userDTO exchange(){
    String url = "http://xxxx";
    
    MultiValueMap<String,Object> paramMap = 
        new LinKedMultiValueMap<>();
    
    paramMap.add("userId",100L);
    paramMap.add("userName","名称");
    
    //HttpEntity 包装了传参
    httpEntity<MultiValueMap> requestEntity = new HttpEntity<>(paramMap);
    
    //ResponseEntity 包装返回结果 
    ResponseEntity<UserDTO> response = 
        restTemplate.exchage(url, HttpMethod.post,requestEntity,UserDTO.class);
    
    return response.getBody();
    
}
~~~



































