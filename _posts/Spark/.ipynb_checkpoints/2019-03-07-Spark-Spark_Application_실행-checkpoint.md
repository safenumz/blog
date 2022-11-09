---
layout: post
title: '[Spark] Apache Spark Application 실행'
category: Spark
tags: [spark]
comments: true
---

# Spark Application 실행

## Build, Package - Maven
- provided를 설정하면 패키징를 설정할 때 해당 의존성만 빼고 패키징을 할 수 있다.

```html
<scope>provided</scope>
```

```html
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-shade-plugin</artifactId>
      <version>2.3</version>
    </plugin>
  </plugins>
</build>
```

## Maven Project
- clean
- compile
- package

프로젝트 폴더 아래 target 디렉토리 가면 imple-uber-1.0-SNAPSHOT.jar가 있다. 클러스터 상에서 실행하려면 클러스터상에는 하둡이나 스파크가 있기 때문에 하둡이나 스파크 의존성을 뺴야 한다.

## Build, Package - SBT
### assembly plugin 설정
- $USER_HOME/.sbt/0.14/plugins/build.sbt 없으면 생성, 있으면 아래 줄 추가
- addSbtPlugin("com.eed3si9n" % "sbt-assembly" % "0.13.0")

<pre>
name := "edu-project"
version := "1.0.0"
scalaVersion := "Akka Rpository" at "http://repo.akka.io/releases/"
libraryDependencies ++= Seq(
  "org.apache.spark" %% "spark-core" % "1.21" % "provided",
  "org.apache.spark" % "spark-streaming_2.10" % "1.2.1" % "provided",
  "org.apache.spark" % "spark-sql_2.10" % "1.2.1" % "provided",
  "org.apache.spark" % "spark-hive_2.10" $ "1.2.1" $ "provided",
  "org.apache.spark" % "spark-streaming-twitter_2.10" % "1.2.1"
)
</pre>


## Deploy
### Usage: spark-submit [option] <app jar | python file>[app option]
### Options
- --master MASTER_URL : spark://host:port, mesos://host:port, yarn, local, local[\*]
- --deploy-mode DEPLOY_MODE : lcoal(기본 값), cluster
- --class CLASS_NAME : application's main class (Java / Scala apps)
- --name NAME : application 이름
- --jars JARS : driver에 포함될 ","로 구분된 jar 파일 목록과 executor classpath
- --py-files PY_FILES : ","로 구분된 .zip, .egg, .py 파일 목록(PYTHONPATH 상의)
- --files FILES : 각 executor의 실행 디렉토리에 놓여질 파일 목록(","로 구분)
- --conf PROP=VALUES : 속성 파일 지정, 기본값: conf/spark-defaults.conf
- --driver-memory MEM : driver 메모리 (예. 1000M, 2G) (기본값: 512G)
- --driver-java-option --driver-library-path --driver-class-path
- --executor-memory MEM : 각 executor 메모리 (예. 1000M, 2G) (기본값: 1G)

#### Spark standalone with cluster deploy mode only
- --driver-cores NUM : Cores for driver (Default: 1)
- --supervise : 지정하면, 드라이버가 실패했을 떄 다시 시작

#### Spark standalone and Mesos only
- --total-executor-cores NUM : 모든 executor이 총 core 개수

#### YARN-only
- --executor-cores NUM : 각 executor의 core 개수 (기본값 : 1)
- --queue QUEUE_NAME : 사용할 queue 이름 (기본값: "default")
- --num-executors NUM : 실행할 executors 개수 (기본값: 2)
- --archives ARCHIVES : 각 executor의 실행 디렉토리에 풀려질 압축 파일 리스트(","로 구분)

외부에서 전달되는 변수를 받아서 쓰려면 파일 내부에 있는 setMaster("local[\*]") 빼야 함
4040 포트로 들어가면 현재 어떤 Job을 실행하는지 알 수 있다.


```shell
$ cd /temp

$ ls sample-uber-1.0-SNAPSHOT.jar

$ ls -l sample-uer-1.0-SANPSHOT.jar

$ cd /data/kodb

$ cd spark-1.5.2-bin-hadoop2.4/

$ ./spart-all.sh
패스워드 입력

$ cd ..

$ cd bin

$ ./spark-submit --master spark://master.raonserver.com:7077 -class org.kodb.spark.TwitterReader /temp/sample-uber-1.0-SNAPSHOT.jar <트위터 키1> <트위터키2> <트위터키3> <트위터키4>
```

- 106.248.46.183:8099

## Deploy(launcher)

```java
package com.raonbit.edu;

import org.apache.spark.launcher.SparkLaunher;

public class MyLauncher {
  public static void main(String[] args) throws Exception {
    Process spark = new SparkLauncher()
    .setAppResource("/my/app.jar")
    .setMainClass("my.spark.app.Main")
    .setMaster("local")
    .setConf(SparkLauncher.DRIVER_MOMORY, "2g")
    .launch();
    spark.waitFor()''
  }
}
```

## Deploy Strategy
### Client Mode

### Cluster Mode

## Strandalone Cluster
### Cluster 시작/중지
- sbin/start-master.sh : 머신의 마스터 인스턴트를 시작
- sbin/start-slaves.sh : -conf/slaves 파일에 정의된 각 머신에 슬레이브 인스턴스 시작
- sbin/start-all.sh : 마스터와 슬레이브들 시작
- sbin/stop-master.sh : bin/start-master.sh 스크립트를 통해 시작된 마스터 중지
- sbin/stop-slaves.sh : conf/slaves 파일에 정의된 각 머신의 모든 슬레이브 인스턴스 중지
- sbin/stop-all.sh : 마스터와 슬레이브들 중지

### 환경 변수 설정
- conf/spark-env.sh : conf/spark-env.sh.template을 복사해서 생성

### standalone cluster에서 spark-shell 실행
- bin/spark-shell-master spark://HOST:PORT

### application 종료 시키기
- bin/spark-class org.apache.spark.deploy.Client kill <master url> <driver ID>
- driver id는 standalone master web UL에서 확인 가능(http://master_url:web_ui_port)


## Standalone Cluster - High Availability
### ZooKeeper를 이용한 Master 노드 이중화 : spark-env.sh의 SPARK_DEMON_JAVA_OPTS 설정
#### spark.deploy.recoverMode
- 기본값 : NONE
- ZOOKEEPER로 설정하면 Master노드 이중화(대기 Master 노드)

#### spark.deploy.zookeeper.url
- ZooKeeper cluster url

#### spark.deploy.zookeeper.dir
- 기본값 : /spark
- 복구 상태를 저장하는 zookeeper 내의 디렉토리

##### spark master url : spark://host1.port1,host2:port2

### Local File System을 이용한 단순 Master 노드 재시작 : spark-env.sh의 SPARK_DEMON_JAVA_OPTS 설정
- spark.deploy.recoverMode : FILESYSTEM으로 설정하면 재시작 모드
- spark.deploy.recoveryDirectiory : 복구상태를 저장하는 디렉토리(Master 노드가 접근가능한 디렉토리)

##### stop-master.sh로 master 노드를 중지 시키면 복구 상태가 제거되지 않음
#### 복구 디렉토리로 NFS 사용 가능

## 추천 Hardware 구성
### Storage Systems
- 가능하면 HDFS와 같은 노드에서 실행
- HDFS와 같은 로컬 네트워크 영역에 있는 다른 노드에서 실행
- Hbase와 같은 low-latency 저장소에서는 간섭을 피하기 위해 다른 노드에서 실행

### Local Disks
- RAID 구성 없이 4~8개 디스크, noatime으로 mount 권장
- HDFS를 사용한다면 HDFS와 동일한 디스크 사용

### Memory
- 8 ~ 100GB의 memory에서 잘 동작 (전체 메모리의 75%만 사용 권장)

### Network
- 10G 이상 사용 권장

### CPU Core
- 8 ~ 16개

## Spark Configuration
### 설정방법
#### SparkConf Object

```scala
val conf = new SparkConf().setMaster("local[2]").setAppName("CountingSheep").set("spark.executor.memory", "1g")
val sc = new SparkContext(conf)
```

#### Java System Properties

```java
-D"spark.executor.memory"="1g"
```


#### Command Line Argument

```shell
$ ./bin/spark-submit --name "My app" --master local[4] --conf spark.shuffle.spill=false
--conf "spark.executor.extraJavaOptions=-XX:+PrintGCDetails -XX:+PrintGCTimeStamps" myApp.jar
```

#### $SPARK_HOME/conf
- fairscheduler.xml.template
- log4j.properties.template
- metrics.properties.template
- spark-default.conf.template
- spark-env.sh.template
