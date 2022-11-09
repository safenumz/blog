---
layout: post
title: '[PySpark] Spark DataFrame'
category: Spark
tags: [spark, pyspark, dataframe]
comments: true
---

# Spark DataFrame 생성

~~~python
from datetime import datetime

raw = sqlContext.read.format("parquet").load("s3://spotify-folder/top-tracks/dt=2020-01-29/top-tracks.parquet")
# raw = sqlContext.read.format("parquet").load("s3://spotify-folder/top-tracks/dt={}/top-tracks.parquet".format(dt))
# data = sc.textFile("text.txt")
raw.printSchema()
~~~

<pre>
root
 |-- artist_id: string (nullable = true)
 |-- external_url: array (nullable = true)
 |    |-- element: string (containsNull = true)
 |-- id: array (nullable = true)
 |    |-- element: string (containsNull = true)
 |-- name: array (nullable = true)
 |    |-- element: string (containsNull = true)
 |-- popularity: array (nullable = true)
 |    |-- element: long (containsNull = true)
</pre>

~~~python
df = raw.toDF("id", "artist_id", "name", "popularity", "external_url")
df.show()
~~~


<pre>
+--------------------+--------------------+--------------------+--------------------+------------+
|                  id|           artist_id|                name|          popularity|external_url|
+--------------------+--------------------+--------------------+--------------------+------------+
|00FQb4jTyendYWaN8...|[https://open.spo...|[6zegtH6XXd2PDPLv...|[Don’t Call Me An...|        [82]|
|00FQb4jTyendYWaN8...|[https://open.spo...|[6zegtH6XXd2PDPLv...|[Don’t Call Me An...|        [82]|
|00FQb4jTyendYWaN8...|[https://open.spo...|[6zegtH6XXd2PDPLv...|[Don’t Call Me An...|        [82]|
|00FQb4jTyendYWaN8...|[https://open.spo...|[6zegtH6XXd2PDPLv...|[Don’t Call Me An...|        [82]|
|00FQb4jTyendYWaN8...|[https://open.spo...|[0Oqc0kKFsQ6MhFOL...|        [Doin' Time]|        [78]|
|00FQb4jTyendYWaN8...|[https://open.spo...|[0Oqc0kKFsQ6MhFOL...|        [Doin' Time]|        [78]|
|00FQb4jTyendYWaN8...|[https://open.spo...|[0Oqc0kKFsQ6MhFOL...|        [Doin' Time]|        [78]|
|00FQb4jTyendYWaN8...|[https://open.spo...|[0Oqc0kKFsQ6MhFOL...|        [Doin' Time]|        [78]|
|00FQb4jTyendYWaN8...|[https://open.spo...|[2nMeu6UenVvwUktB...|[Young And Beauti...|        [75]|
|00FQb4jTyendYWaN8...|[https://open.spo...|[2nMeu6UenVvwUktB...|[Young And Beauti...|        [75]|
|00FQb4jTyendYWaN8...|[https://open.spo...|[2nMeu6UenVvwUktB...|[Young And Beauti...|        [75]|
|00FQb4jTyendYWaN8...|[https://open.spo...|[2nMeu6UenVvwUktB...|[Young And Beauti...|        [75]|
|00FQb4jTyendYWaN8...|[https://open.spo...|[3RIgHHpnFKj5Rni1...|[Norman fucking R...|        [75]|
|00FQb4jTyendYWaN8...|[https://open.spo...|[3RIgHHpnFKj5Rni1...|[Norman fucking R...|        [75]|
|00FQb4jTyendYWaN8...|[https://open.spo...|[3RIgHHpnFKj5Rni1...|[Norman fucking R...|        [75]|
|00FQb4jTyendYWaN8...|[https://open.spo...|[3RIgHHpnFKj5Rni1...|[Norman fucking R...|        [75]|
|00FQb4jTyendYWaN8...|[https://open.spo...|[7MtVPRGtZl6rPjMf...|[Fuck it I love you]|        [75]|
|00FQb4jTyendYWaN8...|[https://open.spo...|[7MtVPRGtZl6rPjMf...|[Fuck it I love you]|        [75]|
|00FQb4jTyendYWaN8...|[https://open.spo...|[7MtVPRGtZl6rPjMf...|[Fuck it I love you]|        [75]|
|00FQb4jTyendYWaN8...|[https://open.spo...|[7MtVPRGtZl6rPjMf...|[Fuck it I love you]|        [75]|
+--------------------+--------------------+--------------------+--------------------+------------+
only showing top 20 rows
</pre>


~~~python
raw = sqlContext.read.format("parquet").load("s3://spotify-folder/audio-features/dt=2020-01-29/top-tracks.parquet")
raw.printSchema()
~~~

<pre>
root
 |-- acousticness: double (nullable = true)
 |-- analysis_url: string (nullable = true)
 |-- danceability: double (nullable = true)
 |-- duration_ms: long (nullable = true)
 |-- energy: double (nullable = true)
 |-- id: string (nullable = true)
 |-- instrumentalness: double (nullable = true)
 |-- key: long (nullable = true)
 |-- liveness: double (nullable = true)
 |-- loudness: double (nullable = true)
 |-- mode: long (nullable = true)
 |-- speechiness: double (nullable = true)
 |-- tempo: double (nullable = true)
 |-- time_signature: long (nullable = true)
 |-- track_href: string (nullable = true)
 |-- type: string (nullable = true)
 |-- uri: string (nullable = true)
 |-- valence: double (nullable = true)
</pre>


