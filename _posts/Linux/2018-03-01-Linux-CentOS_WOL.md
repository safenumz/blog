---
layout: post
title: '[Linux] CentOS7 WOL 설정'
category: Linux
tags: [linux, centos, wol]
comments: true
---

# Environment
- OS: CentOS 7
- MainBoard: biostar


# PME 부팅: 컴퓨터를 통하여 부팅하는 방법(원격부팅)

## biostart bios 설정

```sh
# WOL ON
Advanced > PME Wake up from S5 > Enabled
# 전력 관련
Chipset > Eup Control > Disabled
# Advanced > Restore AC Power Loss > Last State
```

## ethtool 설치

```sh
$ yum install -y ethtool
```

## 인터페이스명 확인
- 여기선 enp5s0, 인터페이스명은 사용자마다 다름

```sh
$ ifconfig | grep -e enp -e eth
```
enp5s0


## WOL ON

```sh
$ ethtool -s enp5s0 wol g
```

## WOL 설정 확인
- **Wake-on: d**가 아닌 **Wake-on: g**로 나와야 함

```sh
$ ethtool enp5s0 | grep Wake-on
```
Wake-on: g

## /etc/rc.local에 등록
- 재부팅해도 적용하기 위해 /etc/rc.local에 등록

```sh
$ vi /etc/rc.local
```

```sh
# /etc/rc.local
/sbin/ethtool -s enp5s0 wol g
```

- 아래 명령어를 반드시 해줘야 함, 아니면 WOL 작동 안함

```sh
$ chmod +x /etc/rc.d/rc.local
```

## Trouble shooting

```sh
$ nmcli con show "enp5s0" | grep wake
```
802-3-ethernet.wake-on-lan: magic

- default가 아니라 magic으로 되어 있어야 함
- 만약 default로 되어 있다면, 아래 명령어로 수정

```sh
$ nmcli con modify "enp5s0" 802-3-ethernet.wake-on-lan magic
```

```sh
nmcli con show "enp5s0" | grep negotidate

```
802-3-ethernet.auto-negotiate: yes

- no가 아니라 yes로 되어 있어야 함
- 만약 no로 되어 있다면, 아래 명령어로 수정

```sh
$ nmcli con modify "enp5s0" 802-3-ethernet.auto-negotiate yes
```