# Spring之IOC容器创建过程

### refresh()调用过程

1. prepareRefresh();	//容器刷新前准备

   1. this.scanner.clearCache();	//清除所有缓存资源

   2. initPropertySources();	//初始化任何占位符属性源;**Spring未做任何处理，交由子容器实现。**

   3. //验证标记为所需的所有属性是否可解析

      getEnvironment().validateRequiredProperties()

   4. //创建一个收集早期应用程序事件的容器

      this.earlyApplicationEvents = new LinkedHashSet<ApplicationEvent>();

2. 告诉子类刷新内部bean工厂

   ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory()；

   1. refreshBeanFactory();
      1. this.beanFactory.setSerializationId(getId()); //给beanFactory一个序列化Id
   2. 









```java
public void refresh() throws BeansException, IllegalStateException {
		synchronized (this.startupShutdownMonitor) {
			// Prepare this context for refreshing.
			prepareRefresh();

			// Tell the subclass to refresh the internal bean factory.
			ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();

			// Prepare the bean factory for use in this context.
			prepareBeanFactory(beanFactory);

			try {
				// Allows post-processing of the bean factory in context subclasses.
				postProcessBeanFactory(beanFactory);

				// Invoke factory processors registered as beans in the context.
				invokeBeanFactoryPostProcessors(beanFactory);

				// Register bean processors that intercept bean creation.
				registerBeanPostProcessors(beanFactory);

				// Initialize message source for this context.
				initMessageSource();

				// Initialize event multicaster for this context.
				initApplicationEventMulticaster();

				// Initialize other special beans in specific context subclasses.
				onRefresh();

				// Check for listener beans and register them.
				registerListeners();

				// Instantiate all remaining (non-lazy-init) singletons.
				finishBeanFactoryInitialization(beanFactory);

				// Last step: publish corresponding event.
				finishRefresh();
			}

			catch (BeansException ex) {
				if (logger.isWarnEnabled()) {
					logger.warn("Exception encountered during context initialization - " +
							"cancelling refresh attempt: " + ex);
				}

				// Destroy already created singletons to avoid dangling resources.
				destroyBeans();

				// Reset 'active' flag.
				cancelRefresh(ex);

				// Propagate exception to caller.
				throw ex;
			}

			finally {
				// Reset common introspection caches in Spring's core, since we
				// might not ever need metadata for singleton beans anymore...
				resetCommonCaches();
			}
		}
	}
```

