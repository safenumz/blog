---
layout: post
title: '[PySpark] Select Subset Columns, Filter'
category: Spark
tags: [spark, pyspark, select, filter]
comments: true
---

# Select Subset Columns

~~~python
%pyspark
raw = sqlContext.read.format("parquet").load("s3://spotify-folder/audio-features/dt=2020-01-29/top-tracks.parquet")
df1 = raw.toDF("danceability", "energy", "key", "loudness", "mode", "speechiness", "acousticness", "instrumentalnesss", "liveness", "valence", "tempo", "type", "id", "uri", "track_href", "analysis_url", "duration_ms", "time_signature")
df1.show()
~~~

<pre>
+------------+--------------------+-----+--------+-----+--------------------+------------+-----------------+--------+-------+-----+------+-------+---+--------------------+--------------+--------------------+--------------+
|danceability|              energy|  key|loudness| mode|         speechiness|acousticness|instrumentalnesss|liveness|valence|tempo|  type|     id|uri|          track_href|  analysis_url|         duration_ms|time_signature|
+------------+--------------------+-----+--------+-----+--------------------+------------+-----------------+--------+-------+-----+------+-------+---+--------------------+--------------+--------------------+--------------+
|     0.00237|https://api.spoti...|0.644|  190067|0.755|6zegtH6XXd2PDPLvy...|     7.64E-6|                1|  0.0859| -5.325|    1|0.0448|102.861|  4|https://api.spoti...|audio_features|spotify:track:6ze...|         0.334|
|     0.00237|https://api.spoti...|0.644|  190067|0.755|6zegtH6XXd2PDPLvy...|     7.64E-6|                1|  0.0859| -5.325|    1|0.0448|102.861|  4|https://api.spoti...|audio_features|spotify:track:6ze...|         0.334|
|     0.00237|https://api.spoti...|0.644|  190067|0.755|6zegtH6XXd2PDPLvy...|     7.64E-6|                1|  0.0859| -5.325|    1|0.0448|102.861|  4|https://api.spoti...|audio_features|spotify:track:6ze...|         0.334|
|     0.00237|https://api.spoti...|0.644|  190067|0.755|6zegtH6XXd2PDPLvy...|     7.64E-6|                1|  0.0859| -5.325|    1|0.0448|102.861|  4|https://api.spoti...|audio_features|spotify:track:6ze...|         0.334|
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
+------------+--------------------+-----+--------+-----+--------------------+------------+-----------------+--------+-------+-----+------+-------+---+--------------------+--------------+--------------------+--------------+
only showing top 20 rows
</pre>



~~~python
%pyspark
df2 = df1.select(df1["danceability"], df1["id"], df1["acousticness"])
df2.show()
~~~

<pre>
+------------+-------+------------+
|danceability|     id|acousticness|
+------------+-------+------------+
|     0.00237|102.861|     7.64E-6|
|     0.00237|102.861|     7.64E-6|
|     0.00237|102.861|     7.64E-6|
|     0.00237|102.861|     7.64E-6|
|       0.372|144.979|     0.00632|
|       0.372|144.979|     0.00632|
|       0.372|144.979|     0.00632|
|       0.372|144.979|     0.00632|
|       0.262|113.986|     3.69E-5|
|       0.262|113.986|     3.69E-5|
|       0.262|113.986|     3.69E-5|
|       0.262|113.986|     3.69E-5|
|       0.967|  76.74|      0.0847|
|       0.967|  76.74|      0.0847|
|       0.967|  76.74|      0.0847|
|       0.967|  76.74|      0.0847|
|       0.892|129.895|     0.00113|
|       0.892|129.895|     0.00113|
|       0.892|129.895|     0.00113|
|       0.892|129.895|     0.00113|
+------------+-------+------------+
only showing top 20 rows
</pre>


# Filter

~~~python
df3 = df2.filter((df2["danceability"] >= 0.30) & (df2["acousticness"] >= 0.1)).distinct()
df3.show()
~~~

<pre>
+------------+-------+------------+
|danceability|     id|acousticness|
+------------+-------+------------+
|       0.924|204.049|       0.768|
|       0.961|152.929|       0.204|
|       0.839| 67.836|       0.345|
|       0.991| 92.249|       0.902|
|       0.981|116.882|       0.688|
|       0.984|119.471|       0.896|
|       0.944|129.272|       0.922|
|       0.564| 98.445|       0.299|
|       0.938|120.781|       0.916|
|       0.991| 89.408|       0.948|
|       0.991|119.381|       0.912|
|       0.932| 155.94|       0.826|
|       0.875|107.601|       0.432|
|       0.955|104.465|       0.706|
|       0.978|   83.4|       0.823|
|       0.984| 62.287|       0.749|
|       0.937|111.705|       0.898|
|       0.986| 66.283|         0.6|
|       0.957|107.971|       0.809|
|       0.987| 173.61|       0.927|
+------------+-------+------------+
only showing top 20 rows
</pre>