#### 配置元件

##### CSV Data Set Config

```shell
# Filename：
	引用文件地址，可以是相对路径也可以是绝对路径。相对路径的根节点是JMeter的启动目录(%JMETER_HOME%\bin)
# File encoding：
	读取参数文件用到的编码格式。一般是utf-8
# Variable Names (comma-delimited):
	定义的参数名称，用逗号隔开，将会与参数文件中的参数对应，如果这里的参数个数比参数文件中的参数列多，多余的参数将取不到值；反之参数文件中部分列将没有参数对应
# Delimiter (use '\t' for tab)：
	用来分隔参数文件的分隔符，默认为逗号，也可以用tab来分隔，如果参数文件用tab分隔，在此应该填写“\t”
# Allow quoted data?：见csv-01
	是非选项，如果选择是，那么可以允许拆分完成的参数里面有分隔符出现
# Recycle on EOF?：见csv-02
	是非选项，是，参数文件循环遍历；否，参数文件遍历完成后不循环
# Stop thread on EOF?：见csv-02
	与 "Recycle on EOF"中的False选择复用；是，停止测试；否，不停止测试。
# Sharing mode：
	参数文件共享模式，有以下三种。 
		"All threads"：参数文件对所有线程共享，这就包括同一测试计划中的不同线程组
		"Current thread group"：只对当前线程组中的线程共享
		"Current thread": 仅当前线程获取
```

###### csv-01

```shell
参数："test,001",100
Allow quoted data = true
# 结果
var1=test,001
var2=100
```

![a-07](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-07.png)

###### csv-02

| 配置                                             | 描述                                                       |
| ------------------------------------------------ | ---------------------------------------------------------- |
| Recycle on EOF = false，Stop thread on EOF=false | 循环次数大于参数行数，参数不进行循环，也不同时循环         |
| Recycle on EOF = false，Stop thread on EOF=true  | 循环次数大于参数行数，参数不进行循环，停止循环             |
| Recycle on EOF = true，Stop thread on EOF=false  | 循环次数大于参数行数，参数进行循环，不停止循环             |
| Recycle on EOF = true，Stop thread on EOF=true   | 循环次数大于参数行数，参数进行循环，虽然设置true，但是无效 |

false,false

![a-03](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-03.png)

false,true

![a-04](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-04.png)

true,false

![a-05](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-05.png)

true,true

![a-06](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-06.png)

##### HTTP Authorization Manager（HTTP授权管理器）

```shell
	HTTP认证是一种安全机制，在客户端、浏览器或者程序向服务器发起请求时需要提供用户名及密码且验证通过后（拿到凭证）才能继续发起交互。其实就是：请求头中的Authorization字段
# Clear auth on each iteration?:
	是否每次迭代清空凭证，如果清空则每次请求前都会进行验证
# 存储在授权管理器中的授权
	可以在此处保存授权信息，比如账号密码等信息
```

##### HTTP Cache Manager （HTTP缓存管理器）

```shell
# Clear cache each iteration?（每次迭代清空缓存）:
	如果选择该项，则该属性管理器下的所有Sampler每次执行时都会清除缓存；
# Use Cache-Control/Expires header when processing GET requests:
	在处理GET请求时使用缓存/过期信息头；
# Max Number of elements in cache(缓存中的最大元素数):
	默认数值为5000，可以根据需要自行修改；
	
# 响应头：Cache-Control
	值为：no-cache，浏览器和缓存服务器都不应该缓存页面信息，强制每次请求直接发送给源服务器，而不经过本地缓存版本的校验
	值为：max-age=xxx,是通知浏览器：xxx秒之内不要烦我，自己从缓冲区中刷新。
	值为：public，浏览器和缓存服务器都可以缓存页面信息；
	值为：no-store，请求和响应的信息都不应该被存储在对方的磁盘系统中；
	值为：must-revalidate，对于客户机的每次请求，代理服务器必须想服务器验证缓存是否过时
	
# 响应头：Expires
	用于控制请求文件的有效时间
```

##### JDBC Connection Configuration

###### 例子(mysql)

配置驱动

![a-35](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-35.png)

配置configuration

![a-36](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-36.png)

![a-37](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-37.png)

配置jdbc request

![a-38](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-38.png)

##### Random Variable

```shell
# Variable Name：
	生成的随机数保存到此变量中。
# Output Format：
	变量输出格式。 
# Minimum Value: 
	随机数最小值。 
# Maximum Value:
	随机数最大值。 
# Seed for Random function：
	随机数种子。
# Per Thread (User)?：
	生成的随机变量是否在线程组中共享，True,共享；FALSE,不共享。
```

![a-39](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-39.png)

![a-40](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-40.png)

##### Counter

```shell
# 启动：
	记录数量起始值。
# 递增：
	记录迭代次数的步长，1后是2 , 步长就是1
# 最大值：
	记录的最大值。
# Number fbrmat：
	计数器格式，可以是数字，例如"000000"(6位长度);"000,000"(6位长度，3位间隔开);字符加数字，例如CUST_000000（字符加6位数字)
# 引用名称：
	记数器记录的值可以存入此引用名(变量)，可供其他元件调用。与每用户独立的跟踪计数器：每个线程都有自己的记数器，相互不干扰。
# Track counter independently for each user:见c-01
	每个线程(用户)从start-max
# Reset counter on each Thread Group Iteration: 见c-02
	每次迭代复原计数器
```

###### c-01

![a-41](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-41.png)

<img src="C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-42.png" alt="a-42" style="zoom:150%;" />

###### c-02

![a-43](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-43.png)