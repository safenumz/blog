---
layout: post
title: '[Linux] Ubuntu 한영전환 설정'
category: Linux
tags: [linux, ubuntu, 한영전환]
comments: true
---

# 우분투 한영전환 설정
우분투를 영문 버전으로 설치했으면 한영전환 키가 작동하지 않는다. 입력기 uim 내부에 있는 벼루 패키지를 설치함으로써 해결가능하다.

### uim 패키지 설치

~~~shell
# 설치 명령어
$ sudo apt install uim
~~~

위 설치 명령어를 실행하는 도중에 에러가 뜬 경우에는 삭제 후 다시 설치하면 된다.

~~~shell
# 삭제 명령어
$ sudo apt remove uim
$ sudo apt autoremove
~~~


우분투 설정에서 '지역 및 언어'를 선택하고 오른쪽 '입력 소스'에서 영어(미국)만 남기고 모두 제거한다.

다음으로 '언어 지원' 메뉴를 실행하고 키보드 입력기를 'uim'으로 바꿔주고 리부팅한다.

### 기본 입력기 설정

입력기 어플을 실행하고,

- 전체적인 설정 : 사용되는 입력기에 '벼루' 하나만 남기고 전부 삭제한다.

- 툴바 : display를 Never로 바꿔준다.

- 전체적인 키 설정 1 : [전체] 켜기, [전체] 끄기 부분을 전부 삭제한다.

- 벼루 키 설정 1 : [벼루] 한글모드로, [벼루] 영문모드로 부분을 "hangul"로 변경해주고, [벼루] 한자 및 기호 확정 부분을 "hangul-hanja"로 변경한다.


### 추가적인 설정
- 위 과정을 진행했는데도 한영변환이 안될 경우 아래 과정을 진행한다.

~~~shell
# 오른쪽 Alt키의 기본 키 맵핑을 제거하고 'Hangul'키로 맵핑
$ xmodmap -e 'remove mod1 = Alt_R'
$ xmodmap -e 'keycode 108 = Hangul'

# 오른쪽 Ctrl키의 기본 키 맵핑을 제거하고 'Hangul_Hanja'키로 맵핑
$ xmodmap -e 'remove control = Control_R'
$ xmodmap -e 'keycode 105 = Hangul_Hanja'

# 키 맵핑 저장
$ xmodmap -pke > ~/.Xmodmap
~~~
