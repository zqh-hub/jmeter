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
