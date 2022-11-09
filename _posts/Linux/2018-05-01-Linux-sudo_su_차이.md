---
layout: post
title: '[Linux] su vs sudo'
category: Linux
tags: [linux, su, sudo]
comments: true
---

# su와 sudo의 차이 및 root 계정 암호 설정

## su
- Super User의 약자, 최상위 권한을 갖은 유저를 의미, 리눅스 운영체제에서는 최고 관리 권한을 갖는 계정은 root 계정이므로, 터미널에서 su 명령어를 입력하면 해당 터미널 세션에 한해서 일시적으로 root 계정처럼 사용할 수 있는 권한을 부여함

## sudo
- Super User do의 약자, 세션에 대한 권한이 아닌 하나의 명령에 대해 일시적으로 최고 관리 권한을 가짐, sudo의 경우에는 리눅스 기본 명령어가 아니기 때문에 sudo 명령어가 지원하는 지 확인해야 함, 우분투와 센토스의 경우에는 sudo를 지원함.

## root 계정 암호 설정하기

~~~bash
$ sudo passwd root
현재 로그인 된 계정의 암호를 입력하면 root 계정의 암호를 설정할 수 있다.

$ su
~~~
