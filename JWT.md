# JWT

### 一、什么是JWT

> ​	json web token:（JWT）是一个开放标准(rfc 7519)，它定义了一种紧凑的、自包含的方式。用于在各个方之间以Json对象安全地传递信息。此信息可以验证和信任，因为它是数字签名的，jwt可以使用密钥(使用HMAC算法或使用RSA或ECDSA的公钥/私钥对进行签名。一般用于用户认证(前后端分离/微信小程序/app开发)

### 二、JWT能做什么

> 1、授权

```java
	这是使用jwt的常用方案，一旦用户登录，每个后续请求将包括jwt，从而允许用户访问该令牌允许的路由，服务和资源，单点登录是当前广泛使用jwt的一项功能，因为它的开销很小并且可以在不同的域中轻松使用。
```

> 2、信息交换

```java
Json Web Token是在各方之间安全地传递信息的好方法，因为可以对JWT进行签名(使用公钥/私钥对),所以您可以确保发件人是他们所说的人。此外，由于签名是使用表头和有效负载计算的，因此您可以验证内容是否遇到篡改。
```

### 三、为什么使用JWT

>

### 四、JWT的结构

#### 令牌组成

```java
1、标头(Header)
2、有效负载(Payload)
3、签名(signature)
```

##### Header

>表头通常由两部分组成：令牌的类型(即JWT)和所使用的签名算法，例如HMAC、SHA256或RSA。并会使用Base64编码。

```json
{
    "alg":"HS256",
    "typ":"JWT"
}
```

##### Payload

>对有关实体(通常是用户)和其他数据的申明。同样的，它使用Base64编码。

```json
{
    "sub":"123456789",
    "name":"hym",
    "admin":true
}
```

##### signature

>使用编码后的 Header 和 Payload 以及我们提供的一个密钥，然后使用 header中指定的签名算法(HS256)进行签名。签名的作用是保证JWT没有被篡改过。

```java
如：
    HMACSHA256(base64UrlEncode(Header)+"."+base64UrlEncode(Payload)+"."+secret)
```

### 五、SpringBoot整合JWT

#### 导入依赖

```xml
<!--jwt-->
<dependency>
    <groupId>com.auth0</groupId>
    <artifactId>java-jwt</artifactId>
    <version>3.10.3</version>
</dependency>
<!--mybatis-->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.3</version>
</dependency>
<!--lombok-->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <optional>true</optional>
</dependency>
<!--mysql-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
<!--druid-->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.23</version>
</dependency>
<!--spingBoot WEB 启动器-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <version>1.5.0.RELEASE</version>
</dependency>
```

#### JWT工具类

```java
public class JWTUtils {
    //公钥
    private static String SECRET = "hym";
    /**
     * 生成Token
     * @param map
     * @return
     */
    public static String getToken(Map<String,String> map){
        JWTCreator.Builder builder = JWT.create();
        //设置负载payload
        map.forEach((k,v) ->{
            builder.withClaim(k,v);
        });
        Calendar instance = Calendar.getInstance();
        instance.add(Calendar.SECOND,7);
        //设置过期时间
        builder.withExpiresAt(instance.getTime());
        //设置签名算法
        return builder.sign(Algorithm.HMAC256(SECRET)).toString();
    }
    /**
     * 验证token
     * @param token
     * @return
     */
    public static DecodedJWT verify(String token){
         return JWT.require(Algorithm.HMAC256(SECRET)).build().verify(token);
    }
}
```

#### JWT请求拦截器

```java
public class JWTInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        Map<String,Object> map = new HashMap<>();
        // 获取请求头中的令牌
        String token = request.getHeader("token");
        try {
            //验证令牌
            JWTUtils.verify(token);
            //放行请求
            return true;
        }catch (SignatureVerificationException e){
            e.printStackTrace();
            map.put("message","无效签名!");
        }catch (TokenExpiredException e) {
            e.printStackTrace();
            map.put("message","token已过期!");
        }catch (AlgorithmMismatchException e){
            e.printStackTrace();
            map.put("message","token算法不一致!");
        }catch (Exception e){
            e.printStackTrace();
            map.put("message","token无效!");
        }
        map.put("state",false); //设置失败状态
        //将map 装换为json
        String json = new ObjectMapper().writeValueAsString(map);
        response.setContentType("application/json;charset=UTF-8");
        response.getWriter().println(json);
        return false;
    }
}
```

#### SpringBoot拦截器配置类

```java
@Configuration
public class InterceptorConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new JWTInterceptor())
                .addPathPatterns("/user/test")	//拦截路径
                .excludePathPatterns("/user/login");	//放行路径
    }
}
```



