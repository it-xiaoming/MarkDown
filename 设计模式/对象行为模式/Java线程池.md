# Java线程池

### 一、ExecutorService介绍

>​		ExecutorService接口是java内置的线程池接口，通过学习接口中的方法，可以快速的掌握java内置线程池的基本使用。
>
>常用方法:

```java
void shutdown()	:启动一次顺序关闭，执行以前提交的任务，但不接收新任务
List<Runnable> shutdownNow() : 停止所有正在执行的任务，暂停处理正在等待的任务，并返回等待执行的任务列表
<T> Future<T> submit(Callable<T> task) : 执行待返回值的任务，返回一个Future对象
Future<?> submit(Runnable task) : 执行Runnable任务，并返回一个表示该任务的Future
<T> Future<T> submit(Runnale task,T result) : 执行Runnable任务，并返回一个表示该任务的Future
```

### 二、ExecutorService获取

>获取ExecutorService可以使用JDK中的Executors类中的静态方法，常用获取方式如下:

```java
static ExecutorService newCachedThreadPool() :
	创建一个默认的线程池对象，里面的线程可重用，且在第一次使用时才创建
```

```java
static ExecutorService newCachedThreadPool(ThreadFactory threadFactory)
    线程池中的所有线程都使用ThreadFactory来创建，这样的线程无需手动启动，自动执行
```

```java
static ExecutorService newFixedThreadPool(int nThreads)
    创建一个可重用固定线程数量的线程池
```

```java
static ExecutorService newFixedThreadPool(int nThreads,ThreadFactory threadFactory)
    创建一个可重用固定线程数量的线程池，且线程池中的所有线程都使用ThreadFactory来创建
```

```java
static ExecutorService newSingleThreadExecutor()
    创建一个使用单个worker线程的Executor，以无界队列方式来运行该线程
```

```java
static ExecutorService newSingleThreadExecutor(ThreadFactory threadFactory)
     创建一个使用单个worker线程的Executor，且线程池中的所有线程都使用ThreadFactory来创建
```

### 三、ScheduleExecutorService

> ScheduleExecutorService常用方法如下:

```java
<V> ScheduleFuture<V> schedule(Callable<V> callable,long delay,TimeUnit unit)
    延迟时间单位是unit，数量是delay的时间后执行callable。
```

```java
ScheduleFuture<?> schedule(Runnable command,long delay,TimeUnit unit)
    延迟时间单位是unit，数量是delay的时间后执行command。
```

```java
ScheduleFuture<V> scheduleAtFixedRate(
    						Runnable command,long initialDelay,long period,TimeUnit unit)
    延迟时间单位是unit，数量是initialDelay的时间后,每间隔period时间重复执行一次command
```

```java
ScheduleFuture<V> scheduleWithFixedDelay(
    						Runnable command,long initialDelay,long period,TimeUnit unit)
    创建并执行一个在给定初始延迟后首次启动的定期操作，随后，在每一次执行终止和下一次执行开始之间都存在给定的延迟。
```

### 四、ScheduleExecutorService获取

> ScheduleExecutorServcie是ExecutorService的子接口，具备了延迟运行或定期执行任务的能力
>
> 常用获取方法如下:

```java
static ScheduleExecutorService newScheduledThreadPool(int corePoolSize)
    创建一个可重用固定线程数的线程池且允许延迟运行或定期执行任务。
```

```java
static ScheduleExecutorService newScheduledThreadPool(int corePoolSize,ThreadFactory fac)
    创建一个可重用固定线程数的线程池,且线程池中的所有线程都使用ThreadFactory来创建，且允许延迟运行或定期执行任务。
```

```java
static ScheduleExecutorService newSinglethreadScheduledExecutor()
    创建一个单线程执行程序，它允许在给定延迟后运行命令或定期执行
```

```java
static ScheduleExecutorService newSinglethreadScheduledExecutor(ThreadFactory factory)
    创建一个单线程执行程序，它允许在给定延迟后运行命令或定期执行,且线程池中的所有线程使用factory创建。
```

### 五、Future

> 异步计算结果：我们可以通过Future对象获取线程计算的结果。

> Future的常用方法:

```java
boolean cancel(boolean mayInterruptIfRunning)
    试图取消对此任务的执行
```

```java
V get()
    如果必要，等待计算完成，然后获取其结果
```

```java
V get(long timeout,TimeUnit unit)
    如果必要,最多等待为：使计算完成
```

