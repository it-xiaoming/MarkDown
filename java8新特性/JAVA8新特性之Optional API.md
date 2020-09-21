# JAVA8新特性之Optional API

### 一、概念

> Optional<T>  类（java.util.Optional）是一个容器，代表一个值存在或不存在，原来用null标识一个值不存在，现在Optional可以更好的表达这个概念。并且可以避免空指针异常。

### 二、常用方法

```java
<T> Optional<T> of(T t) : 创建一个Optional实例
<T> Optional<T> empty() : 创建一个空的Optional实例
<T> Optional<T> ofNullable(T t) : 如果t不为null，创建Optional实例，否则创建空实例
boolean isPresent() : 判断是否包含值
T orElse(T t) : 如果调用对象包含值，返回该值，否则返回t
T orElseGet(Supplier s) : 如果调用对象包含值，返回该值，否则返回s获取的值
map(Function f) : 如果有值对其处理，并返回处理后的Optional，否则返回Optional。empty（）
flatMap(Function f) : 与map类似，要求返回值必须是Optioanal。
```

