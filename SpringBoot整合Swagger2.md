# SpringBoot整合Swagger2

### 一、添加依赖

```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.2.2</version>
</dependency>

<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.2.2</version>
</dependency>
```

### 二、编写配置类

```java
@Configuration
@EnableSwagger2	//开启Swagger2支持
public class SwaggerConfig{
    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
            	//增加API基本信息
                .apiInfo(apiInfo())
            	//返回一个ApiSelectorBuilder实例,用来控制哪些接口暴露给Swagger来展现
                .select()
            	//设置哪个包中内容被扫描
                .apis(RequestHandlerSelectors.basePackage("com.abel.example.controller"))
                .paths(PathSelectors.any())
                .build();
    }

    /**
     * 创建该API的基本信息（这些基本信息会展现在文档页面中）
     * 访问地址：http://项目实际地址/swagger-ui.html
     * @return
     */
    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("Spring Boot中使用Swagger2构建RESTful APIs")
                .description("更多请关注https://blog.csdn.net/u012373815")
                .termsOfServiceUrl("https://blog.csdn.net/u012373815")
                .contact("abel")
                .version("1.0")
                .build();
    }
}
```

### 三、常用注解

