---
layout: post
title: '[Spark] Spark Streaming'
category: Spark
tags: [spark]
comments: true
---

# Spark Streaming
## Overview
### 대규모의 실시간 데이터 처리를 위한 고성능의 장애 허용 framework, Spark Core 확장 API
- spark straming은 Kafka, Flume, HDFS/S3, Kinesis, Twitter 등의 다양한 input 소스로부터 받아서 처리하고, 처리된 데이터를 HDFS, Databases, Dashboards 등으로 보내주는 역할을 함

### Streaming 연산을 아주 작은 batch 작업의 연속으로 처리
- 실시간 stream을 X초의 batch들로 나눔, 0.5초 보다 작은 batch의 지연은 대략 1초
- 각각의 batch는 RDD이고 RDD 연산을 사용하여 처리 가능
- RDD 연산의 처리 결과가 batch에 반환

## DStream
### Spark Streaming의 Programming Model
- Stream Data를 표현하는 연속된 RDD
- RDD와 마찬가지로 input source에서 생성하거나 Dstream을 transform하여 생성
- RDD 연산을 그대로 사용 --Batch(Historical) Data와 Stream Data를 동일한 방식으로 처리

## Spark Streaming Application

```scala
// spark streaming 예제
package org.apache.spark.examples.streaming

import org.apache.spark.SparkConf
import org.apache.spark.straming.{Seconds, StreamingContext}

object HdfsWordCount {
  def main(args: Array[String]) {
    if (args.length < 1) {
      System.err.println("Usage: HdfsWordCount <directory>")
      System.exit(1)
    }

    StreamingExamples.setStreamingLogLevels()
    val sparkConf = new SparkConf().setAppName("HdfsWordCount")
    val ssc = new StreamingContext(sparkConf, Seconds(2))

    val lines = ssc.textFileStream(args(0))
    val words = lines.flatMap(_.split(" "))
    val wordCounts = words.map(x => (x, 1)).reduceByKey(_+_)
    wordCounts.print()
    ssc.start()
    ssc.awaitTermination()
  }
}
```

## Start Streaming Programming
### Libraries Dependency
- Streaming 의존성 추가

```html
<dependency>
  <groupId>org.apache.spark</groupId>
  <artifactId>spark-streaming_2.10</artifactId>
  <version>1.5.2</version>
</dependency>
```

### Streaming programming Structure

```scala
// 1. Spark Conf 생성
val sparkConf = new SparkConf().setAppName("TwitterPopularTags").setMaster("local[*]")

// 2. Streaming Context 생성
// 들어오는 단위를 2초단위로 모아서 배치 처리
val ssc = new StreamingContext(sparkconf, Seconds(2))

// 3. Input DStream 생성
val stream = twitterUtils.createStream(ssc)

// 4. Streaming 처리 정의
// Business logic

// 5. Streaming context 시작
ssc.start()
ssc.awaitTermination()

// 6. Streaming context 종료
ssc.stop()
```

## Input Dstream
### Streaming 소스에서 들어오는 데이터 스트림을 표현하는 것으로 StreamingContext API에서 직접 제공하는 Basic source와 외부 library에 존재하는 Advanced source로 구분
- File Stream을 제외한 모든 input dstream은 하나의 receiver 객체와 연결, 따라서 모든 input dstream은 데이터의 단일 스트림을 받음
- receiver는 streaming application에 할당된 하나의 core 점유
- application에 할당된 core의 개수가 receiver의 개수 보다 작으면 데이터 수집 불가
- application을 로컬에서 하나의 core로 실행하면 처리 불가

## Input DStreams - Basic Source
- actorStream : 아카에서 나오는 데이터를 스트림할 때 사용
- socketTextStream
- socketStream
- rawSocketStream
- fileStream
- fileStream
- textFileStream
- queueStream
- queueStream

## Input DStreams - Advanced Source
- Twitter (TwitterUtils)
- Kafaka (KafkaUtils) : 카프카 토픽에 직접 연결해 데이터를 가져오는 방식 권장
- Flume (FlumeUtils)
- Kinesis (KinesisUtils)
- MQTT (MQTTUtils)
- ZeroMQ (ZeroMQUtils)

<br>
# [Transformation Operation]()
## transfrom
- def transform[U: ClassTag](transfromFunc: RDD[T] => RDD[U]): DStream[U]
- def transform[U: ClassTag](transfromFunc: (RDD[T], Time) => RDD[U]): DStream[U]
- Dstream의 모든 RDD 마다 함수를 적용하여 새로운 DStream을 생성, DStream에 직접 사용할 수 없는 RDD 연산 수행 가능, Mllib, GraphX를 Dstream에 직접 적용 가능

```scala
val spamRDD .. // spam 정보를 담고 있는 RDD
val cleanedDStream = wordCounts.transform(rdd => {
  rdd.join(spamInfoRDD).filter(...) // DStream의 각 RDD와 spamRDD join
})
```

## UpdateStateByKey
- def updateStateByKey[S: ClassTag](updateFunc: (Seq[V], Option[S]) => Option[S]): DStream[(K, S)]
- def updateStateByKey[S: ClassTag](updateFunc: (Seq[V], Option[S]) => Option[S], numPartitions: Int): DStream[(K, S)]
- def updateStateByKey[S: ClassTag](updateFunc: (Seq[V], Option[S]) => Option[S], partitioner: Partitioner): DStream[(K, S)]
- def updateStateByKey[S: ClassTag])updateFunc: (Iterator[(K, Seq[V], Option[S])] => Iterator[(K, S)], partitioner: Partitioner, rememberPartitioner: Boolean): DStream[(K, S)]


## CheckPointing
### 장애 복구를 위한 정보 저장
- Metadata checkpointing: Streaming 구성 정보를 HDFS 같은 장애 허용 저장소에 저장, driver 노드에서 발생한 장애를 복구할 때 사용
  - Configuration - Streaming application을 생성하는 데 사용했던 구성 정보
  - Distream Operaton - Streaming application을 정의하는데 사용했던 DStream 연산 집합
  - Incomplete batches - queue에 쌓여 있으나 처리되지 않는 batch 들
- Data checkpointing : 생성된 RDD들을 저장, 여러 개의 batch들 사이에서 데이터를 합하는 stateful transformation에서는 필수 (예, updateStateByKey, window operations)

### 설정 방법

```scala
// StreamContext or Spark Context
ssc.checkpoint(directory)
//DStream
stream.checkpoint(Duration)
```

### Driver 실패 복구

```scala
def functionToCreateContext() : StreamingContext = {
  val ssc = new StreamingContext(...) // new context
  val lines = ssc.socketTextStream(...) // create DStreams

  ssc.checkpoint(checkpointDirectory) // set checkpoint directory
  ssc
}
// Get StreamingContext from checkpoint data or create a new one
val context = StreamingContext.getOrCreate(checkpointDirectory, functionToCreateContext_)
```

## Operation
### Transformation
- map(func), flatMap(func), filter(func), count()
- repartiton(numPartitons)
- union(otherStream)
- reduce(func), countByValue(), reduceByKey(func, [numTasks])
- join(otherStream, [numTasks]), cogroup(otherStream, [numTasks])
- transform(func)
- updateStateByKey(func)

### Window
- window(length, interval)
- countByWindow(length, interval)
- reduceByWindow(func, length, interval)
- reduceByeKeyAndWindow(func, length, interval, [numTasks])
- countByValueAndWindow(length, interval, [numTasks])

### Output
- Print()
- foreachRDD(func)
- saveAsObjectFiles(prefix, [suffix])
- saveAsTextFiles(prefix, [suffix])
- saveAsHadoopFiles(prefix, [suffix])

#### window length : window의 기간
#### sliding interval : window 연산이 수행되는 간격 (batch 크기의 배수로 지정해야 함)
#### checkpointing 필수 : checkpointing duration은 sliding interval의 5~10배가 적당

# Window Operation(Transformation)
## window
- def window(windowDuration: Duration)
- def window(windowDuration: Duration, slideDuration: Duration)
- 새로운 window DStream 생성

## countByWindow
- def countByValueAndWindow( WindowDuration: Duration, slideDuration: Duration, numPartitions: Int = ssc.sc.defaultParallelism)(implicit ord: Ordering[T] = null) : DStream[(T, Long)]
- Window item count 함수

## countByValueAndWindow
- def countByValueAndWindow( windowDuration: Duration, slideDuration: Duration, numPartitions: Int = ssc.sc.defaultParallelism)(implicit ord: Ordering[T] = null) : DStream[(T, Long)]
- windowDuration 동안의 데이터를 Key, 해당 데이터의 개수를 Value로 담고 있는 DStream 생성

## reduceByWindow
- def reduceByWindow(reduceFunc: (T, T) => T, windowDuration: Duration, slideDuration: Duration): DStream[T]
- def reduceByWindow(reduceFunc: (T, T) => T, invReduceFunc: (T, T) => T, windowDuration: Duration, slideDuration: Duration): DStream[T]
- Window reduce 함수

```scala
val retweetCnt = stream.map(status => statusgetRetweetCount)
val retSum = retweetCnt.reduceByWindow(_+_, Secounds(4), Seconds(4))
retSum.print

val retSum = retweetCnt.reduceByWindow(_+_,_-_, Seconds(4), Seconds(4))
retSum.print
```

## Join Operation
### join

```scala
val stream1 = DStream[String, String] = ...
val stream2 = DSTream[String, String] = ...

val joinedStream = stream1.join(stream2)
```

### window join

```scala
val windowedStream1 = stream1.window(Seconds(20))
val windowedStream2 = stream2.window(Minutes(1))
val joinedStream = windowedStream1.join(windowedStream2)
```

### Stream dataset join

```scala
val dataset: RDD[String, String] = ...
val windowedStream = stream.window(Seconds(20))
val joinedStream = windowedStream.transform { rdd => rdd.join(dataset)}
```

# Output Operation
## print
- def print()
- 드라이버 노드에서 실행되며 DStream의 각 배치에서 처음 10개의 데이터 출력, 개발시에 디버깅 용도로 유요

## foreachRDD
- def foreachRDD(foreachFunc: RDD[T] => Unit)
- def foreachRDD(foreachFunc: (RDD[T], Time) => Unit)
- Dstream 안의 각 RDD에 foreachFunc 적용, RDD를 파일로 저장하거나 네트워크를 통해 데이터베이스에 저장하는 것 처럼 각 RDD를 외부 시스템으로 보냄, foreachFunc 함수는 stream application이 실행되고 있는 드라이버 프로세스에서 실행되고 보통 RDD action을 가지고 있어서 스트림의 RDD 연산 수행

## foreachRDD Design Pattern

```scala
dstrem.foreachRDD(rdd => {
  val connection = createNewConnection() // executed at the driver
  rdd.foreach(record => {
    connection.send(record) // executed at the worker
  })
})

// connection 객체가 각 worker로 보내져야 함
// connection 객체는 직렬화 할 수 없음

dstream.foreachRDD(rdd => {
  rdd.foreach(record =>{
    val connection = createNewConnection()
    connection.send(record)
    connection.close()
  })
})

// 모든 record마다 connection을 만드는 대신 파티션마다 connection 생성

dstream.foreachRDD(rdd => {
  rdd.foreachPartition(record => {
    val connection = createNewConnection()
    record.foreach(r => connection.send(r))
    connection.close()
  })
})

// ConnectionPool 사용
dstream.foreachRDD(rdd => {
  rdd.foreachPartition(record => {
    val connection = ConnectionPool.getConnection()
    record.foreach(r => connection.send(r))
    ConnectionPool.returnConnection(connection)
  })
})
```

# Spark Stream 예제

```scala
// NetworkWordCount

import org.apache.spark.SparkConf
import org.apache.spark.streaming.{Seconds, StreamingContext}
import org.apache.spark.storage.StorageLevel

object NetworkWordCount {
  def main(args: Array[String]) {
    if (args.length < 2) {
      System.err.println("Usage: NetworkWordCount <hostnmae> <port>")
      System.exit(1)
    }

    StreamingExamples.setStreamingLogLevles()

    // Create the context with a 1 second batch size
    val sparkConf = new SparkConf().setAppname("NetworkWordCount")
    val ssc = new StreamingContext(sparkConf, Seconds(1))

    // Create a socket stream on target ip: port and count the words
    // in input stream of \n delimited text (eg. generated by 'nc')
    // Note that no duplication in distributed scenario for fault tolerance
    // Replication necessary in distributed scenario for fault tolerance
    // hostname은 args(0), port는 args(1)로 받는다
    val lines = ssc.socketTextStream(args(0), args(1).toInt, StorageLevel.MEMORY_AND_DISK_SER)
    val word = lines.flatMap(_.split(" "))
    val wordCounts = words.mp(x => (x, 1)).reduceByKey(_+_)
    wordCounts.print()
    ssc.start()
    ssc.awaitTermInation()
  }
}

```

```scala
// StatefulNetworkWordCount

object StatefulNetworkWordCount {
  def main(args: Array[String]) {
    if (args.lenth < 2) {
      System.err.println("Usage: StatefulNetworkWordCOunt <hostname> <port>")
      System.exit(1)
    }

    StreamingExamples.setStreamingLogLevelIs()

    val updateFunc = (values: Seq[Int], state: Option[Int]) => {
      val currentCount = values.sum

      val previousCount = state.getOrElse(0)

      some(currentCount + previousCount)
    }

    val newUpdateFunc = (iterator: Iterator[(String, Seq[Int], Option[Int])]) => {
      iterator.flatMap(t => updateFunc(t._2, t._3)).map(s =(t._1, s))
    }

    val sparkConf = new SparkConf().setAppName("StatefulNetworkWordCount")
    // Create the context with a 1 second batch size

    // 현재 디렉토리 . 에 체크포인트 저장이 된다.
    val ssc = new StreamingContext(sparkConf, Seconds(1))
    ssc.checkpoint(".")

    // Initial RDD input to updateStateByKey
    val initialRDD = ssc.sparkContext.parallelize(List(("hello", 1), ("world", 1)))

    // Create a ReceiverInputDStream on target ip:port and count the words
    // in input stream of \n delimited test (eg. generated by 'nc')
    val lines = ssc.socketTextStream(args(0), args(1).toInt)
    val words = lines.flatMap(_.split(" "))
    val wordDStream = words.map(x => (x, 1))

    // Update the cumulative count using updateStateByKey
    // This wil give a DStream made of state (which is the cumulative count of the words)

    vla stateDstream  = wordDstream.updateStateByKey[Int](newUpdateFunc, new HashPartitioner (ssc.sparkContext.defaultParallelism), true, initialRDD)
    stateDstream.print()
    ssc.start()
    ssc.awaitTermination()
  }
}

```


```
// 9999 포트가 점유되고 있는지 확인
$ netstat -anp | grep 9999

// shell1 : netcat 접속
$ nc -lk 9999

// shell2 : spark 하위 bin 디렉토리에서
$ ./run-example streaming.StatefulNetworkWordCount localhost 9999

// shell1 에서 단어를 치면 실시간으로 shell2에서 단어가 집계가 된다
```


## tweeter 인증키 얻는 방법
- [https://apps.twitter.com](https://apps.twitter.com)
- Create app
- 자신의 트위터 계정의 핸드폰 번호가 등록되어야 함
- Keys and Access Tokens 확인
- Your Access Token 생성
- 한국어만 집계하도록 등록할 수 있음
- Consumer Key, Comsumer Key Secret, Access Token, Access Token Secret

```
$ ./run-example streaming.TwitterPoularTags <Consumer Key> <Consumer Key Secret> <Access Toeken> <Access Token Secret>
```

TwitterReader.scala

```scala
object TwitterReader extends App {
  System.setProperty("twitter4j.oauth.consumerKey", args(0))
  System.setProperty("twitter4j.oauth.consumerSecret", args(1))
  System.setProperty("twitter4j.oauth.accessToken", args(2))
  System.setProperty("twitter4j.oauth.accessTokenSecret", args(3))

  val conf = new SparkConf().setAppName("TwitterReader").setMaster("local[*]")
  val ssc = new StreamingContext(conf, Seconds(5))

  val filters = Array()
  val tweets = TwitterUtils.createStream(ssc, None)

  tweets.foreachRDD( rdd => {
    rdd.foreach(status => {
      println("text ==> %s".format(status.getText))
      println("id ==> %s".format(status.getId))
      println("retweet cot ==> %s".status.getRetweetCount)
    })
  })
  ssc.start()
  ssc.awitTermination()
}
```

- dependency 추가, spark-streaming-twitter는 spark 2.0.0 이상의 버전부터는 지원하지 않음, 따라서 org.apache.bahir에서 써야 함

```html
<dependency>
    <groupId>org.apache.spark</groupId>
    <artifactId>spark-streaming_${scala.binary.version}</artifactId>
    <version>${spark.version}</version>
</dependency>

<dependency>
    <groupId>org.apache.bahir</groupId>
    <artifactId>spark-streaming-twitter_${scala.binary.version}</artifactId>
    <version>2.0.1</version>
</dependency>
```
