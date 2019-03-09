title: Spark
author: weizhao
tags:
  - Spark
categories: []
date: 2018-09-17 20:49:00
---
# Spark 笔记
- ## Spark架构设计
**基本概念：**

1.RDD:弹性分布式数据集，是分布式内存的一个抽象概念，提供了一种高度受限的共享内存模型。
2.Executor:运行在工作节点（Worker Node）上的一个进程，负责运行任务，并为应用程序存储数据。
3.DAG:有向无环图（Driected Acyclic Graph）,反映RDD之间的依赖关系（宽依赖和窄依赖）。
4.应用：用户编写的Spark应用程序。
5.任务：运行在Exector的工作单元。
6.作业：一个作业包含多个RDD及作用在相应RDD上的各种操作。
7.阶段：是作业的基本调度单位，一个作业会分为多组任务，每组任务被成为”阶段“或者“任务集”。

**架构图**

![Github](/images/Spark-1.png "Spark")

**运行基本流程**

1.当一个Spark应用被提交时，首先为这个应用构建起基本的运行环境，即由任务控制节点（Driver）创建一个SparkContext,由SparkContext负责和资源管理器（Cluster Manager）的通信以及资源的申请、任务的分配和监控等。SparkContext会向资源管理器注册并申请运行Executor的资源。
2.资源管理器为Executor分配资源，并启动Executor进程，Executor运行情况将随着“心跳”发送给资源管理器。
3.SparkContext根据RDD的依赖关系构建DAG图，DAG图提交给DAG调度器进行解析，将DAG图分解为多个“阶段”（每个阶段就是一个“任务集”），并且计算出各个阶段之间的依赖关系，然后把一个个“任务集”提交给底层的任务调度器进行处理，Executor向SparkContext申请任务，任务调度器将任务分发给Executor运行。同时，SparkContext将应用程序代码发给Executor。
4.任务在Executor上运行，把执行结构反馈给任务调度器，然后反馈给DAG调度器，运行完毕后写入数据并释放所有资源。

参照：http://dblab.xmu.edu.cn/blog/1709-2/