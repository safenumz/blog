---
layout: post
title: '[MySQL] Windows MySQL 외부접속 설정'
category: Architecture
tags: [sql, mysql, oracle]
comments: true
---

# Environment
- Windows 10

# MySQL 설정
## 사용자 계정이 외부의 IP에서 접속이 가능하도록 하는 단계

~~~sql
-- 특정 IP 접근 허용 설정
mysql> grant all privilege on *.* to 'root'@'<ip주소>' identified by '<root의 패스워드>'

-- 특정 IP 대역 접근 허용 설정
mysql> grant all privilege on *.* to 'root'@'<ip주소>' identified by '<root의 패스워드>'

-- 모든 IP의 접근 허용 설정
mysql> grant all privileges on *.* to 'root'@'%' identified by '<root의 패스워드>'

-- check
mysql> select host, user, password from user;

mysql> flush privileges;
~~~

# 방화벽 열어주기
- 제어판 > Windows Defender 방화벽 > 고급설정 > 인바운드 규칙 > 새규칙
- 프로그램 설정 > 다른 프로그램 경로 > 찾아보기 > mysqld.exe 등록
- 예) C://Program Files/MySQL/MySQL Server 5.7/bin/mysqld.exe


# LISTENER 체크

~~~sh
$ lsnrctl status

$ lsnrctl start

$ lsnrctl stop

$ lsnrctl reload
~~~