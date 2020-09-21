## Day 01

## 1、String、StringBuilder、StringBuffer的区别

```
1.String对象是不可变的,StringBuilder和StringBuffer是可变的
2.String和StringBuffer是线程安全的,StringBuilder是线程不安全的
```

## 2、面向对象语言的特点

```
1.封装：
2.继承
3.多态
```

## 3、java有哪些数据结构

- 基本数据类型
  - 数值型
    - 整型	 byte  short  int long
    - 浮点型 float double
  - 布尔型 blooean
  - 字符型 char
- 引用数据类型
  - 类
  - 接口
  - 数组
  - 枚举

## 4、final有什么作用

```
1.final修饰类:该类为最终类不能被继承
2.final修饰方法:该方法不能被重写
3.final修饰变量:即为常量，只能赋值一次
```

## 5、final、finally、finalize的区别

- final：可以修饰类、变量、方法，修饰类表示该类不能被继承、修饰方法表示该方法不能被重写、修饰变量表
  示该变量是一个常量不能被重新赋值。
- finally一般和try-catch代码块一起使用，一般把不管是否出现异常都必须执行的代码放在finally中，例如IO操作的关闭流操作
- finalize是Object类中的方法，当调用System.gc()方法的时候,垃圾回收器会自动调用finalize()方法。



## Day 02



