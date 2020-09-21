# 外观模式(Facade)

### 一、定义

>​		外观（Facade）模式的定义：又叫门面模式，是一种通过**为多个复杂的子系统提供一个一致的接口**，而使这些子系统更加容易被访问的模式。该模式对外有一个统一接口，外部应用程序不用关心内部子系统的具体的细节，这样会大大降低应用程序的复杂度，提高了程序的可维护性。

### 二、优缺点

>**优点：**
>
>- 简化了调用过程，无需了解深入子系统，防止带来风险
>- 减少系统依赖、松散耦合
>- 更好的划分访问层次
>- 符合迪米特法则，即最少知道原则
>
>**缺点：**
>
>- 增加子系统、扩展子系统行为容易引入风险
>- 不符合开闭原则

### 三、实现

>​		外观（Facade）模式的结构比较简单，主要是定义了一个高层接口。它包含了对各个子系统的引用，客户端可以通过它访问各个子系统的功能。现在来分析其基本结构和实现方法。
>
>　　外观（Facade）模式包含以下主要角色。
>
>- 外观（Facade）角色：为多个子系统对外提供一个共同的接口。
>- 子系统（Sub System）角色：实现系统的部分功能，客户可以通过外观角色访问它。
>- 客户（Client）角色：通过一个外观角色访问各个子系统的功能。　　　　　　　　　　　　　　

#### 1、所有子系统实现统一接口

##### 系统顶级接口

```java
public interface System{
    public void dosomething();
}
```

##### 子系统

```java
public class SystemA implements System{
    public void dosomething(){
        System.out.println("子系统方法A");
    }
}

public class SystemB implements System{
    public void dosomething(){
        System.out.println("子系统方法B");
    }
}
```

##### 外观类

```java
public class Facade {
    //被委托的对象
    System a,b;
    
    public Facade() {
        a = new SubSystemA();
        b = new SubSystemB();
    }
    
    //提供给外部访问的方法
    public void methodA() {
        this.a.dosomething();
    }
    
    public void methodB() {
        this.b.dosomething();
    }
}
```

##### 客户端

```java
public class Client {
    public static void main(String[] args) {
        Facade facade = new Facade();
        facade.methodA();
        facade.methodB();
    }
}
```

#### 2、未实现统一接口

##### 子系统类

```java
public class SystemA{
    public void dosomething(){
        System.out.println("子系统方法A");
    }
}

public class SystemB{
    public void dosomething(){
        System.out.println("子系统方法B");
    }
}
```

##### 外观类

```java
public class Facade {

    //被委托的对象
    SubSystemA a;
    SubSystemB b;
    
    public Facade() {
        a = new SubSystemA();
        b = new SubSystemB();
    }
    
    //提供给外部访问的方法
    public void methodA() {
        this.a.dosomethingA();
    }
    
    public void methodB() {
        this.b.dosomethingB();
    }
}
```

##### 客户端

```java
public class Client {
    public static void main(String[] args) {
        Facade facade = new Facade();
        facade.methodA();
        facade.methodB();
    }
}
```

### 四、应用场景

>- 子系统越来越复杂，增加外观模式提供简单接口调用
>- 构建多层系统结构，利用外观对象作为每层的入口，简化层间调用