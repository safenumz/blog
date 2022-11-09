---
layout: post
title: '[Python] MinIO 연결'
category: Python
tags: [python, minio]
comments: true
---

# Python에서 minio 연결

```python
import boto3

client = boto3.client('s3', 
    aws_access_key_id='<MINIO_ACCESS_KEY>',
    aws_secret_access_key='<MINIO_SECRET_KEY>',
    endpoint_url='http://<MINIO_URL>:<MINIO_PORT>',
    verify=False
)

# 버킷 리스트 불러오기
client.list_buckets()

import pandas as pd
# 데이터 읽기
response = client.get_object(Bucket='bike', Key="raw_data/csv/2015.csv")
df = pd.read_csv(response.get("Body"), encoding='cp949')
df

# 데이터 쓰기
client.upload_file(Filename="./raw_bike_2015.parquet", Bucket="bike", Key="raw_data/parquet/raw_bike_2015.parquet")

client.put_object(
    Body=open("2015.csv").read(),
    Bucket="bike",
    Key="raw_data/test/2015.csv"
)
```