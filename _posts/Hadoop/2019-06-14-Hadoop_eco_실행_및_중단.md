---
layout: post
title: '[Hadoop] Hadoop Eco System 실행 및 중단'
category: Hadoop
tags: [spark, hadoop, cluster]
comments: true
---

# Hadoop eco Cluster System 실행 및 중단
- 서버는 총 5대로 구성되어 있음
- 1번 서버: 분석서버1
- 2번 서버: 수집서버
- 3번 서버: 분석서버2
- 4번 서버: namenode, datanode1
- 5번 서버: secondnode, datanode2

## 실행 명령 흐름 정리

~~~shell
# [4번 서버] start hadoop
$ hadoop/sbin/start-dfs.sh

# [5번 서버] start yarn
$ hadoop/sbin/start-yarn.sh

# [3번 서버] start MR history server
$ hadoop/sbin/mr-jobhistory-daemon.sh start historyserver

# [3번 서버] start metastore server
$ hive --service metastore -p 8011 & ​

# [4번 서버] start presto server
$ presto/bin/launcher start

# [5번 서버] start presto server
$ presto/bin/launcher start

# [3번 서버] start presto
$ ./presto --server hd4.cluster.kr:8012 --catalog hive --schema default ​

# [2번 서버] move kafka floder
$ cd kafka

# [2번 서버] start zookeeper
$ bin/zookeeper-server-start.sh config/zookeeper.properties &

# [2번 서버] start kakfa broker
$ bin/kafka-server-start.sh config/server.properties &

# [2번 서버] create kafka topics
$ bin/kafka-topics.sh --create --zookeeper hd2.cluster.kr:2181 --replication-factor 1 --partitions 1 --topic twitter &
$ bin/kafka-topics.sh --list --zookeeper hd2.cluster.kr:2181 &
~~~

## 서비스 시작 순서 : HDFS -> YARN -> MR-History Server

~~~shell
# namenode: 하둡 실행
$ hadoop/sbin/start-dfs.sh

# secondnode: YARN 서비스 실행
$ hadoop/sbin/start-yarn.sh

# namenode: MR-History Server 실행
$ hadoop/sbin/mr-jobhistory-daemon.sh start historyserver


$ zookeeper/bin/zkServer.sh    start

# client : 하둡 제대로 실행되었는지 테스트
$ hadoop  fs  -lsr /
$ hdfs dfs -chmod 777 /tmp
$ hdfs dfs -mkdir /user
$ hdfs dfs -mkdir /user/hadoop
$ echo 'test' > test.txt
$ hdfs dfs -put test.txt /user/hadoop/hadooptest.txt
$ hdfs dfs -ls -R /user
$ hdfs dfs -ls

## client 테스트용 파일 만들어 HDFS에 올리기
$ echo 'aaa,100' > pig.txt
$ echo 'bbb,200' >> pig.txt
$ echo 'ccc,300' >> pig.txt
$ echo 'aaa,400' >> pig.txt
$ hdfs dfs -put  pig.txt  /user/hadoop/pig.txt
$ hdfs dfs -ls  .
$ hdfs dfs -cat   pig.txt

# client : pig shell
$ pig

# client : pig test
grunt> ls
grunt> cat  pig.txt
grunt> k2 = load 'pig.txt' using PigStorage(',')
                  as ( str:chararray, price:int );
grunt> k4 = GROUP k2 BY $0;
grunt> k8 = foreach k4 generate group, SUM(k2.price);
grunt> dump  k8;
grunt> store  k8  into 'mr_pig';
grunt> cat  mr_pig
grunt> illustrate  k8;
grunt> quit


# client : pyspark local mode
$ spark/bin/pyspark --master local[2]

# client : spark local mode
$ spark/bin/spark-shell

# client : spark cluster mode
$ spark/bin/spark-shell --master yarn

# client : spark cluster mode test
scala> var scalastr = sc.textFile("hdfs:///user/hadoop/pig.txt")
scala> scalastr.count()

# client : hive MetaStore 서비스 시작하기
$ hive --service metastore -p 8011 &

# client : hive shell
$ hive

# client : hive test
hive> show tables;
hive> create table hive
         ( str string, price int )
         ROW FORMAT DELIMITED FIELDS TERMINATED BY ',';
hive> show tables;
hive> load data local inpath 'hive.txt' into table hive;
hive> select * from hive;
hive> select str,sum(price) from hive group by str;
hive> quit;


# namenode : presto 코디네이터 demon 실행
$ presto/bin/launcher start

# secondnode : presto worker demon 실행
$ presto/bin/launcher start

# client : presto
$ ./presto --server namenode.hadoop.kr:8012 --catalog hive --schema default

# client : presto test
presto:default> show tables;
presto:default> select * from hive;
presto:default> select str,sum(price) from hive group by str;
presto:default> quit;
~~~

## 서비스 중단하기 순서 : YARN -> MR-History Server -> HDFS

~~~shell
# secondnode : YARN 서비스 중단
$ hadoop/sbin/stop-yarn.sh

# namenode : MR-History Server 중단
$ hadoop/sbin/mr-jobhistory-daemon.sh stop historyserver

# namenode : HDFS 서비스 중단하기
$ hadoop/sbin/stop-dfs.sh

zookeeper/bin/zkServer.sh    stop
~~~

# 실행 디폴트 폴더 설정

~~~shell
$ hadoop fs -lsr /user

$ hadoop fs -cp /user/hive/warehouse/movies/movies.csv .
~~~

# wordcount 예제 실행

~~~shell
$ bin/yarn jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.4.jar wordcount user/hadoop/conf output
~~~
