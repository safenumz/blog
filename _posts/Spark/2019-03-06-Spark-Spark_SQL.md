---
layout: post
title: '[Spark] Spark SQL'
category: Spark
tags: [spark]
comments: true
---

# Spark SQL

## Overview
#### SQL, HiveQL, Scala로 표현된 관계형 질의가 Spark을 이용하여 실행될 수 있도록 하는 확장 API
#### DataFrames
- 이름있는 컬럼으로 구성된 분산 데이터 컬렉션
- 관계형 데이터 베이스의 테이블이나 R/Python의 data frame과 개념적으로 동일
- RDD, Parquet 파일, JSON dataset, Hive에 저장된 데이터로부터 생성 가능



## Spark SQL Application

```scala
import org.apache.spark.{SparkConf, SparkContext}
import org.apache.spark.sql.SQLContext
import org.apache.spark.sql.functions._

case class Record(key: Int, value: String)
object RDDRelation{
  def main(args: Array[String]) {}
  val sparkConf = new SparkConf().setAppName("RddRelation")
  val sc = new SparkContext(sparkConf)
  val sqlContext = new SQLContext(sc)
  import sqlContext.implicits._
  val df = sc.parallelize((1 to 100).map(i => Record(i, s"val_$i"))).toDF()
  df.registerTempTable("records")
  pritnln("Result of SELECT *:")
  sqlContext.sql("SELECT * FROM records").collect().foreach(println)
  val count = sqlContext.sql("SELECT COUNT(*) FROM records").collect().head.getLong(0)
  println(s"COUNT(*): $count")
  val rddFromSql = sqlContext.sql("SELECT key, value FROM records WHERE key < 10")
  println("Result of RDD.map:")
  rddFromSql.map(row => s"Key: ${row(0)}, Value: ${row(1)}").collect().foreach(println)
  df.where($"key" === 1).orderBy($"value".asc).select($"key").collect().foreach(println)
  df.write.parquet("pair.parquet")
  sc.stop()
}
```

## SQLContext

### libraries Dependency

```html
<dependency>
  <groupId>org.apache.spark</groupId>
  <artifactId>spark-sql_2.10</artifactId>
  <version>1.5.2</version>
</dependency>
```

### SQLContext

```scala
val sparkConf = newConf().setAppName("RDDRelation").setMaster("locat[*]")
val sc = new SparkContext(sparkConf)
val sqlContext = newSQLContext(sc) // SQlContext 생성
```

### HiveContext

```scala
val sqlContext = new HiveContext(sc) // SQLContext 생성
sqlContext.setConf("spark.sql.dialect", "sql") // 기본값은 hiveql
```

## DataFrame 생성
### Parquet

```scala
val df = sqlContext.read().load("users.parquet")
val df = sqlContext.read().parquet("users.parquet")
val df = sqlContext.read().format("parquet").load("users.parquet")
```

### JSON

```scala
val df = sqlContext.read().json("people.json")
val df = sqlContext.read().format("json").load("people.json")
```

### JDBC

```scala
val jdbcDF = sqlContext.read.format("jdbc").options(
  Map("url" -> "jdbc:postgresql:dbserver",
  "dbtable" -> "schema.tablename")).load()
)
```

### CSV

```scala
val df = sqlContext.read().format("com.databricks.spark.csv").option("header", "true").load("cars.csv")

// 외부 라이브러리에서 제공
--package com.databricks:spark-csv_2.10:1.1.0
```

## DataFrame Operation

```scala
 df.show();

 df.printSchema();

 df.select("name").show();

 df.select(df.col("name"), df.col("age").plus(1)).show();

 df.filter(df.col("age").gt(21)).show();

 df.groupBy("age").count().show();
```

## Schema Mapping
### Reflection

```scala
val sqlContext = new org.apache.spark.sql.SQLContext(sc)
import sqlContext.implicits._
case class Person(name: String, age: Int)
val people = sc.textFile("examples/scr/main/resources/people.txt").map(_.split(",")).map(p => Person(p(0), p(1).trim.toInt)).toDF()
val teenagers = sqlContext.sql("SELECT name, age FROM people WHERE age >= 13 AND age <= 19")

teenagers.map(t => "Name: " + t(0)).collect().foreach(println)
teenagers.map(t => "Name: " + t.getAs[String]("name")).collect().foreach(println)
teenagers.map(_.getValuesMap[Any](List("name", "age"))).collect().foreach(println)
```

### Programmatically

```scala
val sqlContext = new org.apache.spark.sql.SQLContext(sc)
val people = sc.textFile("")
val schemaString = "name age"

import org.apache.spark.sql.Row
import org.apache.spark.sql.types.{StructType, StructField, StringType}
val schema = StructType(schemaString.split(",").map(fieldName => StructField(fieldName, StringType, true)))
val rowRDD = people.map(_.split(",")).map(p => Row(p(0), p(1).trim))
val peopleDataFrame - sqlContext.createDataFrame(rowRDD, schema)
peopleDataFrame.registerTempTable("people")
val reusult = sqlContext.sql("SELECT name FROM people")
result.map(t => "Name: " + t(0)).collect().foreach(println)
```


## Spark Streaming과의 연동

```scala
val word: DStream[String] = ...
words.foreachRDD { rdd =>
  val sqlContext = SQLContext.getOrCreate(rdd.sparkContext)
  import sqlContext.implicits._
  val wordsDataFrame = rdd.toDF("word")
  wordsDataFrame.registerTempTable("words")
  val wordCountsDataFrame = sqlContext.sql("select word, count(*) as total from words group by word")
  wordCountsDataFrame.show()
}
```

## DataFrame 저장
- df.select("name", "age").write().format("parquet").save("namesAndAges.parquet");
- df.select("name", "age").write().format("parquet").mode(SaveMode.ErrorIfExists).save("namesAndAges.parquet");

### Save Mode
= SaveMode.ErrorIfExists(Default) : 데이터가 존재하면 Exception 발생
- SaveMode.Append : 존재하는 데이터 뒤에 추가
- SaveMode.Overwrite : 기존데이터가 지워지고 새로운 데이터로 쓰여짐
- SaveMode.Ignore : 기존 데이터를 그대로 두고 새로운 데이터는 쓰여지지 않음

## Performance Tuning
### Cache
- sqlContext.cacheTable("records") / dataFrame.cache()
- sqlContext.uncacheTable("records")

### 성능 관련 설정
#### spark.sql.inMemoryColumnarStorage.compressed
- 기본 값 : true
- 데이터 통계에 근거하여 각 행의 압축 코덱을 선택할지 여부

#### spark.sql.inMemoryColumnarStorage.batchSize
- 기본 값 : 10000
- Columnar caching 배치 크기 조정, 큰 배치는 메모리 사용성과 압축을 높이지만 데이터를 가져올 때 메모리 부족을 일으킬 수 있다.

#### spark.sql.autoBroadcastJoinThreshold
- 기본 값 : 10MB
- Join을 실행 할 때 모든 워커 노드에 전달될 테이블의 최대 크기 설정, -1로 설정하면 전달되지 않음, 현재 통계정보는 "ANALYZE TABLE <tableName> COMPUTE STATISTICS noscan" 명령이 실행되는 Hive metastore 테이블에서만 지원

#### spark.sql.codegen
- 기본 값 : false
- True로 지정하면 특정 질의의 표현을 평가하기 위한 코드가 동적으로 생성

### spark.sql.shuffle.partitions
- 기본 값 : 200
- Join이나 aggregation 시 shuffling에 사용할 파티션 개수


## Data Type

# [실습]()

## 국토교통부 교통정보공개서비스
- [https://openapi.its.go.kr](https://openapi.its.go.kr)

### dependency 추가

```html
<dependency>
  <groupId>org.apache.spark</groupId>
  <artifactId>spark-sql_2.10</artifactId>
  <version>${spark.version}</version>
  <scope>provided</scope>
</dependency>

<dependency>
  <groupId>org.apache.httpcomponets</groupId>
  <artifactId>httpclient</artifactId>
  <version>4.5.1</version>
</dependency>

<dependency>
  <groupId>org.codehaus.jettison</groupId>
  <artifactId>jettison</artifactId>
  <version>1.3.7</version>
</dependency>
```

```scala
// 공사 정보
import org.apache.spark.SparkConf
import org.apache.spark.sql.SQLContext
import org.apache.spark.storage.StorageLevel
import org.apache.spark.streaming.receiver.Receiver
import org.apache.spark.streaming.{Seconds, StreamingContext}
import org.codehaus.jettison.json.{JSONObject, JSONArray}

import scala.concurrent.ExecutionContext.Implicits.global
import scala.concurrent.duration._

object Traffic {
  def main(args: Array[String]) : Unit = {
    val conf = new SparkConf().setAppName("Traffic").setMaster("local[*]")
    val ssc = new StreamingContext(conf, Seconds(20))

    val eventStream  = ssc.receiverStream(new EventReceiver)
    val incidentStream = ssc.receiverStream(new IncidentReceiver)

    val info = eventStream.union(incidentStream)

    info.foreachRDD( rdd => {
      if(rdd.count() > 0){
        val sqlContext = new SQLContext(rdd.context)
        val trafficinfo = sqlContext.read.json(rdd)
        trafficinfo.registerTempTable("TrafficData")

        val result = sqlContext.sql("select type, time, x, y, message from TrafficData").distinct()
        result.foreach(println)
      }
    })

    ssc.start()
    ssc.awitTermination()

  }

  class IncidentReceiver extends Receiver[String](StorageLevels.MEMORY_AND_DISK) {
    override def onStart(): Unit = {
      println("start....")
      receive()
    }

    override def onStop(): Unit = {
      println("stop....")
    }

    def receive() = {
      val actorSystem = ActorSystem("MyActorSystem")
      import scala.concurrent.ExcutionContext.Implicits.global
      import scala.concurrent.duration._

      actorSystem.scheduler.shedule(0 seconds, 20 seconds) {
        val json = getEventData()
        parse(json)
      }
    }

    def getEventData() = {
      // json getType 추가
      val baseUrl = "http://openapi.its.go.kr/api/NIncidentIdentify?key=....&ReqType=2&getType=json"

      val client = HttpClients.createDefault()
      val httpGet = new HttpGet(baseUrl)
      val response = client.execute(httpGet)

      try{
        val entity = respose.getEntity
        new String(EntityUtils.toByteArray(entity))
      }finally{
        response.close()
        client.close()
      }
    }

    def parse(json:String) = {
      val jsonArray = new JSONArray(json)

      for(i<-0 to jsonArray.length() -1) {
        val jstr = jsonArray.getJSONObject()
        val newJson = new JSONObject()

        newJson.put("type", "Event")
        newJson.put("x", jstr.getDouble("coordx"))
        newJson.put("y", jstr.getDouble("coordy"))
        newJson.put("message", jstr.getString("eventstatusmsg"))
        newJson.put("time", new java.util.Data().toString)

        store(newJson.toString)
      }
    }
  }

  class EventReceiver extends Receiver[String](StorageLevels.MEMORY_AND_DISK) {
    override def onStart() : Unit = {
      println("start....")
      receive();
    }

    override def onStop() : Unit = {
      println("stop....")
    }

    def receive() = {
      val actorSystem = ActorSystem("MyActorSystem")

      actorSystem.scheduler.shedule(0 seconds, 20 seconds) {
        val json = getEventData()
        parse(json)
      }
    }

    def getEventData() = {
      // json getType 추가
      val baseUrl = "http://openapi.its.go.kr/api/NEventIdentify?key=....&ReqType=2&getType=json"

      val client = HttpClients.createDefault()
      val httpGet = new HttpGet(baseUrl)
      val response = client.execute(httpGet)

      try{
        val entity = respose.getEntity
        new String(EntityUtils.toByteArray(entity))
      }finally{
        response.close()
        client.close()
      }
    }

    def parse(json:String) = {
      val jsonArray = new JSONArray(json)

      for(i<-0 to jsonArray.length() -1) {
        val jstr = jsonArray.getJSONObject()
        val newJson = new JSONObject()

        newJson.put("type", "Event")
        newJson.put("x", jstr.getDouble("coordx"))
        newJson.put("y", jstr.getDouble("coordy"))
        newJson.put("message", jstr.getString("eventstatusmsg"))
        newJson.put("time", new java.util.Data().toString)

        store(newJson.toString)
      }
    }
  }
}
```
