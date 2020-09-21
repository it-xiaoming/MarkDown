

# 								Maven

###  一、环境变量的配置

![image-20200326132133232](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200326132133232.png)	

![image-20200326132636456](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200326132636456.png)	

----

### 二、仓库分类

- 本地仓库
- 远程仓库(私服)
- 中央仓库

-----

### 三、本地仓库

- 默认地址 ```C:\Users\Administrator\.m2\repository```

- 自定义仓库地址   ```进入Maven按装目录下的conf\setting,xml```

````
<localRepository>D:/dubboDevelop/repository_pinyougou</localRepository>
````

----

### 四、标准目录结构

- src/main/java	核心代码部分
- src/main/resources     配置文件部分
- src/test/java      测试代码部分
- src/test/resources        测试配置文件
- src/main/webapp     页面资源(Js,Css,图片等)

----

### 五、常用命令

```diff
mvn clean	:	清除	target目录(编译好的class文件目录)
mvn compile	:	编译	生成target目录
mvn test	:	测试
mvn package	:	打包	(根据pom.xml文件中的标签<packaging>war\jar\...</packaging>)
mvn install	: 	安装	将项目打包,再放入本地仓库
mvn deploy	:	发布
```

| 清除项目编译信息 |  编译   | 测试 |     打包     |  安装   |  发布  |              |
| :--------------: | :-----: | :--: | :----------: | :-----: | :----: | ------------ |
|    mvn clean     | compile | test |   paskage    | install | deploy |              |
|   清理生命周期   |         |      | 默认生命周期 |         |        | 站点生命周期 |

---

### 六、Idea集成Maven插件

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200326142317627.png" alt="image-20200326142317627"  />

```````
确保断网的情况下也可以使用Maven搭建项目，前提本地仓库中存在所需要的jar包
```````

![image-20200326143014661](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200326143014661.png)

-----

### 七、修改运行环境

````xml
<build>
	<plugins>
        <!--tomcat7的插件-->
		<plugin>
			<groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.2</version>
            <configuration>
            	<port>8888</port>
            </configuration>
        </plugin>
        <!--jdk的插件-->
		<plugin>
			<groupId>org.apache.maven.plugin</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <configuration>
            	<target>1.8</target>
                <source>1.8</source>
                <encoding>UTF-8</encoding>
            </configuration>
        </plugin>
	</plugins>
</build>
````

---

### 八、Idea设置动态模板

![image-20200326151144034](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200326151144034.png)

---

### 九、scope依赖范围

| 依赖范围 | 编译有效 | 测试有效 | 运行有效 |            例子             |
| :------: | :------: | :------: | :------: | :-------------------------: |
| compile  |    Y     |    Y     |    Y     |         spring-core         |
|   test   |    -     |    Y     |    -     |            junit            |
| provided |    Y     |    Y     |    -     |         servlet-api         |
| runtime  |    -     |    Y     |    Y     |          JDBC驱动           |
|  system  |    Y     |    Y     |    -     | 本地的，Maven仓库之外的类库 |

----

# Maven高级

### 一、解决jar包冲突

```
maven导入jar包的一些概念：
	直接依赖,项目中直接导入的jar包,就是该项目的直接依赖包
	依赖传递,项目中没有直接导入的jar包,可以通过项目直接依赖jar包传递到项目中
```

- 方式一

  ```
  第一申明原则,哪个jar包的坐标在靠上的位置,这个jar包就是先声明的.
  先申明的jar包坐标下的依赖,可以优先进入到项目中。
  ```

- 方式二

  ```
  路径近者优先原则,直接依赖路径比传递依赖路径近,那么最终项目进入的jar包会是路径近的直接依赖包
  ```

- 方式三    【直接排除法】

  ```xml
  在要排除的jar包的坐标中添加
  	<exclusions>
          <!--要排除的依赖的坐标-->
          <exclusion>
              <groupId></groupId>
              <artifactId></artifactId>
          </exclusion>
  	</exclusions>
  ```

----

### 二、锁定jar包版本

```xml
<dependencyManagement></dependencyManagement>
```

---

### 三、Maven拆分与聚合



