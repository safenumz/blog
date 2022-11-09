---
layout: post
title: '[ELK Stack] CentOS에 ELK Stack 설치'
category: Architecture
tags: [logstash, elasticsearch, kibana, elk stack]
comments: true
---

# Environment
- CentOS 7
- Java 8
- Logstash 6.5.4
- Elasticsearch 6.5.4
- Kibana 6.5.4


# Logstash
## Logstash 다운로드

~~~sh
# logstash 6.5.4
$ wget https://artifacts.elastic.co/downloads/logstash/logstash-6.5.4.tar.gz

# 압축 해제
$ tar xvzf logstash-6.5.4.tar.gz

# 심볼릭 링크
$ ln -s logstash-6.5.4 logstash
~~~

# Elasticsearch
## Elasticsearch 다운로드

~~~sh
# Elasticsearch 다운로드
$ wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.5.4.tar.gz

# 압축 해제
$ tar xvzf elasticsearch-6.5.4.tar.gz

# 심볼릭 링크
$ ln -s elasticsearch-6.5.4 elasticsearch
~~~

## Elasticsearch 설정

~~~sh
$ vi elasticsearch/config/elasticsearch.yml
~~~

~~~sh
# elasticsearch.yml

# Set the bind address to a specific IP (IPv4 or IPv6):
network.host: <내부 Ip 주소>

# Set a custom port for HTTP: 
# Default: 9200
http.port: <Port 번호>
~~~

## Elasticsearch 실행

~~~sh
$ elasticsearch/bin/elasticsearch
~~~


## Elasticsearch 실행시 vm.max_map_count 에러나는 경우
- file descriptor, memory 부족으로 인한 에러이다.

> [1]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]  
[2]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]

### max file descriptors 변경

~~~sh
$ sudo vi /etc/security/limits.conf
~~~

~~~sh
# limits.conf

# <domain> <type> <item> <value>
a    hard    nofile    65536
a    soft    nofile    65536
a    hard    nproc     65536
a    soft    nproc     65536
~~~

### vc.max_map_count 설정 변경

~~~sh
$ sudo vi /etc/sysctl.conf
~~~

~~~sh
# sysctl.conf
vm.max_map_count=262144
~~~



# Kibana
## Kibana 다운로드

~~~sh
# Kibana 다운로드
$ wget https://artifacts.elastic.co/downloads/kibana/kibana-6.5.4-linux-x86_64.tar.gz

# 압축 해제
$ tar xvzf kibana-6.5.4-linux-x86_64.tar.gz

# 심볼릭 링크
$ ln -s kibana-6.5.4-linux-x86_64 kibana
~~~

## Kibana 설정
~~~sh
$ vi kibana/config/kibana.yml
~~~

~~~sh
# kibana.yml

# Kibana is served by a back end server. This setting specifies the port to use.
# server.port: 45601
server.port: <Kibana Port>

# To allow connections from remote users, set this parameter to a non-loopback address.
# server.host: hd2.cluster.kr
server.host: <Kibana Ip Address>

# The URL of the Elasticsearch instance to use for all your queries.
# elasticsearch.url: "http://hd2.cluster.kr:9200"
elasticsearch.url: <Elasticsearch url>
~~~

## Kibana 실행

~~~sh
$ kibana/bin/kibana
~~~