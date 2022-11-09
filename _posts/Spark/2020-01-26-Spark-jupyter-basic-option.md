---
layout: post
title: '[Spark] PySpark에서 Jupyter Notebook 시작시 기본 설정'
category: Spark
tags: [spark, jupyter, notebook]
comments: true
---

# PySpark에서 jupyter notebook 시작시 전체 기본 설정

```python
%load_ext sparksql_magic

spark.conf.set("spark.sql.repl.eagerEval.enabled", True)

import os
MINIO_ACCESS_KEY = os.environ['MINIO_ACCESS_KEY']
MINIO_SECRET_KEY = os.environ['MINIO_SECRET_KEY']

spark.sparkContext._jsc.hadoopConfiguration()\
    .set("fs.s3a.access.key", MINIO_ACCESS_KEY)
spark.sparkContext._jsc.hadoopConfiguration()\
    .set("fs.s3a.secret.key", MINIO_SECRET_KEY)
spark.sparkContext._jsc.hadoopConfiguration()\
    .set("fs.s3a.endpoint", "http://lab101:10170")
spark.sparkContext._jsc.hadoopConfiguration()\
    .set("fs.s3a.impl", "org.apache.hadoop.fs.s3a.S3AFileSystem")
spark.sparkContext._jsc.hadoopConfiguration()\
    .set("fs.s3a.connection.ssl.enabled", "false")
spark.sparkContext._jsc.hadoopConfiguration()\
    .set("fs.s3a.path.style.access", "true")
spark.sparkContext._jsc.hadoopConfiguration()\
    .set("com.amazonaws.services.s3.enableV2", "true")
spark.sparkContext._jsc.hadoopConfiguration()\
    .set("fs.s3a.aws.credentials.provider", "org.apache.hadoop.fs.s3a.SimpleAWSCredentialsProvider")
```


## jupyter notebook에서 Spark DataFrame 깔끔하게 보기

- 아래 옵션을 주면 Spark DataFrame **df**에 대하여, **df.show()** 명령어 없이 **df** 만으로도 Spark DataFrame을 깔끔하게 볼 수 있음
- Jupyter Notebook에서만 가능한 옵션

```python
spark.conf.set("spark.sql.repl.eagerEval.enabled", True)
```


## jupyter notebook에서 sparksql 사용

```sh
# 설치
(pyspark)$ pip install sparksql-magic
```

```sh
%load_ext sparksql_magic
```

```sh
%%sparksql
SELECT * FROM dfTable
```

## MinIO Config

```python
import os
MINIO_ACCESS_KEY = os.environ['MINIO_ACCESS_KEY']
MINIO_SECRET_KEY = os.environ['MINIO_SECRET_KEY']

spark.sparkContext._jsc.hadoopConfiguration()\
    .set("fs.s3a.access.key", MINIO_ACCESS_KEY)
spark.sparkContext._jsc.hadoopConfiguration()\
    .set("fs.s3a.secret.key", MINIO_SECRET_KEY)
spark.sparkContext._jsc.hadoopConfiguration()\
    .set("fs.s3a.endpoint", "http://<MINIO_SERVICE_URL>:<MINIO_SERVICE_PORT>")
spark.sparkContext._jsc.hadoopConfiguration()\
    .set("fs.s3a.impl", "org.apache.hadoop.fs.s3a.S3AFileSystem")
spark.sparkContext._jsc.hadoopConfiguration()\
    .set("fs.s3a.connection.ssl.enabled", "false")
spark.sparkContext._jsc.hadoopConfiguration()\
    .set("fs.s3a.path.style.access", "true")
spark.sparkContext._jsc.hadoopConfiguration()\
    .set("com.amazonaws.services.s3.enableV2", "true")
spark.sparkContext._jsc.hadoopConfiguration()\
    .set("fs.s3a.aws.credentials.provider", "org.apache.hadoop.fs.s3a.SimpleAWSCredentialsProvider")
```