---
layout: post
title: '[DB] Oracle sqlplus 화면 크기 조절'
category: etc
tags: [sql, oracle, sqlplus]
comments: true
---

## sqlplus 화면설정

~~~sql
set linesize 1000
set pagesize 100
~~~

## 고정설정
- <ORACLE_HOME>/sqlplus/admin/glogin.sql 파일 마지막 부분에 입력

~~~sql
set linesize 1000
set pagesize 100
~~~

## cmd 옵션변경
- 속성 -> 레이아웃 -> 화면 버퍼 크기(너비), 창크기(너비) 200

----------

## 참고

~~~sql
set numwidth 10;
set lines 150;
set trimout on;
set pagesize 10000;
set tab off;
set wrap on;
~~~
