---
layout: post
title:  "spark基本命令"
date:   2015-09-22 23:54:31
categories: 代码之术
---
##基本命令 

启动集群 

    start-master.sh
    start-slaves.sh

停止集群
    
    stop-master.sh
    stop-slaves.sh

进入pyspark 

    pyspark

进入spark-shell 

    spark-shell 

退出sparkshell ：ctrl+z  

##spark-shell入门使用 

下载测试文件到本地 

    wget http://www-stat.stanford.edu/~tibs/ElemStatLearn/datasets/spam.data  

加载文件到spark 

    scala> val inFile = sc.textFile("./spam.data")

保证所有的客户机都能访问到该文件 

    scala> import org.apache.spark.SparkFiles;
    scala> val file = sc.addFile("spam.data")
    scala> val inFile = sc.textFile(SparkFiles.get("spam.data"))

使用map功能将文本转化成数字数组  

    scala> val nums = inFile.map(x => x.split(' ').map(_.toDouble)) 

打印部分结果 

    scala> inFile.first()
    [...]
    res2: String = 0 0.64 0.64 0 0.32 0 0 0 0 0 0 0.64 0 0 0 0.32 0 1.29
    1.93 0 0.96 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
    0 0.778 0 0 3.756 61 278 1 

    scala> nums.first()
    [...]
    res3: Array[Double] = Array(0.0, 0.64, 0.64, 0.0, 0.32, 0.0, 0.0, 0.0,
    0.0, 0.0, 0.0, 0.64, 0.0, 0.0, 0.0, 0.32, 0.0, 1.29, 1.93, 0.0, 0.96,
    0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0,
    0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0,
    0.0, 0.0, 0.778, 0.0, 0.0, 3.756, 61.0, 278.0, 1.0)

##使用spark进行logist回归 

    scala> import org.apache.spark.util.Vector
    import spark.util.Vector
    scala> case class DataPoint(x: Vector, y: Double)
    defined class DataPoint
    scala> def parsePoint(x: Array[Double]): DataPoint = {
    DataPoint(new Vector(x.slice(0,x.size-2)) , x(x.size-1))
    }
    parsePoint: (x: Array[Double])this.DataPoint
    scala> val points = nums.map(parsePoint(_))
    points: spark.RDD[this.DataPoint] = MappedRDD[3] at map at
    <console>:24
    scala> import java.util.Random
    import java.util.Random
    scala> val rand = new Random(53)
    rand: java.util.Random = java.util.Random@3f4c24
    scala> var w = Vector(nums.first.size-2, _ => rand.nextDouble)
    13/03/31 00:57:30 INFO spark.SparkContext: Starting job: first at
    <console>:20
    ...
    13/03/31 00:57:30 INFO spark.SparkContext: Job finished: first at
    <console>:20, took 0.01272858 s
    w: spark.util.Vector = (0.7290865701603526, 0.8009687428076777,
    0.6136632797111822, 0.9783178194773176, 0.3719683631485643,
    0.46409291255379836, 0.5340172959927323, 0.04034252433669905,
    0.3074428389716637, 0.8537414030626244, 0.8415816118493813,
    0.719935849109521, 0.2431646830671812, 0.17139348575456848,
    0.5005137792223062, 0.8915164469396641, 0.7679331873447098,
    0.7887571495335223, 0.7263187438977023, 0.40877063468941244,
    0.7794519914671199, 0.1651264689613885, 0.1807006937030201,
    



