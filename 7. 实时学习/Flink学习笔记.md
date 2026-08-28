组件架构：

JobManager：类似与saprk中的Driver，存在于Nodemanger中的AM中。

* ResourceManager：

* Dispatcher：
* JobMaster

<img src="C:\Users\jason\AppData\Roaming\Typora\typora-user-images\image-20260614105814127.png" alt="image-20260614105814127" style="zoom:40%;" />

TaskManager：类似于spark中的Executor，存在于其他worker节点中的container中。

<img src="C:\Users\jason\AppData\Roaming\Typora\typora-user-images\image-20260614105629291.png" alt="image-20260614105629291" style="zoom:40%;" />

 

#### 经典的四张图的关系

<img src="C:\Users\jason\Pictures\flink的四张图.png" alt="flink的四张图" style="zoom:30%;" />

steamgraph：在客户端根据算子来生成执行图。

jobgraph：在客户端根据one to one的关系来合并算子。

executiongraph：引出子任务，其中每个子任务在一个slot中执行。

物理执行图：根据上下游关系，在算子之间生成中间数据集。

#### 并行度的计算

1. slot没有等待执行的这个概念，都是在使用一个slot来执行任务，不会进行等待，如果没有资源整个任务就一直等待。
2. 同一个任务的子任务不会在一个slot中；不同的group组不会共享slot；在yarn中会根据任务的最大并行度来分配slot个数。
3. one to one的两个相邻算子会被合并成一个task(chain)。



一个任务的提交流程：

以application模式为例

客户端将任务不进行初始化直接发送到yarn，然后根据job的个数来生成对应个数的job manager。



三种部署模式的区别：


三个时间：事件时间，摄入时间（source）和处理时间（operator）



#### 水位线

水位线：每个数据中都会有一个水位线，如果这个水位线刚好等于一个窗口的结束时间，就会启动一个新的窗口同时将这个数据放在新的窗口中。水位线 = eventtime - 容忍时间 t。



一个算子有多个分区，其中就分为分区的水位线和算子的水位线：核心的原则就是水位线是递增的。
分区的水位线每次来都取最大的；算子的水位线取各个分区中的最小值。



水位线创建的三种方式：noWaterMark，BoundWaterMark，AescendingWatermark



空闲时间：防止长时间没有数据导致窗口没有计算。



发往下游的时候先发数据，再发水位线。

#### 窗口

窗口类型：滚动，滑动，会话和全局；计数窗口。



窗口计算函数：全量计算和增量计算。



触发器：

驱逐器：驱逐器会避免预聚合，导致增量计算函数退化成全量计算函数。

**结论**：增量计算会在窗口触发前进行执行，

双流join：在同一个窗口并且相同key（inner join）

CoGroup：在同一个窗口，可以不同key（full join）

interval join：在窗口额外指定上下界进行相同key的关联，防止两条流数据时间不一致。



#### 处理函数

功能：处理函数 > 富函数 > 普通转换函数

处理函数的内部使用方法都是一样的，区别在于不同的流使用不同的处理函数。

这三个函数的区别是什么

定时器的类型：

测输出流：处理分流和收集迟到数据。



#### 状态

状态保存在operator中的一个task中，可以理解是task的一个变量。checkpoint会将全局的状态都进行保存，包括所有task的状态。



状态分类：operate stat（task粒度级别），keyed stat（task+key粒度级别）

keygroup：里面包含若干个key；operator task：里面包含若干个keygroup；keygrouprange：对应一个operator task里面key的范围。



operator stat：list，unionlist，Broadcast



不同的流都是从datasteam转换的，最终输出的时候又要转换成datasteam。

状态后端可以用于管理本地状态以及将其写入到磁盘中。

TTL：状态的生命周期，结束时间=创建时间+TTL。需要掌握TTL的更新类型（永不更新；创建或者更新状态；读写都更新）；可见性（过期依然可用；过期不可见）；清理策略（过期状态不主动清理；增量清理；rocks合并时清理。）



问题：

1. 流计算中task的切分是怎么判断的？

2. operator和task的关系是什么？

3. 设置水位线策略和设置水位线时间（两个类）有什么区别吗？

4. 我们原本是要将事件时间8-9点的放在一起进行处理，那在设置水位线大小的时候是怎么设计的，按照事件时间+容忍时间t吗？

   分区水位线：最大 eventTime - 最大乱序时间

   多个分区：min(各活跃分区 Watermark)

   水位线触发对应分区的计算，但是如果有容忍时间存在会将计算结果暂时进行缓存。

5. 如果现在现在同时来了两条数据（eventtime：9.01，watermaker：9.03；eventtime：8.59，watemark：9.05），这个时候9.05刚好触发8-9的计算，那会将事件时间9.01的进行计算吗？

6. 当水印达到一个窗口的结束时间的

7. trigger可以写在窗口函数的后面吗？在delta trigger中前后两个元素相比，前一个元素是增量计算之后的元素，后一个是新来的元素吗？

8. trigger可以作用在全量计算函数中吗？比如根据前后两个到来的元素来进行判断是否触发。

9. 在窗口中：指定时间戳，划分水位线，指定时间窗口，计算函数，触发器，驱逐器。这些的执行顺序是什么？

10. 在双流join中是不是先把一个窗口中的所有元素拿到之后再进行join，那岂不是增量计算函数不起作用了。

11. 老版本的状态后端和新版本有啥区别？

