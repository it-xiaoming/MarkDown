# 命令（command）模式

### 一、定义

>​	命令（Command）模式的定义如下：将一个请求封装为一个对象，使发出请求的责任和执行请求的责任分割开。这样两者之间通过命令对象进行沟通，这样方便将命令对象进行储存、传递、调用、增加与管理。

### 二、优缺点

>优点：
>
>- 降低系统的耦合度。命令模式能将调用操作的对象与实现该操作的对象解耦。
>- 增加或删除命令非常方便。采用命令模式增加与删除命令不会影响其他类，它满足“开闭原则”，对扩展比较灵活。
>- 可以实现宏命令。命令模式可以与组合模式结合，将多个命令装配成一个组合命令，即宏命令。
>- 方便实现 Undo 和 Redo 操作。命令模式可以与后面介绍的备忘录模式结合，实现命令的撤销与恢复。
>
>缺点：
>
>- 可能产生大量具体命令类。因为计对每一个具体操作都需要设计一个具体命令类，这将增加系统的复杂性。

### 三、结构

>- 抽象命令类（Command）角色：声明执行命令的接口，拥有执行命令的抽象方法 execute()。
>- 具体命令角色（Concrete  Command）角色：是抽象命令类的具体实现类，它拥有接收者对象，并通过调用接收者的功能来完成命令要执行的操作。
>- 实现者/接收者（Receiver）角色：执行命令功能的相关操作，是具体命令对象业务的真正实现者。
>- 调用者/请求者（Invoker）角色：是请求的发送者，它通常拥有很多的命令对象，并通过访问命令对象来执行相关请求，它不直接访问接收者。

### 四、代码实现

#### 抽象命令类（Command）

```java
interface Command{
    public abstract void execute();
}
```

#### 具体命令类（Concrete  Command）

```java
public class ConcreteCommand implements Command{
    private Receiver receiver;
    ConcreteCommand(){
        receiver=new Receiver();
    }
    public void execute(){
        receiver.action();
    }
}
```

#### 请求者类

```java
public class Invoker{
    private Command command;
    public Invoker(Command command){
        this.command=command;
    }
    public void setCommand(Command command){
        this.command=command;
    }
    public void call(){
        System.out.println("调用者执行命令command...");
        command.execute();
    }
}
```

#### 接收者类

```java
public class Receiver{
    public void action(){
        System.out.println("接收者的action()方法被调用...");
    }
}
```

#### 客户端

```java
public class CommandPattern{
    public static void main(String[] args){
        Command cmd=new ConcreteCommand();
        Invoker ir=new Invoker(cmd);
        System.out.println("客户访问调用者的call()方法...");
        ir.call();
    }
}
```

### 五、应用场景

>- 当系统需要将请求调用者与请求接收者解耦时，命令模式使得调用者和接收者不直接交互。
>- 当系统需要随机请求命令或经常增加或删除命令时，命令模式比较方便实现这些功能。
>- 当系统需要执行一组操作时，命令模式可以定义宏命令来实现该功能。
>- 当系统需要支持命令的撤销（Undo）操作和恢复（Redo）操作时，可以将命令对象存储起来，采用备忘录模式来实现。