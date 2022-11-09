---
layout: post
title: '[Linux] Mac, Linux 간에 자소분리 현상'
category: Linux
tags: [linux, mac, NFC, NFD, 자소분리]
---

# NFC와 NFD 서로 변환하기
- OS/X는 NFD 형식으로 파일 이름을 저장한다. 이 파일을 다른 OS로 복사했을 때 해당 OS에서 NF이를 NFC로 바꾸기 위해서는 convmv을 사용하면 된다.

## 1. 설치
- OS/X 에서는 brew을 이용해서 설치하는 것이 편하다.

~~~shell
$ brew install convmv
~~~

- Debian/Linux 에서는 아래 명령으로 간단하게 설치할 수 있다.

~~~shell
$ sudo apt-get install convmv
~~~

## 2. 사용법
### 2.1 NFD를 NFC로
- 대상 파일 시스템이 utf-8를 쓴다고 가정하면

~~~shell
$ convmv -f utf-8 -t utf-8 --nfc -notest 파일명
~~~

### 2.2 NFC를 NFD로
- 원본 파일 시스템이 utf-8을 쓴다고 가정하면

~~~shell
$ convmv -f utf-8 -t utf-8 --nfd -notest 파일명
~~~