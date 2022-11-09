---
layout: post
title: '[MySQL] CentOS에 MySQL 설치 및 설정'
category: Architecture
tags: [architecture, centos, mysql]
comments: true
---

# Environment
- CentOS 7
- MySQL Community Server 14.14

# MySQL 설치

~~~sh
# Maria DB 대신 MySQL 설치할 수 있게 해주는 패키지 다운로드
$ wget www.db21.co.kr/big/mysql-community-release-el7-5.noarch.rpm

# 패키지 설치
$ yum -y install mysql-community-release-el7-5.noarch.rpm

# MySQL 설치
$ yum -y install mysql-community-server

# MySQl 서버 시작
$ systemctl start mysqld

# MySQL 서버 자동 시작 설정
$ systemctl enable mysqld
~~~

# MySQL 설정
- Centos의 MySQL 설정 파일 위치는 /etc/my.cnf 이다.

~~~sh
$ vi /etc/my.cnf
~~~

## 원격접속 및 charset 설정
- 원격 접속을 허용하기 위해 bind-adress는 반드시 주석처리 한다.
- MySQL port도 변경 가능하다.
- 우리나라 표준시는 KST(UTC +9:00)로 변경한다. 
- emoji 사용이 있는 게시판 등의 테이블을 사용한다면 utf-8 대신 utf8mb4 charset을 사용한다.
- utf8mb4 charset을 사용하지 않으면 emoji를 포함한 데이터는 저장이 되지 않는다.

~~~bash
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
default-character-set = utf8mb4

[mysql]
default-character-set = utf8mb4

[mysqldump]
default-character-set = utf8mb4
~~~

### 참고

~~~sh
character_set_client     = utf8mb4            
character_set_connection = utf8mb4            
character_set_database   = utf8mb4            
character_set_filesystem = binary             
character_set_results    = utf8mb4            
character_set_server     = utf8mb4            
character_set_system     = utf8               
collation_connection     = utf8mb4_unicode_ci 
collation_database       = utf8mb4_unicode_ci 
collation_server         = utf8mb4_unicode_ci
~~~


## 원격 접속 권한 부여

~~~shell
# mysql 접속
mysql> mysql -u root -p

mysql> select user, host from mysql.user;

# user 생성
mysql> create user '<username>'@'%' identified by '<password>';

mysql> select user, host from mysql.user;

# 모든 권한 부여
mysql> grant all privileges on *.* to '<username>'@'%' with grant option;

# 권한 확인
mysql> show grants for '<username>'@'%';

mysql> flush privileges;

mysql> quit;
~~~





