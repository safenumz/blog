---
layout: post
title: '[Trouble centos] CentOS 6에서 yum update가 되지 않을 때'
category: Trouble
tags: [centos]
comments: true
---

# Environment
- CentOS 6

# Trouble
- CentOS 6 yum update가 되지 않을 때
- yum install 및 yum update와 같이 패키지 설치나 업데이트시 Could not retrieve mirrorlist문제로 설치가 되지 않는 문제가 발생할 때

# Shooting
- CentOS 6 버전부터는 ~/etc/resolv.conf 파일에 servername을 추가해주는 것으로는 이 문제를 해결할 수 없다.
- /etc/sysconfig/network-scripts/ifcfg-ens33 파일을 열어 맨 밑에 있는 ONBOOT=no 를 yes로 바꿔줘야 한다.

~~~shell
$ vi /etc/sysconfig/network-scripts/ifcfg-ens33
~~~

~~~bash
# ifcfgens33 파일
ONBOOT=yes
~~~

~~~shell
$ dhclient

$ yum update
~~~
