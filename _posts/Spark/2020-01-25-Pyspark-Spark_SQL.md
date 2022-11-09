---
layout: post
title: '[PySpark] Spark SQL'
category: Spark
tags: [spark, pyspark, sql]
comments: true
---

# Spark SQL
- zeppelin에서는 %sql 밑에 쿼리를 쓰면 sql 문법으로 데이터를 불러올 수 있음

~~~sql
%sql
SELECT name, popularity, AVG(abs(popularity-track_popularity)) AS diff FROM master WHERE name IN ('Drake', 'Post Malone', 'Travis Scott') GROUP BY 1, 2 ORDER BY 3 ASC LIMIT 20
~~~

~~~sql
%sql
SELECT track_popularity, COUNT(*) FROM master GROUP BY 1 ORDER BY 1 ASC
~~~

~~~sql
%sql
SELECT AVG(acousticness), AVG(liveness), AVG(speechiness), AVG(tempo) FROM master WHERE popularity > ${pupularity=80} AND track_popularity > ${track_popularity=80}
~~~

~~~sql
%sql
SELECT ROUND(acousticness, 2), COUNT(*) FROM amster GROUP BY 1 ORDER BY 1 ASC
~~~