# 中介者（Mediator）模式

### 一、定义

> ​		中介者（Mediator）模式的定义：定义一个中介对象来封装一系列对象之间的交互，使原有对象之间的耦合松散，且可以独立地改变它们之间的交互。中介者模式又叫**`调停模式`**，它是迪米特法则的典型应用。

### 二、优缺点

> 优点：
>
> - 降低了对象之间的耦合性，使得对象易于独立地被复用。
> - 将对象间的一对多关联转变为一对一的关联，提高系统的灵活性，使得系统易于维护和扩展。
>
> 缺点：
>
> - 当同事类太多时，中介者的职责将很大，它会变得复杂而庞大，以至于系统难以维护。

### 三、结构

> - 抽象中介者（Mediator）角色：它是中介者的接口，提供了同事对象注册与转发同事对象信息的抽象方法。
> - 具体中介者（ConcreteMediator）角色：实现中介者接口，定义一个 List 来管理同事对象，协调各个同事角色之间的交互关系，因此它依赖于同事角色。
> - 抽象同事类（Colleague）角色：定义同事类的接口，保存中介者对象，提供同事对象交互的抽象方法，实现所有相互影响的同事类的公共功能。
> - 具体同事类（Concrete Colleague）角色：是抽象同事类的实现者，当需要与其他同事对象交互时，由中介者对象负责后续的交互。

### 四、代码实现

#### 抽象中介类

```java
public abstract class Mediator{
    public abstract void register(Colleague colleague);
    public abstract void relay(Colleague colleague); //转发
}
```

#### 具体中介类

```java
public class ConcreteMediator extends Mediator{
    private List<Colleague> colleagues=new ArrayList<Colleague>();
    public void register(Colleague colleague){
        if(!colleagues.contains(colleague)){
            colleagues.add(colleague);
            colleague.setMedium(this);
        }
    }
    public void relay(Colleague colleague){
        for(Colleague ob:colleagues){
            if(!ob.equals(colleague)){
                ((Colleague)ob).receive();
            }   
        }
    }
}
```

#### 抽象同事类

```java
public abstract class Colleague{
    protected Mediator mediator;
    public void setMedium(Mediator mediator){
        this.mediator=mediator;
    }   
    public abstract void receive();   
    public abstract void send();
}
```

#### 具体同事类

```java
//具体同事类1
public class ConcreteColleague1 extends Colleague{
    public void receive(){
        System.out.println("具体同事类1收到请求。");
    }   
    public void send(){
        System.out.println("具体同事类1发出请求。");
        mediator.relay(this); //请中介者转发
    }
}
//具体同事类2
public class ConcreteColleague2 extends Colleague{
    public void receive(){
        System.out.println("具体同事类2收到请求。");
    }   
    public void send(){
        System.out.println("具体同事类2发出请求。");
        mediator.relay(this); //请中介者转发
    }
}
```

#### 客户端

```java
public class MediatorPattern{
    public static void main(String[] args){
        Mediator md=new ConcreteMediator();
        Colleague c1,c2;
        c1=new ConcreteColleague1();
        c2=new ConcreteColleague2();
        md.register(c1);
        md.register(c2);
        c1.send();
        System.out.println("-------------");
        c2.send();
    }
}
```

### 五、应用场景

>- 当对象之间存在复杂的网状结构关系而导致依赖关系混乱且难以复用时。
>- 当想创建一个运行于多个类之间的对象，又不想生成新的子类时。