---
layout: post
title: '[Spark] Spark Core'
category: Spark
tags: [spark]
comments: true
---

# Spark Core
## Spark Application Structure
- Spark Context 생성 -> RDD 생성 -> RDD Transformation -> Action

## Spark Context

```scala
val conf = new SparkConf().setAppName(appName).setMaster(masterURL)
val sc = new SparkContext(conf)
```

- appName: Application 이름
- masterURL : Spark Master URL

### Spark Mater URL
- local : 하나의 Worker thread로 로컬 실행(parallelism 아님)
- local[n] : N개의 worker thread로 로컬 실행(core 개수로 설정)
- local[\*] : 가능한 최대의 worker thread로 로컬 실행(최대 core 수)
- spark://HOST:PORT : Spark standalone cluster URL
- mesos://HOST:PORT : Mesos cluster url
- mesos://zk://HOST:PORT : Zookeeper를 사용하는 mesos cluster URL
- yarn-client : Yarn cluster(client mode)
- yarn-cluster : Yarn cluster(cluster mode)

## RDD 생성
### Parallelized Collection : Collection을 이용하여 병렬화 된 RDD 생성
- paralleize function의 두번째 인자는 파티션 개수, 지정하지 않으면 클러스터의 CPU에 기반하여 자동할당

```scala
val data = Array (1, 2, 3, 4, 5)

// parallelize 메소드를 사용하여 RDD로 만듬
val disData = sc.parallelize(data)

// RDD가 되었기 때문에 first나 count 같은 RDD 명령어를 사용할 수 있음
distData.first

// res4: Int = 1

distData.count

// res5: Long = 5

val data 1 to 100000

// 5개의 파티션으로 나뉜 RDD 생성
val distData = sc.parallelize(data, 5)
```


### Exteranl Datasets : 외부 저장소로부터 RDD 생성
- local file system, HDFS, Casandara, Hbase, Amazon S3, ElasticSearh 등
- textFile Function의 두번쨰 인자는 파티션 개수(기본값: 각 block 당 1개, 64MB)
- 두번째 인자는 block보다 작게 지정할 수 없음

```scala
val file = sc.testFile("/tmp/data.txt")
val file = sc.textFile("/directory")
val file = sc.textFile("/directory/*.txt")
val file = sc.textFile("/directory/*.gz", 5)

val file: RDD[String] = sc.textFile("/tmp")
val file: RDD[String, String] = sc.wholeTextFile("/tmp")
```

```scala
// spark home 디렉토리에 있는 모든 RDD를 읽어옴
val textFile = sc.textFile("..")

// RDD의 처음 확인, 에러 나옴, 각기 다른 형태의 파일이기 때문에
textFile.first

// java.lang.RuntimeException: Error while running command to get file permissions

// wholeTextFiles는 파일이름을 키로, 그 파일의 내용을 밸류로 갖는 RDD를 생성하기 때문 그 디렉토리 안에 여러 다른 파일들이 섞여 있어도 RDD 오퍼레이션이 문제 없이 수행 된다
val wtextFile = sc.wholeTextFiles("..")

wtextFile.first
```


# Transformation Operation
- RDD를 변형시켜서 새로운 Opertaion을 만들어 냄
- Action Operation는 RDD의 실제 동작이 일어나게 함

## map
- 함수를 통하여 소스의 각 요소를 전달하여 만든 새로운 RDD 생성

```scala
def map [U: ClassTag](f: => U): RDD[U]
````

```scala
rdd.map(_*2).collect // Array[Int] = Array(2, 4, 6, 8, 10)|
rdd.map(=>(x.toString, (x*x*x).toDouble)).collect|
// Array[(String, Double)] = Array((1, 1.0), (2.8, 0), (3, 27.0), (4, 64.0), (5, 125.0)|
```

```scala
// map 예제

// 숫자 1부터 5까지의 RDD 생성
val rdd = sc.parallelize(1 to 5)

// action operator의 하나 rdd에 있는 내용을 리스트로 변환해서 반환하라는 의미
rdd.map(_*3).collect // rdd.map(x => x*3).collect

// res9: Array[Int] = Array(3, 6, 9, 12, 15)

val newrdd = rdd.map(_*3)

newrdd.collect

// res10: Array[Int] = Array(3, 6, 9, 12, 15)
```

## filter
- 함수가 참을 반환하는 소스의 각 요소를 선택하여 만든 RDD 생성

```scala
def filter (f : T => Boolean) : RDD[T]
```

```scala
rdd.filter(_%2!=0).collect // Array[Int] = Array(1, 3, 5)
```

```scala
// filter 예제

// 짝수인 것만 찾음
rdd.filter(_%2 == 0).collect

// res11: Array[Int] = Array(2, 4)
```

## flatMap
- map과 비슷하지만 함수의 결과가 하나 이상이 나올 수 있음
- Function은 단일 item이 아닌 Sequence를 반환해야 함

```scala
def fatMap [U: ClassTag](f: T => TraversableOnce[U]) : RDD[U]
```

```scala
rdd.flatMap(1 to _).collect

// Array[Int] = Array(1, 1, 2, 1, 2, 3, 1, 2, 3, 4, 1, 2, 3, 4, 5)

rdd.map(x => x) vs. rdd.flatMap(x => x)
rdd.map(x => List(x)) vs. rdd.flatMap(x => List(x))
```

```scala
// flatMap 예제

// base rdd 생성
val rdd = sc.parallelize(1 to 5)

// 기존의 rdd를 map 명령어로 바꿔서
rdd.map(1 to _).collect

// res12: Array[scala.collection.immutable.Range.Inclusive] = Array(Range(1), Range(1, 2), Range(1, 2, 3), Range(1, 2, 3, 4), Range(1, 2, 3, 4, 5))

rdd.flatMap(1 to _).collect

// res13: Array[Int] = Array(1, 1, 2, 1, 2, 3, 1, 2, 3, 4, 1, 2, 3, 4, 5)



rdd.map(x=>x).collect

// res15: Array[Int] = Array(1, 2, 3, 4, 5)


// 시퀀스 요소가 아니라 단일 요소를 리턴하고 있기 때문에 flatMap을 사용할 수 없다.
rdd.flatMap(x=>x).collect

// <console>:26: error: type mismatch;
//  found   : Int
//  required: TraversableOnce[?]
//        rdd.flatMap(x=>x).collect

rdd.map(x=>List(x)).collect

// res18: Array[List[Int]] = Array(List(1), List(2), List(3), List(4), List(5))

rdd.flatMap(x=>List(x)).collect

// res19: Array[Int] = Array(1, 2, 3, 4, 5)
```

## mapPartitions
- 각 파티션(블록)별로 각자 수행되는 map fuction, 각 파티션의 모든 content는 입력함수(f: Iterator[T])를 통해 값들의 sequence로 변환될 수 있어야 하며 그 함수는 Iterator[U]를 반환해야 한다

```scala
def mapPartitions [U: ClassTag](f: Iterator[T] => Iterator[U], preservesPartitioning : Boolean = false) : RDD[U]
```

```scala
def myfunc[T](iter: Iterator[T]): Iterator[(T, T)] = { val tc = TaskContext.get() // mapPartitionWithContext
  println("PartitionID : %s, Attempt ID : %s".format(tc.partitionId(), tc.attempId()))
  var res = List[(T, T)](); var pre = iter.next
  while (iter.hasNext) { val cur = iter.next; res.::=(pre, cur); pre = cur; }
  res.iterator
}
rdd.mapPartition(myfunc_).collect
val rdd = sc.parallelized(1 to 9, 3); rdd.mapPartitions(myfunc).collect
```

```scala
// mapPartitions 예제
import org.apache.spark.SparkContext.

object RddOperationSuit extends App {
  val conf = new SparkConf().setMaster("local[*]").setAppName("RddOperation")
  val sc = new SparkContext(conf)

  def myfunc[T](iter: Iterator[T]): Iterator[(T, T)] = {
    val tc = TaskContext.get()
    print("Partition ID: %s, Attempt ID: %s".format(tc.partitionId(), tc.attemptId()))
    var res = List[(T, T)]()
    var pre = iter.next
    while (iter.hasNext){
      val cur = iter.next
      res .:: = (pre, cur)
      pre = cur
    }
    res.iterator
  }
  val rdd = sc.parallelize(List(1, 2, 3, 4, 5, 6, 7, 8, 9), 3)
  val mapPartitionRdd = rdd.mapPartitions(myfunc_).collect

  mapPartitionRdd.foreach(println)

  def myfunc2(index: Int, iter: Iterator[Int]): Iterator[String] = {
    println("Partition ID: %s".format(index))
    iter.toList.map(x => index + "," + x).iterator

  }

  val mapPartitionsWithindexRdd = rdd.mapPartitionsWithIndex(myfunc2).collect
  mapPartitionsWithIndexRdd.foreach(println)
}

```

## mapPartitionsWithIdex
- f 함수의 첫번째 인자 : 파티션 번호

```scala
def mapPartitionsWithIndex[U: ClassTag](f: (Int, Iterator[T]) => Iterator[U], preservesPartitioning: Boolean = false): RDD[U]
```

```scala
def myfunc(index: Int, Iter: Iterator[Int]): Iterator[String] = {
  iter.toList.map(x=> index + "," + x).iterator
}
rdd.mapPartitionsWithIndex(myfunc).collect

// Array(0,1, 0,2, 1,3, 1,4, 1,5, 2,6, 2,7, 3,8, 3,9, 3,10)
```

## sample
- Fraction과 seed를 이용하여 RDD의 item들을 무작위로 선택하여 새로운 RDD 생성, withReplacement가 true이면 값이 여러번 나올 수 있음

```scala
def sample(withReplacement: Boolean, fraction: Double, seed: Int): RDD[T]
```

```scala
rdd.sample(false, 0.5, 1).collect // Array(2, 4, 5, 6, 7, 8)
rdd.sample(true, 0,5, 1).collect // Array(1, 6, 6, 7, 7, 9, 10)
```

```scala
// sample 예제
val rdd = sc.parallelize(1 to 20)
rdd.collect
// res95: Array[Int] = Array(1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20)

// 전체 수의 50% 정도를 샘플링
rdd.sample(false, 0.5, 1)
// res97: Array[Int] = Array(1, 6, 8, 10, 12, 15, 16, 17, 19, 20)
```

## union
- def union(other:[RDD[T]]): RDD[T]
- 합집합

```scala
a.union(b).collect; (a ++ b).collect
```

## intersection
- def intersection(other: RDD[T], numPartitions: Int): RDD[T]
- 교집합

```scala
// HashPartitioner를 이용하여 3의 파티션에 저장
rdd.interaction(other, 3)

// myPart를 이용하여 파티션에 저장
rdd.interaction(other, myPart)
```

## distinct
- def distinct(): RDD[T]
- 중복제거

## groupBy
- def groupBy[K: ClassTag](f: T => K): RDD[(K, Iterable[T])]
- 함수에 따른 그룹화

```scala
rdd.groupBy(x => x % 2 match {case 0 => "even"; case_ => "odd"}).collect
// Array[(String, Iterable[Int])] = Arry((even, CompactBuffer(2, 4, 6, 8, 10)), (odd, CompactBuffer(1, 3, 5, 7 ,9)))
```

```scala
// groupBy 예제
rdd.groupBy(x => x % 2 match {
    case 0 => "even"
    case _ => "odd"
}).collect
// res105: Array[(String, Iterable[Int])] = Array((even,CompactBuffer(2, 4)), (odd,CompactBuffer(1, 3, 5)))
```

## groupByKey
- def groupByKey(): RDD[(K, Iterable[V])]
- def groupByKey(numPartitions: Int): RDD[(K, Iterable[V])]
- def groupByKey(partitioner: Partitioner): RDD[(K, Iterable[V])]
- 그룹화를 위한 함수를 제공하지 않고 partitioner에 의한 자동 분류
- 주의: 각 키마다의 집계(합 또는 평균 같은)를 수행하기 위해 군집화를 수행하려 한다면, reduceByKey나 comebineByKey를 사용하는 것이 성능이 좋을 것이다
- 주의: 기본적으로, 출력의 병렬화 수준은 부모 RDD의 파티션 숫자에 의존한다

```scala
val myPartitioner = rdd.keyBy(_%2)
myPartitioner.groupByKey.collect
// Array[(Int, Iterable[Int])] = Array((0, CompactBuffer(2,4,6,8,10)),(1,CompactBuffer(1,3,5,7,9)))
```

```scala
// groupByKey 예제
val part = rdd.keyBy(_%2)
part.collect
 // res108: Array[(Int, Int)] = Array((1,1), (0,2), (1,3), (0,4), (1,5))

part.groupByKey.collect
// res110: Array[(Int, Iterable[Int])] = Array((0,CompactBuffer(2, 4)), (1,CompactBuffer(1, 3, 5)))
```

## reduce (Action Operation)
- def reduce(f: (T, T) => T): T
- Reduce 함수, 항상 동일한 결과를 얻으려면 함수 f는 반드시 commutative 해야 함

```scala
// 순서가 섞이기 때문에 -는 실행할 때마다 결과가 달라질 수 있다
rdd.reduce(_+_); rdd.reduce(_-_)
```

## reduceByKey
- def reduceByKey(func: (V, V) => V): RDD[(K, V)]
- def reduceByKey(func: (V, V) => V, numPartitions: Int): RDD[(K, V)]
- def reduceByKey(partitioner: Partitioner, func: (V, V) => V): RDD[(K, V)]
- def reduceByKeyLocally(func: (V, V) => V): Map[K, V]
- RDD[(K, V)]에 대한 reduce 함수

```scala
val mappedRdd = rdd.map(x => (x%2, x))
mappedRdd.reduceByKey(_+_).collect
// Array[(Int, Int)] = Array((0, 30), (1, 25))
```

```scala
// reduceByKey
val pair = rdd.map(x => x%2, x)
pair.collect
// res113: Array[(Int, Int)] = Array((1,1), (0,2), (1,3), (0,4), (1,5))

pair.reduceByKey(_+_).collect
// res116: Array[(Int, Int)] = Array((0,6), (1,9))
```

## aggregate (Action Operation)
- def aggregate[U: ClassTag](zeroValue: U)(seqOp: (U, T) => U, combOp: (U, U) => U): U
- RDD 각 파티션의 데이터를 seqOp함수로 reduce하여 combOp 함수로 combine. 이 떄, zeroValue는 각 파티션에서 reduce의 처음에서 적용되고 combine 시 다시 한번 적용

```scala
val z = sc.parallelize(List(1, 2, 3, 4, 5, 6), 2)
z.aggregate(0)(math.max(_,_),_+_)
// 9

val z = sc.parallelize(List("a", "b", "c", "d", "e", "f"), 2)
z.aggregate("")(_+_, _+_)
// xxdefxabc
```

```scala
// aggregate 예제
// 파티션이 2개기 때문에 1번째 파티션에는 1,2,3이 2번째 파티션에는 4,5,6이 들어가 있다
val z = sc.parallelize(List(1, 2, 3, 4, 5, 6), 2)
z.collect
// res119: Array[Int] = Array(1, 2, 3, 4, 5, 6)

// 파티션1의 max값은 3, 파티션2의 max값은 6이다
// 0 + 3 + 6
z.aggregate(0)(math.max(_,_), _+_)
// res123: Int = 9


// 1 + 3 + 6
z.aggreate(1)(math.max(_,_), _+_)
// res127: Int = 10

// 10이 가장 크다
// 10 + 10 + 10 = 30
z.aggreate(10)(math.max(_,_), _+_)
// res125: Int = 30

```

## agrregateByKey
- def aggregateByKey[U: ClassTag](zeroValue: U)(seqOp: (U, V) => U, combOp: (U, U) => U): RDD[(K, U)]
- def aggregateByKey[U: ClassTag](zeroValue: U, numPartitions: Int)(seqOp: (U, V) => U, combOp: (U, U) => U): RDD[(K, U)]
- def aggregateByKey[U: ClassTag](zeroValue: U, partitioner: Partitioner)(seqOp: (U, V) => U, combOp: (U, U) => U): RDD[(K, U)]
- RDD[(K, V)]에 대한 aggregate 함수

```scala
val mappedRdd = rdd.map(x => (x%2, x))
mappedRdd.aggregateByKey(0)(math.max(_,_), _+_).collect
// Array((0, 18), (1, 17))
```

## sortBy
- def sortBy[K](f: (T) => K, ascending: Boolean = true, numPartitons: Int = this.partitions.size)(implicit ord: Ordering[K], ctag: ClassTag[K]): RDD[T]
- 정렬

```scala
val z = sc.parallelize(Array(("H", 10), ("A", 26), ("Z", 1), ("L", 5)))
z.sortBy(c => c._1, true).collect
// Array((A, 26), (H, 10), (L, 5), (Z, 1))

z.sortBy(c => c._2, true).collect
// Array((Z, 1), (L, 5), (H, 10), (A, 26))
```

## sortByKey
- def sortByKey(ascending: Boolean = true, numPartions: Int = self.partitions.size): RDD[P]
- RDD[(K, V)]에 대한 sortBy 함수

```scala
val a = sc.parallelize(List("dog", "cat", "owl", "gnu", "ant"), 2)
val b = sc.parallelize(1 t0 a.count.toInt, 2)
val c = a.zip(b)
c.sortByKey(true).collect
//Array((ant, 5), (cat, 2), (dog, 1), (gnu, 4), (owl, 3))

c.sortByKey(false).collect
// Array((owl, 3), (gnu, 4), (dog, 1), (cat, 2), (ant, 5))
```

## join, leftOuterJoin, rightOuterJoin, fullOuterJoin
- def join[W](other: RDD[(K, W)]): RDD[(K, (V, W))]
- def join[W](other: RDD[(K, W)], numPartitions: Int): RDD[(K, (V, W))]
- def join[W](other: RDD[(K, W)], partitioner: Partitioner): RDD[(K, (V, W))]
- RDD[(K, V)]에 대한 inner Join 함수

```scala
val a = sc.parallelize(List("dog", "salmon", "salmon", "rat", "elephant"), 3)
val b = a.keBy(_.length)
val c = sc.parallelize(List("dog", "cat", "gnu", "salmon", "rabbit", "turkey", "wolf", "bear", "bee"), 3)
val d = c.KeyBy(_.lenght)
b.join(d).collect
b.leftOuterJoin(d)
b.rightOuterJoin(d)
b.fullOuterJoin(d)
```

## cogroup (groupWith)
- def cogroup[W](other: RDD[(K, W)]): RDD[(K, (Iterable[V], Iterable[W]))]
- def cogroup[W1, W2](other1: RDD[(K, W1)], other2: RDD[(K, W2)]): RDD[(K, (Iterable[V]))]
- 3 key-value RDD 생성

<br>
# [Action Operation]()
## collect(toArray)
- def collect(): Array[T]
- def collect[U: Classtag](f: PartialFunction[T, U]): RDD[U]
- 드라이버 프로그램에서 RDD의 모든 데이터를 List로 반환

## count
- def count(): Long
- RDD 데이터 개수 반환

## first
- def first(): T
- RDD의 첫 번째 데이터 반환 (= take(1))

## take
- def take(num: Int): Array[T]
- RDD의 n번째 데이터 반환

## takeOrdered
- def takeOrdered(num: Int)(implicit ord: Ordering[T]): Array[T]
- RDD의 데이터를 정렬하여 처음 n개의 데이터 반환

```scala
val b = sc.parallelize(List("dog", "cat", "ape", "salmon", "gnu"), 2)
b.takeOrdered(2)

// Array(ape, cat)
```

## takeSample
- def takeSample(withReplacement: Boolean, num: Int, seed: Int): Array[T]
- Sample과 유사하지만 takeSample은 정확히 num개의 데이터를 무작위 순위로 Array로 반환

```scala
rdd.takeSample(true, 3, 1)
// Array(9, 4, 7)
```

## saveAsTextFile
- def saveAsTextFile(path: String)
- def saveAsTextFile(path: String, codec: Class[_ <: CompressionCodec])
- RDD를 로컬 파일 시스템, HDFS 또는 다른 하둡 지우너 파일 시스템에 저장, 파티션 개수 만금 파일 생성

```scala
rdd.saveAsTextFIle("c:/temp/rdd")
import org.apache.hadoop.io.compress.GzipCodec
rdd.saveAsTextFile("mydata_b", classOf[GzipCodec]) // 입측
```

## saveAsObjectFile
- def saveAsObjectFile(path: String)
- RDD를 직렬화하여 바이너리 형태로 저장

```scala
rdd.saveAsObjectFile("objFile")
val loadRdd = sc.objectFile("objFile")
```

## saveAsSequenceFile
- def saveAsSequenceFile(path: String, codec: Option[Class[_ <: CompressionCodec]] = None)
- RDD를 Hadoop sequence 파일로 저장

## countByKey
- def countByKey(): Map[K, Long]
- RDD[(Km V)]에 대한 count 함수

## countByValue
- def countByValue(): Map[T, Long]
- Value 값과 그 값이 나타나는 회수에 대한 Map 반환, 단일 reducer에서 aggregation이 일어남

```scala
val b = sc.parallelize(List(1,2,3,4,5,6,7,8,2,4,2,1,1,1,1,1))
b.countByValue
// Map(5 -> 1, 8-> 1, 3 -> 1, 6 -> 1, 1 -> 6, 2 -> 3, 4 -> 2, 7 -> 1)
```

## foreach
- def foreach(f: T => Unit)
- RDD의 각 데이터에 대하여 f 함수 실행

```scala
rdd.foreach(x=> println(x))
rdd.foreach(println(_))
rdd.foreach(println)
```

## foreachPartition
- def foreachPartition(f: Iterator[T] => Unit)
- RDD의 각 파티션에서 f 수행

<br>
# [Spark Application]()
## Spark Application 작성준비
  1. idea 실행
  2. Create New Project > maven
  3. Scala library 확인
  4. pom.xml 작성 - Spark Dependency 추가
  5. Add Framework Support > Scala 추가
  6. New > Scala class > 이름 입력 > Object 선택
  7. Coding


- IntelliJ에서 Maven으로 새 프로젝트를 생성한다.

- Maven Projects nedd to be imported라는 팝업이 뜨고 Import Changes와 Enable auto import를 선택할 수 있게 나오는 데, 여기서는 Enable Auto-Import를 선택한다. Maven의 pom.xml이 변경될 때 마다 자동으로 관련 라이브러리들이 다운로드되게 된다.

- scr/pom.xml을 열고 필요한 dependecy를 추가한다. 여기서는 spark 라이브러리 dependency를 추가한다. 아래 코드를 pom.xml에 끼워 넣으면 된다. 자동으로 관련 라이브러리들이 다운로드 된다.

```html
<dependencies>
    <dependency>
        <groupId>org.apache.spark</groupId>
        <artifactId>spark-core_2.11</artifactId>
        <version>2.4.0</version>
    </dependency>
</dependencies>
```

```html
<!-- 위 코드와 동일하지만 이렇게 쓸 수도 있다 -->
<properties>
    <scala.version>2.11.12</scala.version>
    <scala.compat.version>2.11.12</scala.compat.version>
    <scala.binary.version>2.11</scala.binary.version>
</properties>

<dependencies>
    <dependency>
        <groupId>org.apache.spark</groupId>
        <artifactId>spark-core_${scala.binary.version}</artifactId>
        <version>2.4.0</version>
    </dependency>
</dependencies>
```

- scala를 사용할 것이기 때문에 일단 main 디렉토리 밑에 scala 디렉토리를 생성한다. scala 디렉토리를 마우스 우클릭 후, Mark directory As에서 Sources Root를 선택한다.

- scala 디렉토리 안에 org.kodb.spark라는 package를 생성한다.

- 기본적으로 Maven에서는 scala 프레임워크를 사용할 수 없다. Scala를 실행하기 위해 root 디렉토리(여기서는 sample 폴더)를 우클릭하고 Add Framework Support를 클릭 후 Scala를 체크 해준다.

- 이제 위에서 생성한 org.kodb.spark package를 마우스 우클릭 후 New에 보면 Scala class를 생성할 수 있게 된다. Scala Class를 선택 후 Name에는 First, Kind에는 Class에서 Object로 바꿔 준다.

- App 이라는 어플리케이션을 extends하면 Main 매소드를 따로 작성하지 않고도 패키지를 실행할 수 있다.

- intellJ에서는 alt + enter를 누르면 자동으로 import 문이 뜬다.

- spark conf를 생성하고 그 spark conf를 spark context 생성자에 전달

- SparkContext와 SparkConf를 import 한다.
- 추가적으로 SparkContext 밑에 있는 모든 것을 import 한다.
- spark에서는 암묵적으로 변환되는 것들이 많기 때문에 현재 사용되지 않는 것 같아 보여도 SparkContext에 있는 모든 것을 import한다. 그렇지 않으면 프로그램이 복잡해 질 때 문제가 발생할 여지가 있다.

- 현재는 분산환경이 아닌 로컬에서 실행하고 있다.
- 로컬모드의 코어를 모두 사용할 수 있도록 local[\*]을 setMaster 해준다.
- argument로 받아서 수행하면 local 모드 cluster 모드 전부 실행이 가능한 프로그램을 작성할 수 있다.

```scala

```


- 만약 이러한 에러가 뜬다면 이것은 Add Framework Support에서 scala를 추가할 때 버전을 맞추지 못해 발생하는 에러이다. 만약 스칼라 버전을 2.11.12으로 쓰고 있다면 2.11.12 버전을 다운받아 맞춰줘야 한다.

```
Exception in thread "main" java.lang.NoSuchMethodError:
```


## MapReduce vs. Spark Application

```scala
// word count
package com.raonbit.edu

import org.apache.spark.SparkContext
import org.apache.spark.SparkContext._
import org.apache.spark.SparkConf

object WordCount {
  def main(args: Array[String]) {
    val conf = new SparkConf().setAppName("WordCount").setMaster("local[*]")
    val sc = new SparkContext(conf)

    val = file = sc.textFile(args(0))
    val word = file.flatMap(_.splict("")).map(w => (w, 1)).cache()
    word.reduceByKey(_+_).saveAsTextFile(args(1))
  }
}

```

## RDD Persistence(Caching)
### rdd.persist() (== rdd.cache()) 를 통해 RDD를 저장, 인자로 저장 수준을 전달하지 않으면 메모리에만 저장
#### cache() == cache(StroageLevel.MEMORY_ONLY)
- MEMORY_ONLY : 기본값, RDD를 직렬화 하지 않은 객체로 메모리에 저장, RDD가 메모리 보다 크면 어떤 파티션은 저장되지 않고 필요할 때마다 재 산출 된다.
- MEMORY_AND_DISK : RDD를 직렬화 하지 않은 객체로 메모리에 저장, RDD가 메모리 보다 크면 나머지 파티션들은 디스크에 저장하고 필요할 때 마다 읽어온다.
- MEMORY_AND_SER : RDD를 직렬화 한 객체로 메모리에 저장, 공간이 적게 필요하지만 읽는데 CPU 소모가 많다
- MEMORY_AND_DISK_SER : RDD를 직렬화 한 객체로 메모리에 저장, RDD가 메로리 보다 크면 나머지 파티션들은 디스크에 저아하고 필요할 때마다 읽어온다.
- DISK_ONLY : RDD를 디스크에만 저장
- MEMORY_ONLY_2, MEMORY_AND_DIS_2, .. : 위와 동일, 차이점은 각 피티션을 두 개의 클러스터 노드에 복제

### 명시적으로 저장하지 않아도 reduceByKey와 같은 shuffle 연산 중에 중간 데이터 자동 저장

### 중간 데이터(RDD)를 재상용할 계획이라면 명시적으로 persist(cache) 호출 권장

### 저장 수준 선택 가이드
- RDD의 크기가 Memory 용량 보다 작으면 MEMORY_ONLY 사용, 가장 빠르고 효율적
- 그렇지 않으면, MEMORY_ONLY_SER 사용. 이때 Kryo serialization을 사용하면 빠른 접근 가능
- RDD를 생성하는데 아주 많은 자원을 사용하지 않거나 많은 양의 데이터를 필터링 하지 않는다면 디스크에 저장하지 말 것. 디스크에서 읽어 오는 것보다 재 산출이 보다 효율적
- 빠른 장애 복구를 원한다면 클러스터에 복제하는 저장 수준 사용

### 오래된 데이터 파티션은 LRU(latest-recently-used)에 의해 제거

### 명시적으로 제거하려면 unpersist() 호출


## Broadcas Variables, Accumulator
### Broadcast Variable
- 읽기 전용 변수의 복사본을 클러스터 상의 각 take에 복사하여 보내는 대신 각 노드에 cache에 보관하는 것
- 큰 입력 데이터의 복사본을 각 노드에 효율적으로 보내는 데 사용

```scala
val broadcastVar = sc.broadcast(Array(1, 2, 3))
broadcastVar.value()

// return [1, 2, 3]
```

### Accumulator
- associative 연산을 통하여 누적만 가능한 변수, counter나 sum을 병렬로 효율적으로 구현하는 데 상요
- 숫자 형과 standard mutable collection에 대해 accumulator를 기본 제공하고, 확장 가능
- driver 프로그램만이 accumulator 값을 읽을 수 있음

```scala
val accum = sc.accumulator(0)
sc.parallelize(List(1, 2, 3, 4)).foreach(x => accum.add(x))
accum.value()
```

## Spark Application Operator Graph

-----

```scala
val rdd = sc.textFile("../README.md")

rdd.first

rdd.flatMap(line => line.split(" "))

rdd.flatMap(line => line.split(" ")).map(w => (w, 1))

rdd.flatMap( line => line.split(" ")).map( w => (w, 1)).reduceByKey((a, b) => a + b)

rdd.flatMap( line => line.split(" ")).map( w => (w, 1)).reduceByKey((a, b) => a + b).sortBy(_._2, false).take(10)

rdd.flatMap(_.split(" ")).map(w => (w, 1)).reduceByKey(_+_).sortBy(_._2, false)

rdd.flatMap(_.split(" ")).map(w => (w, 1)).count

rdd.flatMap(_.split(" ")).map(w => (w, 1)).countByKey


val rdd2 = rdd.flatMap(_.split(" "))

rdd.take(5)

rdd2.take(5)

val rdd3 = rdd2.map(w => (w, 1))


// 로컬파일: sc.textFile("file:///...")
// HDFS: sc.textFile("hdfs://...")
// Amazon S3: sc.textFile("s3://...")

val rdd = sc.makeRDD(0 to 100000000)

rdd.count

rdd.filter(_ > 10000000).count


val rdd = sc.makeRDD(List((1, "A"), (1, "B"), (2, "C"), (2, "D"), (3, "E")))

rdd.collect

rdd.groupBy(_._1).collect

rdd.groupBy(_._1).collect.foreach(println)
```
