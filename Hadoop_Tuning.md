# Hadoop Tuning
Hadoop 安装容易，可使用起来并不容易，需要很多的性能调优方面的工作要做，本文就是知识总结


以下的应用场景是两台1G内存的单核Amazon服务器

## mapred-site.xml

	mapred.child.java.opts  #  设定jvm的内存数，默认为200M. 一般来说，都是reduce耗费内存比较大，这个选项正是用来设置JVM堆的最大可用内存，但是也不要设置太大，如果超过2G，应该考虑从程序设计角度去优化。

	mapreduce.map.memory.mb
	mapreduce.reduce.memory.mb


### Map related

	mapred.min.split.size     
	这个配置项决定了每个 Input Split的最小值，也间接决定了一个Job的map 数目。 Input Split的大小，决定了一个Job拥有多少个map，默认64M每个Split，如果输入的数据量巨大，那么默认的64M的block会有几万甚至几十万的Map Task，集群的网络传输会很大，最严重的是给Job Tracker的调度、队列、内存都会带来很大压力。  

	mapred.compress.map.output     
	压缩Map的输出应该作为一个习惯，数据序列化其实效果会更好，无论是磁盘IO还是数据大小，都会明显的降低

	io.sort.mb     
	以MB为单位，默认100M，通常来看，这个值太小了。这个选项定义了map输出结果在内存占用buffer的大小，当buffer达到一定阈值，会启动一个后台线程来对buffer的内容进行排序，然后写入本地磁盘(一个spill文件)

	io.sort.spill.percent    
	这个值就是上述buffer的阈值，默认是0.8，既80%，当buffer中的数据达到这个阈值，后台线程会起来对buffer中已有的数据进行排序，然后写入磁盘，此时map输出的数据继续往剩余的20% buffer写数据，如果buffer的剩余20%写满，排序还没结束，map task被block等待。

	如果你确认map输出的数据基本有序（很少见），排序时间很短，可以将这个阈值适当调高，更理想的，如果你的map输出是有序的数据（基本不可能吧？），那么可以把buffer设的更大，阈值设置为1.

	Io.sort.factor     
	同时打开的文件句柄的数量，默认是10    
	当一个map task执行完之后，本地磁盘上(mapred.local.dir)有若干个spill文件，map task最后做的一件事就是执行merge sort，把这些spill文件合成一个文件（partition），有时候我们会自定义partition函数，就是在这个时候被调用的。

	mapred.tasktracker.map.tasks.maximum
	一个tasktracker最多可以同时运行的map任务数量   
	默认值：2

	优化值：mapred.tasktracker.map.tasks.maximum = cpu数量   

		cpu数量 = 服务器CPU总核数 / 每个CPU的核数   
		服务器CPU总核数 = more /proc/cpuinfo | grep 'processor' | wc -l   
		每个CPU的核数 = more /proc/cpuinfo | grep 'cpu cores'  

	mapred.map.tasks     
	默认值：2   
	优化值：CPU数量   

	mapred.tasktracker.reduce.tasks.maximum  
	一个task tracker最多可以同时运行的reduce任务数量
	默认值：2    
	优化值： (CPU数量 > 2) ? (CPU数量 * 0.50): 1 （mapr的官方建议）  


### Reduce related

	io.sort.mb/io.sort.factor/ io.sort.spill.percent     
	其作用和Map阶段的作用一样，都是控制排序时使用内存的大小、句柄数量、阈值大小

	mapred.reduce.parallel.copies    
	Reduce copy数据的线程数量，默认值是5
	Reduce到每个完成的Map Task copy数据（通过RPC调用），默认同时启动5个线程到map节点取数据。这个配置还是很关键的，如果你的map输出数据很大，有时候会发现map早就100%了，reduce一直在1% 2%。。。。。。缓慢的变化，那就是copy数据太慢了，5个线程copy 10G的数据，确实会很慢，这时就要调整这个参数了，但是调整的太大，又会事半功倍，容易造成集群拥堵，所以 Job tuning的同时，也是个权衡的过程，你要熟悉你的数据

	mapred.job.shuffle.input.buffer.percent
	当指定了JVM的堆内存最大值以后，上面这个配置项就是Reduce用来存放从Map节点取过来的数据所用的内存占堆内存的比例，默认是0.7，既70%，

	mapred.job.shuffle.merge.percent(默认值0.66)

	mapred.inmem.merge.threshold(默认值1000)

	mapred.reduce.tasks   
	一个Job会使用task tracker的reduce任务槽数量
	默认值：1
	优化值：0.95 * mapred.tasktracker.tasks.maximum




## 参考

[Hadoop Mapreduce default value](http://hadoop.apache.org/common/docs/r0.20.205.0/mapred-default.html)