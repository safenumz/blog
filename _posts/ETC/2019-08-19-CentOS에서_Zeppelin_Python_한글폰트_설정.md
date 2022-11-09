---
layout: post
title: CentOS에서 Zeppelin Python 한글폰트 설정
category: etc
tags: [linux, ubuntu, centos, python]
comments: true
---

# Environment
- CentOS 7
- Zeppelin 0.8.1

# 나눔고딕 폰트 다운로드

~~~shell
# 나눔고딕 폰트 다운로드
$ cd /usr/share/fonts/

$ wget http://cdn.naver.com/naver/NanumFont/fontfiles/NanumFont_TTF_ALL.zip

$ unzip NanumFont_TTF_ALL.zip -d NanumFont

$ rm -f NanumFont_TTF_ALL.zip
~~~

~~~shell
# matplotlib font 폴더로 나눔 폰트 이동
$ cp /usr/share/fonts/NanumFont/* /home/a/anaconda3/lib/python3.6/site-packages/matplotlib/mpl-data/fonts/ttf/

# 캐시 삭제
$ rm -rf /home/a/.cache/matplotlib/*
~~~~
