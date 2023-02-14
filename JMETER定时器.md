#### JMETER定时器

##### Synchronizing Timer

```shell
同步定时器类似LoadRunner的集合点，作用是阻塞线程，达到指定的线程数量后，再一起释放。
# 注意：同步的是用户(线程),循环是无效的
```

```shell
# Number of Simulated Users to Group by(需要等待的线程数): 见st-01
        设置为0则并发数等于线程租中的线程数；设置大于0则等待达到这个数量再并发执行。
	
	设置同步的线程数量，我们在运行测试时，每一个线程的运行时间可能不一样，想要让所有线程都集合在一起可能会等待较长时间，这种情况下我们可以先让一部分集合完毕的线程运行起来。另外，有些场景不一定要等待所有的线程集合完毕，只需要部分线程保证同步就可以了，基于这些要求设置这个选项即可
	
	
# Timeout in milliseconds(等待时间):
        如果设置为0，该定时器将会等待线程数达到" Number of Simulated Users to Group by "设置的值才释放；
	设置大于0，超过设置的时间但是没达到" Number of Simulated Users to Group by "的线程数，将不再等待，释放当前的线程数。
```

###### st-01

![a-11](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-11.png)<br>![a-12](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-12.png)<br>![a-13](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-13.png)

##### Constant Timer

```
固定定时器：请求之间的间隔时间为固定值
```

```python
# ThreadDelay
  设定每个线程请求之前的等待时间（单位为毫秒）
  注意：固定定时是有作用域的。当放到线程组下，其作用域是所有请求都会延迟设置的时间；如果放到请求内，作用域是单个请求延迟时间
```

##### Uniform Random Timer

```
统一（均匀）随机定时器：
  它产生的延迟时间是个随机值，而各随机值出现的概率均等。总的延迟时间等于一个随机延迟时间加上一个固定延迟时间
  总的延时 = Constant Delay Offset + Random Delay Maximum
```

```python
# Random Delay Maximum
	  随机延迟最大的时间
# Constant Delay Offset
    固定延迟时间
如果设置Constant Delay Offset 为2000毫秒，Random Delay Maximum 设置500毫秒，那么总的延迟时间范围是2000毫秒~2500毫秒之间的值

1、如果设置1个线程 3次循环，每次循环的时候，中间的间隔时间是随机值，范围是2000~2500毫秒
2、如果设置3个线程 1次循环，每个线程中间的间隔时间是随机值，范围是2000~2500毫秒
```

![a-11](/Users/coco/Documents/Learn/jmeter-master/img\a-11.png)<br>
