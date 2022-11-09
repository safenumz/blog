---
layout: post
title: '[Trouble zeppline] Spark Interpreter 에러'
category: Trouble
tags: [trouble, spark]
comments: true
---

# Environment
- Windows 10
- Spark 2.4.0
- Zeppelin 0.8.0
- Python 3.7

# Trouble
- Zeppline 실행시 Spark Interpreter 에러가 발생하는 경우

# Shooting
- %ZEPPLINE_HOME%/conf 폴더의 zeppelin-env.cmd 수정 (맥, 리눅스는 zeppelin-env.sh)

~~~sh
REM set JAVA_HOME= C://Program Files/Java/jre1.8.0_201
REM set SPARK_HOME=C://spark-2.4.0-bin-hadoop2.7  
REM set PYSPARK_PYTHON=%SPARK_HOME%/python;%SPARK_HOME%/python/lib/py4j-0.10.7-src.zip;%SPARK_HOME%\python\lib/pyspark.zip
REM set PYTHONPATH=%SPARK_HOME%\python;%SPARK_HOME%\python\lib\py4j-0.10.7-src.zip
~~~
