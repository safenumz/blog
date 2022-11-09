---
layout: post
title: '[PySpark] 사용자 지정 함수 (UDF)'
category: Spark
tags: [spark, pyspark, udf]
comments: true
---

# 사용자 지정 함수 (UDF)

~~~python
import pyspark.sql.functions as F

df_new = df1.select(df1["danceability"], df1["acousticness"], df1["liveness"]).agg(F.avg(df1["danceability"]).alias("avg_danceability"), F.max(df1["acousticness"]).alias("max_acousticness"))
df_new.show()
~~~

<pre>
+------------------+----------------+
|  avg_danceability|max_acousticness|
+------------------+----------------+
|0.4411668899999993|           0.948|
+------------------+----------------+
</pre>

~~~python
from pyspark.sql.functions import udf
from pyspark.sql.types import *

udf1 = udf(lambda e: e.upper())

@udf(returnType=BooleanType())
def udf2(e):
    if e >= 0.06:
        return True
    else:
        return False
        
df_filtered = df1.filter(udf2(df1["danceability"]))
df_filtered.show()
~~~

<pre>
+------------+--------------------+-----+--------+-----+--------------------+------------+-----------------+--------+-------+-----+------+-------+---+--------------------+--------------+--------------------+--------------+
|danceability|              energy|  key|loudness| mode|         speechiness|acousticness|instrumentalnesss|liveness|valence|tempo|  type|     id|uri|          track_href|  analysis_url|         duration_ms|time_signature|
+------------+--------------------+-----+--------+-----+--------------------+------------+-----------------+--------+-------+-----+------+-------+---+--------------------+--------------+--------------------+--------------+
|       0.372|https://api.spoti...|0.649|  202193|0.538|0Oqc0kKFsQ6MhFOLB...|     0.00632|                7|  0.0951|-11.196|    0|0.0519|144.979|  4|https://api.spoti...|audio_features|spotify:track:0Oq...|         0.496|
|       0.372|https://api.spoti...|0.649|  202193|0.538|0Oqc0kKFsQ6MhFOLB...|     0.00632|                7|  0.0951|-11.196|    0|0.0519|144.979|  4|https://api.spoti...|audio_features|spotify:track:0Oq...|         0.496|
|       0.372|https://api.spoti...|0.649|  202193|0.538|0Oqc0kKFsQ6MhFOLB...|     0.00632|                7|  0.0951|-11.196|    0|0.0519|144.979|  4|https://api.spoti...|audio_features|spotify:track:0Oq...|         0.496|
|       0.372|https://api.spoti...|0.649|  202193|0.538|0Oqc0kKFsQ6MhFOLB...|     0.00632|                7|  0.0951|-11.196|    0|0.0519|144.979|  4|https://api.spoti...|audio_features|spotify:track:0Oq...|         0.496|
|       0.262|https://api.spoti...|0.324|  236053|0.416|2nMeu6UenVvwUktBC...|     3.69E-5|               11|    0.11|  -8.92|    0|0.0368|113.986|  4|https://api.spoti...|audio_features|spotify:track:2nM...|         0.151|
|       0.262|https://api.spoti...|0.324|  236053|0.416|2nMeu6UenVvwUktBC...|     3.69E-5|               11|    0.11|  -8.92|    0|0.0368|113.986|  4|https://api.spoti...|audio_features|spotify:track:2nM...|         0.151|
|       0.262|https://api.spoti...|0.324|  236053|0.416|2nMeu6UenVvwUktBC...|     3.69E-5|               11|    0.11|  -8.92|    0|0.0368|113.986|  4|https://api.spoti...|audio_features|spotify:track:2nM...|         0.151|
|       0.262|https://api.spoti...|0.324|  236053|0.416|2nMeu6UenVvwUktBC...|     3.69E-5|               11|    0.11|  -8.92|    0|0.0368|113.986|  4|https://api.spoti...|audio_features|spotify:track:2nM...|         0.151|
|       0.967|https://api.spoti...|0.218|  248934|0.215|3RIgHHpnFKj5Rni1s...|      0.0847|                5|  0.0948| -12.49|    1|0.0368|  76.74|  1|https://api.spoti...|audio_features|spotify:track:3RI...|         0.138|
|       0.967|https://api.spoti...|0.218|  248934|0.215|3RIgHHpnFKj5Rni1s...|      0.0847|                5|  0.0948| -12.49|    1|0.0368|  76.74|  1|https://api.spoti...|audio_features|spotify:track:3RI...|         0.138|
|       0.967|https://api.spoti...|0.218|  248934|0.215|3RIgHHpnFKj5Rni1s...|      0.0847|                5|  0.0948| -12.49|    1|0.0368|  76.74|  1|https://api.spoti...|audio_features|spotify:track:3RI...|         0.138|
|       0.967|https://api.spoti...|0.218|  248934|0.215|3RIgHHpnFKj5Rni1s...|      0.0847|                5|  0.0948| -12.49|    1|0.0368|  76.74|  1|https://api.spoti...|audio_features|spotify:track:3RI...|         0.138|
|       0.892|https://api.spoti...|0.532|  218288|0.452|7MtVPRGtZl6rPjMfL...|     0.00113|                9|   0.127|-10.654|    0| 0.043|129.895|  4|https://api.spoti...|audio_features|spotify:track:7Mt...|         0.203|
|       0.892|https://api.spoti...|0.532|  218288|0.452|7MtVPRGtZl6rPjMfL...|     0.00113|                9|   0.127|-10.654|    0| 0.043|129.895|  4|https://api.spoti...|audio_features|spotify:track:7Mt...|         0.203|
|       0.892|https://api.spoti...|0.532|  218288|0.452|7MtVPRGtZl6rPjMfL...|     0.00113|                9|   0.127|-10.654|    0| 0.043|129.895|  4|https://api.spoti...|audio_features|spotify:track:7Mt...|         0.203|
|       0.892|https://api.spoti...|0.532|  218288|0.452|7MtVPRGtZl6rPjMfL...|     0.00113|                9|   0.127|-10.654|    0| 0.043|129.895|  4|https://api.spoti...|audio_features|spotify:track:7Mt...|         0.203|
|       0.839|https://api.spoti...|0.292|  300683|0.334|2mdEsXPu8ZmkHRRtA...|       0.345|                6|   0.151|-10.679|    0|0.0378| 67.836|  4|https://api.spoti...|audio_features|spotify:track:2md...|         0.148|
|       0.839|https://api.spoti...|0.292|  300683|0.334|2mdEsXPu8ZmkHRRtA...|       0.345|                6|   0.151|-10.679|    0|0.0378| 67.836|  4|https://api.spoti...|audio_features|spotify:track:2md...|         0.148|
|       0.839|https://api.spoti...|0.292|  300683|0.334|2mdEsXPu8ZmkHRRtA...|       0.345|                6|   0.151|-10.679|    0|0.0378| 67.836|  4|https://api.spoti...|audio_features|spotify:track:2md...|         0.148|
|       0.839|https://api.spoti...|0.292|  300683|0.334|2mdEsXPu8ZmkHRRtA...|       0.345|                6|   0.151|-10.679|    0|0.0378| 67.836|  4|https://api.spoti...|audio_features|spotify:track:2md...|         0.148|
+------------+--------------------+-----+--------+-----+--------------------+------------+-----------------+--------+-------+-----+------+-------+---+--------------------+--------------+--------------------+--------------+
only showing top 20 rows
</pre>