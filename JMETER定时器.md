#### JMETER定时器

##### Synchronizing Timer

```shell
# 注意：同步的是用户(线程),循环是无效的
```

```shell
# Number of Simulated Users to Group by: 见st-01
	设置同步的线程数量，我们在运行测试时，每一个线程的运行时间可能不一样，想要让所有线程都集合在一起可能会等待较长时间，这种情况下我们可以先让一部分集合完毕的线程运行起来。另外，有些场景不一定要等待所有的线程集合完毕，只需要部分线程保证同步就可以了，基于这些要求设置这个选项即可
	
# Timeout in milliseconds:
	超时时间，默认值是0：一直等待集合完毕；如果设置的时间到了，但是线程数不够，就把已经集合的跑掉
```

###### st-01

![a-11](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-11.png)<br>![a-12](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-12.png)<br>![a-13](C:\Users\coco\Documents\doc\document\资料\Jmeter\img\a-13.png)