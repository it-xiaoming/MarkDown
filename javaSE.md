# 							javaSE

##### 1、[字符、字符集、字符编码的区别](https://blog.csdn.net/weixin_42627035/article/details/87168582)

```
字符：各种文字和符号的总称
字符集：系统支持的所有抽象字符的集合。通常以二维表的形式存在，二维表的内容和大小是由使用者的语言而定。如			ASCII,GBxxx,Unicode等。
字符编码:把字符集中的字符编码为特定的二进制数，以便在计算机中存储。
		每个字符集中的字符都对应一个唯一的二进制编码。
```

##### 2、是否存在i+1<i的数：

```
存在:假如i为int类型，当i为int类型的最大值时，i+1就小于i了
```

##### 3、是否存在i-1>i的数:

```
存在:假如i为int类型，当i为int类型的最小值时，i-1就大于i了
```

##### 4、[是否存在 i > j || i <=j 不成立的值]:

```
存在:  i = Double.NaN j = Doubel.NaN; 或者Float.NaN
```

##### 5、在一个类中可以定义一个和类名相同额的非构造函数的方法吗

```
可以	class A{
		public A()}{}	//构造函数
		public void A(){}	//普通方法
	}
```

##### 6、java中创建对象的方式

```
1、通过new关键字直接创建	(会执行构造函数)
2、通过反射获取类的字节码对象,通过字节码对象的newInstance()方法创建对象	(会执行构造函数)
3、通过
4、
```

##### 7、当所在的方法的形参需要被匿名内部类使用时，为什么必须声明为 final。

```
保证参数值传递的一致性
```

##### 8、42==42.0的结果为什么

```
true
```

##### 9、包装类型的比较

```
当 "=="运算符的两个操作数都是 包装器类型的引用，则是比较指向的是否是同一个对象，而如果其中有一个操作数是表达式（即包含算术运算）则比较的是数值（即会触发自动拆箱的过程）
```

#### 10、



