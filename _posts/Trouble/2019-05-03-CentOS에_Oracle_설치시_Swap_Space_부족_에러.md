---
post: layout
title: '[Trouble centos] Oracle 설치시 Swap Space 부족 에러'
category: Trouble
tags: [centos, oracle, swap space]
comments: true
---

# Environment
- CentOS 7
- Oracle 11g EE

# Trouble
- CentOS에 Oracle 설치시 Swap Space 부족 에러가 발생하는 경우

<pre>
This system does not meet the minimum requirements for swap space.  Based on
the amount of physical memory available on the system, Oracle Database 11g
Express Edition requires 2048 MB of swap space. This system has 199 MB
of swap space.  Configure more swap space on the system and retry the
installation.
</pre>

# Shooting
- Swap Space를 늘려 준다.

~~~shell
$ mkdir /swap

$ dd if=/dev/zero of=/swap/swapfile bs=1024 count=2097152

$ cd /swap

$ mkswap swapfile
~~~