title: Spark reduceByKey使用时遇到的问题
author: weizhao
date: 2018-09-27 14:40:56
tags:
---
# reduceByKey(func)不会处理单一值的RDD

* reduceByKey(func)的功能是，使用func函数合并具有相同键的值。比如，reduceByKey((a,b) => a+b)，有四个键值对(“spark”,1)、(“spark”,2)、(“hadoop”,3)和(“hadoop”,5)，对具有相同key的键值对进行合并后的结果就是：(“spark”,3)、(“hadoop”,8)。

* __但是，当reduceByKey(func)中的func函数对值进行split操作时，对于只有唯一值的RDD不会被split。下面使用创建的简单数据进行说明:__

> 统计原始数据中相同姓名都出现了哪些数字，原始数据如下图所示：

![github](/images/reduceByKey-question/rawdata.jpg)

可以看到数据中姓名后面的数字中含有"\t"。

> 读取文件数据并将其map成键值对的形式，得到的结果如下图所示：

![github](/images/reduceByKey-question/FirstMap.jpg)

我们看到数据读入后"\t"变为"\\t",姓名成为键，后面的数字成为了值。

> 使用reduceByKey合并相同姓名后面出现的数字，在这个过程中使用split("\\t")对值进行切分。

![github](/images/reduceByKey-question/reduceByKey.jpg)

从结果中可以看出，“xiaoguniang” 的值并没有被split.reduceByKey中不能对唯一值的RDD进行后面的func。也就是说当只有一个值的RDD出现时reduceByKey是不对其进行处理的。

> 为此，为了达到想要的效果我们对代码进行改进，使用mapValue()对Value进行预处理，之后再进行reduceByKey()。

![github](/images/reduceByKey-question/improve.jpg)