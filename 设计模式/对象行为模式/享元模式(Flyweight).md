# 享元模式(Flyweight)

### 一、定义及特点

>​		享元（Flyweight）模式的定义：运用共享技术来有効地支持大量细粒度对象的复用。它通过共享已经存在的对象来大幅度减少需要创建的对象数量，避免大量相似类的开销，从而提高系统资源的利用率。
>
>　　String常量池、数据库连接池、缓冲池等等都是享元模式的应用，所以说享元模式是池技术的重要实现方式。

### 二、优缺点

>享元模式的主要优点是：
>
>- 相同对象只要保存一份，这降低了系统中对象的数量，从而降低了系统中细粒度对象给内存带来的压力。
>
>其主要缺点是：
>
>- 为了使对象可以共享，需要将一些不能共享的状态外部化，这将增加程序的复杂性。
>- 读取享元模式的外部状态会使得运行时间稍微变长。

### 三、结构

>享元模式中存在以下两种状态：
>
>- 内部状态，即不会随着环境的改变而改变的可共享部分；
>- 外部状态，指随环境改变而改变的不可以共享的部分。享元模式的实现要领就是区分应用中的这两种状态，并将外部状态外部化。下面来分析其基本结构和实现方法。
>
>享元模式的主要角色有如下。
>
>- 抽象享元角色（Flyweight）:是所有的具体享元类的基类，为具体享元规范需要实现的公共接口，非享元的外部状态以参数的形式通过方法传入。
>- 具体享元（Concrete Flyweight）角色：实现抽象享元角色中所规定的接口。
>- 非享元（Unsharable Flyweight)角色：是不可以共享的外部状态，它以参数的形式注入具体享元的相关方法中。
>- 享元工厂（Flyweight Factory）角色：负责创建和管理享元角色。当客户对象请求一个享元对象时，享元工厂检査系统中是否存在符合要求的享元对象，如果存在则提供给客户；如果不存在的话，则创建一个新的享元对象。

### 四、代码实例

#### 抽象享元类

```java
public interface Flyweight{
    public void operation(UnsharedConcreteFlyweight state);
}
```

#### 具体享元类

```java
public class ConcreteFlyweight implements Flyweight{
    private String key;
    ConcreteFlyweight(String key){
        this.key=key;
        System.out.println("具体享元"+key+"被创建！");
    }
    public void operation(UnsharedConcreteFlyweight outState){
        System.out.print("具体享元"+key+"被调用，");
        System.out.println("非享元信息是:"+outState.getInfo());
    }
}
```

#### 非享元类

```java
public class UnsharedConcreteFlyweight{
    private String info;
    UnsharedConcreteFlyweight(String info){
        this.info=info;
    }
    public String getInfo(){
        return info;
    }
    public void setInfo(String info){
        this.info=info;
    }
}
```

#### 享元工厂类

```java
public class FlyweightFactory{
    private HashMap<String, Flyweight> flyweights
        									=new HashMap<String, Flyweight>();
    public Flyweight getFlyweight(String key){
        Flyweight flyweight=(Flyweight)flyweights.get(key);
        if(flyweight!=null){
            System.out.println("具体享元"+key+"已经存在，被成功获取！");
        }
        else{
            flyweight=new ConcreteFlyweight(key);
            flyweights.put(key, flyweight);
        }
        return flyweight;
    }
}
```

#### 客户端

```java
public class FlyweightPattern{
    public static void main(String[] args)
        FlyweightFactory factory=new FlyweightFactory();
        Flyweight f01=factory.getFlyweight("a");
        Flyweight f02=factory.getFlyweight("a");
        Flyweight f03=factory.getFlyweight("a");
        Flyweight f11=factory.getFlyweight("b");
        Flyweight f12=factory.getFlyweight("b");
        f01.operation(new UnsharedConcreteFlyweight("第1次调用a。"));
        f02.operation(new UnsharedConcreteFlyweight("第2次调用a。"));
        f03.operation(new UnsharedConcreteFlyweight("第3次调用a。"));
        f11.operation(new UnsharedConcreteFlyweight("第1次调用b。"));
        f12.operation(new UnsharedConcreteFlyweight("第2次调用b。"));
    }
}
```

### 五、应用场景

>- 系统中存在大量相同或相似的对象，这些对象耗费大量的内存资源。
>- 大部分的对象可以按照内部状态进行分组，且可将不同部分外部化，这样每一个组只需保存一个内部状态。
>- 由于享元模式需要额外维护一个保存享元的数据结构，所以应当在有足够多的享元实例时才值得使用享元模式。

