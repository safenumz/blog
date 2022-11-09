---
layout: post
title: '[Oracle] Ubuntu Oracle 설치 및 설정'
category: Architecture
tags: [ubuntu, oracle]
comments: true
---

# Environment
- Ubuntu 18.4
- Oracle XE 11.2.0

# Oracle 설치
### Oracle 프로그램 다운로드
- [오라클 홈페이지 접속](https://www.oracle.com/downloads/index.html#database)
- Database 11g Express Edition 다운로드
- Oracle Database Express Edition 11g Release 2 for Linux x64
- home/<개인컴퓨터계정>/Downloads 폴더에 저장

### root 계정 로그인
- sudo su root / 자신의 패스워드

~~~shell
$ sudo su root
~~~

### 압축 파일 풀기
- 오라클 압축파일을 다운받은 경로(다운로드 폴더)로 이동
- unzip 명령어로 압축파일 풀어줌
- oracle-xe-11.2.0-1.0.x86_64.rpm
- 압축 해제 후 생성된 Disk1 폴더로 이동

~~~shell
$ cd Downloads

$ unzip oracle*

# 압축 해제 후 생성된 Disk1 폴더로 이동
$ cd Disk1
~~~

### alien libaio1 unixodbc 설치

~~~shell
$ apt-get -y install alien libaio1 unixodbc
~~~

### rpm 파일을 deb 파일로 변환

~~~shell
$ alien --scripts -d oracle*
~~~

### 오라클 설치

~~~shell
$ dpkg --install oracle*.deb
~~~

### /etc/init.d/oracle-xe configure 설정
- 포트는 기본 포트로 유지, 건드리지 않고 엔터만 치면 됨
- 비밀번호는 신규 설정

### 오라클 실행 확인

~~~shell
$ systemctl start oracle-xe

$ systemctl status oracle-xe
~~~


### 환경변수 등록
- . /u01/app/oracle/product/11.2.0/xe/bin/oracle_env.sh 수정

~~~shell
$ . /u01/app/oracle/product/11.2.0/xe/bin/oracle_env.sh

# vim 에디터 설치, 다른 에디터를 쓰고 있다면 설치하지 않아도 된다.
$ sudo apt-get install vim

# vim 에디터로 bash.bashrc 파일을 열어 준다.
$ vim bash.bashrc

$ source bash.bashrc
~~~

- bash.bashrc 파일 하단에 아래 설정을 넣어준다.

~~~bash
. /u01/app/oracle/product/11.2.0/xe/bin/oracle_env.sh
~~~


### 데이터베이스 저장 폴더 생성

~~~shell
$ mkdir /oradata/

$ chmod 777 /oradata/
~~~


# scott 계정 및 hr 계정 생성

~~~shell
$ sqlplus system/<system계정 비밀번호>

# 계정 생성
SQL> CREATE USER scott IDENTIFIED BY tiger;

# 자원과 연결 권한 부여
SQL> GRANT resource, connect TO scott

# 테이블 저장 공간 기본값으로 설정
SQL> ALTER USER scott DEFAULT tablespace USERS;
SQL> ALTER USER scott temporay tablespace TEMP;
~~~

## 연습용 데이터셋 넣기
- [https://github.com/mv/mvdba/tree/master/demo](https://github.com/mv/mvdba/tree/master/demo)
- demobld.sql 파일을 다운받고,
- 오라클이 설치된 경로 /u01/app/oracle/product/11.2.0/xe/sqlplus/admin 에 demobld.sql 파일을 넣어준다.

~~~shell
# scott계정 연결 확인
SQL> conn scott/tiger

# demobld.sql 파일이 있는 경로, 스크립트 실행
SQL> @/u01/app/oracle/product/11.2.0/xe/sqlplus/admin/demobld.sql   

# 연습용 데이터셋 확인
SQL> SELECT * FROM emp;
~~~

- 위 방식으로 실행했는데도 불구하고 데이터 테이블만 생성되고 데이터가 비어서 나올경우 데이터를 수작업으로 집어넣는다.

~~~sql
INSERT INTO EMP VALUES (7369,'SMITH','CLERK',7902,to_date('17-12-1980','dd-mm-yyyy'),800,NULL,20);
INSERT INTO EMP VALUES (7499,'ALLEN','SALESMAN',7698,to_date('20-2-1981','dd-mm-yyyy'),1600,300,30);
INSERT INTO EMP VALUES (7521,'WARD','SALESMAN',7698,to_date('22-2-1981','dd-mm-yyyy'),1250,500,30);
INSERT INTO EMP VALUES (7566,'JONES','MANAGER',7839,to_date('2-4-1981','dd-mm-yyyy'),2975,NULL,20);
INSERT INTO EMP VALUES (7654,'MARTIN','SALESMAN',7698,to_date('28-9-1981','dd-mm-yyyy'),1250,1400,30);
INSERT INTO EMP VALUES (7698,'BLAKE','MANAGER',7839,to_date('1-5-1981','dd-mm-yyyy'),2850,NULL,30);
INSERT INTO EMP VALUES (7782,'CLARK','MANAGER',7839,to_date('9-6-1981','dd-mm-yyyy'),2450,NULL,10);
INSERT INTO EMP VALUES (7788,'SCOTT','ANALYST',7566,to_date('13-7-1987','dd-mm-yyyy'),3000,NULL,20);
INSERT INTO EMP VALUES (7839,'KING','PRESIDENT',NULL,to_date('17-11-1981','dd-mm-yyyy'),5000,NULL,10);
INSERT INTO EMP VALUES (7844,'TURNER','SALESMAN',7698,to_date('8-9-1981','dd-mm-yyyy'),1500,0,30);
INSERT INTO EMP VALUES (7876,'ADAMS','CLERK',7788,to_date('13-7-1987','dd-mm-yyyy'),1100,NULL,20);
INSERT INTO EMP VALUES (7900,'JAMES','CLERK',7698,to_date('3-12-1981','dd-mm-yyyy'),950,NULL,30);
INSERT INTO EMP VALUES (7902,'FORD','ANALYST',7566,to_date('3-12-1981','dd-mm-yyyy'),3000,NULL,20);
INSERT INTO EMP VALUES (7934,'MILLER','CLERK',7782,to_date('23-1-1982','dd-mm-yyyy'),1300,NULL,10);

INSERT INTO DEPT VALUES (10,'ACCOUNTING','NEW YORK');
INSERT INTO DEPT VALUES (20,'RESEARCH','DALLAS');
INSERT INTO DEPT VALUES (30,'SALES','CHICAGO');
INSERT INTO DEPT VALUES (40,'OPERATIONS','BOSTON');

INSERT INTO SALGRADE VALUES (1,700,1200);
INSERT INTO SALGRADE VALUES (2,1201,1400);
INSERT INTO SALGRADE VALUES (3,1401,2000);
INSERT INTO SALGRADE VALUES (4,2001,3000);
INSERT INTO SALGRADE VALUES (5,3001,9999);
~~~


## 계정 삭제

~~~shell
# 계정 삭제, system 계정 로그인 후
SQL> DROP USER scott CASCADE
~~~


# 원격 접속 설정
- /u01/app/oracle/product/11.2.0/xe/network/admin
- 위 경로에 있는 listener.ora 파일과 tnsnames.ora 파일을 수정한다.


## listener.ora
- <내부 IP> 부분을 자신의 내부 IP로 변경한다.

~~~bash
 # listener.ora Network Configuration File:
  2 
  3 SID_LIST_LISTENER =
  4   (SID_LIST =
  5     (SID_DESC =
  6       (SID_NAME = PLSExtProc)
  7       (ORACLE_HOME = /u01/app/oracle/product/11.2.0/xe)
  8       (PROGRAM = extproc)
  9     )
 10   )
 11 
 12 LISTENER =
 13   (DESCRIPTION_LIST =
 14     (DESCRIPTION =
 15       (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC_FOR_XE))
 16       (ADDRESS = (PROTOCOL = TCP)(HOST = <내부 IP>)(PORT = 1521))
 17     )
 18   )
 19 
 20 DEFAULT_SERVICE_LISTENER = (XE)
~~~

## tnsnames.ora
- <내부 IP> 부분을 자신의 내부 IP로 변경한다.
- SID와 SERVICE_NAME이 XE인지 확인한다.(변경 가능)
- (SERVER = DEDICATED) 부분은 삭제

~~~bash
  1 # tnsnames.ora Network Configuration File:
  2 
  3 XE =
  4   (DESCRIPTION =
  5     (ADDRESS = (PROTOCOL = TCP)(HOST = <내부 IP>)(PORT = 1521))
  6     (CONNECT_DATA =
  7       (SERVICE_NAME = XE)
  8     )
  9   )
 10 
 11 EXTPROC_CONNECTION_DATA =
 12   (DESCRIPTION =
 13     (ADDRESS_LIST =
 14       (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC_FOR_XE))
 15     )
 16     (CONNECT_DATA =
 17       (SID = XE)
 18       (PRESENTATION = RO)
 19     )
 20   )
~~~

## Oracle 중지 후 재실행

~~~shell
$ systemctl stop oracle-xe

$ systemctl start oracle-xe

$ systemctl status oracle-xe
~~~


