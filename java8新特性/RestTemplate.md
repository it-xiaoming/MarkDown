# RestTemplate

### 一、简介

>[`RestTemplate`](https://docs.spring.io/spring/docs/current/spring-framework-reference/integration.html#rest-resttemplate)：具有同步模板方法API的原始Spring REST客户端。
>
>从5.0版本开始，`RestTemplate`它处于维护模式，以后只有很少的更改和错误请求被接受。请考虑使用提供更现代API并支持同步，异步和流方案的 [WebClient](https://docs.spring.io/spring/docs/current/spring-framework-reference/web-reactive.html#webflux-client)。

### 二、常用API

### 1、GTE请求

> 参数说明
>
> url ： 请求地址
>
> responseType : 相应数据类型
>
> uriVariables : 请求路径参数

```java
<T> T getForObject(URI url, Class<T> responseType)
```

```java
<T> T getForObject(String url, Class<T> responseType, Map<String, ?> uriVariables)
```

```java
<T> T getForObject(String url, Class<T> responseType, Object... uriVariables)
```

#### 2、POST请求

> 参数说明
>
> url ： 请求路径
>
> request ： 请求体参数
>
> responseType ： 响应数据类型
>
> uriVariables ： 请求路径参数

```java
<T> T postForObject(URI url, @Nullable Object request, Class<T> responseType)
```

```java
<T> T postForObject(String url, @Nullable Object request, Class<T> responseType,
			Map<String, ?> uriVariables)
```

```java
<T> T postForObject(String url, @Nullable Object request, Class<T> responseType,
			Object... uriVariables)
```

### 3、exchange

> 参数说明
>
> url : 请求路径
>
> method : 请求方式
>
> requestEntity : 请求体
>
> responseType ： 响应数据类型
>
> uriVariables ： 请求路径参数

```java
exchange(String url, HttpMethod method,
			@Nullable HttpEntity<?> requestEntity, Class<T> responseType, Object... uriVariables)
```

