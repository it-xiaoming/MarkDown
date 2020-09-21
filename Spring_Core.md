# 							Spring

### 一、给容器注册组件

1. 包扫描+组件标注注解(@Component,@Repository,@Service,@Controller)

2. @Bean【导入的第三方包中的组件】

   方法参数的值从容器中获取。

3. @Import【快速给容器导入一个组件】

   1. @Import（要导入到容器中的组件）：容器中就会自动注册这个组件，id默认我全类型
   2. ImportSelector:返回需要导入的组件的全类名数组
   3. ImportBeanDefinitionRegistrar:手动注册bean到容器中

4. 使用Spring提供的FactoryBean（工程bean）

   1. 默认获取到的是工厂bean调用getObject创建的对象

      ```java
      applicationContext.getBean("colorFactorybean");
      ```

   2. 要获取工厂bean本身，需要在id前面加一个```&```

      ```java
      applicationContext.getBean("&colorFactorybean");
      ```

### @Conditional({Condition})

​	按照一定的条件进行判断，给容器注入满足条件的bean

```java
public class Person{
    private String name;
    private Integer age;
}
```

​	**主配置类**

```java
Public class MainConfig{

    @Conditional({WindowsCondition.class})
    @Bea1111n
    public Person Person1{
        return new Person("hym",18);
    }
    
    @Conditional({LinuxCondition.class})
    @Bean
    public Person Person1{
        return new Person("xf",18);
    }
}
```

​	**判断当前环境是不是windows**

```java
public class WindowsCondition implements Condition{
    public boolean matchs(ConditionContext context,AnnotatedTypeMetadata metadata){
		//获取当前环境信息
        Environment environment = context.getEnvironment();
        //获取当前操作系统的名称
        String property = environment.getProperty("os.name");
        if(property.contains("windows")){
            return true;
        }
        return false; 
    }
}
```

**判断当前环境是不是Linux**

```java
public class LinuxCondition implements Condition{
    public boolean matchs(ConditionContext context,AnnotatedTypeMetadata metadata){
        //判断是否是Linux系统
        //1.获取beanFactory
        ConfigurableListableBeanFactory beanFactory = context.getBeanFactory();
        //2.获取类加载器
        ClassLoader classLoader = context.getClassLoader();
        //3.获取当前环境信息
        Environment environment = context.getEnvironment();
        //4.获取bean定义的注册类
        BeanDefinitionRegistry registry = context.getRegistry();
        
        //获取当前操作系统的名称
        String property = environment.getProperty("os.name");
        if(property.contains("linux")){
            return true;
        }
        return false; 
    }
}
```

### @Import

​	导入的组件的```id```默认为全类名

​	**外部组件**

```java
public class Color{}

public class Red{}

public class Yellow{}
```

​	**导入外部组件**

```java
@Configuration
@Import({Color.class,Red.class,MyImportSelector.class，MyImportBeanDefinitionRegistrar.class})
public class MainConfig{
    
}
```

#### ImportSelector

​	**自定义组件导入选择器**

```java
public class MyImportSelector implements ImportSelector{
    
    //返回值，就是要导入到容器的组件全类名
    //AnnotationMetaData:当前标注@Import注解的类的所有注解信息
    @Override
    public String[] selectImports(AnnotationMetaData impotingClassMetaData){
        //返回值不能为null
        return new String[]{com.hym.bean.Yellow};
    }
}
```

#### ImportBeanDefinitionRegistrar

​	**自定义组件的注册器**

```java
public class MyImportBeanDefinitionRegistrar implements ImportBeanDefinitionRegistrar{
    @Override
    public void registerBeanDefinitions(AnnotationMetaData 			impotingClassMetaData,BeanDefinitionRegistry registry){
        //判断容器中是否有bean名为‘red’的bean
        boolean definition = registry.containBeanDefinition("red");
        if(definition){
            RootBeanDefinition beanDefinition = new RootBeanDefinition(Blue.class)
           	//blue为指定的bean名
            registry.registerBeanDefinition("blue",beanDefinition)
        }
    }
}
```

### FactoryBen

​	**自定义FactoryBean**

```java
public class ColorFactoryBean implements FactoryBean<Color>{
    //返回一个Color对象，这个对象会添加到容器中
    @Override
    public Color getObject() throws Exception{
        return new Color();
    }
    @Override
    public Class<?> getObjectType(){
        return Color.class;
    }
    //控制添加容器中的bean是不是单例
    @Override
    public boolean isSingleton(){
        return true;
    }
} 
```

### 二、bean的生命周期

> bean我的生命周期：
>
> ​	bean的创建 ---> 初始化 --->销毁的过程
>
> 容器管理bean的生命周期：
>
> ​	我们可以自定义初始化和销毁的方法,容器在bean进行到当前生命周期的时候来调用我们自定义的初始化和销毁方法
>
> ​	bean的创建时机：
>
> ​		单实例：在容器启动的时候创建对象
>
> ​		多实例：在每次获取的时候创建对象
>
> ​	初始化：
>
> ​		对象创建完成,并赋值,调用初始化方法...
>
> ​	销毁
>
> ​		单实例：在容器管理的时候。
>
> ​		多实例：容器不管理这个bean,即不会调用销毁方法。



#### 1、指定初始化和销毁

@Bean(initMethod="init",destroyMethod="destory")

**自定义类**

```java
public class Car{
    public Car(){
        sout("car Constructor...")
    }
    public void init(){
        sout("car init...")
    }
    public void destroy(){
        sout("car destroy...")
    }
}
```

**注入到容器**

```java
@Configuration
public class MainConfig{
    
    @Bean(initMethod="init",destroyMethod="destroy")
    public Car getCar(){
        return new Car();
    }
}
```

#### 2、通过Bean实现InitializingBean(定义初始化逻辑)、DisposableBean(定义销毁逻辑)

​	**自定义类**

```java
@Component
public class Cat implements InitializingBean,DisposableBean{
    public Cat(){
        System.out.println("cat constructor...")
    }
    
    @Override
    public void destroy() throw Exception{
        Sysout.out.println("cat destroy...")
    }
    
    @Override
    public void afterPropertiesSet() throw Exception{
        Sysout.out.println("cat afterPropertiesSet...")
    }
}
```

#### 3、可以使用JSR250：

​	@PostConstructor：在bean创建完成并赋值完成之后,来执行初始化方法

​	@PerDestroy：在容器销毁bean之前进行清理工作

```java
public class Dog{
    public Dog(){
        System.out.println("dog constructor...")
    }
    //创建对象并赋值完成之后调用
    @PostConstructor
    public void init() throw Exception{
        Sysout.out.println("dog @PostConstructo...")
    }
    //在容器销毁bean之前调用
    @PerDestroy
    public void destroy() throw Exception{
        Sysout.out.println("dog @PerDestroy...")
    }
}
```

#### 4、BeanPostProcessor：bean的后置处理器

​	在bean 初始化前后进行一些处理工作：

​		postProcessBeforeInitialization：在初始化之前调用

​		postProcessAfterInitialization：在初始化之后调用

```java
@Component	//将后置处理器加入到容器中
public class MyBeanPostProcessor implements BeanPostProcessor{
     @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        Sysout.out.println("postProcessBeforeInitialization..."+beanName+"=>"+bean)
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        Sysout.out.println("postProcessAfterInitialization..."+beanName+"=>"+bean)
        return bean;
    }
}
```

### 三、属性赋值

​	**主配置类**

```java
@Configuration
public class MainConfig{
    
    @Bean
    public Person person(){
        return new Person();
    }
}
```

#### 1、@Value赋值

> 1、基本数值
>
> 2、使用SpEL：#{}
>
> 3、使用${}:取出配置文件中的值，（在运行环境变量中的值）

```java
public class Person{
    @Value("hym")
    private String name;
    @Value("#{20-2}")
    private Integer age;
}
```

	##### @PropertySource（value={classpath:xxx.properties}）

​	读取外部配置文件中的K/V保存到运行的环境变量中

### 四、自动装配

> Spring利用DI完成对IOC容器中每个组件的依赖关系赋值

#### 1、@Autowired

```javaa
可以标注在构造器、参数、方法、属性:都是从容器中获取参数组件的值
	1、方法上：@Bean+方法参数：参数从容器中获取;默认不写@Autowired效果是一样的
	2、构造器上:如果组件只有一个有参构造器,这个有参构造器的@Autowired可以省略
	3、参数位置
```

> 1、默认优先按照类型去容器中找对应的组件。
>
> 2、如果找到了多个相同类型的组件，再将属性的名称作为组件的id去容器中查找	
>
> 3、@Qualifier("BeanId"),指定需要装配的主键的Id。
>
> 4、默认一定必须属性装配好,否则报错
>
> ​	可以使用@Autowired（required = false）修改为不一定必须装配成功
>
> 5、@Primary：让Spring进行自动装配的时候，默认使用首选的bean
>
> ​		也可以继续使用@Qualifier指定需要装配的bean的名字

```java
@Service
public class UserService{
    @Qualifier("userDao2")
    @Autowired(required = true)	//必须装配成功
	private UserDao userDao; 
}
```

```java
@Configuration
public class MainConfig{
    @Primary
    @Bean("UserDao")
    public UserDao userDao(){
        return new UserDao();
    }
    @Bean("UserDao2")
    public UserDao userDao2(){
        return new UserDao();
    }
}
```

#### 2、@Resource（JSR250）和@Inject(JSR330)[java规范的注解]

##### 2.1、@Resource

> 默认按照组件名称进行装配	
>
> 没有能支持@Primary和@Autowired（requested=false）

```java
@Service
public class UserService{
    @Resource(name="userDao2")
	private UserDao userDao; 
}
```

##### 2.2、@Inject

> ​	首先需要导入依赖
>
> ​	和@Autowired的功能一样，没有requested=false的功能

```java
<dependency>
    <groupId>javax.inject</groupId>
    <artifactId>javax.inject</artifactId>
    <version>1</version>
</dependency>
```

```java
@Service
public class UserService{
    @Inject
	private UserDao userDao; 
}
```

### 五、Profile

> Spring为我们提供的可以根据当前环境，动态的激活和切换一系列组件的功能

```java
@Configuration
public class MainConfig{
    
    @Profile("test")
    @Bean("test")
    public void test(){
        System.out.println("test");
    }
    
    @Profile("dev")
    @Bean("dev")
    public void dev(){
        System.out.println("dev");
    }
    
    @Profile("prod")
    @Bean("prod")
    public void prod(){
        System.out.println("prod");
    }
}
```

##### 激活指定的Profile

> 1. 在配置文件中指定：spring.profile.active = test/dev/prod
> 2. 在命令行： java -jar xxx.jar --spring.profile.active = test/dev/prod
> 3. 配置虚拟机参数：-Dspring.profile.active = test/dev/prod











