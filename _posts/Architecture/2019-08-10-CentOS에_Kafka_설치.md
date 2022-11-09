---
layout: post
title: '[Kafka] CentOS에 Kafka 설치'
category: Architecture
tags: [kafka]
comments: true
---

# Environment
- Centos 7 서버 2대
- Kafka 2.3.0

# Kafka 구조
- Kafka는 Producer가 Consumer에게 다이렉트로 전송할 시에 발생하는 데이터 유실 장애를 방지하기 위해 중간에서 버퍼 역할을 한다.
- 편의상 Kafka Producer와 Kafka Server를 1번 서버에, Kafka Consumer를 2번 서버에 설치

> Kafka Producer > Kafka Server > Kafka Consumer


# [1번 서버] Kafka 다운로드

~~~shell
# scala 2.12 - kafka 2.3.0
$ wget http://apache.tt.co.kr/kafka/2.3.0/kafka_2.12-2.3.0.tgz

# 압축 해제
$ tar xvzf kafka_2.12-2.3.0.tgz

# 심볼링 링크
$ ln -s kafka_2.12-2.3.0 kafka
~~~

# [1번 서버] Zookeeper 서버 실행

~~~shell
$ cd kafka

# start Zookeeper
$ ./bin/zookeeper-server-start.sh config/zookeeper.properties &
~~~

# [1번 서버] Kafka Server 실행

~~~shell
# start kafka
$ ./bin/kafka-server-start.sh config/server.properties &

# zookeeper, kafka 서버가 잘 실행되었는지 확인
$ sudo netstat -anp | egrep "9092|2181"
~~~

# [1번 서버] kafka topic 생성

~~~sh
# twitter topic 생성
$ bin/kafka-topics.sh --create --zookeeper <zookeeper 서버 ip>:2181 --replication-factor 1 --partitions 1 --topic twitter &

# twitter list 확인
$ bin/kafka-topics.sh --list --zookeeper hd2:2181 &
~~~


# [1번 서버] Kafka Producer를 통해 메세지 보내기

~~~sh
# kafka producer
$ bin/kafka-console-producer.sh --broker-list hd2:9092 --topic twitter

>>> Hello Kafka
~~~


# [2번 서버] 카프카 다운로드

~~~shell
# scala 2.12 - kafka 2.3.0
$ wget http://apache.tt.co.kr/kafka/2.3.0/kafka_2.12-2.3.0.tgz

# 압축 해제
$ tar xvzf kafka_2.12-2.3.0.tgz

# 심볼링 링크
$ ln -s kafka_2.12-2.3.0 kafka
~~~

# [2번 서버] Kafka Producer(1번 서버)가 보낸 메시지를 Kafka Consumer(2번 서버)에 전송되는지 확인
- Kafka Producer(1번 서버)에서 보낸 'Hello Kafka' 메시지가 Kafka Consumer(2번 서버)에 들어 오면 성공

~~~shell
$ cd kafka

$ ./bin/kafka-console-consumer.sh --bootstrap-server hd2:9092 --topic twitter --from-beginning
Hello Kafka
~~~


