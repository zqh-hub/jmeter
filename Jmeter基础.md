### Jmeter基础

#### 性能概念

###### 性能测试相关概念：

- 性能测试：使用一定的技术，找出或验证某个性能指标的测试。强调的是值
- 负载测试：强调逐步逐步。逐渐增加模拟用户的数量， 直到应用程序响应时间超时
- 压力测试：强调在一个长时间。在一定的负载下系统长时间运行的稳定性

###### 性能测试的前提

- 性能测试的必要性研究——关键项评估
  - 第三方的主管部门、监管部门要审查的
  - 设计生命财产安全
  - 大型新系统
  - 核心系统（架构、业务功能）
  - 架构调整
  - 业务剧增
  - 重大缺陷修复
- 可测性

###### 性能测试指标

- 响应时间：从发送请求到收到请求响应时间
- 并发数：单位时间内发送请求的用户数。单用户时，接近TPS
- TPS：每秒处理事务数
- 吞吐量\吞吐率：网络传输的数据量
- 资源利用率：

###### 开展性能测试必备条件

- 网络要求：内网与外网要独立分开，不能使用内网访问外网
- 独立环境：不能与功能测试共用环境

#### jmeter概念

性能测试工具底层通过````协议````模拟压力

###### jmeter自身的特点

- 开源、轻量级，更适合自动化和持续集成
- 学习难度大、资料少

###### 性能测试工具选型的原则

- 成本：工具成本、学习成本
- 通信协议：标准协议、自由协议
- 生命力
- 跨平台

###### jmeter与loadrunner比较

- 界面上简洁
- 安装简单
- 协议较少
- 函数库少
- 成本低
- 开源

#### 二、jmeter配置文件

###### 1、修改语言（D:\software\apache-jmeter-5.2.1\bin\jmeter.properties）

![01](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\01.png)

###### 2、配置日志级别

- 低版本：在bin\jmeter.properties中配置

  ```java
  // 日志级别：FATAL_ERROR, ERROR, WARN, INFO,DEBUG
  log_level.jmeter=INFO
  ```

- 高版本：在bin\log4j.xml中配置

  ```xml
  <!--
  <Logger name="org.apache.jmeter.protocol.http.control" level="debug" />
  <Logger name="org.apache.jmeter.protocol.ftp" level="warn" />
  <Logger name="org.apache.jmeter.protocol.jdbc" level="debug" />
  <Logger name="org.apache.jmeter.protocol.java" level="warn" />
  <Logger name="org.apache.jmeter.testelements.property" level="debug" />
  -->
  <Logger name="org.apache.jorphan" level="info" />
  ```

###### 3、配置堆大小(/bin/jmeter.bat)

- 低版本

  ```java
  //堆
  set HEAP=-Xms1024m -Xmx2048m
  set NEW=-XX:NewSize=256m -XX:MaxNewSize=1024m
  ```

- 高版本(https://testerhome.com/topics/19528)

  ```java
  if not defined HEAP (
      rem See the unix startup file for the rationale of the following parameters,
      rem including some tuning recommendations
      set HEAP=-Xms1g -Xmx1g -XX:MaxMetaspaceSize=256m
  )
  ```

###### 4、设置监听器默认保存文件格式<br>![61](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\61.PNG)



#### 三、jmeter组成模块

###### 线程组

![02](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\02.png)

###### Cookies Manager![10](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\10.png)

###### 自定义属性并使用函数显示

自定义属性：/bin/jmeter.properties<br>![13](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\13.png)

使用函数读取：<br>![11](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\11.png)<br>![12](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\12.png)

#### 四、jmeter录制

1. 非测试组件---->HTTP代理服务器
2. 配置<br>![3](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\03.png)
3. 排除模式:<br>![80](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\80.png)
4. 设置浏览器的代理

#### 五、元素的作用范围及执行顺序

###### 执行顺序

1. 配置元件
2. 前置处理器
3. 定时器
4. 取样器
5. 后置处理器
6. 断言
7. 监听器

###### 作用域

1. 配置元件在某个取样器下，只作用于该取样器<br>![04](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\04.png)

2. 两个配置元件作用于一个取样器（一个是同一级别，一个是子配置元件）![05](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\05.png)

   ```
   对于请求1：
   	1、两个配置元件的  参数  都会起作用,即使参数是一样的
   	2、只有其子配置元件的url请求才会起作用
   ```

3. 两个default元件在同一级别，作用于同一个取样器![](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\09.png)

   ```
   两个默认元件相同的部分按照第一个的配置执行
   ```

   

4. 定时器先于取样器,会对同一级的取样器都起作用<br>![06](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\06.png)

   ```xml
   先定时器5s，然后请求1，再5s，再请求2
   ```

5. 定时器在取样器中，仅仅作用于该取样器<br>![07](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\07.png)

6. 两个后置处理器作用于同一级别的取样器，谁在上，谁先![08](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\08.png)

#### 六、变量定义：大小写敏感

###### 定义变量方式1：测试计划<b>![14](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\14.png)

###### 定义变量方式2:配置元件（***<u>无论在哪里，所有的取样器都能获取到在此定义的变量</u>***）<br>![16](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\16.png)

###### CSV读取文件变量<br>![48](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\48.png)

```java
//注意：线程就是用户，同一个线程下的取样器，所获取到的值是一样的
```

![51](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\51.png)![50](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\50.png)

```java
//在多线程，循环多次的情况下：
```

![53](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\53.png)![52](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\52.png)

```java
//在一个用户下，每次循环都会取不同的值；在多用户下，每个用户同样取不同的值；在多用户多循环下，同一个用户状态下的多循环，去不同的值
```

```java
//注意:
```

![54](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\54.png)

![55](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\55.png)![56](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\56.png)

```java
//不同的线程，使用不同的csv文件
```

![57](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\57.png)

###### 显示变量debug sampler取样器<br>![15](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\15.png)

#### 七、逻辑控制器

###### 简单控制器:仅仅将内部的元素捆绑在一起，不会改变整体的执行顺序<br>![17](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\17.png)

###### 循环控制器<br>![18](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\18.png)

<u>注意：逻辑控制器下的取样器，在多线程的情况下，执行顺序并不是：线程1执行后再到线程2，依次执行的，是混乱的。在线程少，循环次数少的情况下，因为jmeter执行快，顺序可能是依次执行的，但是实际仍是混乱的</u><br>![21](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\21.png)![20](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\20.png)

在线程组中，设置多次循环<br>![23](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\23.png)![23](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\22.png)

###### 仅一次控制器

即使在线程中设置循环多次，也只执行一次<br>![23](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\23.png)![24](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\24.png)

如果在`线程组`中设置循环多次，并且在“仅循环一次控制器”上添加一个循环控制器（循环多次）<br>![25](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\25.png)

###### ForEach控制器：配合”用户定义的变量“元件使用

通过堆“用户定义的变量”元件中符合条件的变量进行遍历，控制循环次数<br>![27](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\27.png)![26](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\26.png)

用户自定义的变量，必须以数字1开头，并且是连续的，不能是：1，2，5...

外面的取样器也能访问到在循环控制器中生成的变量，但智能访问到第一个<br>![28](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\28.png)

###### 事务控制器

用于统计取样器消耗的资源（连接时间、请求资源大小等等）<br>![30](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\30.png)![29](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\29.png)

事务控制器的两个配置：

- 配置1：生成父级请求：<br>![31](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\31.png)![32](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\32.png)
- 配置2：思考时间和前置/后置控制器时间<br>![33](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\34.png)![34](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\33.png)

###### if控制器<br>![35](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\35.png)

非默认判断的方式：<br>![37](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\37.png)![36](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\36.png)

每个取样器都进行if判断：<br>![38](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\38.png)

#### 八、取样器

###### 文件上传<br>![40](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\40.png)<br>![41](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\41.png)

###### 保存文件<br>![42](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\42.png)

###### 读取本地文件内容：<br>![46](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\46.png)![47](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\47.png)

###### IP欺骗：<br>![45](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\45.png)![44](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\44.png)

各个属性的含义：<br>![39](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\39.png)

![43](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\43.png)<br>

#### 九、函数

###### machineName:获取机器的名字<br>![58](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\58.png)

###### random():随机值<br>![59](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\59.png)

###### V函数:函数嵌套<br>![81](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\81.png)
效果<br>![82](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\82.png)

###### time：时间函数<br>![83](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\83.png)

#### 十、BeanShell

###### 语法：

```java
// 添加变量
vars.put("jmeter","jojo");
// 获取
vars.get("jmeter");
// 获取属性，并设置为变量
vars.put("myLanguagle",props.get("language"));
// 引用.java
source("C:/myClass.java");
int c = new myClass().add(1,2);
vars.put("myclass",c.toString);
// 引用.class文件
addClassPath("路径");
import myTest.MyClass;
int a = new MyClass().add(1,2);
// 使用参数
vars.put("sum",bsh.args[1]);
vars.put("all",Parameters);//Parameters：代表所有的参数
// 日志
log.info("日志");
// 返回码
ResponseCose=500;
// 响应信息
ResponseMessage = "响应信息";
// 设置取样器是否请求成功
IsSuccess = false/true;
// 响应数据
SampleResult.setResponseData("response data");
```

引用jar包<br>![60](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\60.PNG)

#### 十一、监听器

###### 1、summary report<br>![62](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\62.PNG)

###### 2、监听tomcat服务器资源情况

1、编辑tomcat，在/conf/tomcat-users.xml文件中,并重启：<br>![69](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\69.png)

2、双击start.bat,开启服务器，打开网址：<br>![70](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\70.png)

3、输入密码，进入管理界面：<br>![71](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\71.png)

4、进行监控：循环多次，每次间隔固定时间<br>![72](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\72.png)

```java
http://localhost:9090/manager/status?XML=true
```

![73](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\73.png)

![74](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\74.png)

![75](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\75.png)

###### 扩展：lambda probe监控tomcat

```java
https://blog.csdn.net/qq_23053701/article/details/76687576
```

###### 插件



#### 十二、解析二进制文件

###### 使用tika在http响应中查看二进制文件内容

1、下载到/lib中并重启:<br>![63](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\63.png)

2、查看解析后的响应：<br>![64](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\64.png)

###### 正则表达式后置处理器，获取二进制的响应内容：<br>![65](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\65.png)

###### 使用beash shell后置处理器，解析正则提取到的信息：

1、编码：<br>![66](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\66.png)

```java
import org.apache.jmeter.util.Document;
String converted = Document.getTextFromDocument(data);//data就是从文件中获取到的内容
vars.put("response",converted);
```

2、查看变量：<br>![67](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\67.png)

###### 使用poi解析excel文件，并生成变量:<br>![68](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\68.png)

```java
import java.io.ByteArrayInputStream;

import org.apache.poi.ss.usermodel.Cell;
import org.apache.poi.ss.usermodel.CellType;
import org.apache.poi.ss.usermodel.Row;
import org.apache.poi.ss.usermodel.Sheet;
import org.apache.poi.ss.usermodel.Workbook;
import org.apache.poi.ss.usermodel.WorkbookFactory;
import org.apache.poi.ss.usermodel.Row.MissingCellPolicy;

ByteArrayInputStream fis = new ByteArrayInputStream(data);
Workbook workbook = WorkbookFactory.create(fis); 
Sheet sheet = workbook.getSheet("Sheet1");
Row row = sheet.getRow(0);
Cell cell = row.getCell(0,MissingCellPolicy.CREATE_NULL_AS_BLANK);
cell.setCellType(CellType.STRING);
String val = cell.getStringCellValue();

vars.put("A1",val);
```

#### 十三、参数传递（正则——session）

1、找到具有session的请求,并在jmeter中创建请求：<br>![78](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\78.png)

2、使用后置处理器--正则表达式提取器，提取到session值：<br>![79](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\79.png)

#### 十四、参数传递（json提取器）

1、使用json提取器<br>![84](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\84.PNG)

2、设置断言<br>![85](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\85.PNG)

#### 十五、分布式

```java
//使用分布式负载生成器的特点：
    1、真实的性能测试，而不存在网络瓶颈问题
    2、快速响应GUI
    3、将测试结果存储在本地的一台机器上
    4、使用一台机器管理多个jmeter Engines
```

1、在slaves机上运行/bin/jmeter-server.bat，默认启动1099端口

2、在master机上编辑/bin/jmeter.properties，并重启<br>![76](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\76.png)

3、运行<br>![77](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\77.png)

#### 十六、分布式——linux

注意：助攻机与主机jmeter完全一致，jdk一致。直接将jmeter压缩，发送到助攻机。

###### 1、配置jmetet.properties

- 服务端口<br>![86](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\86.png)
- server.rmi.port端口<br>![87](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\87.png)
- disable<br>![88](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\88.png)

###### 2、将主机上的jmeter打包，上传到服务器，并解压

###### 3、赋予权限

```shell
在jmeter目录
chmod +x -R bin/
```

执行：<br>![91](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\91.png)

###### 4、关闭防火墙

```shell
#centos 6：
	#直接关闭
	service iptables stop:临时关闭
	chkconfig iptables off：永久关闭
	#打开指定端口
		#方法1
		①/sbin/iptables -I INPUT -p tcp --dport xxxx -j ACCEPT
		②/etc/init.d/iptables save 
		③ service iptables restart
		#方法2：
		vi /etc/sysconfig/iptables 
		-A INPUT -p tcp -m state --state NEW -m tcp --dport xxx -j ACCEPT
#centos 7
	#方式1：直接关闭防火墙
		systemctl stop firewalld
	#方式2：开放指定端口
		firewall-cmd --permanent --add-port=1234/tcp
		firewall-cmd --reload
```

执行<br>![92](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\92.png)

###### 5、进入jmeter的/bin目录，执行：

```shell
sh jmeter-server -Djava.rmi.server.hostname=192.168.122.128
```

执行：<br>![93](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\93.png)

###### 6、配置jmetet.properties

- 注销server.rmi.port、service_port
- 添加助攻机的IP，多个使用英文逗号“,”隔开<br>![89](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\89.png)
- 设置mode<br>![90](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\90.png)

#### 十六、持续集成——方法1

1、配置环境变量

```SHELL
JMETER_HOME：D:\software\Jmeter\jmeter5.2.1
Path:%JMETER_HOME%\bin
CLASSPATH:%JMETER_HOME%\lib\ext\ApacheJMeter_core.jar;%JMETER_HOME%\lib\jorphan.jar
```

2、修改jmeter.properties文件<br>![95](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\95.png)

3、解压Ant到：D:\software\Jmeter\jmeter5.2.1\myself\ant

4、将jmeter5.2.1\extras目录下的“ant-jmeter-1.1.1.jar”复制到Ant/lib下<br>![90](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\96.png)

5、建立目录结构

6、配置ant环境变量

```shell
ANT_HOME:D:\software\Jmeter\jmeter5.2.1\myself\ant
Path:%ANT_HOME%\bin
CLASSPATH:%ANT_HOME%\lib
```

7、email邮件

activation.jar、mail.jar、commos-email-1.1.jar放入ant\lib下

8、放置build.xml在pc或者app下，配置build.xml<br>![80](D:\student\doc\linux\img\80.png)<br>![81](D:\student\doc\linux\img\81.png)<br>![83](D:\student\doc\linux\img\83.png)

9、将jenkins.war放入webapps下，启动tomcat：tomcat/bin/startup.bat

注意：修改端口(tomcat\conf)<br>![](D:\student\doc\linux\img\84.png)

#### 十七、持续集成——方法2

1、msi安装jenkins

2、启动：

```shell
#方法1:管理员运行cmd
	net start/stop jenkins
#方法2：执行端口
	#进入jenkins/目录下
	java -jar jenkins.war --ajp13Port=-1 --httpPort=7070
```

3、配置ant

```shell
ANT_HOME:D:\software\Jmeter\jmeter5.2.1\myself\apacheAnt
Path:%ANT_HOME%\bin
```

4、修改jmter.properties<br>![95](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\95.png)

5、将jmeter\extras\ant_jmeter-1.1.1.jar拷贝搭配ant\lib下

验证：在jmeter\extras下cmd执行

```

```

#### 十八、无界面运行

```shell
# 执行脚本jmeter\bin
jmeter -n -t '脚本.jmx' -l report.jtl
-n：无界面模式
-t：待执行的测试计划
-l：输出结果报告文件路径
-r/R：分布式制定机器ip分压运行
jmeter -n -t 脚本路径 -r ip地址:port -l 报告路径
-j：制定执行日志路径
-H：制定代理服务区域名或ip
-P：制定代理服务器端口
```

执行：<br>![98](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\98.PNG)

#### 十九、插件

###### 1、安装

- 将‘’jmeter-plugins-manager-1.3.jar“放入到/lib/ext下
- 在jmeter的”选项“中找到插件管理器，进行安装

#### 二十、分析

1、分析思路

```
服务器硬件瓶颈 --->网络瓶颈 ---> 服务器操作系统瓶颈（参数配置、数据库、web服务器）---> 应用瓶颈(sql语句、数据库设计、业务逻辑、算法）
```

2、tps

- 直接下降：不能支持
- 先上升后下降：

3、响应时间

- 直接下降
- 先下降后上升
- 先上升后下降：宕机、任务结束

4、吞吐量、吞吐率

- 直接下降：服务器处理不过来、网络阻塞
- 先上升后下降
- 先下降后上升

5、服务资源监控：不能超过80%

- 直接下降
- 先上升后下降
- 先下降后上升

#### 二十一、服务器监控

1、将serverAgent上传到服务器中

2、在serverAgent目录下执行：

```shell
sh startAgent.sh --udp-port 0 --tcp-port 1234
```

3、防火墙开放端口（直接关闭防火墙）

#### 二十二、遇到的问题

##### 1、响应报415

```java
是因为：Content-Type: application/x-www-form-urlencoded
如果传递的参数是json的格式，就需要在HTTP信息头管理器将Content-Type修改为：application/json
```

![99](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\99.png)