---
layout: post
title: '[MySQL] MySQL Replication'
category: Architecture
tags: [mysql, replication]
comments: true
---

# 1. MySQL Replication 특징
- 비동기식 (Asynchronous Replication)
- 1개의 Master와 n개의 Slave로 구성되어 있음
- Master에서만 읽기 / 쓰기를 수행할 수 있음
- Master / Slave 데이터 동기화 시간차가 있음 (쓰기가 발생하면 순차적으로 한대씩 각 DB로 복제 진행)
- 장애 복구시 노드간 유실되는 트랜잭션 있음
- 읽기만 확장 용이함 (쓰기 확장시 난점이 있음)
- Master 장애시 매우 크리티컬한 장애 발생 (복구 장기간 소요, 데이터 유실 위험 높음)
- 부분 장애시 장애 노드만 장애 발생
- 장애 요인 제거 후 단기간 / 순차적 정상화
- MySQL의 부하 분산과 효율을 최우선으로 하는 경우 유리

## 1) Master 역할
- 웹서버로 부터 데이터 등록 / 수정 / 삭제 요청시 바이너리 로그 (Binary log)를 생성하여 Slave 서버로 전달함
- 웹서버로부터 요청한 데이터 등록 / 수정 / 삭제 기능을 하는 DBMS로 사용

## 2) Slave 역할
- Master DBMS로부터 전달받은 바이너리 로그 (Binary log)를 데이터로 반영
- 웹서버로부터 요청을 통해 데이터를 불러오는 DBMS로 사용

## 3) MySQL Replication 주의사항
- 호환성을 위해 Replication을 사용하는 MySQL의 버전을 동일하게 맞추는 것이 좋음
- MySQL 버전이 다른 경우 반드시 Slave 서버가 상위버전이어야 함
- Replication을 가동시에 Master, Slave 순으로 가동시켜야 함


# 2. XtraBackup
- Percona에서 개발한 오픈소스 MySQL 백업도구
- mysqldump가 테이블 생성, 데이터 쿼리에 대한 SQL 생성문을 갖는 논리적 백업이라면, XtraBackup은 엔진 데이터를 그대로 복사하는 물리적 백업 방식
- XtraBackup의 백업 방식은 크게 전체 백업, 증분 백업, 개별(db, table) 백업, Encrypted 백업 방식이 있음


## 1) mysqldump vs XtraBackup

## 2) xtrabackup vs innobackupex
- XtraBackup에서 innobackupex는 2.2 next major 부터 삭제된다고 명시
- 실제로는 XtraBackup 2.4 버전까지는 유지되며 8 버전부터 삭제됨
- 실무에서 주로 사용하는 MySQL 5.6 ~5.7 버전은 XtraBackup 2.0~2.4 버전에서 호환되기에 여기서는 innobackupex 방식 사용
- XtraBackup 8 버전은 MySQL 8 버전부터 호환됨


## 3) 전체 백업
- 장애 시 복구 과정을 단순하게 하기 위해 증분 백업을 사용하지 않고 전체 백업을 사용함
- 백업하는 동안에 table lock을 걸지 않으려면 --no-lock 옵션을 추가해야 함
- 단, InnoDB table이 아닌 경우에는 일관성 없는 백업 결과가 나올 수 있음
- 지정한 백업 경로에 테이블, 리두로그, XtraBackup 관련 테이터들이 함께 백업


# 3. 사전 환경설정

## 1) Dependency
- MySQL 5.6
- XtraBackup 2.4
- pv 1.4.6


## 2) MySQL 설치
- 생략

## 3) XtraBackup 설치
- 2.4 버전 설치

```sh
$ yum install -y http://www.percona.com/downloads/percona-release/redhat/0.1-4/percona-release-0.1-4.noarch.rpm
$ yum list | grep percona
$ yum update -y percona-release
$ yum install -y percona-xtrabackup-24.x86_64
$ xtrabackup --version
```

## 4) PV 설치

```sh
$ yum install -y pv
```

## 5) 서버간 SSH Public Key 연결
- 생략

# 4. MySQL Replication 설정

## 1) Master 서버 설정

### my.cnf 설정파일 수정

```sh
$ vi /etc/my.cnf
```

```sh
# /etc/my.cnf

[client]
port = <mysql_port>
default-character-set = utf8

[mysql]
default-character-set=utf8

[mysqld]
server-id=1
log-bin=mysql-bin
log-slave-updates=1

port = <mysql_port>
max_connections = 2000
max_connect_errors = 1000000
max_allowed_packet = 1G
innodb_log_file_size = 1G
innodb_buffer_pool_size = 8G
innodb_log_file_size = 256M
innodb_thread_concurrency = 16
datadir = /var/lib/mysql
socket = /var/lib/mysql/mysql.sock


character-set-server=utf8
collation-server=utf8_general_ci
init_connect=SET collation_connection = utf8_general_ci
init_connect=SET NAMES utf8

character-set-client-handshake = FALSE
skip-character-set-client-handshake

skip-host-cache
skip-name-resolve

[xtrabackup]
datadir=/var/lib/mysql

[mysqldump]
password=dsldsl
default-character-set=utf8
```

### MySQL 재기동으로 설정파일 반영

```sh
$ systemctl restart mysqld
```

### Replication 계정 생성 및 권한 부여

```sh
$ mysql -u root -p
```

```sh
mysql> GRANT REPLICATION SLAVE ON *.* TO '<repl_user>'@'%' IDENTIFIED BY '<repl_password>';
mysql> GRANT REPLICATION SLAVE ON *.* TO '<repl_user>'@'localhost' IDENTIFIED BY '<repl_password>';
mysql> GRANT RELOAD, LOCK TABLES, PROCESS, REPLICATION CLIENT ON *.* TO '<repl_user>'@'%';
mysql> GRANT RELOAD, LOCK TABLES, PROCESS, REPLICATION CLIENT ON *.* TO '<repl_user>'@'localhost';
mysql> FLUSH PRIVILEGES;
```

## 2) Slave 서버 설정

### my.cnf 설정파일 수정

```sh
$ vi /etc/my.cnf
```

```sh
# /etc/my.cnf

[client]
port = <mysql_port>
default-character-set = utf8

[mysql]
default-character-set=utf8

[mysqld]
server-id=2
log-bin=mysql-bin
log-slave-updates=1
datadir=/var/lib/mysql

port = <mysql_port>
max_connections = 2000
max_connect_errors = 1000000
max_allowed_packet = 1G
innodb_log_file_size = 1G
innodb_buffer_pool_size = 8G
innodb_log_file_size = 256M
innodb_thread_concurrency = 16

character-set-server=utf8
collation-server=utf8_general_ci
init_connect=SET collation_connection = utf8_general_ci
init_connect=SET NAMES utf8

character-set-client-handshake = FALSE
skip-character-set-client-handshake

read-only=1

slave-skip-errors = 1062
skip-host-cache
skip-name-resolve

[xtrabackup]
datadir=/var/lib/mysql

[mysqldump]
password=<password>
default-character-set=utf8
```

### 백업 폴더 생성

```sh
$ mkdir -p /data/mysql_backup/2020-07-16_11-00-00
$ cd /data/mysql_backup/2020-07-16_11-00-00
```

### XtraBackup을 이용하여 Master를 백업하여 Slave로 가져옴
- `--no-lock` 옵션이 있어야 운영 중단 없이 백업 가능
- innobackupex.log에는 “completed OK!”로 끝이 나야 하며, xbstream.log에는 아무 내용도 없어야 함

```sh
$ ssh -o StrictHostKeyChecking=no root@<master_ip> -p <master_port> "innobackupex --host=127.0.0.1 --user=<repl_user> --password=<repl_password> --no-lock --stream=xbstream /data/mysql_backup/2020-07-16_11-00-00 | pv --rate-limit 50000000" 2> innobackupex.log | xbstream -x 2> xbstream.log
```

### 데이터 복구 준비
- 백업이 특정 시점에 완벽하게 이뤄진다면 좋겠지만 데이터의 크기와 서버의 성능에 따라 백업하는 시간이 수십 시간까지도 소요될 수 있음
- 백업하는 중에 INSERT, UPDATE, DELETE 쿼리가 유입되면 백업된 데이터의 일관성이 사라질 수 있음
- 복원 준비 단계에서는 백업 중 수행된 트랜잭션 로그파일(xtrabackup_logfile)을 적용하여 데이터를 일관성 있게 만들어 줌

```sh
$ innobackupex --defaults-file=/etc/my.cnf --apply-log /data/mysql_backup/2020-07-16_11-00-00  2>> innobackupex.log
```

### Master의 binary log 파일과 위치를 기록해둠

```sh
# file
$ sed -r "s/^(.*)\s+([0-9]+)/\1/g" xtrabackup_binlog_info

# position
$ sed -r "s/^(.*)\s+([0-9]+)/\2/g" xtrabackup_binlog_info
```

### MySQL 중지

```sh
$ systemctl stop mysqld
```

### 기존 MySQL 파일 백업

```sh
$ mkdir -p /data/mysql_backup/native
$ /bin/cp -r /var/lib/mysql/* /data/mysql_backup/native
```

### 기존 MySQL 파일 삭제

```sh
$ rm -rf /var/lib/mysql/*
```

### 복원
- 준비된 Master의 백업 파일을 Slave의 datadir로 옮겨줌
- `--copy-back` 명령어를 사용하면 백업된 내용을 원본 디렉토리 (/var/log/mysql)에 이동시켜 줌

```sh
$ innobackupex --defaults-file=/etc/my.cnf --copy-back /data/mysql_backup/2020-07-16_11-00-00
```

### MySQL 파일이 재대로 들어왔는지 확인

```sh
$ ls -al /var/lib/mysql
```

### MySQL 권한 재설정

```sh
$ cd /var/lib/mysql
$ chown -R mysql:mysql *
$ chmod g+w /var/run/mysqld/
$ chgrp mysql /var/run/mysqld/
```

### MySQL 재기동

```sh
$ systemctl stop mysqld
$ systemctl start mysqld
```

### Slave에 Master 정보 등록

```sh
$ mysql -uroot -p
```

```sh
# slave 초기화
mysql> stop slave;
mysql> reset slave all;

# 마스터 설정
mysql> change master to master_host='<master_ip>', master_user='<repl_user>', master_port=<master_port>, master_password='<repl_password>', master_log_file='<master_binary_log_file>', master_log_pos=<master_binary_log_position>;

mysql> show slave status \G

mysql> start slave;

mysql> show slave status \G
```

