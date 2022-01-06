# Spark

[toc]

## 摘要

apachen yyds! 学习地址：[Spark 2.2.0 中文文档](http://spark.apachecn.org)，大概看来一下 Flink，这些都是面向大数据计算的，我也不知道用啥，还不知道怎么用，感觉自己一个人有点摸着石头过河的感觉

## Spark 概述

Apache Spark 是一个快速的，通用的集群计算机系统。它对 Java，Scala，Python 和 R 提供了高层 API，并有一个经优化的支持通用执行图计算的引擎。它还支持一组丰富的高级工具，包括用于 SQL 和结构化数据处理的 [Spark SQL](http://spark.apachecn.org/#/sql-programming-guide.html)，用于机器学习的 [MLlib](http://spark.apachecn.org/#/ml-guide.html)，用于图计算的 [GraphX](http://spark.apachecn.org/#/graphx-programming-guide.html) 和 [Spark Streaming](http://spark.apachecn.org/#/streaming-programming-guide.html)。

#### 安全

默认情况下，Spark中的安全性处于关闭状态。这意味着您默认情况下容易受到攻击。在下载和运行Spark之前，请参阅[Spark Security](https://spark.apache.org/docs/latest/security.html)。