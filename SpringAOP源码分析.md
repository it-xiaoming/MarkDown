# SpringAOP源码分析

### 一、SpringAop开发三部曲

> 1、导入依赖

```xml
<dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aspects</artifactId>
      <version>4.3.2.RELEASE</version>
    </dependency>
```

> 2、编写切面类

```java
package com.hym.aspect;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.*;

@Aspect
public class MyAspect {

    //切点
    @Pointcut("execution(public * com.hym.service.BookService.*(..))")
    public void MyCutPoint(){

    }
	//前置通知
    @Before("MyCutPoint()")
    public void before(JoinPoint joinPoint){
        System.out.println(joinPoint.getSignature().getName()+"()执行前置通知");
    }
	//后置通知
    @After("MyCutPoint()")
    public void after(JoinPoint joinPoint){
        System.out.println(joinPoint.getSignature().getName()+"()执行后置通知");
    }
	//最终通知
    @AfterReturning("MyCutPoint()")
    public void afterReturning(JoinPoint joinPoint){
        System.out.println(joinPoint.getSignature().getName()+"()执行最终通知");
    }
	//异常通知
    @AfterThrowing("MyCutPoint()")
    public void afterThrowing(JoinPoint joinPoint){
        System.out.println(joinPoint.getSignature().getName()+"()执行异常通知");
    }
}
```

> 3、在配置类上添加@EnableAspectJAutoProxy开启AOP支持

```java
@Configuration
@EnableAspectJAutoProxy
public class MainConfogOfAOP {
}
```

### 二、@EnableAspectJAutoProxy分析

> 1、主要使用AspectJAutoProxyRegistrar自定义给容器中注册Bean
>
> ​	internalAutoProxyCreator = AnnotationAwareAspectJAutoProxyCreator

```java
@Import(AspectJAutoProxyRegistrar.class)
public @interface EnableAspectJAutoProxy
```

### 三、AnnotationAwareAspectJAutoProxyCreator的继承关系

> 主要是实现了【BeanPostProcessor后置处理器】和【BeanfactoryAware】

```java
AnnotationAwareAspectJAutoProxyCreator extends AspectJAwareAdvisorAutoProxyCreator
    
AspectJAwareAdvisorAutoProxyCreator extends AbstractAdvisorAutoProxyCreator
    
AbstractAdvisorAutoProxyCreator extends AbstractAutoProxyCreator
    
AbstractAutoProxyCreator extends ProxyProcessorSupport
		implements SmartInstantiationAwareBeanPostProcessor, BeanFactoryAware
```

#### 1、关注后置处理器和自动装配BeanFactory

> 后置处理器：BeanPostProcessor主要在bean初始化的前后做一些事情

**AbstractAutoProxyCreator**

> 1、**AbstractAutoProxyCreator.setBeanFactory();**
>
> 2、**AbstractAutoProxyCreator.postProcessBeforeInstantiation**()
>
> 3、**AbstractAutoProxyCreator.postProcessAfterInitialization()**

**AbstractAdvisorAutoProxyCreator**

> 1、**AbstractAdvisorAutoProxyCreator.setBeanFactory(); >> initBeanFactory()**

**AnnotationAwareAspectJAutoProxyCreator**

> 1、**AnnotationAwareAspectJAutoProxyCreator.initBeanFactory()**

#### 2、流程

1. 传入配置类，创建IOC容器

2. 注册配置类，调用refresh()刷新容器

3. registerBeanPostProcessors(beanFactory);注册bean的后置处理器来拦截bean的创建

   1. 先获取ioc容器中已经定义了的需要创建对象的所有BeanPostProcessor

   2. 给容器中加别的BeanPostProcessor

   3. 优先注册实现了PriorityOrdered接口的BeanPostProcessor

   4. 在注册实现了Ordered接口的BeanPostProcessor

   5. 注册没有实现优先级接口的BeanPostProcessor

   6. 注册BeanPostProcessor实际上就是创建BeanPostProcessor的对象保存到容器中

      创建internalAutoProxyCreator 的BeanPostProcessor【AnnotationAwareAspectJAutoProxyCreator】

      1. createBeanInstance():创建bean的实例
      2. populateBean()：给bean的属性赋值
      3. initializeBean():初始化bean
         1. invokeAwareMethods():处理Aware接口的方法回调
         2. applyBeanPostProcessorsAfterInitialization():应用后置处理器的BeforeInitialization()方法

   