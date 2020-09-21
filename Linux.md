# 								Linux

## 一、Linux的概述







## 二、Linux安装

### 1、安装虚拟器工具

### 2、安装虚拟器

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200326154009693.png" alt="image-20200326154009693"  />	

![image-20200326154053171](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200326154053171.png)	

![image-20200326154146958](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200326154146958.png)	

![image-20200326154250552](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200326154250552.png)	

![image-20200326154332706](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200326154332706.png)	

![image-20200326163909506](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200326163909506.png)	

![image-20200326164700108](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200326164700108.png)

![image-20200326170217147](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200326170217147.png)

![image-20200326170603635](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200326170603635.png)

![image-20200326170837050](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200326170837050.png)

---

## 三、SecureCRT

### 1.设置窗口背景

![image-20200328202952349](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200328202952349.png)	

![image-20200328203020159](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200328203020159.png)	

### 2.设置字体大小

![image-20200328203358175](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200328203358175.png)	

## 四、Linux的目录结构

![image-20200328204147866](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200328204147866.png)

## 五、LInux常用命令

### 1.切换目录命令：cd

~~~yml
使用"Tab"键可以不全文件路径
cd app	:	切换到app目录			cd ..	:	切换到上一层目录
cd /	:	切换到系统根目录		  cd  -	  :	  切换到上一个所在目录
~~~

### 2.列出文件列表：ls ll

```yaml
在Linux系统中已‘.’开头的文件都是隐藏文件
ls	:	显示当目录下的内容			ls -a	:	显示所有文件或目录(包含隐藏文件)
ls -l	: 缩写成 ll :显示文件的详细信息
```

### 3.创建目录和移除目录：mkdir rmdir

```yaml
madir	:	在当前目录下创建子目录
rmdir	:	删除"空"的子目录
```

### 4.浏览文件：cat	more	less	tail

```yaml
cat		:	一次性显示完文件的全部内容
more	:	一般用于显示的内容超过一个画面长度的情况。
			* 按空格显示下一个画面	按回车显示下一行的数据		按q键退出查看
less	:	和more类似,不同的是可以使用paup和pgdn键来控制
			* 按pgup和pgdn进行上下翻页
tail	:	用于显示文件后几行的内容
			tail -10 xxx	:	查看后10行数据
			tail -f  xxx	:	动态查看文件		ctrl + c : 结束查看		
```

### 5.文件操作	rm	cp	mv	tar	find	grep 

```yaml
rm	:	删除文件
rm -f xxx : 不询问直接删除		rm -r xxx : 递归删除	rm -rf xxx : 递归不询问删除
rm -rf *  : 删除所有文件		 rm -rf /* : 自杀

cp	:	复制文件
mv	:	剪切文件

tar	:	打包或解压
	tar命令位于/bin目录下。它能将一个文件或目录打包成一个文件,但不做压缩。一般Linux上常用的压缩方式是选中tar将许多文件打包成一个文件,再以gzip压缩命令压缩成xxx.tar.gz(或xxx.tar.gz)的文件。常用参数
	-c : 创建一个新的tar文件
	-v : 显示运行过程的信息
	-f : 指定文件名称
	-z : 调用gzip压缩命令进行压缩
	-t : 查看压缩文件的内容
	-x : 解开tar文件
	
find : 在当前目录及其子目录下查找符合条件的文件和目录,并将路径打印在控制台
grep : 在文件中查找符合条件的字符串
```

### 6. 其他命令

```yml
pwd	:	显示当前所在目录
touch	:	创建有个空的文件
clear/crtl+L	:	清屏
```

## 六、Vi和Vim编辑器



## 七、Linux权限



## 八、Linux常用网络操作

- 1、主机名配置

  ```yml
  hostname	:	查看主机名
  hostname xxx	:	修改主机名,重启后无效
  想要永久生效，可以修改/etc/sysconfig/network文件
  ```

- 2、IP地址配置

  ```
  ifconfig	:	查看(修改)ip地址,(重启后无效)
  ifconfig ens33 192.168.12.22	:	修改IP地址
  想要永久生效，可以修改/etc/sysconfig/network-scripts/ifcfg-ens33文件
  TYPE="Ethernet"
  PROXY_METHOD="none"
  BROWSER_ONLY="no"
  BOOTPROTO="dhcp"
  DEFROUTE="yes"
  IPV4_FAILURE_FATAL="no"
  IPV6INIT="yes"
  IPV6_AUTOCONF="yes"
  IPV6_DEFROUTE="yes"
  IPV6_FAILURE_FATAL="no"
  IPV6_ADDR_GEN_MODE="stable-privacy"
  UUID="8dd6d31f-da32-4792-b20e-270766f1172b"
  DEVICE="ens33"
  ONBOOT="yes"
  ```

  |         Key-Value         |              说明              |
  | :-----------------------: | :----------------------------: |
  |       NAME="ens33"        |            网卡名称            |
  |     BOOTPROTO="dhcp"      | 获取Ip的方式(static/dhcp/none) |
  |  IPADDR="12.168.177.129"  |                                |
  |  NETMASK="255.255.255.0"  |                                |
  |  NETWORK="192.168.177.0"  |                                |
  | BROADCAST="192.168.0.255" |                                |
  |                           |                                |
  |                           |                                |

- 3、域名

- 4、网络服务管理

- 5、防火墙设置

## 九、Linux上软件安装

### 1、安装软件常见方式

- 二进制发布包

  ```
  软件已经具体平台编译打包发布，只要解压，修改配置即可
  ```

- PRM包

  ```
  软件已经按照redhat的包管理工具规范RPM进行打包发布，需要获取到相应的软件RPM发布包，然后RPM命令进行安装
  ```

- Yum在线安装

  ```
  软件已经以RPM规范打包，但发布在网络上的一些服务器上，可用yum在线安装服务器上的rpm软件，并且会自动解决软件安装过程中的库依赖问题
  ```

- 源码编译安装

  ````
  软件以源码工程的形式发布，需要获取到源码工程后再用相应的开发工具进行编译打包部署
  ````

### 2、上传和下载工具介绍

- FileZilla

- lrzsz

  ```
  使用yum安装方式安装	yum install lrzsz
  ```

- sftp

  ```
  使用alt+p打开sftp窗口
  ```

  

### 七、Linux权限







## 七、Linux系统调优

### 1、CPU调优

#### 1.1、CPU处理数据的方式

```
1、批处理,顺序处理请求.(切换次数少,吞吐量大)
2、分时处理.(如同"独占",吞吐量小)(时间片，把请求分为一个一个的时间片，一片一片的分给 CPU 处理) 我们现在使用 x86 就是这种架构
3、实时处理.

批处理——以前的大型机（Mainframe）上所采用的系统，需要把一批程序事先写好（打孔纸带），然后计算得出结果  
分时——现在流行的 PC 机和服务器都是采用这种运行模式，即把 CPU 的运行分成若干时间片分别处理不同的运算请求  
实时——通常一般用于单片机上，比如电梯的上下控制，对于按键等动作要求进行实时处理
```

