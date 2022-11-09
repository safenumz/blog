---
layout: post
title: '[Linux] 파일내용 정렬 (sort)'
category: Linux
tags: [linux]
comments: true
---

# sort
- sort는 텍스트로 된 파일의 행단위 정렬을 할 때 사용하는 명령어이다


## 오름차순 정렬

~~~bash
$ sort textfile.txt
~~~

## 내림차순 정렬

~~~bash
$ sort -r textfile.txt
~~~

## 지정한 두번째 필드(-k 옵션)를 기준으로 정렬

~~~bash
$ sort -k 2 textfile.txt
~~~

## 중복된 내용을 하나로 취급하여 유일 정렬
~~~bash
$ sort -u textfile.txt
~~~

## 용량크기 순으로 오름차순 정렬

~~~bash
$ ls -l /var/log | sort -k 5
~~~

## 파일이름 대상으로 오름차순 정렬

~~~bash
$ ls -l /var/log | sort -k 8
~~~