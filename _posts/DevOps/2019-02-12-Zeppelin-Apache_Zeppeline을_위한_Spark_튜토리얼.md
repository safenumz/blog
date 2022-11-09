---
layout: post
title: '[Zeppelin] Zeppeline을 위한 Spark 튜토리얼'
category: DevOps
tags: [zeppeline, spark]
comments: true
---

# Spark 튜토리얼
## Transformations
- map()
- filter()
- flatMap()
- mapPartition()
- intersection()
- distinct()
- groupByKey()
- reduceByKey() : 키와 밸류로 구성된 데이터가 있을 때
- sortByKey()
- join()
- coalesce()

## Actions
- reduce()
- collect() : 데이터가 작을 때만 써야 함 수백mb 이내
- count() : 숫자 세기
- first() : 첫번째만 봄
- take()
- takeSample()
- foreach()
- countByKey()

```scala
// rdd 생성
val list = List(1, 2, 3)
val rdd = sc.parallelize(list)

// scala는 size를 사용
list.size
```

<pre>
res12: Int = 3
</pre>

~~~scala
// spark rdd는 count를 쓴다
rdd.count
~~~

<pre>
res14: Long = 3
</pre>

~~~scala
list.filter(x => x > 1).size
~~~

<pre>
res16: Int = 2
</pre>

~~~scala
rdd.filter(x => x > 1).count
~~~

<pre>
res20: Long = 2
</pre>

~~~scala
// 데이터가 아무리커도 데이터를 불러오지 않기 때문에 바로바로 됨
rdd.filter(x => x > 1).map(x => x * 2).map(x => x * 3).count
~~~

<pre>
res24: Long = 2
</pre>


~~~scala
rdd.collect
~~~

<pre>
res26: Array[Int] = Array(1, 2, 3)
</pre>

### map

~~~scala
rdd.map(x => x * 2).collect
~~~

<pre>
res28: Array[Int] = Array(2, 4, 6)
</pre>

### filter
~~~scala
rdd.filter(x => x > 2).collect
~~~

<pre>
res30: Array[Int] = Array(3)
</pre>

### flatMap
- 리스트의 리스트를 하나의 리스트로 줄이는 것

~~~scala
val list2 = List(List(1, 2), List(3, 4))
val rdd2 = sc.parallelize(list2)

rdd2.flatMap(list => list).collect
~~~

<pre>
res34: Array[Int] = Array(1, 2, 3, 4)
</pre>

### intersection
- 교집합

~~~scala
val rdd1 = sc.parallelize(List(1, 2, 3))
val rdd2 = sc.parallelize(List(2, 3, 4))

rdd1.intersection(rdd2).collect
~~~

<pre>
res36: Array[Int] = Array(3, 2)
</pre>

### distinct
- 중복 제거

~~~scala
val rdd3 = sc.parallelize(List(1, 1, 1, 2, 3))
rdd3.distinct.collect
~~~

<pre>
res46: Array[Int] = Array(1, 3, 2)
</pre>

### join 
- 공통된 1 키를 기준으로 조인

~~~scala
val rdd1 = sc.parallelize(List((1, "A"), (2, "B")))
val rdd2 = sc.parallelize(LIst((1, "C"), (3, "D")))

rdd1.join(rdd2).collect
~~~

<pre>
res49: Array[(Int, (String, String))] = Array((1,(A,C)))
</pre>

### reduce
- 순차적으로 누적 계산, 문자를 합칠 수도 있는 데 합쳐지는 순서를 보장해주지는 않음

~~~scala
val rdd = sc.parallelize(List(1, 2, 3, 4))

rdd.reduce((a, b) => a + b)
~~~

<pre>
// res53: Int = 10
</pre>

### reduceByKey
- reduceByKey는 Action이 아닌 Transforms이다

~~~scala
val file = spark.textFile("hdfs://;...")
val counts = file.flatMap(line => line.split(" ")).map(word => (word, 1)).reduceByKey(_ + _)
counts.saveAsTextFile("hdfs://...")
~~~
