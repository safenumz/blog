---
layout: post
title: '[Trouble elasticsearch] vm.max_map_count 에러'
category: Trouble
tags: [trouble, elasticserarch, vm.max_map_count]
comments: true
---

# Enviroment
- CentOS 7
- Elasticsearch 6.5.4

# Trouble
- Elasticsearch 실행시 vm.max_map_count is too low 및 max file descriptors for elasticsearch process is too low 에러나는 경우

<pre>
[1]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]  
[2]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
</pre>

# Shooting
- file descriptor, memory 부족으로 인한 에러이다. max file descriptors와 vc.max_map_count 설정을 변경 해준다.


## max file descriptors 변경

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

## vc.max_map_count 설정 변경

~~~sh
$ sudo vi /etc/sysctl.conf
~~~

~~~sh
# sysctl.conf
vm.max_map_count=262144
~~~

로그아웃 후 재접속하여 Elasticsearch를 실행하면 정상적으로 작동한다.
