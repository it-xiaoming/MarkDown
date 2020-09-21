# MyBatis

### 一、MyBatis的核心组件

- SqlSessionFactoryBuilder（构建器）：

>根据配置信息或Java代码来构建SqlSessionFactory对象。
>
>作用：创建SqlSessionFactory对象。

- SqlSessionFactory（会话工厂）：

>作用：创建SqlSession对象

- SqlSession（会话）：

>作用：提供操作数据库的 增删改查方法，可以调用操作方法，也可以操作Mapper组件。

- Executor（执行器）：

>SqlSession本身不能直接操作数据库，需要Executor来完成，该接口有两个实现：缓存执行器（缺省）、基本执行器。

- MappedStatement

>映射语句封装执行语句时的信息如SQL、输入参数、输出结果。 

### 二、获取自动生成主键

#### xml

```xml
<insert id="save" parameterType="User" 
        useGeneratedKeys="true" keyColumn="id" keyProperty="id">
</insert>

userGeneratedKeys : 是否使用JDBC的getGeneratedKeys方法获取数据库自动生成的主键，缺省值为false
keyProperty : MyBatis通过getGeneratedKeys方法获取主键值后给对象的哪一个属性值赋值
keyColumn : 通过生成的键值设置表中的列名，PostgreSQL中必须，MySQL不需要设置
```

#### Annotation

```java
 @SelectKey(
     statement="select last_insert_id()",
     before=false,
     keyColumn="id",
     resultType=long.class,
     keyProperty="id")
```

### 三、#{}和${}的区别

> 1. #传递参数时，先将参数转为？占位符，然后再赋值（PreparedStatement）
>
>    $传递参数时，直接把值作为SQL语句的一部分（可能存在SQL注入问题）
>
> 2. #设置参数时，如果是字符串，会自动加上‘’
>
>    $设置参数时，直接进行拼接（如果是字符串类型的值没有‘’执行时会报错）
>
> 3. 获取简单类型值的时候#{}里面可以随便写,${}中必须写${value}