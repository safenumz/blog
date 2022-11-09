---
layout: post
title: '[PySpark] Join'
category: Spark
tags: [spark, pyspark, join]
comments: true
---

# Join

~~~python
artists = sqlContext.read.format("parquet").load("s3://spotify-folder/artists/dt=2020-01-30/artists.parquet")
artists.toDF("id", "name", "followers", "popularity", "url", "image_url")
artists.show()
~~~

<pre>
+---------+--------------------+--------------------+--------------------+----------+--------------------+
|followers|                  id|           image_url|                name|popularity|                 url|
+---------+--------------------+--------------------+--------------------+----------+--------------------+
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|        Lana Del Rey|        89|https://open.spot...|
|   294843|01C9OoXDvCKkGcf73...|https://i.scdn.co...|    Serge Gainsbourg|        63|https://open.spot...|
|  1053365|02rd0anEWfMtF7iMk...|https://i.scdn.co...|       Reba McEntire|        66|https://open.spot...|
|   367631|02uYdhMhCgdB49hZl...|https://i.scdn.co...|Eagles Of Death M...|        61|https://open.spot...|
|  1498582|03r4iKL2g2442PT9n...|https://i.scdn.co...|        Beastie Boys|        73|https://open.spot...|
|   131513|03YhcM6fxypfwckPC...|https://i.scdn.co...|      Wes Montgomery|        57|https://open.spot...|
| 21913582|04gDigrS5kc9YWfZH...|https://i.scdn.co...|            Maroon 5|        93|https://open.spot...|
|    75688|04tBaW21jyUfeP5iq...|https://i.scdn.co...|        Scott Walker|        52|https://open.spot...|
|   581793|0543y7yrvny4Kymoa...|https://i.scdn.co...|      Peter Frampton|        62|https://open.spot...|
|    85779|05E3NBxNMdnrPtxF9...|https://i.scdn.co...|        Lester Young|        53|https://open.spot...|
| 26220658|06HL4z0CvFAxyc27G...|https://i.scdn.co...|        Taylor Swift|        94|https://open.spot...|
|   356508|06nevPmNVfWUXyZkc...|https://i.scdn.co...|      Gregory Porter|        68|https://open.spot...|
|   366125|06nsZ3qSOYZ2hPVIM...|https://i.scdn.co...|           J.J. Cale|        66|https://open.spot...|
|  2834825|085pc2PYOi8bGKj0P...|https://i.scdn.co...|           will.i.am|        78|https://open.spot...|
|   158043|08avsqaGIlK2x3i2C...|https://i.scdn.co...|      Keith Richards|        49|https://open.spot...|
|   265703|09C0xjtosNAIXP36w...|https://i.scdn.co...|         Fats Domino|        61|https://open.spot...|
|  4928415|0BvkDsjIUla7X0k6C...|https://i.scdn.co...|          Luke Bryan|        80|https://open.spot...|
|    24213|0bvRYuXRvd14RYEE7...|https://i.scdn.co...|Linton Kwesi Johnson|        44|https://open.spot...|
|  5118810|0C0XlULifJtAgn6ZN...|https://i.scdn.co...|         The Killers|        82|https://open.spot...|
|  2356170|0cc6vw3VN8YlIcvr1...|https://i.scdn.co...|         Mötley Crüe|        77|https://open.spot...|
+---------+--------------------+--------------------+--------------------+----------+--------------------+
only showing top 20 rows
</pre>


~~~python
import pandas as pd
import pymysql

host = <endpoint>
port = 3306
username = <id>
database = <database>
password = <password>

try:
    conn = pymysql.connect(host, user=username, passwd=password, db=database, port=port, use_unicode=True, charset='utf8')
    cursor = conn.cursor()
except:
    logging.error("could not connect to rds")
    sys.exit(1)
    
cursor.execute("SELECT * FROM artists")
colnames = [d[0] for d in cursor.description]
artists = [dict(zip(colnames, row)) for row in cursor.fetchall()]
artists = pd.DataFrame(artists)
artists.head()

artists = sqlContext.createDataFrame(artists)
artists.show()
~~~

<pre>
+---------+--------------------+--------------------+--------------------+----------+--------------------+
|followers|                  id|           image_url|                name|popularity|                 url|
+---------+--------------------+--------------------+--------------------+----------+--------------------+
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|        Lana Del Rey|        89|https://open.spot...|
|   294843|01C9OoXDvCKkGcf73...|https://i.scdn.co...|    Serge Gainsbourg|        63|https://open.spot...|
|  1053365|02rd0anEWfMtF7iMk...|https://i.scdn.co...|       Reba McEntire|        66|https://open.spot...|
|   367631|02uYdhMhCgdB49hZl...|https://i.scdn.co...|Eagles Of Death M...|        61|https://open.spot...|
|  1498582|03r4iKL2g2442PT9n...|https://i.scdn.co...|        Beastie Boys|        73|https://open.spot...|
|   131513|03YhcM6fxypfwckPC...|https://i.scdn.co...|      Wes Montgomery|        57|https://open.spot...|
| 21913582|04gDigrS5kc9YWfZH...|https://i.scdn.co...|            Maroon 5|        93|https://open.spot...|
|    75688|04tBaW21jyUfeP5iq...|https://i.scdn.co...|        Scott Walker|        52|https://open.spot...|
|   581793|0543y7yrvny4Kymoa...|https://i.scdn.co...|      Peter Frampton|        62|https://open.spot...|
|    85779|05E3NBxNMdnrPtxF9...|https://i.scdn.co...|        Lester Young|        53|https://open.spot...|
| 26220658|06HL4z0CvFAxyc27G...|https://i.scdn.co...|        Taylor Swift|        94|https://open.spot...|
|   356508|06nevPmNVfWUXyZkc...|https://i.scdn.co...|      Gregory Porter|        68|https://open.spot...|
|   366125|06nsZ3qSOYZ2hPVIM...|https://i.scdn.co...|           J.J. Cale|        66|https://open.spot...|
|  2834825|085pc2PYOi8bGKj0P...|https://i.scdn.co...|           will.i.am|        78|https://open.spot...|
|   158043|08avsqaGIlK2x3i2C...|https://i.scdn.co...|      Keith Richards|        49|https://open.spot...|
|   265703|09C0xjtosNAIXP36w...|https://i.scdn.co...|         Fats Domino|        61|https://open.spot...|
|  4928415|0BvkDsjIUla7X0k6C...|https://i.scdn.co...|          Luke Bryan|        80|https://open.spot...|
|    24213|0bvRYuXRvd14RYEE7...|https://i.scdn.co...|Linton Kwesi Johnson|        44|https://open.spot...|
|  5118810|0C0XlULifJtAgn6ZN...|https://i.scdn.co...|         The Killers|        82|https://open.spot...|
|  2356170|0cc6vw3VN8YlIcvr1...|https://i.scdn.co...|         Mötley Crüe|        77|https://open.spot...|
+---------+--------------------+--------------------+--------------------+----------+--------------------+
only showing top 20 rows
</pre>



~~~python
top_tracks = sqlContext.read.format("parquet").load("s3://spotify-folder/top-tracks/dt=2020-01-29/top-tracks.parquet")
top_tracks = top_tracks.toDF("id", "artist_id", "name", "popularity", "external_url")
top_tracks = top_tracks.withColumnRenamed("id", "track_id").withColumnRenamed("name", "track_name")
top_tracks = top_tracks.select(top_tracks["track_id"].alias("track_id"), top_tracks["track_name"][0].alias("track_name"), top_tracks["artist_id"], top_tracks["popularity"][0].alias("track_popularity"))

joined = artists.join(top_tracks, top_tracks["track_id"] == artists["id"])
joined.show()

features = sqlContext.read.format("parquet").load("s3://spotify-folder/audio-features/dt=2020-01-29/top-tracks.parquet")
features = features.toDF("acousticness", "analysis_url", "danceability", "duration_ms", "energy", "id", "instrumentalness", "key", "liveness", "loudness", "mode", "speechiness", "tempo", "time_signature", "track_href", "type", "uri", "valence")
features = features.withColumnRenamed("id", "track_id")

master = joined.join(features, joined["track_id"] == features["track_id"], "leftouter")
master.show()
~~~

<pre>
+---------+--------------------+--------------------+------------+----------+--------------------+--------------------+--------------------+--------------------+--------------------+
|followers|                  id|           image_url|        name|popularity|                 url|            track_id|          track_name|           artist_id|    track_popularity|
+---------+--------------------+--------------------+------------+----------+--------------------+--------------------+--------------------+--------------------+--------------------+
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|3hwQhakFwm9soLEBn...|[https://open.spo...|        Venice Bitch|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|3hwQhakFwm9soLEBn...|[https://open.spo...|        Venice Bitch|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|3hwQhakFwm9soLEBn...|[https://open.spo...|        Venice Bitch|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|3hwQhakFwm9soLEBn...|[https://open.spo...|        Venice Bitch|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|6PUIzlqotEmPuBfjb...|[https://open.spo...|Summertime Sadnes...|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|6PUIzlqotEmPuBfjb...|[https://open.spo...|Summertime Sadnes...|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|6PUIzlqotEmPuBfjb...|[https://open.spo...|Summertime Sadnes...|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|6PUIzlqotEmPuBfjb...|[https://open.spo...|Summertime Sadnes...|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|0fB77VOZ2FkQeKLv1...|[https://open.spo...|hope is a dangero...|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|0fB77VOZ2FkQeKLv1...|[https://open.spo...|hope is a dangero...|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|0fB77VOZ2FkQeKLv1...|[https://open.spo...|hope is a dangero...|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|0fB77VOZ2FkQeKLv1...|[https://open.spo...|hope is a dangero...|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|6OG05bPAwUuV3OMvy...|[https://open.spo...|Mariners Apartmen...|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|6OG05bPAwUuV3OMvy...|[https://open.spo...|Mariners Apartmen...|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|6OG05bPAwUuV3OMvy...|[https://open.spo...|Mariners Apartmen...|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|6OG05bPAwUuV3OMvy...|[https://open.spo...|Mariners Apartmen...|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|2mdEsXPu8ZmkHRRtA...|[https://open.spo...|       Cinnamon Girl|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|2mdEsXPu8ZmkHRRtA...|[https://open.spo...|       Cinnamon Girl|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|2mdEsXPu8ZmkHRRtA...|[https://open.spo...|       Cinnamon Girl|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|2mdEsXPu8ZmkHRRtA...|[https://open.spo...|       Cinnamon Girl|
+---------+--------------------+--------------------+------------+----------+--------------------+--------------------+--------------------+--------------------+--------------------+
only showing top 20 rows

+---------+--------------------+--------------------+------------+----------+--------------------+--------------------+--------------------+--------------------+--------------------+------------+------------+------------+-----------+------+--------+----------------+----+--------+--------+----+-----------+-----+--------------+----------+----+----+-------+
|followers|                  id|           image_url|        name|popularity|                 url|            track_id|          track_name|           artist_id|    track_popularity|acousticness|analysis_url|danceability|duration_ms|energy|track_id|instrumentalness| key|liveness|loudness|mode|speechiness|tempo|time_signature|track_href|type| uri|valence|
+---------+--------------------+--------------------+------------+----------+--------------------+--------------------+--------------------+--------------------+--------------------+------------+------------+------------+-----------+------+--------+----------------+----+--------+--------+----+-----------+-----+--------------+----------+----+----+-------+
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|3hwQhakFwm9soLEBn...|[https://open.spo...|        Venice Bitch|        null|        null|        null|       null|  null|    null|            null|null|    null|    null|null|       null| null|          null|      null|null|null|   null|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|3hwQhakFwm9soLEBn...|[https://open.spo...|        Venice Bitch|        null|        null|        null|       null|  null|    null|            null|null|    null|    null|null|       null| null|          null|      null|null|null|   null|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|3hwQhakFwm9soLEBn...|[https://open.spo...|        Venice Bitch|        null|        null|        null|       null|  null|    null|            null|null|    null|    null|null|       null| null|          null|      null|null|null|   null|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|3hwQhakFwm9soLEBn...|[https://open.spo...|        Venice Bitch|        null|        null|        null|       null|  null|    null|            null|null|    null|    null|null|       null| null|          null|      null|null|null|   null|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|6PUIzlqotEmPuBfjb...|[https://open.spo...|Summertime Sadnes...|        null|        null|        null|       null|  null|    null|            null|null|    null|    null|null|       null| null|          null|      null|null|null|   null|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|6PUIzlqotEmPuBfjb...|[https://open.spo...|Summertime Sadnes...|        null|        null|        null|       null|  null|    null|            null|null|    null|    null|null|       null| null|          null|      null|null|null|   null|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|6PUIzlqotEmPuBfjb...|[https://open.spo...|Summertime Sadnes...|        null|        null|        null|       null|  null|    null|            null|null|    null|    null|null|       null| null|          null|      null|null|null|   null|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|6PUIzlqotEmPuBfjb...|[https://open.spo...|Summertime Sadnes...|        null|        null|        null|       null|  null|    null|            null|null|    null|    null|null|       null| null|          null|      null|null|null|   null|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|0fB77VOZ2FkQeKLv1...|[https://open.spo...|hope is a dangero...|        null|        null|        null|       null|  null|    null|            null|null|    null|    null|null|       null| null|          null|      null|null|null|   null|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|0fB77VOZ2FkQeKLv1...|[https://open.spo...|hope is a dangero...|        null|        null|        null|       null|  null|    null|            null|null|    null|    null|null|       null| null|          null|      null|null|null|   null|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|0fB77VOZ2FkQeKLv1...|[https://open.spo...|hope is a dangero...|        null|        null|        null|       null|  null|    null|            null|null|    null|    null|null|       null| null|          null|      null|null|null|   null|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|0fB77VOZ2FkQeKLv1...|[https://open.spo...|hope is a dangero...|        null|        null|        null|       null|  null|    null|            null|null|    null|    null|null|       null| null|          null|      null|null|null|   null|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|6OG05bPAwUuV3OMvy...|[https://open.spo...|Mariners Apartmen...|        null|        null|        null|       null|  null|    null|            null|null|    null|    null|null|       null| null|          null|      null|null|null|   null|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|6OG05bPAwUuV3OMvy...|[https://open.spo...|Mariners Apartmen...|        null|        null|        null|       null|  null|    null|            null|null|    null|    null|null|       null| null|          null|      null|null|null|   null|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|6OG05bPAwUuV3OMvy...|[https://open.spo...|Mariners Apartmen...|        null|        null|        null|       null|  null|    null|            null|null|    null|    null|null|       null| null|          null|      null|null|null|   null|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|6OG05bPAwUuV3OMvy...|[https://open.spo...|Mariners Apartmen...|        null|        null|        null|       null|  null|    null|            null|null|    null|    null|null|       null| null|          null|      null|null|null|   null|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|2mdEsXPu8ZmkHRRtA...|[https://open.spo...|       Cinnamon Girl|        null|        null|        null|       null|  null|    null|            null|null|    null|    null|null|       null| null|          null|      null|null|null|   null|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|2mdEsXPu8ZmkHRRtA...|[https://open.spo...|       Cinnamon Girl|        null|        null|        null|       null|  null|    null|            null|null|    null|    null|null|       null| null|          null|      null|null|null|   null|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|2mdEsXPu8ZmkHRRtA...|[https://open.spo...|       Cinnamon Girl|        null|        null|        null|       null|  null|    null|            null|null|    null|    null|null|       null| null|          null|      null|null|null|   null|
|  9824434|00FQb4jTyendYWaN8...|https://i.scdn.co...|Lana Del Rey|        89|https://open.spot...|00FQb4jTyendYWaN8...|2mdEsXPu8ZmkHRRtA...|[https://open.spo...|       Cinnamon Girl|        null|        null|        null|       null|  null|    null|            null|null|    null|    null|null|       null| null|          null|      null|null|null|   null|
+---------+--------------------+--------------------+------------+----------+--------------------+--------------------+--------------------+--------------------+--------------------+------------+------------+------------+-----------+------+--------+----------------+----+--------+--------+----+-----------+-----+--------------+----------+----+----+-------+
only showing top 20 rows
</pre>