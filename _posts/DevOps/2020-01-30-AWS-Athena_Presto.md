---
layout: post
title: '[AWS] Presto 기반의 Athena'
category: DevOps
tags: [aws, athena, presto]
comments: true
---

# 서비스 > 분석 > Athena
- Presto 엔진 사용
- EXTERNAL TABLE을 만들고 이를 S3 저장소의 데이터와 연결하여 쿼리 결과를 얻음

## EXTERNAL TABLE 생성

~~~sql
CREATE EXTERNAL TABLE IF NOT EXISTS top_tracks (
	id string,
	artist_id string,
	popularity int,
	external_url string
) PARTITIONED BY (dt string)
STORED AS PARQUET LOCATION "s3://spotify-folder/top-tracks/" 
tblproperties("parquest.compress"="SNAPPY")


MSCK REPAIR TABLE top_tracks
~~~

~~~sql
CREATE EXTERNAL TABLE IF NOT EXISTS audio_features (
	id string,
    danceability DOUBLE,
    energy DOUBLE,
    key int,
    loudness DOUBLE,
    mode int,
    speechiness DOUBLE,
    acousticness DOUBLE,
    instrumentalness DOUBLE
) PARTITIONED BY (dt string)
STORED AS PARQUET LOCATION "s3://spotify-folder/audio-features/" 
tblproperties("parquest.compress"="SNAPPY")

MSCK REPAIR TABLE audio_features
~~~

## SELECT Data

~~~sql
-- 최근 7일 동안의 파티션 데이터 확인
SELECT * FROM top_tracks
WHERE CAST(dt AS date) >= CURRENT_DATE - INTERVAL '7' DAY
LIMIT 10

SELECT * FROM audio_features LIMIT 10

SELECT 
AVG(danceability),
AVG(loudness)
FROM audio_features
WHERE CAST(dt AS date) = CURRENT_DATE
~~~