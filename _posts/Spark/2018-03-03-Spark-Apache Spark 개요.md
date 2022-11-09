---
layout: post
title: '[Spark] Apache Spark 개요'
category: Spark
tags: [spark]
comments: true
---

# Apache Spark 개요
- 빠르고 범용적으로 사용할 수 있는 cluster computing platform
- Berkely AMPLab의 BDAS(Berkeley Data Analytics Stack)의 일부분

## Overview
- Microsoft Dryad Paper를 기반으로 개발
- Scala로 작성(소스 코드 양 대폭 감소)
- Scala, Java, Python API 제공
- In-memory Architecture
- MapReduce에 비해 10배(disk) ~ 100배(memory)의 성능
- Hadoop과 완벽 호완
  - HDFS, SerDes, UDF

## RDD(Resilient Distributed Dataset)
- Spark의 핵심 추상화 기법
- Immutable, recomputable, fault-tolerant partitioned collections of records
- 클러스터 노드들 사이의 파티션을 표현
- Data Set의 병렬 처리를 가능하게 함
- 파티션은 메모리나 디스크에 존재

- lazy-evaluation : transformation 연산은 실제 데이터를 가져와서 RDD를 만드는 대신 RDD를 생성할 수 있는 lineage 정보만 생성, action 연산이 실행되면 그 때 실제 데이터를 가져와서 RDD를 생성하고 연산 수행
- 자원을 효율적으로 분배하여 사용가능

## RDD Dependency
- Narrow Dependencies: map, filter, uion, join with inputs co-partitioned
- Wide Dependencies(Shuffling) : groupByKey, join with inputs not co-partitioned

## Spark Execution Model
- Parallel, Distributed
- DAG-based : RDD operation은 여러개의 Task Stage, Tree 형태, 병렬로 실행되기도 하고 파이프라인 형태로 실행되기도 함
- Lazy evaluation
- Optimizations
  - Reduce disk I/O
  - Reduce shuffle I/O
  - Parallel execution
  - Task pipelining
- 데이터의 위치 파악
- fault tolerance는 각 파티션의 RDD lineage graph를 이용

## Spark Cluster
- 지원 가능 Cluster : Standalone, Mesos, YARN

### 용어 정의
- Application : Spark으로 작성된 사용자 프로그램, 드라이버(Driver) 프로그램과 실행자들(Executors)로 구성
- Application jar : 사용자의 Spark 어플리케이션이 들어있는 Jar. 대부분 어플리케이션과 dependency jar들을 모두 포함하고 있는 "uber jar" 형태로 사용. Hadoop이나 Spark 라이브러리는 절대 포함되서는 안됨 그러나 이것들은 실행 시에 추가될 것임
- Driver Program : 어플리케이션의 main() 함술ㄹ 실행하고 SparkContext를 생성하는 프로세스
- Cluster manager : 클러스터의 리소스를 획득하기 위한 외부서비스(standalone manager, Mesos, YARN)
- Deploy mode : 드라이버의 프로세스가 실행되는 곳 구분, 클러스터 모드에서 프레임워크는 클러스터 내의 드라이버를 런치함, 클러스터 모드에서는 클러스터 외부에 드라이버를 런치 함
- Worker node : 클러스터에서 어플리케이션 코드를 실행할 수 있는 노드
- Executor : 워커 노드에서 어플리케이션을 위해 런치된 프로세스, 태스크를 실행하고 메모리나 디스크에 데이터를 보관함, 각 어플리케이션은 자신의 실행자를 가지고 있음
- Task : 하나의 실행자에 보내지는 작업의 단위
- Job : Spark Action (예: save, collect)에 반응하여 만들어진 여러 개의 작업들로 구성된 병렬 계산, 이 용어를 드라이버 로그에서 볼 수 있음
- Stage : 각 job은 서로 의존성을 가지고 있고 stage라고 불리는 더 작은 작업의 집합으로 나뉨, (MapReduce 에서 map과 reduce stages와 비슷) 이 용어를 드라이버 로그에서 볼 수 있음

1. master는 어플리케이션들 사이에 리소스를 배분하기 위해 cluster manage에 접근, 클러스터 상에서 task를 수행하고 데이터를 cache할 수 있는 executor 획득
2. app code를 executor에 전송
3. task를 executor에 전송

- 각 application은 여러개의 thread에서 task를 수행하는 executor process를 획득
  - 서로 다른 application들은 서로 독립적으로 수행(driver process, executor process 모두)
  - 따라서, 서로 다른 application들 사이에 데이터는 공유될 수 없음

- Spark은 특정 cluster manager에 종속적이지 않음
  - executro process들을 얻을 수 있고 그것들이 서로 통신할 수 있다면 어떠한 cluster manager도 사용 가능

- Driver가 cluster 상의 모든 작업에 대한 Scheduling을 담당
  - 가능한 driver와 worker node는 로컬 네티워크 내에서 실행
  - 원격으로 cluster에 요청을 보내고 싶다면, driver를 worker node와 먼 곳에서 실행하지 않고 RPC를 통해 worker node와 가까운 곳에서 드라이버에게 요청을 보내야 함

<br>
# [Build Apache Spark]()
## Maven(Maven 3.3.3+, Java 7+)

## Build Profile

```
$ export MAVEN_OPTS="-Xmx2g -XX:MaxPermSize=512M -XX:ReservedCodeCacheSize=512m"

$ build/mvn -Pyarn -Phadoop-2.4 -Dhadoop.version=2.4.0 -DskipTests clean package
```

### 다양한 Build 예제

```
Apache Hadoop 2.2.X
$ mvn -Pyarn Phadoop-2.2 -Dhadoop.version=2.2.0 -DskipTests clean package

Cloudera CDH 4.2.0
$ mvn -Pyarn -Dhadoop.verson=2.0.0-cdh4.2.0 -DskipTests clean package

Apache Hadoop 2.4.X with Hive 13 support
$ mvn -Pyarn -Phadoop-2.4 -Dhadoop.version=2.4.0 -Phive -Phive-thriftserver -DskipTests clean package

for Scala 2.11
$ mvn -Pyarn -Phadoop-2.4 -Dscala-2.11 -DskipTests clean package

Debian Packages
$ mvn -Pdeb -DskipTests clean package

sbt build
$ build/sbt -Pyarn -Phadoop2.3 assembly
```

## Hadoop Free Build Version
- Pre-build with user-provided Hadoop [can use with most Hadoop distributions] 다운 시에 아래 과정 진행
- 자신의 Hadoop 버전에 맞춰 spark 환경설정 가능

```
in/conf/spark-env.sh

if 'hadoop' binary is on your PATH
$ export SPARK_DIST_CLASSPATH=$(dadoop classpath)

With explicit path to 'hadoop' binary
$ export SPARK_DIST_CLASSPATH=$(/path/to/hadoop/bin/hadoop classpath)

Passing a Hadoop configuration directory
$ export SPARK_DIST_CLASSPATH=$(hadoop --config /path/to/configs classpath)
```


## Spark Shell
### Spark Shell: Interactive Analysis, Spark Context가 내장되어 있는 Scala Shell
- spark 다운로드 된 폴더 밑에 bin 폴더 (바이너리) 진입
- ./spark-shell

```
%SPARK_HOME%\bin\spark-shell
scala> val data = 1 to 10000; data.filter(_ < 10).collect
scala> val distData = sc.parallelize(data); distData.filter(_ < 10).collect

scala> val textFile = sc.textFile("README.md") // 파일로 부터 RDD 생성, sc.textFile("hdfs://... ...")
textFile: org.apache.spark.rdd.RDD[String] = README.md MappedRDD[17] at textFile at <console>:12
scala> textFile.count
scala> testFile.first
scala> val lineWithSpark = textFile.filter(line=>line.contain("Spark"))
scala> textFile.filter(_.contains("Spark")).count

scala> val words = lineWithSpark.map(_.split(" ")).map(r => r(1))
scala> words.cache
scala> words.toDebugString
```

```scala
val textFile = sc.textFile("README.md")

textFile.count

// 에러 발생

val textFile = sc.textFile("README.md")

textFile.count

// res3: Long = 105

textFile.first

// res4: String = # Apache Spark


// "Spark"이란 단어가 들어간 문장 찾기
val lineWithSpark = textFile.filter(line=> line.contains("Spark"))

lineWithSpark.first

// res5: String = # Apache Spark

// "Spark" 단어가 들어간 line의 갯수는 20
lineWithSpark.count

// res6: Long = 20

// 메모리에 저장하라는 것을 의미
lineWithSpark.cache

// lineWithSpark이 어떻게 생성됐는지에 대한 그 동안의 오퍼레이션들을 볼 수 있다
lineWithSpark.toDebugString
```

### pyspark shell

```
# bin/pyspark

> textFile = sc.textFile("README.md")
> textFile.count

```

### SparkR shell

```
# bin/sparkR
> df <- createDataFrame(sqlContext, faithful)
> head(df)
```

### SparkSQL shell

```
# bin/spark-sql
spark-sql> CREATE TEMPORARY TABLE people
        > USING org.apache.spark.sql.json
        > OPTIONS(
          path "examples/src/main/resources/people.json"
          );
spark-sql> select * from people;
```

### beeline shell
- hive 서버에서 쿼리 실행하는 쉘

```
# bin/beeline -u <url> -n <username> -p <password>
# bin/beeline -e "query"
# bin/beeline -f <query file>
```

### run-example shell
- spark에 있는 예제 실행하는 쉘

```
# bin/run-example SparkPi
# bin/run-example sql.RDDRelation
```

### spark-submit shell

```
# bin/spark-submit --master local[*] --class org.apache.spark.examples.SparkPilib/spark-examples-1.5.1-hadoop2.6.0.jar
# bin/spark-submit examples/src/main/python/pi.py
# bin/spark-submit examples/scr/main/r/dataframe.R
```


```
$ build/mvn -Pyarn -Phadoop-2.4 -DskipTests cleasn package
```


## Spark Pre-built Library
- R :
  - lib
- bin
    - conf : 환경변수가 들어가 있는 디렉토리
        - log4j.properties.template : 로그인 정보에 대한 설명
        - slaves : spark 클러스터를 구성할 때 워커 노드의 이름들을 모아 얼마나 많은 클러스터를 구성하겠다라는 것들에 대한 정보를 기록
        - spark-defaults.conf.tempate : 기본 환경변수, spark이 실행될 때 이 환경변수 읽어옴
        - fairscheduler.xml.template : 잡스케쥴러 설정
        - metrics.properties.template : spark에서 제공하는 metrics 관련 로그 정보 지정, 로그정보를 csv 파일 포맷으로 떨어뜨릴 수도 있고, 강글리아 같은 시스템 모니터링으로 보낼 수 있고, 데이터베이스로 보낼 수 있음
        - spark-env.sh.template : spark cluster가 돌아가는 환경들을 지정할 수 있음, 마스터 ip를 바꾼다거나, 스파크의 디폴트 포트는 7077로 돌아가는 데 기존에 사용하고 있으면 포트를 여기서 변경할 수 있음, 디폴트로 돌아가는 워커의 메모리를 지정하거나, 코어의 개수를 지정할 때 사용
    - data : 샘플 데이터가 들어 있음, 주로 머신러닝 데이터
    - ec2 : 아마존의 ec2 서버 설정
    - example : spark에서 기본 제공하는 example
    - lib : spark 라이브러리 자료, spark이 실행될 때 run되는 라이브러리들
    - licenses : 라이센스 파일
    - python : python 환경
    - sbin : spark cluster 명령어


```
conf 폴더 아래에서
$ vi slaves

localhost

// 다음과 같이 지정하면 워커 노드가 뜬다
worker1.server.com
worker2.server.com
```


```
// 7077 포트가 떠 있는지 확인
$ netstat -anp | grep 7077

// 8080 포트가 다른 서버에서 사용되고 있는지 확인
$ netstat -anp | grep 8080

// :::8080 :::; LISTEN 30610/java

// 뭐가 포트를 점유하고 있는지 구체적으로 확인
$ ps -ef|grep 30610

// spark-env.sh.template 파일을 수정하여 다른 포트로 잡음
// template 파일은 실행이 안되기 때문에 template 확장자 제거
$ cp spark-env.sh.template spark-env.sh
```

```
// spark-env.sh 파일 내부 하단에 export 명령어 추가, 사용할 포트 변경
$ vi spark-env.sh
export SPARK_MASTER_WEBUI_PORT=8099
```

```
// sbin 폴더에서, 모든 클러스터 실행
$ ./start-all.sh
```

- 웹브라우저에 106.248.46.183:8099 입력하면 Spark Master 확인 가능


```
// 모두 종료
$ ./stop-all.sh
```
