# java8新特性之lambda表达式

### 一、Lambda表达式

#### 1、特点

> Lambda是一个`匿名函数`，是一段可以传递的代码，可以写出更简介、更灵活的代码。

#### 2、基本语法

>1、java8中引入了一个新的操作符`->`,该操作符称为箭头操作符或Lambda表达式
>
>​	左侧： Lambda 表达式的参数列表
>
>​	右侧：Lambda 表达式中所需要执行的功能，即Lambda体
>
>2、Lambda表达式的参数列表的数据类型可以省略不写，因为JVM编译器通过上下文推断出`数据类型`即类型推断

#### 3、语法格式

>1、无参数，无返回值，函数体只有一条语句【`{}`可以省略】

```java
() -> System.out.println("hello Lambda!")
```

> 2、有一个参数【`()`可以省略】，无返回值

```java
i -> System.out.println(i)
```

> 3、多个参数，无返回值

```java
(x,y) -> System.out.println(x+y)
```

### 二、函数式接口

#### 1、定义

>接口中只有一个抽象方法的接口
>
>可以使用@FunctionalInterface 修饰，可以检查是否是函数式接口

#### 2、 java8内置的四大核心函数式接口

##### 1、消费型接口【Consumer】

```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}
```

##### 2、供给型接口【Supplier】

```java
@FunctionalInterface
public interface Supplier<T> {
    T get();
}
```

##### 3、函数型接口【Function】

```java
@FunctionalInterface
public interface Function<T, R> {

    R apply(T t);

    default <V> Function<V, R> compose(Function<? super V, ? extends T> before) {
        Objects.requireNonNull(before);
        return (V v) -> apply(before.apply(v));
    }

    default <V> Function<T, V> andThen(Function<? super R, ? extends V> after) {
        Objects.requireNonNull(after);
        return (T t) -> after.apply(apply(t));
    }

    static <T> Function<T, T> identity() {
        return t -> t;
    }
}
```

##### 4、断言型接口【Predicate】

```java
@FunctionalInterface
public interface Predicate<T> {

    boolean test(T t);

    default Predicate<T> and(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) && other.test(t);
    }

    default Predicate<T> negate() {
        return (t) -> !test(t);
    }

    default Predicate<T> or(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) || other.test(t);
    }

    static <T> Predicate<T> isEqual(Object targetRef) {
        return (null == targetRef)
                ? Objects::isNull
                : object -> targetRef.equals(object);
    }
}
```

### 三、方法引用

>如 Lambda 体中的内容有的方法已经实现，我们可以使用`方法引用`

#### 1、语法格式

>- 对象::实例方法
>- 类::静态方法
>- 类::实例方法【当第一个参数是方法的调用者，第二个参数是方法的参数】

### 四、Stream

#### 1、获取Stream流的四中方式

>1、通过Collection 提供的stream()和paralleStream()

```java
List<String> list = new ArrayList<>();
Stream<String> stream = list.stream()
```

>2、通过Arrays 中的静态方法 stream() 获取数组流

```java
Integer[] arrays = new Integer[10];
Stream<Integer> stream = Arrays.stream(arrays);
```

>3、通过 Stream 中的静态方法 of()

```java
Stream<String> stream = Stream.of("a","b","c")
```

>4、创建无限流

```java
//迭代
Stream.iterate(0,(x) -> x+2)
//生成
Stream.generate(() -> Math.random())
```

#### 2、筛选和切片

```java
List<Employee> list = Arrays.asList(
    new Employee("张三",18,9999.99, Status.FREE),
    new Employee("李四",22,2222.22, Status.BUSY),
    new Employee("王五",35,5555.55, Status.REST),
    new Employee("赵六",66,6666.66, Status.FREE),
    new Employee("田七",7,7777.77, Status.BUSY)
);
```

##### filter

> Stream<T> filter(Predicate<? super T> predicate)		//从流中排除某些元素

```java
//获取age > 20 的
Stream<Emplotee> stream = list.stream()
    						.filter((x) -> x.getAge() > 20)
    						.forEach(System.out::println)
```

##### limit

>Stream<T> limit(long maxSize);		//获取到的元素不超过定的数量:maxSize

```java
//获取 salary > 3000 的前2个
Stream<Emplotee> stream = list.stream()
    						.filter((x) -> x.getSalary() > 3000)
    						.limit(2)
    						.forEach(System.out::println)
```

##### skip

> Stream<T> skip(long n);	//返回第n个以后的所有元素 如果没有元素，则返回空流

```java
//从第3个开始获取(即跳过前两个)
Stream<Emplotee> stream = list.stream()
    						.skip(2)
    						.forEach(System.out::println)
```

##### distinct()

> Stream<T> distinct();    // 筛选，通过流所生成的元素的hashCode()和equals()去除重复元素

```java
Stream<Emplotee> stream = list.stream()	
    						.distinct()
    						.forEach(System.out::println)
```

#### 3、映射

##### map()

><R> Stream<R> map(Function<? super T, ? extends R> mapper);
>
>​	将元素转换成其他形式或提取信息。接收一个函数作为参数，该函数会作用在每一个元素上，并将其映射成一个新元素。

```java
Stream<String> Stream = list.stream()
    							.map(Employee::getName);
								.forEach(System.out::println);
```

#### flatMap()

><R> Stream<R> flatMap(Function<? super T, ? extends Stream<? extends R>> mapper);
>
>​	接收一个函数作为参数，将流中的内个元素都转换为一个流，然后把所有流连接成一个流。

#### 4、排序

##### sorted()

>自然排序(Comparable)

```java
list.stream().sorted().forEach(System.out::println);
```

##### sorted(Comparator comparator)

> 定制排序

```java
list.stream()
    .sorted((v1,v2) -> {
        if(v1.getAge().equals(V2.getAge())){
            return v1.getName().compareTo(v2.getName());
        }else{
            return v1.getAge().comparaTo(v2.getAge())
        }
    })
    .forEach(System.out::println);
```

#### 5、查找与匹配

##### allMatch

>检查是否匹配所有元素

##### anyMatch

> 检查是否至少匹配一个元素

##### noneMatch

> 检查是否没有匹配所有元素

##### findFirst

> 返回第一个元素

##### findAny

> 返回当前流中的任意元素

##### count

> 返回流中元素的总个数

##### max

> 返回流中最大值

##### min

> 返回流中最小值

#### 6、规约

##### reduce

> 将流中元素反复结合起来，得到一个值

```java
list.stream()
    	.reduce(0,(v1,v2) -> v1.getSalary()+v2.getSalary())
```

#### 7、收集

##### collect

> 将流中的元素做汇中

###### 总数	Collectors.counting()

```java
list.stream()
    .collect(Collectors.counting())
```

###### 平均值	Collectors.averagingDouble()

```java
list.stream()
    .collect(Collectors.averagingDouble(Employee::getSalary))
```

###### 总和	Collectors.summingDouble()

```java
list.stream()
    .collect(Collectors.summingDouble(Employee::getSalary))
```

###### 最大值 Colletors.maxBy()

```java
list.stream()
    .collect(Colletors.maxBy((v1,v2) -> v1.getAge().comparaTo(v1.getAge)))
```

###### 最小值 Colletors.minBy()

```java
list.stream()
    .collect(Colletors.minBy((v1,v2) -> v1.getAge().comparaTo(v1.getAge)))
```

###### 分组

```java
//根据状态进行分组
Map<Status,List<Employee>> map =list.stream()
    								.collect(Collectors.groupingBy(Employee::getStatus))
```

###### 多级分组

```java
Map<Status,List<Employee>> map = 
    list.stream()
        .collect(Collectors.groupingBy(Employee::getStatus,Collectors.groupingBy((v) ->{
            if(v.getAge() < 18){
                return "青年";
            }else if(v.getAge() < 40){
                return "中年";
            }else{
                retun "老年";
            }
        })));
```

###### 分区

```java
Map<Boolean,List<Employee>> map =
    list.stream()
    .collect(Collectors.parttioningBy((e) -> e.getAge() > 20);
```

