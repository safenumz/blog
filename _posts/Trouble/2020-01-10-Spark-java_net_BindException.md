---
layout: post
title: '[Trouble Pyspark] java.net.BindException 에러'
category: Trouble
tags: [trouble, pyspark]
comments: true
---

# Environment
- MacOS 10.14.6
- Python 3.7.3
- Spark 2.4.0

# Trouble

<pre>
java.net.BindException: Can't assign requested address: Service 'sparkDriver'
</pre>

# Shooting
- 환경설정(~/.bash_profile, ${SPARK_HOME}/conf/spark-env.sh)에서 SPARK_LOCAL_IP가 사용자의 hostname과 일치하는지 확인

## .bash_profile 수정

~~~sh
SPARK_LOCAL_IP=127.0.0.1
~~~

## /etc/hosts 수정

~~~sh
127.0.0.1   localhost
~~~