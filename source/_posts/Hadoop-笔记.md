title: Hadoop 笔记
author: weizhao
tags:
  - BigData
categories: []
date: 2018-09-16 22:49:00
---
# Hadoop基础
- ## 是什么
> Hadoop是Apache开源组织的一个分布式计算开源框架（http://hadoop.apache.org/)， 能够实现集群中对海量数据进行分布式计算。
- ## Hadoop架构
![Github](/images/Hadoop.png)
- HDFS: 提供对应用程序数据高吞吐量访问的分布式文件系统。
- YARN: 作业调度和集群资源管理的框架
- MapReduce: 基于YARN的大型数据集并行处理系统，实现分布式计算。
- Others:利用YARN的资源管理功能实现其他的数据处理方式。
### HDFS的关键元素
1. Block：将一个文件进行分块，通常是64M。一个大文件会被拆分成一个个的块，然后存储于不同的机器。如果一个文件少于Block大小，那么实际占用的空间为其文件的大小。
2. NameNode: 保存整个文件系统的目录信息、文件信息及分块信息，这是由唯一一台主机专门保存，当然这台主机如果出错，NameNode就失效了。在Hadoop2.*开始支持activity-standy模式----如果主NameNode失效，启动备用主机运行NameNode。
3. DataNode: 保存具体的block数据,负责数据的读写操作和复制操作,DataNode启动时会向NameNode报告当前存储的数据块信息，后续也会定时报告修改信息,DataNode之间会进行通信，复制数据块，保证数据的冗余性。
4. Secondary NameNode：定时与NameNode进行同步（定期合并文件系统镜像和编辑日&#x#x5FD7;，然后把合并后的传给NameNode，替换其镜像，并清空编辑日志，类似于CheckPoint机制），但NameNode失效后仍需要手工将其设置成主机。
### YARN的组件以及架构
#### YARN主要由以下几个组件组成：
> 1. ResourceManager：Global（全局）的进程 
> 2. NodeManager：运行在每个节点上的进程
> 3. ApplicationMaster：Application-specific（应用级别）的进程
> - *Scheduler：是ResourceManager的一个组件*
> - *Container：节点上一组CPU和内存资源*

- YARN的架构图及流程

![Github](/images/YARN.png "YARN")

1.客户端程序向ResourceManager提交应用并请求一个ApplicationMaster实例

2.ResourceManager找到可以运行一个Container的NodeManager，并在这个Container中启动ApplicationMaster实例

3.ApplicationMaster向ResourceManager进行注册，注册之后客户端就可以查询ResourceManager获得自己ApplicationMaster的详细信息，以后就可以和自己的ApplicationMaster直接交互了

4.在平常的操作过程中，ApplicationMaster根据resource-request协议向ResourceManager发送resource-request请求

5.当Container被成功分配之后，ApplicationMaster通过向NodeManager发送container-launch-specification信息来启动Container， container-launch-specification信息包含了能够让Container和ApplicationMaster交流所需要的资料

6.应用程序的代码在启动的Container中运行，并把运行的进度、状态等信息通过application-specific协议发送给ApplicationMaster

7.在应用程序运行期间，提交应用的客户端主动和ApplicationMaster交流获得应用的运行状态、进度更新等信息，交流的协议也是application-specific协议

8.一但应用程序执行完成并且所有相关工作也已经完成，ApplicationMaster向ResourceManager取消注册然后关闭，用到所有的Container也归还给系统
详情请见：https://blog.csdn.net/suifeng3051/article/details/49486927
### MapReduce主要过程
MapReduce把一个任务拆分成了多个小任务，并把子任务分配到多台计算机上进行工作。最终，每台计算机上的计算结果会被搜集起来并合并成最终的结果。在Map阶段将数据集的键值对映射为另一组键值对。Reduce阶段得到Map的输出，并把具有相同键的数据合并成一个更小的键值对数据集。
![Github](/images/MapReduce.png "MapReduce")

- Input Phase: 使用一个Record Reader将输入文件中的每一条数据转换为键值对的形式，并把这些处理好的数据发送给Mapper。
- Map: Map是是用户自定义的一个函数，此函数接收一系列的键值对数据并对它们进行处理，最后生成0个或多个键值对数据。
- Intermediate Keys: 由mapper生成的键值对数据被称为中间状态的键值对。
- Shuffle and Sort: Reducer任务通常以Shuffle（搅动）和Sort（排序）开始。程序把分好组的键值对数据下载到本机，Reducer会在本机进行运行。这些独立的键值对数据会按照键值进行排序并形成一个较大的数据序列，数据序列中键值相等的键值对数据会被分在相同的一组，这样易于在Reducer任务中进行迭代操作。
- Reducer:Reducer任务把分好组的键值对数据作为输入，并且对每一个键值对都执行Reducer函数。在这个阶段，程序会以不同的方式对数据进行合并、筛选。一旦执行完毕，Reducer会生成0个或多个键值对数据，并提供给最后一个处理步骤。
- Output Phase: 在输出阶段，通过record writer把从Reducer函数输出的键值对数据按照一定的格式写入到文件中。
参考：https://www.jianshu.com/p/6162b787a428