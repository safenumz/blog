---
layout: post
title: '[Oracle] CentOS Oracle 설치 및 설정'
category: Architecture
tags: [sql, centos, oracle]
comments: true
---

# Environment
- CentOS 7
- Oracle 11g

# Oracle 설치
## 필요한 패키치 설치

~~~shell
$ yum install wget unzip

$ yum install libaio bc flex
~~~

## Oralce 11g 다운 및 설치
[https://www.oracle.com/technetwork/database/database-technologies/express-edition/downloads/xe-prior-releases-5172097.html](https://www.oracle.com/technetwork/database/database-technologies/express-edition/downloads/xe-prior-releases-5172097.html)

~~~shell
# Oracle 11g 다운
$ wget https://download.oracle.com/otn/linux/oracle11g/xe/oracle-xe-11.2.0-1.0.x86_64.rpm.zip?AuthParam=1562773337_a5433eea07c2e368a6e711a52006727f

# 압축 해제
$ upzip -q oracle-xe-11.2.0-1.0.x86_64.rpm.zip

# Disk1 폴더로 이동
$ cd Disk1

# oracle 설치
$ rpm -ivh oracle-xe-11.2.0-1.0.x86_64.rpm

# makefile 생성
$ cd / . && cd /etc/init.d/oracle-xe configure

$ cd /u01/app/oracle/product/11.2.0/xe/bin/

# sqlplus 명령어를 통해 dbms 접속가능하게 함
$ . ./oracle_env.sh

# oracle db 접속
$ sqlplus
~~~

# Oracle 계정 생성 및 권한 부여

~~~shell
$ sqlplus system/<system계정 비밀번호>

# 계정 생성
SQL> CREATE USER scott IDENTIFIED BY tiger;

# 자원과 연결 권한 부여
SQL> GRANT resource, connect TO scott;

# 테이블 저장 공간 기본값으로 설정
SQL> ALTER USER scott DEFAULT tablespace USERS;
SQL> ALTER USER scott temporary tablespace TEMP;
~~~
