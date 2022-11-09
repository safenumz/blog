---
layout: post
title: '[Spark] PySpark Config 추가'
category: Spark
tags: [spark, pyspark, config]
comments: true
---

## Config 추가

```python
# 기존 config 저장
base_conf = spark.sparkContext._conf.getAll()

# 추가할 config append
base_conf.append(("spark.speculation","false"))

# spark conf 세팅
conf = spark.sparkContext._conf.setAll(base_conf)

# 기존 spark context stop
spark.sparkContext.stop()

spark = SparkSession.builder.config(conf=conf).getOrCreate()
```

## jars 추가

```python
spark.sparkContext.addPyFile("/path/to/jar/xxxx.jar")
```

## extraClassPath 추가
- jars을 extraClassPath에 넣으면 됨

```sh
$ vi ${SPARK_HOME}/conf/spark-defaults.conf  
```

```sh
# spark-defaults.conf  
spark.driver.extraClassPath /Users/a/spark/drivers/*
```


```python

import os
from typing import Tuple
import findspark

findspark.init()

import pyspark
from pyspark.sql import SparkSession
from pyspark.context import SparkContext


def setup_spark(app_name: str) -> Tuple[SparkSession, SparkContext]:
    # Submit args is actually set via the file `jupyter-env.sh`, but I'll leave it here for completion
    os.environ['PYSPARK_SUBMIT_ARGS'] = "--packages org.apache.hadoop:hadoop-aws:3.2.2,com.microsoft.ml.spark:mmlspark_2.11:1.0.0-rc3,org.apache.spark:spark-avro_2.12:2.4.5,com.amazonaws:aws-java-sdk:1.11.956 --repositories https://mmlspark.azureedge.net/maven pyspark-shell"
    
    spark = (
        SparkSession.builder.appName(app_name)
        .config("spark.sql.execution.arrow.enabled", "true")
        .config("spark.sql.repl.eagerEval.enabled", "true")
        .getOrCreate()
    )
    sc = spark.sparkContext
    sc.setSystemProperty("com.amazonaws.services.s3.enableV4", "true")
    sc._jsc.hadoopConfiguration().set(
        "fs.s3a.impl", "org.apache.hadoop.fs.s3a.S3AFileSystem"
    )
    return (spark, sc)
```