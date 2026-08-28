##### 1. spark的整体架构

master：一个进程，负责资源的调度和分配，监控整个集群。

worker：spark集群上一个具体的机器，里面包含多个executor（JVM进程）。

driver：创建上下文环境的应用程序。类似于main方法

executor：JVM进程，里面包含多个Task任务（线程级别）

application master：向yarn提交任务时申请资源。

<img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20250727111546534.png" alt="image-20250727111546534" style="zoom:50%;" />

##### 2. spark on yarn中的client模式和cluster模式的区别

client在本地创建driver，适合测试任务，好处是可以本地断点测试；cluster在nodemanager上创建driver，一般用于生产任务，缺点是需要通过job web界面查看日志。

##### 3. spark-submit的常见参数

executor的核数，内存，driver的内存，队列，任务名称，部署方式。

##### 4. spark任务执行流程

* 创建任务执行环境，driver创建sparkcontext。
* sparkcontext向资源管理器申请资源，资源管理器创建executor
* executor向sparkcontext申请task
* sparkcontext生成DAG图，划分stage，其中每个stage中包含多个task，返回给executor
* executor执行完所有task后释放资源。

##### 5. 讲一下对RDD的理解

弹性分布式数据集，上面存储着要操作的元数据信息。避免反复地进行落盘操作。

五大特性

* 获取分区列表
* 计算算子
* 依赖关系
* 键值分区器
* 最优计算节点

弹性：

* 存储的弹性：内存不够用时使用磁盘
* 容错的弹性：数据丢失可以恢复（依赖关系）
* 计算的弹性：出错重试机制
* 分片的弹性：可以重新计算分片

##### 6. spark集群资源管理的粒度

| 粒度   | 特点           | 优点                           | 缺点                                                 | 例子           |
| ------ | -------------- | ------------------------------ | ---------------------------------------------------- | -------------- |
| 粗粒度 | 提前分配好资源 | 执行速度快，不需要反复申请资源 | 需要等到所有task执行完毕才会释放资源，资源抢占严重。 | spark on yearn |
| 细粒度 | 动态分配资源   | 资源利用更加高效               | task的申请资源和释放资源都要时间，执行速度较慢。     | MR任务         |

##### 7. spark中分区器的介绍

hash partitioner：

* 根据key值进行hash计算，放到对应的分区进行计算
* 可能出现数据倾斜

range partitioner：

* 将数据均匀放到每个分区内计算。
* 需要提前进行排序

自定义分区：

##### 8. cache和checkpoint的区别

cache：内存，计算结果保留在内存中，当任务结束时cache会被删除。

checkpoint：计算结果保留在HDFS文件系统上，当任务结束时不会被删除，但是执行到ck时会额外计算一次。因此在checkpoint之前会先使用cache，将结果保留在内存中，避免重新启动一个job进行计算。

##### 9. spark shuffle详解

未经优化的shuffle：

上游的每一个task都会根据下游的task创建溢写小文件，产生大量小文件、

优化后的shuffle：

上游的一个executor创建一个公共缓冲区，里面的溢出小文件是共享的，task往里面溢写数据。

sortshufflemanager：



##### 10. spark sql和spark core的区别是什么？

spark sql在spark core之上，spark sql负责进行sql的优化，spark core负责执行。

##### 11. spark sql中的join类型

shuffle hash join：

两个表将相同的key放到同一个partition内同时放到同一个节点中，节点根据左右表的大小，将小表构建成hash table进行关联。

<img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20250727180139584.png" alt="image-20250727180139584" style="zoom:50%;" />

broadcast hash join

executor将小表发到driver端，driver端将小表发到各个节点上。

<img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20250727180729943.png" alt="image-20250727180729943" style="zoom:50%;" />

sort merge join

在基础的join方式下将partition进行排序提高效率，前提是join key是排序好的。

<img src="C:\Users\10047\AppData\Roaming\Typora\typora-user-images\image-20250727180934104.png" alt="image-20250727180934104" style="zoom:50%;" />

cartesian join

笛卡尔积

broadcast nested loop join

不等值连接时候使用

##### 12. stage的划分

一个Application可以有很多个job，其中一个job由一个或者多个stage组成。stage的划分的整体思路，根据DAG计算图，从后往前推，如果产生宽依赖则断开，划分为一个stage。

##### 13. job的计算方式