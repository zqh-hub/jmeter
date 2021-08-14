#### JMETER逻辑控制器

##### 事务控制器（Transaction Controller）

```shell
注意：事务控制器下的所有请求，TPS统计时就会被视为一个事务，
```

```shell
# Generate parent sample：见tc-01
	如果事务控制器下有多个取样器(请求)勾选它，那么在“察看结果树”中我们不仅可以看到事务控制器,还可以看到每个取样器；并且事务控制器定义的事务是否成功是取决于子事务是否都成功的，其中任何一个失败即代表整个事务失败
# Include duration of timer and pre-post processors in generated sample:见tc-02
	是否包括定时器、预处理和后期处理延迟的时间
```

###### tc-01

Generate parent sample=true

![a-08](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-08.png)

Generate parent sample=false

![a-09](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-09.png)

###### tc-02

Include duration of timer and pre-post processors in generated sample = true

![a-10](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-10.png)

##### 循环控制器（ForEach Controller）

```shell
# input variable prefix：
	可以在“用户自定义的变量”中定义一组变量，循环控制器可以从中获取到变量对应的值，然后作为循环控制器的循环条件，还可以输出变量作为取样器的参数。
# Start index for loop:
	循环变量下标起点
# End index for loop：
	循环变量下标终点。
# Output variable name：
	循环控制器生成的变量名称。
# Add before number?：见fc-01
	变量前缀后是否加作为分隔符
```

###### fc-0

![a-19](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-19.png)

![a-18](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-18.png)

```shell
# 注意：用于循环的变量命名时要以 数字 结尾,以 1 开始
	是否勾选要根据变量的名字来决定，如果变量的名字是：var_1,在foreach中“输入的变量前缀”是：var，那么就需要勾选；如果前缀：var_，已经带有“_”就不需要了
```

###### 例子：遍历jmeter官网目录

```http
https://jmeter.apache.org/demos/
```

1、访问官网

![a-14](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-14.png)

2、正则提取

![a-15](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-15.png)

3、foreach 控制器

![a-16](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-16.png)

4、访问

![a-17](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-17.png)

##### Runtime Controller

```shell
# Runtime(seconds)：见rc-01
	默认值为1；为空时即为0，此时不再执行其节点下的元件
```

###### rc-01

![a-20](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-20.png)

##### Switch Controller

```shell
# Switch Value：
	匹配值，可以为数字，也可以为字符。
	为数字时，从0开始;其子元件默认都有下标  # 见sc-01
	为字符时，匹配取样器名称，如果匹配不上就会默认并找取样器名称为default的取样器，如果没有则不运行。 # 见sc-02
```

###### sc-01

![a-21](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-21.png)

###### sc-02

```
根据变量的值，来判断运行哪一个http请求
```

![a-22](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-22.png)

![a-23](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-23.png)

##### While Controller

```shell
# Condition：
	接受变量表达式与变量，true/false
	另外提供以下三个常量。
		-- Blank：当循环中有取样器失败后停止。 
		-- LAST：当循环前有取样器失败则不进入循环。
		-- Otherwise：当判断条件为false时停止循环。
```

![a-24](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-24.png)

##### Interleave Controller(交替控制器)

```shell
# Ignore sub-controller blocks：见ic-03
	忽略子控制器，即子控制器失效，由交替控制器接管
```

###### ic-01

```shell
# 注意，针对循环有效，对用户数量是无效的
```

![a-25](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-25.png)

![a-26](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-26.png)

###### ic-02

```shell
# 当有两个交替控制器时，轮流执行
```

![a-27](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-27.png)

###### ic-03

![a-28](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-28.png)

![a-29](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-29.png)

##### Once Only Controller（仅一次控制器）

```shell
	仅一次控制器，也就是此控制器下的子元件只运行一次，即使把仅一次控制器放在循环
控制器下面，也只是运行一

# Percent Executions：
	按执行次数的百分比来计算执行次数，此时Throughput取值是0〜100。Per User是否勾选对Percent Executions模式无影响。
# Per User：
	如果选择Per User则按虚拟用户数来计算执行次数，如果没选中Per User则是 按所有虚拟用户来计算执行次数。
# Total Executions：
	按 Throughput的值来指定执行次数，可以是任意整数，如果小于等于零则一次也不执行。此时Per User与 Total Executions 一起来影响执行次数。
```

| 序号 | 线程数 | 循环次数 | 模式    | Throughput | Per User | 执行次数                              |
| ---- | ------ | -------- | ------- | ---------- | -------- | ------------------------------------- |
| 1    | 2      | 10       | Percent | 10         | N        | 2<br />Through% * （线程数*循环次数） |
| 2    | 2      | 10       | Percent | 10         | Y        | 2<br />Through% * （线程数*循环次数） |
| 3    | 3      | 5        | Total   | 2          | N        | 2<br />Through                        |
| 4    | 3      | 5        | Total   | 2          | Y        | 6<br />Per User * Through * 线程数    |
| 5    | 2      | 3        | Total   | 5          | Y        | 6                                     |
| 6    | 2      | 3        | Total   | 5          | N        | 5                                     |

##### If Controller

```shell
# Condition：
	判断条件，勾选 Interpret Condition as Variable Expression?时 Condition 使用变 量表达式来设置条件
```

![a-30](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-30.png)

##### Module Controller

```
可以通过模块控制器在当前测试计划中引入新的测试片段（也可以叫脚本片段，由控制器、取样器及辅助元件构成，能够完成负载的模拟）
```

![a-31](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-31.png)

![a-32](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-32.png)

##### Random Controller

```shell
# 注意：两个请求为一组，为一次循环，这次循环中，两个请求可能是重复的请求
```

![a-33](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-33.png)

##### Random Order Controller

```shell
# 注意：两个请求为一组，为一次循环，这次循环中，两个请求不可能是重复的请求
```

![a-34](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-34.png)

##### Loop Controller

```shell
循环控制器可以控制在其节点下的元件的执行次数
#Loop Count：
	要么设置Forever,要么填写具体的执行次数，二选一
```

