---
layout: post
title: '[MacOS] Linux ISO 파일을 USB로 굽기'
category: etc
tags: [linux, ubuntu, centos]
comments: true
---

# Environment
- MacOS

# Mac에서 Linux ISO 파일을 USB로 굽기
CentOS로 테스트했지만 Ubuntu도 동일한 방법으로 가능하다.

## centos7.iso 파일 다운
먼저 [https://www.centos.org/](https://www.centos.org/) CentOS 홈페이지에서 centos7.iso 파일을 다운받는다.

## 터미널을 열고 이미지 파일이 있는 경로 로 이동하여 iso 파일을 img 파일로 변환

~~~shell
# /Home/<userName>/Desktop/ 경로에서
$ hdiutil convert -format UDRW -o centos7.img ~/Desktop/centos7.iso
~~~

## centos7.img가 아닌 centos7.img.dmg이 생성됨, dmg 확장자 제거

~~~shell
$ mv centos7.img.dmg centos.img
~~~

## usb 메모리를 삽입하고 포맷
- LaunchPad > 기타 > 디스크 유틸리티에서 USB 메모리 선택 후 지우기

## USB 메모리가 어떤 위치에 마운트 되어 있는지 확인

~~~shell
$ diskutil list
~~~

## 이미지를 USB에 덮어쓸 때 USB가 시스템에 마운트 되어 있으면 작업할 수 없음으로, unmount 명령어를 이용하여 USB를 추출

~~~shell
# 여기서는 /dev/disk2에 마운트되어 있음
$ diskutil unmountDisk /dev/disk2
~~~

## USB 메모리에 이미지를 덮어씀

~~~shell
sudo dd if=centos7.mg of=/dev/disk2 bs=1m
~~~
