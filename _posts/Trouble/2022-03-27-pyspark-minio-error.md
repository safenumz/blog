---
layout: post
title: '[Trouble PySpark] PySpark 3.2.1에서 MinIO 연결 시 dependency 에러'
category: Trouble
tags: [spark, pyspark, read]
comments: true
---

- guava 버전이 낮아서 문제가 생길 수 있음

```sh
$ cd $SPARK_HOME/jars
$ mv guava-14.0.1.jar guava-14.0.1.jar.bak

$ wget https://repo1.maven.org/maven2/com/google/guava/guava/31.1-jre/guava-31.1-jre.jar
$ wget https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-aws/3.3.1/hadoop-aws-3.3.1.jar
$ wget https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk-bundle/1.12.218/aws-java-sdk-bundle-1.12.218.jar
```
