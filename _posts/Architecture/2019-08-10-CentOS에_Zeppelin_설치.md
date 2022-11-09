---
layout: post
title: '[Zeppelin] CentOS에 Zeppelin 설치 및 기본설정'
category: Architecture
tags: [zeppelin]
comments: true
---

# Environment
- Centos 7
- Zeppelin 0.8.1
- Spark 2.4.0
- Python 3.6

# zeppelin 다운로드

~~~sh
# zepplein 다운로드
$ wget http://mirror.navercorp.com/apache/zeppelin/zeppelin-0.8.1/zeppelin-0.8.1-bin-all.tgz

# zeppelin 압축풀기
$ tar xvzf zeppelin-0.8.1-bin-all.tgz

# 심볼릭 링크
$ ln -s zeppelin-0.8.1-bin-all zeppelin
~~~

# zeppelin 설정

## 설정 파일 생성

~~~sh
$ cp zeppelin-site.xml.template zeppelin-site.xml

$ cp zeppelin-env.sh.template zeppelin-env.sh

$ cp shiro.ini.template shiro.ini
~~~

## zeppelin-site.xml
- ip 주소, port 번호, output 제한, cron 기능 설정

~~~html
<!-- zeppelin-site.xml -->

<!-- ip 주소 설정 -->
<property>
  <name>zeppelin.server.addr</name>
  <value>192.168.0.10</value>
  <description>Server address</description>
</property>

<!-- port 설정 -->
<property>
  <name>zeppelin.server.port</name>
  <value>38080</value>
  <description>Server port.</description>
</property>

<!-- output 제한 -->
<property>
  <name>zeppelin.interpreter.output.limit</name>
  <value>10000000</value>
  <description>Output message from interpreter exceeding the limit will be truncated</description>
</property>

<property>
  <name>zeppelin.websocket.max.text.message.size</name>
  <value>1000000000</value>
  <description>Size in characters of the maximum text message to be received by websocket. Defaults to 1024000</description>
</property>
<property>
  <name>zeppelin.ipython.grpc.message_size</name>
  <value>500000000</value>
</property>

<!-- cron 기능 설정 -->
<property>
  <name>zeppelin.notebook.cron.enable</name>
  <value>true</value>
  <description>Notebook enable cron scheduler feature</description>
</property>
<property>
  <name>zeppelin.notebook.cron.folders</name>
  <value>/home/a/zeppelin</value>
  <description>Notebook cron folders</description>
</property>
~~~

## zeppelin-env.sh
- spark 설정, 메모리 설정

~~~sh
export BASE_HOME=/home/a/
export ZAVA_HOME=/usr/local/java
export MASTER=yarn-client
export ZEPPELIN_MEM="-Xms3024m -Xmx6024m -XX:MaxPermSize=512m"
export ZEPPELIN_INTP_MEM="-Xms3024m -Xmx6024m -XX:MaxPermSize=512m"
export ZEPPELIN_INTERPRETER_OUTPUT_LIMIT=10000000
export SPARK_HOME=$BASE_HOME/spark
export HADOOP_CONF_DIR=$BASE_HOME/hadoop/etc/hadoop
export PYSPARK_PYTHON=$BASE_HOME/anaconda3/bin/python3
export PYTHONPATH=$BASE_HOME/anaconda3/bin/python3
export ZEPPELIN_PORT=38080
~~~

## shiro.ini
- 아이디, 비밀번호 설정

~~~sh
[users]
# List of users with their password allowed to access Zeppelin.
# To use a different strategy (LDAP / Database / ...) check the shiro doc at http://shiro.apache.org/configuration.html#Configuration-INISections
# To enable admin user, uncomment the following line and set an appropriate password.
admin = admin, admin
user1 = role1, role2
user2 = password3, role3
user3 = password4, role2
~~~

# Zeppelin Intepreter 설치

~~~sh
# 접근 권한 설정 install-interpreter.log
$ sudo chmod 777 /home/a/zeppelin/logs/install-interpreter.log

# zepplein interpreter 설치
$ sudo ./bin/install-interpreter.sh --name md,shell,jdbc,python,pig,angular,elasticsearch
~~~