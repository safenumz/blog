---
layout: post
title: '[Hadoop] Hadoop Eco System 구축'
category: Architecture
tags: [hadoop, spark]
comments: true
---

# Hadoop
- java 1.7.0
- mysql community server 5.6.44
- mysql connector java 5.1.23
- hadoop 2.6.4
- hive 1.2.1
- pig 0.16.0
- spark 1.6.2
- sqoop 1.4.7
- scala 2.10.1
- mesos 0.21.0
- zookeeper 3.4.6


# 프로토콜 버퍼 설치
- 프로토콜 버퍼는 데이터를 연속된 비트로 만들고, 이렇게 만들어진 비트를 해석해 원래의 데이터를 만들 수도 있다.
- Hadoop 2. 버전은 프로토콜 버퍼 2.5.0만 지원한다. 반드시 버전을 맞춰 설치한다.
- root 계정에서 실행

~~~shell
$ wget https://github.com/google/protobuf/releases/download/v2.5.0/protobuf-2.5.0.tar.gz

$ tar xvzf protobuf-2.5.0.tar.gz

$ yum install gcc-c++

$ ./configure

$ make

$ make install

# 제대로 설치되었는지 확인
$ protoc --version
~~~

# zookeeper 설치

~~~shell
$ adduser zookeeper
$ passwd zookeeper
~~~

## zookeeper 다운로드

~~~shell
# zookeeper 3.4.10
$ wget https://archive.apache.org/dist/zookeeper/zookeeper-3.4.10/zookeeper-3.4.10.tar.gz

$ tar xvzf zookeeper-3.4.10.tar.gz

$ ln -s zookeeper-3.4.10 zookeeper

$ cp zookeeper/conf/zoo_sample.cfg zookeeper/conf/zoo.cfg

$ vi zookeeper/conf/zoo.cfg
~~~

## zoo.cfg 편집

~~~
# zoo.cfg
maxClientCnxns=0
maxSessionTimeout=180000
server.1=namenode.hadoop.kr:2888:3888
server.2=secondnode.hadoop.kr:2888:3888
server.3=client.hadoop.kr:2888:3888
~~~


# zookeeper 설치
[https://blog.naver.com/inho1213/220864633256?proxyReferer=https%3A%2F%2Fm.blog.naver.com%2FPostView.nhn%3FblogId%3Dinho1213%26logNo%3D220890490389%26proxyReferer%3Dhttps%253A%252F%252Fwww.google.com%252F](https://blog.naver.com/inho1213/220864633256?proxyReferer=https%3A%2F%2Fm.blog.naver.com%2FPostView.nhn%3FblogId%3Dinho1213%26logNo%3D220890490389%26proxyReferer%3Dhttps%253A%252F%252Fwww.google.com%252F)

~~~bash
# java 1.7.0
# mysql community server 5.6.44
# mysql-connector-java-5.1.23
$ wget www.db21.co.kr/big/mysql-connector-java-5.1.23-bin.jar

# hadoop 2.6.4
$ wget www.db21.co.kr/big/hadoop-2.6.4.tar.gz

# hive 1.2.1
$ wget www.db21.co.kr/big/apache-hive-1.2.1-bin.tar.gz

# pig 0.16.0
$ wget www.db21.co.kr/big/pig-0.16.0.tar.gz

# spark 1.6.2
$ wget www.db21.co.kr/big/spark-1.6.2-bin-hadoop2.6.tgz

# sqoop 1.4.7
$ wget http://apache.tt.co.kr/sqoop/1.4.7/sqoop-1.4.7.bin__hadoop-2.6.0.tar.gz

$ tar xvzf sqoop-1.4.7.bin__hadoop-2.6.0.tar.gz

$ ln -s sqoop-1.4.7.bin__hadoop-2.6.0 sqoop

# scala 2.10.1
$ wget http://www.scala-lang.org/files/archive/scala-2.10.1.tgz

$ tar xvzf scala-2.10.1.tgz

# presto 0.65
$ wget www.db21.co.kr/big/presto-server-0.65.tar.gz

# mesos 0.21.0
# Spark 1.6.0 is designed for use with Mesos 0.21.0 and does not require any special patches of Mesos.
$ wget http://archive.apache.org/dist/mesos/0.21.0/mesos-0.21.0.tar.gz

# zookeeper 3.4.10
$ wget https://archive.apache.org/dist/zookeeper/zookeeper-3.4.10/zookeeper-3.4.10.tar.gz


~~~shell
# .bashrc

if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi

#-----------------------------------------------
# HADOOP Config Start

pathmunge () {
    case ":${PATH}:" in
        *:"$1":*)
            ;;
        *)
            if [ "$2" = "after" ] ; then
                PATH=$PATH:$1
            else
                PATH=$1:$PATH
            fi
    esac
}

export JAVA_HOME=/usr/local/java
export CLASSPATH=/usr/local/java/jre/lib/*
pathmunge /usr/local/java before
pathmunge /usr/local/java/bin before

export BASEHOME=/home/hadoop

export HADOOP_PREFIX=$BASEHOME/hadoop
export HADOOP_HOME=$BASEHOME/hadoop
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop

export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native"
export HADOOP_CLASSPATH=$JAVA_HOME/lib/tools.jar

export PIG_HOME=$BASEHOME/pig
export PIG_CLASSPATH=$BASEHOME/hadoop/conf

export HIVE_HOME=$BASEHOME/hive
export HIVE_CONF_DIR=$BASEHOME/hive/conf
export HIVE_CLASS_PATH=$HIVE_CONF_DIR
export HADOOP_USER_CLASSPATH_FIRST=true

export SPARK_HOME=/home/hadoop/spark
export SPARK_CONF_DIR=$SPARK_HOME/conf

#pathmunge $BASEHOME/sqoop/bin
pathmunge $BASEHOME/pig/bin
pathmunge $BASEHOME/hive/bin
pathmunge $BASEHOME/hadoop/bin

# HADOOP Config End
#-----------------------------------------------

~~~

# 하둡 클러스터 실행 및 중단

~~~shell
# 서비스 시작 순서 : HDFS -> YARN -> MR-History Server
# namenode: 하둡 실행
$ hadoop/sbin/start-dfs.sh

# secondnode: YARN 서비스 실행
$ hadoop/sbin/start-yarn.sh

# namenode: MR-History Server 실행
$ hadoop/sbin/mr-jobhistory-daemon.sh start historyserver

# client : 하둡 제대로 실행되었는지 테스트
$ hadoop  fs  -lsr /
$ hdfs dfs -chmod 777 /tmp
$ hdfs dfs -mkdir /user
$ hdfs dfs -mkdir /user/hadoop
$ echo 'test' > test.txt
$ hdfs dfs -put test.txt /user/hadoop/hadooptest.txt
$ hdfs dfs -ls -R /user
$ hdfs dfs -ls


# 서비스 중단하기 순서 : YARN -> MR-History Server -> HDFS
# secondnode : YARN 서비스 중단
$ hadoop/sbin/stop-yarn.sh

# namenode : MR-History Server 중단
- MR-History Server 중단하기
$ hadoop/sbin/mr-jobhistory-daemon.sh stop historyserver

# namenode : HDFS 서비스 중단하기
$ hadoop/sbin/stop-dfs.sh
~~~

# spark cluster mode test

~~~scala
$ spark/bin/spark-shell --master yarn --num-executors 2
scala> var scalastr = sc.textFile("hdfs:///user/hadoop/pig.txt")
scala> scalastr.count()
scala> scalastr.first()
scala> println(scalastr.collect().mkString("\n"))
scala> CTRL+D
~~~


# spark 모니터링
- 노트북에 hosts 파일 편집
- HDFS 네임노드 웹UI 호스트명/포트 -> 네임노드 호스트명:50070
- http://namenode.hadoop.kr:50070/

- HDFS 보조네임노드 웹UI 호스트명/포트  -> 보조네임노드 호스트명 :50090
- http://secondnode.hadoop.kr:50090/

- YARN 리소스매니저 웹UI 호스트명/포트 -> 리소스매니저 호스트명:8088
- http://secondnode.hadoop.kr:8088/

- YARN 노드매니저 웹UI 호스트명/포트 -> 노드매니저(1~N) 호스트명:8042
- http://datanode1.hadoop.kr:8042/
- http://datanode2.hadoop.kr:8042/


- Spark 드라이버 포트 -> 클라이언트 호스트명:4040
- http://client.hadoop.kr:4040/

- Presto Coordinator 웹 UI
- http://namenode.hadoop.kr:8012/



~~~shell
# 윈도우: C:\Windows\System32\drivers\etc\hosts

$ sudo vi /private/etc/hosts



192.168.- client.hadoop.kr
192.168.- namenode.hadoop.kr
192.168.- secondnode.hadoop.kr
192.168.- datanode1.hadoop.kr
192.168.- datanode2.hadoop.kr


# DNS cache 갱신
$ dscacheutil -flushcache
~~~


~~~
-> 클라이언트 머신에서 Hive MetaStore 서비스 시작하기
[8]$ hive --service metastore -p 8011 &

# Presto Server 파일 내려받기
[1]$ wget www.db21.co.kr/big/presto-server-0.65.tar.gz


-> Presto Server 파일을 2번,3번 머신에 보내기
[2]$ scp presto-server-0.65.tar.gz hadoop@namenode:/home/hadoop/
    scp presto-server-0.65.tar.gz hadoop@secondnode:/home/hadoop/

# Presto CLI 실행파일 내려받기
[3]$ wget www.db21.co.kr/big/presto-cli-0.65-executable.jar

-> Presto로 이름을 변경하고 실행권한 주기
[4]$ mv presto-cli-0.65-executable.jar  presto
[5]$ chmod 755 presto



-> namenode, secondnode Presto Worker 데몬 시작하기
[11]$ presto/bin/launcher start

-> Presto Server는 코디네이터 주소를 지정하면 됨
[3]$ ./presto --server namenode.hadoop.kr:8012 --catalog hive --schema default

[1]presto:default> show tables;
[2]presto:default> select * from hive;
[3]presto:default> select str,sum(price) from hive group by str;
[4]presto:default> quit;

~~~


# flume 설치

~~~shell
$ wget http://mirror.navercorp.com/apache/flume/1.8.0/apache-flume-1.8.0-bin.tar.gz
~~~
