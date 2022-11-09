---
layout: post
title: '[Zeppelin] Zeppeline을 위한 Spark 개요'
category: DevOps
tags: [zeppeline, spark]
comments: true
---

# 빅데이터 분석의 시초
## 빅데이터?
> 빅데이터란 기존 데이터베이스 관리도구의 능력을 넘어서는 대량의 정형 또는 심지어 데이터베이스 형태가 아닌 비정형의 데이터 집합조차 포함한 데이터로부터 가치를 추출하고 결과를 분석하는 기술이다

- SQL 기반의 데이터베이스
- 주로 컴퓨터 1대에서 돌아감, 고성능이 필요하면 좋은(비싼 컴퓨터) 필요
- 대량 : 컴퓨터 1대로 처리할 수 없는 양(수십 TB 이상)
- 3V (by IBM) : Volume, Velocity, Variety

- 기술 - 컴퓨터 1대로 처리하지 못하므로, 여러대를 연결해서 데이터를 저장하고 처리하자.
- 주로 구글 등 검색엔진 회사들이 웹 전체를 저장하고 처리하려다보니 기술 개발이 필요하게 됨
- 구글이 이끌고, 야후 등이 오픈소스를 통해 (하둡) 적극 지원, 접근하기 쉬워지고 널리 쓰이기 시작
- SQL 기반의 데이터는 거의 행렬 형태로 정형화된 데이터였으나 일반 문서(웹 문서) 등과 같이 비정형화 데이터도 초점

- 데이터를 저장만 해서는 쓸모가 없음
- 데이터를 읽어들이고 변환하고 핵심을 추출하는 것도 마찬가지고 컴퓨터 1대로 할 수 있는 것보다 훨씬 빨라져야 함
- 맵리듀스 (MapReduce) : 분산 데이터 처리
- 현재는 스파크 (Apache Spark) 가 널리 쓰임


## GFS(Google File System) 논문 (2003)
- 막대한 양의 웹 문서를 저장 조회해야 하는데, 컴퓨터 1대로는 당연히 처리가 불가능
- 저렴한 하드웨어를 사용하면서, 대신 중복저장을 통해 파일 유실을 방지
- 파일을 새로 추가하는데 집중, 삭제나 파일 덮어쓰기는 어려움
- Latency보다 Throughput을 중시
- 클러스터 갯수를 늘릴수록 저장용량과 throughput이 점점 올라감
- 여러 컴퓨터를 연결하여 저장용량과 I/O 성능을 scale
- 이를 구현한 오픈소스 프로젝트인 Hadoop HDFS

## MapReduce 논문 (2004)
- Google MapReduce (2004)
- 여러대의 분산 저장소에 존재하는 데이터를 변환하거나 계산하기 위한 프레임 워크
- Functional Programming의 Map() 함수와 Reduce() 함수를 조합하여 효율적으로 분산 환경에서 다양한 계산을 함
- Map과 Reduce 연산을 조합하여 클러스터에서 실행, 큰 데이터를 처리
- 이를 구현한 오픈소스 프로젝트인 Hadoop MapReduce

## Hadoop
- GPS(2003)과 MapReduce(2004) 논뭉를 보고, Doug Cutting과 Mike Cafarella가 이를 오픈소스로 구현
- 야후에서 프로젝트를 하던 중 그 한 부분으로 만듬, 이후 오픈소스로 공개 (2006)
- Hadoop : 아들의 노란 코끼리 장난감의 이름을 따 만듬


## Hive
- SQL로 분석 쿼리를 실행하면, 이를 MapReduce 코드로 변환해주는 도구
- MapReduce 코드는 작성하기 아주 불편하므로 큰 인기를 끔
- 쿼리로 MapReduce의 거의 모든 기능을 표현할 수 있다
- HDFS등에 있는 파일을 읽어들여 쿼리로 분석 수행
- HiveQL을 작성하면 MapReduce 코드로 변환되어 실행


> 그렇게 10년이 지나고,
> 지금까지도 MapReduce와 Hive는 많이 사용되고 있는 빅데이터 기술입니다
> MR, Hive에 도전하는 기술들
> Impala, Pheonix, Pig, Tez 등등

## 하둡 생태계의 많은 프로젝트들
### pig
- Pig Latin이라는 하이레벨 언어로 MapReduce를 실행할 수 있는 도구
- Netflix 등에서 사용하면서 주목을 받았고, 현재는 거의 사용되지 않음

### Tez
- MapReduce의 성능적, 표현적 한계를 극복하고자 하는 실행엔진
- Spark의 급부상으로 거의 주목을 받지 못함

### Impala
- MapReduce 기반의 Hive의 느린 응답성을 개선한 도구
- Spark의 급부상으로 (Spark SQL) 초기에 약간 주목을 받다가 현재는 거의 사용되지 않음

## MapReduce/Hive 장단점
### 장점
- 빅데이터 시대를 열어준 선구적인 기술
- 거대한 데이터를 안정적으로 처리
- 많은 사람드이 사용 중

### 단점
- 오래된 기술이다 보니,
- 발전이 느리다
- 불편한 점이 많다

## MapReduce의 문제점
- MapReduce는 Map의 입출력 및 Reduce의 입출력 및 Reduce의 입출력을 매번 HDFS에 쓰고 읽는다 - 느리다
- MapReduce 코드는 작성하기 불편하다 - 좀 더 좋은 인터페이스가 있으면 좋겠다


# Apache Spark
- 비교적 최근에 (2012) 등장하여 선풍적인 인기를 얻고 있는 분산처리 프레임워크
- 메모리 기반의 처리를 통한 고성능과 Functional Programming 인터페이스를 활용한 편리한 인터페이스가 특징
- 핵심 개념 : RDD (Resilient Distributed Dataset)
- 인터페이스 : Scala
- 인메모리, 메모리 상에 데이터를 올려 놓은 상태로 데이터 처리

> 단순한 연산인 경우 데이터를 읽고 연산하는 방식이 나쁘지 않지만 반복연산일 수록 효율이 떨어진다. 다단계의 연산이나 반복 연산의 경우 중간 결과를 메모리에 저장하면 매번 디스크에 쓰는 것보다 훨씬 빠르다

## RDD
- Resilient Distributed Dataset - 탄력적으로 분산된 데이터셋
  - 오류 자동복구 기능이 포함된 가상의 리스트
  - 다양한 계산을 수행 가능, 메모리를 활용하여 높은 성능을 지님
- 클러스터에 분산된 메모리를 활용하여 계산되는 List
- 데이터를 어떻게 구해낼지를 표현하는 Transformation을 기술한 Lineage(계보)를 interactive하게 만들어 낸 후, Action을 통해 lazy하게 값을 구해냄
- 클러스터 중 일부의 고장등으로 작업이 중간에 실패하더라도, Lineage를 통해 데이터를 복구

## Interface - Scala
- 매우 간결한 표현이 가능한 모던 프로그래밍 언어
- REPL(aka Shell) 제공, interactive하게 데이터를 다루는 것이 가능
- Functional Programming이 가능하므로 MapReduce와 같은 functional한 개념을 표현하기에 적합함

```scala
val file = spark.textFile("hdfs://...")
val counts = file.flatMap(line => line.split(" "))
  .map(word = word, 1)
  .reduceByKey(_ + _)
counts.saveAsTextFile("hdfs://...")
```

## 확장 프로젝트들
- Spark SQL : Hive와 비슷하게 SQL로 데이터 분석
- Spark Streaming : 실시간 분석
- MLlib : 머신러닝 라이브러리
- GraphX : 페이지랭크같은 그래프 분석
- SparkR
- Zeppelin

## 장점들
### 시간과 비용을 아껴준다
- 수십대의 Hadoop Cluster를 10대 이하의 Cluster로 대체할 수 있다
- 수십분 기다려야 하던 작업이 1분만에 완료된다

### 작업 능률 향상
- MR 작업 코드 만들고, 패키징하고, submit하고 하던 복잡한 과정이, shell에서 코드 한줄 치는 것으로 대체된다
- 처음 접하는 사람도 배우기 쉽다

### 다양한 제품을 조합해야 했던 작업이 Spark으로 다 가능하다
