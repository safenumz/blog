---
layout: post
title: '[Spark] PySpark에서 MinIO 연결'
category: Spark
tags: [spark, pyspark, minio]
comments: true
---

## MinIO 연결을 위한 Pyspark & jars dependency
- Pyspark: 3.1.2
- org.apache.hadoop:hadoop-aws:3.1.3
- com.amazonaws:aws-java-sdk-bundle:1.11.563
- com.google.guava:guava:27.1-jre

## jupyter notebook에서 dependency 적용 예시

```sh
# PYSPARK_SUBMIT_ARGS
--master=spark://<SPARK_MASTER_URL>:7077 --name 'pyspark.jupyter' --packages org.apache.hadoop:hadoop-aws:3.1.3,com.amazonaws:aws-java-sdk-bundle:1.11.563,com.google.guava:guava:27.1-jre  --deploy-mode client pyspark-shell
```

## Trobule Shooting
- 읽기나 쓰기 중에 에러가 발생하면 dependency 문제일 가능성이 크니 **--packages** 옵션을 통해 jars를 넣지 말고, **$SPARK_HOME/jars** 기본 경로에 jars 파일을 직접 넣거나 사용자 경로에 jars 파일을 넣고 우선순위를 설정하면 됨
- --conf "spark.driver.userClassPathFirst=true" --conf "spark.executor.userClassPathFirst=true" --conf "spark.driver.extraClassPath=/Users/a/spark/drivers/* " --conf "spark.executor.extraClassPath=/Users/a/spark/drivers/* "
- $SPARK_HOME/conf/spark-defaults.conf에다 기본값으로 넣을 수도 있음

```sh
# $SPARK_HOME/conf/spark-defaults.conf
spark.executor.userClassPathFirst   true
spark.driver.userClassPathFirst true
spark.driver.extraClassPath /Users/a/spark/drivers/*
spark.executor.extraClassPath /Users/a/spark/drivers/*
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

## 참고: S3 Config

```python
config = configparser.ConfigParser()

config.read_file(open('aws/dl.cfg'))

os.environ["AWS_ACCESS_KEY_ID"]= config['default']['AWS_ACCESS_KEY_ID']
os.environ["AWS_SECRET_ACCESS_KEY"]= config['default']['AWS_SECRET_ACCESS_KEY']

spark = SparkSession \
      .builder \
      .config("spark.jars.packages", "org.apache.hadoop:hadoop-aws:2.7.0") \
      .config("spark.hadoop.fs.s3a.impl","org.apache.hadoop.fs.s3a.S3AFileSystem") \
      .config("spark.hadoop.fs.s3a.awsAccessKeyId", os.environ['AWS_ACCESS_KEY_ID']) \
      .config("spark.hadoop.fs.s3a.awsSecretAccessKey", os.environ['AWS_SECRET_ACCESS_KEY']) \
      .getOrCreate()
```

## CSV Read

```python
df = spark.read\
    .option("header", "true")\
    .option("inferSchema", "true")\
    .csv("s3a://data/flight-data/csv/2015-summary.csv")

df.show()
```

## CSV Write
```python
df.write.format("csv").mode("overwrite").option("sep", "\t")\
    .save("s3a://tmp/my-tsv-file.csv")
```

### Spark TrainValidationSplitModel MinIO에 저장

```python
$ spark_ml_model.write().overwrite().save("s3a://test/modelLocation")

# model = TrainValidationSplitModel.load("s3a://test/modelLocation")
```

### 기타 Write

```python
tempdf \
      .save \
      .mode("append") \
      .partitionBy("p_country","pt_year", "pt_month", "pt_day", "pt_secs") \                         
      .parquet(f"s3a://{params['bucket_name']}/output/landing/{system_}/{params['interface']}/{str(uuid.uuid4())}")


tempdf=tempdf.repartition(col("pt_country"),col("pt_year"),col("pt_month"),col("pt_day"),col("pt_secs"))
tempdf \
      .coalesce(2) \
      .write \
      .partitionBy('pt_country','pt_year','pt_month','pt_day','pt_secs') \                              
      .parquet(f"s3a://{params['bucket_name']}/output/landing/{system_}/{params['interface']}/new_{str(uuid.uuid4())}.parquet",mode='overwrite')


tempdf \
      .write \
      .format("parquet") \
      .mode("append") \
      .partitionBy(*["p_country","pt_year", "pt_month", "pt_day", "pt_secs"]) \
      .save(f"s3a://{params['bucket_name']}/output/landing/{system_}/{params['interface']}/{str(uuid.uuid4())}")
```