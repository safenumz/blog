---
layout: post
title: '[MySQL] Ubuntu MySQL 설치 및 설정'
category: Architecture
tags: [architecture, ubuntu, mysql]
comments: true
---

# Environment
- Ubuntu 18.4

# Ubuntu에 MySQL 서버 설치

~~~bash
# mysql-server 설치
$ apt-get update
$ apt-get install mysql-server

# 설정한 MySQL 버전 확인을 위해 dkpg 명령을 사용한다.
$ dpkg --list | grep mysql

# MySQL 서버 시작
$ service mysql start

# MySQL 서버 종료
$ service mysql stop
~~~


# MySQL 설정
- Ubuntu의 MySQL 설정 파일 위치는 /etc/mysql/my.cnf 이다.

~~~sh
$ vi /etc/mysql/my.cnf
~~~
 
## utf8mb4 설정
- emoji 사용이 있는 게시판 등의 테이블을 사용한다면 utf-8 대신 utf8mb4 charset을 사용한다.
- utf8mb4 charset을 사용하지 않으면 emoji를 포함한 데이터는 저장이 되지 않는다.

~~~bash
# /etc/mysql/my.cnf

[mysqld]
# /etc/my.cnf

[mysqld]
# 원격 접속을 허용하기 위해 bind-adress는 반드시 주석처리 한다.
# bind-adress = 0.0.0.0


# 원하는 포트로 변경가능하다.
# port=3306

# 우리나라 표준시 KST로 변경, UTC +9:00
default-time-zone='+9:00'

character-set-server=utf8mb4
collation-server=utf8mb4_unicode_ci
skip-character-set-client-handshake

[client]
# port=3306
default-character-set = utf8

[mysql]
default-character-set = utf8mb4

[mysqldump]
default-character-set = utf8
~~~


## 원격 접속 권한 부여

~~~shell
mysql> mysql -u root -p

mysql> select user, host from mysql.user;

mysql> create user '<username>'@'%' identified by '<password>'

mysql> select user, host from mysql.user;

mysql> grant all privileges on *.* to '<username>'@'%' with grant option;

mysql> show grants for '<username>'@'%';

mysql> flush privileges;

mysql> quit;
~~~

## Port 열기

~~~sh
$ sudo service mysql restart

$ sudo ufw allow out 3306/tcp

$ sudo ufw allow in 3306/tcp

$ sudo service mysql restart
~~~

