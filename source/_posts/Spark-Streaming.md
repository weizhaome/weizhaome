title: Spark Streaming
author: John Doe
date: 2018-09-18 15:32:40
tags:
---
# Spark Streaming
> Spark Streaming是构建在Spark上的实时计算框架，它扩展了Spark处理大规模流式数据的能力。Spark Streaming可结合批处理和交互查询，适合一些需要对历史数据和实时数据进行结合分析的应用场景。Spark Streaming最主要的抽象是DStream（Discretized Stream，离散化数据流），表示连续不断的数据流。在内部实现上，Spark Streaming的输入数据按照时间片（如1秒）分成一段一段的DStream，每一段数据转换为Spark中的RDD，并且对DStream的操作都最终转变为对相应的RDD的操作。
## Spark Streaming程序基本步骤
1. 通过创建输入DStream来定义输入源。
2. 通过对DStream应用转换操作和输出操作来定义流计算。
3. 用streamingContext.start()来开始接收数据和处理流程。
4. 通过streamingContext.awaitTermination()方法来等待处理结束（手动结束或因为错误而结束）。
5. 可以通过streamingContext.stop()来手动结束流计算进程。
## RDD队列流
> 在调试Spark Streaming应用程序的时候，我们可以使用streamingContext.queueStream(queueOfRDD)创建基于RDD队列的DStream。

下面是参考Spark官网的QueueStream程序设计的程序，每隔1秒创建一个RDD，Streaming每隔2秒就对数据进行处理。

![Github](/images/RDD队列流.png "RDD队列流")

![Github](/images/RDD队列流-2.png "结果")

Spark也支持从兼容HDFS API的文件系统读取数据和通过Socket端口监听并接收数据，创建数据流。
![Github](/images/RDD文件流.png "RDD文件流")