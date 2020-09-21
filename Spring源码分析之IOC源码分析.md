# Spring源码分析之IOC源码分析

### 一、BeanPostProcessor

> BeanPostProcessor:bean的后置处理器，顶级接口
>
> ​	作用：在bean初始化的前后做一些事情

```java
public interface BeanPostProcessor {
    /**
    *	在bean初始化之前调用
    **/
	Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException;
    
    /**
    *	在bean初始化之后调用
    **/
	Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException;

}

```

### 二、BeanDefinitionRegistryPostProcessor

#### 1、继承关系及抽象方法定义

```java
public interface BeanDefinitionRegistryPostProcessor extends BeanFactoryPostProcessor {

	/**
	 * Modify the application context's internal bean definition registry after its
	 * standard initialization. All regular bean definitions will have been loaded,
	 * but no beans will have been instantiated yet. This allows for adding further
	 * bean definitions before the next post-processing phase kicks in.
	 * @param registry the bean definition registry used by the application context
	 * @throws org.springframework.beans.BeansException in case of errors
	 */
	void postProcessBeanDefinitionRegistry(BeanDefinitionRegistry registry) throws BeansException;

}
```

```java
public interface BeanFactoryPostProcessor {

	/**
	 * Modify the application context's internal bean factory after its standard
	 * initialization. All bean definitions will have been loaded, but no beans
	 * will have been instantiated yet. This allows for overriding or adding
	 * properties even to eager-initializing beans.
	 * @param beanFactory the bean factory used by the application context
	 * @throws org.springframework.beans.BeansException in case of errors
	 */
	void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException;

}
```

#### 2、执行流程

> 1、加载配置类创建IOC容器
>
> 2、调用refresh()  =>  invokeBeanFactoryPostProcessors(beanFactory);
>
> 3、从容器中拿到所有的BeanDefinitionRegistryPostProcessor组件
>
> ​	1、依次触发所有的postProcessBeanDefinitionRegistry()方法
>
> ​	2、再来触发postProcessBeanFactory()方法
>
> 4、然后触发不是BeanDefinitionRegistryPostProcessor类型的beanFactoryPostProcessor组件的postProcessBeanFactory()方法

### 三、ApplicationListener

> 监听容器中发布的事件。事件驱动模型开发
>
> ​	监听ApplicationEvent及其子类的事件

```java
public interface ApplicationListener<E extends ApplicationEvent> extends EventListener {

	/**
	 * Handle an application event.
	 * @param event the event to respond to
	 */
	void onApplicationEvent(E event);

}
```

#### 开发步骤

> 1. 写一个监听来监听某个事件(Applicationevent及其子类)
> 2. 把监听器加入到容器中
> 3. 只要容器中有相关事件的发布，我们就能监听到这个事件。
> 4. 发布一个事件

### 四、

