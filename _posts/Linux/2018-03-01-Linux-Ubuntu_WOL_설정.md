---
layout: post
title: '[Linux] Linux WOL 설정'
category: Linux
tags: [linux, ubuntu, centos, macos, wol]
comments: true
---

# Ubuntu WOL 설정

- [https://www.cyberciti.biz/tips/linux-send-wake-on-lan-wol-magic-packets.html](https://www.cyberciti.biz/tips/linux-send-wake-on-lan-wol-magic-packets.html)

## Ubuntu wakeonlan 설치

```sh
$ sudo dpkg --configure -<사용자명>

# $ sudo aptitude install etherwake
# $ sudo apt install etherwake
$ sudo apt-get install net-tools ethtool wakeonlan
```

## CentOS 7 net-tools 설치

```sh
$ sudo yum install net-tools
```

## MacOS wakeonlan 설치

```sh
$ brew install wakeonlan
```

## WOL 설정

```sh
$ ifconfig

# 인터페이스명 확인
$ sudo ethtool -s <인터페이스명> wol g

$ sudo ethtool <인터페이스명: enp3s0>

$ sudo vi /etc/network/interfaces
```

<pre>
# 아래 내용을 넣어준다.
post-up /sbin/ethtool -s <인터페이스명> wol g
post-down /sbin/ethtool -s <인터페이스명> wol g
</pre>

<pre>
# 부팅시 F2키 연타하여 바이오스 진입
Advanced > Advanced/APM Configuration

Power On By PCI-E abled 설정
</pre>

~~~shell
$ sudo poweroff

$ sudo reboot

# 컴퓨터 종료
$ sudo shutdown -h now

# 컴퓨터 재부팅
$ sudo shutdown -r now
~~~

# Magic Packets

```sh
# Mac, Ubuntu
# wakeonlan xx:yy:zz:11:22:33
# wakeonlan -i 192.168.1.255 -p 4242 mac
$ wakeonlan <MAC-Address>

# CentOS 7
$ ether-wake <MAC-Address>
```

- 설정파일을 불러와서 매직패킷을 날릴 수도 있음

```sh
$ wakeonlan -f homelab.wol <MAC-Address>
```

> homelab.wol

```sh
# homelab.wol
# File structure
# --------------
# - blank lines are ignored
# - comment lines are ignored (lines starting with a hash mark '#')
# - other lines are considered valid records and can have 3 columns:
#
#       Hardware address, IP address, destination port
#
#   the last two are optional, in which case the following defaults
#   are used:
#
#       IP address: 255.255.255.255 (the limited broadcast address)
#       port:       9 (the discard port)
#
 
00:16:3e:a3:9d:a8	192.168.1.255		9
00:16:3e:08:ed:c6	255.255.255.255
f0:1f:af:1f:2c:60
```