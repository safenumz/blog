---
layout: post
title: '[nvidia-smi] CentOS에서 nvidia-smi 설치'
category: Architecture
tags: [centos, nvidia-smi]
comments: true
---

# Environment
- CentOS 7
- Nvidia-smi 390.59

# 관련 라이브러리 설치

~~~shell
$ yum update
$ yum install kernel-devel kernel-headers gcc make
~~~

~~~shell
$ yum -y install kernel-devel-$(uname -r) kernel-header-$(uname -r) gcc make
~~~

## nouveau 을 활성화시키면 충돌가능성이 있기에 blacklist해준다.

~~~shell
$ vi /etc/modprobe.d/blacklist-nouveau.conf

# add to the end (create new if it does not exist)
blacklist nouveau
options nouveau modeset=0

$ dracut --force
~~~

~~~shell
$ echo 'blacklist nouveau' >> /etc/modprobe.d/blacklist.conf
$ dracut /boot/initramfs-$(uname -r).img $(uname -r) --force
$ reboot
~~~

# CentOS Nvidia-smi 설치
## NVIDIA-Linux-x86_64-390.59.run 다운로드

~~~shell
$  wget http://kr.download.nvidia.com/XFree86/Linux-x86_64/390.59/NVIDIA-Linux-x86_64-390.59.run
~~~

## 설치

~~~shell
$ bash NVIDIA-Linux-x86_64-390.59.run

$ nvidia-smi
~~~
