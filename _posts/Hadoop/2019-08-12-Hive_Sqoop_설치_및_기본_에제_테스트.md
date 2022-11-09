---
layout: post
title: '[Hive Sqoop] Hive Sqoop 설치 및 기본 예제 테스트'
category: Hadoop
tags: [hive, sqoop]
comments: true
---

# install maria db

~~~shell
$ yum install mariadb-server mariadb

$ rpm -qa | grep maria

$ systemctl enable mariadb.service

$ systemctl start mariadb.service

$ mysql_secure_installation
~~~

~~~shell
$ vi /etc/my.cnf
~~~

~~~bash
# /etc/my.cnf
bind-address=192.168.56.102
~~~

~~~shell
# maria db restart
$ systemctl restart mariadb.service

# mysql 접속
$ mysql -u root -p
~~~

~~~shell
# databases 확인
MariaDB> show databases

# 모든 사용자에게 모든 권한 부여
MariaDB> grant all privileges on *.* to hive@'%' identified by "hive" with grant option;

# 자기 자신에게 모든 권한 부여
MariaDB> grant all privileges on *.* to hive@'dn01' identified by "hive" with grant option;

MariaDB> flush privileges;

# mysql database 접속
MariaDB> use mysql;

# user 권한 확인
MariaDB [mysql]> select user, host from user;

# exit
MariaDB [mysql]> exit;
~~~

# hive

~~~shell
$ cd /tmp
$ wget http://apache.mirror.cdnetworks.com/hive/hive-2.3.5/apache-hive-2.3.5-bin.tar.gz

$ mkdir -p /opt/hive/2.3.5

$ sudo mv apache-hive-2.3.5-bin/* /opt/hive/2.3.5/

$ sudo ln -s /opt/hive/2.3.5 /opt/hive/current

$ cd /opt/hive/current/

$ sudo chmod -R 775 /opt/hive/2.3.5/

$ sudo chown -R hadoop:hadoop /opt/hive/
~~~

~~~shell
$ vi ~/.bash_profile
~~~

~~~bash
# ~/.bash_profile
#### HIVE 2.3.5 #######################
export HIVE_HOME=/opt/hive/current
export PATH=$PATH:$HIVE_HOME/bin
export CLASSPATH=.:${JAVA_HOME}/lib:${JREHOME}/lib:/opt/hive/current/lib
#### HIVE 2.3.5 #######################
~~~

~~~shell
$ cp /opt/hive/current/conf/hive-env.sh.template /opt/hive/current/conf/hive-env.sh
$ cp /opt/hive/current/conf/hive-default.xml.template /opt/hive/current/conf/hive-site.xml
~~~

~~~shell
$ vi /opt/hive/current/conf/hive-env.sh
~~~

~~~bash
HADOOP_HOME=/opt/hadoop/current
~~~

~~~shell
$ vi /opt/hive/current/conf/hive-site.xml
~~~

~~~bash
<property>
  <name>javax.jdo.option.ConnectionURL</name>
  <value>jdbc:mysql://192.168.56.102:3306/hive?createDatabaseIfNotExist=true</value>
  <description>JDBC connect string for a JDBC metastore</description>
</property>
<property>
  <name>javax.jdo.option.ConnectionDriverName</name>
  <value>com.mysql.jdbc.Driver</value>
  <description>Driver class name for a JDBC metastore</description>
</property>
<property>
  <name>javax.jdo.option.ConnectionUserName</name>
  <value>hive</value>
  <description>username to use against metastore database</description>
</property>
<property>
  <name>javax.jdo.option.ConnectionPassword</name>
  <value>hive</value>
  <description>password to use against metastore database</description>
</property>

 <property>
 <name>hive.exec.local.scratchdir</name>
 <value>/home/hadoop/iotmp</value>
 <description>Local scratch space for Hive jobs</description>
 </property>

 <property>
 <name>hive.downloaded.resources.dir</name>
 <value>/home/hadoop/iotmp</value>
 <description>Temporary local directory for added resources in the remote file system.</description>
 </property>

<property>
    <name>hive.cli.print.current.db</name>
    <value>true</value>
    <description>Whether to include the current database in the Hive prompt.</description>
</property>
~~~

~~~bash
$ mkdir -p /home/hadoop/iotmp

$ chmod -R 775 /home/hadoop/iotmp/
~~~

# MySQL connector

~~~shell
$ cd /tmp

$ wget http://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.38.tar.gz

$ cd mysql-connector-java-5.1.38

$ mv mysql-connector-java-5.1.38-bin.jar /opt/hive/current/lib/
~~~

~~~shell
$ hdfs dfs -mkdir /tmp

$ hdfs dfs -mkdir -p /user/hive/warehouse

$ hdfs dfs -chmod -R 777 /tmp

$ hdfs dfs -chmod -R 777 /user/hive/warehouse
~~~

#### 만약 권한 설정 부분에서 에러가 뜬다면 아래 부분 hdfs-site.xml 추가

~~~shell
# hdfs-site.xml
<property>
<name>dfs.namenode.rpc-bind-host</name>
    <value>0.0.0.0</value>
</property>
~~~

~~~shell
$ schematool -initSchema -dbType mysql

# ( 에러가 발생한다면 기존에 같은 이름의 데이타베이스가 있으니깐
#  show databases에서 drop database hive; 제거 )
~~~


> ( hive 실행전에 메타스토어를 초기화 해야 한다. 즉 15번 하고 hive 명령어 실행  )
> 브라우저에서 http://192.168.56.101:50070
>메뉴 >  Utitlies > Browser Directory >
> /    
> user
> hive
> warehouse 에서 앞으로 확인

~~~shell
$ hive
~~~


> beeline  접속하기 위한 추가 작업
> beeline은 그룹과 유저가 other이기 때문에

> 모든 노드의 core-site.xml 에 수정 :
> 모든 그룹과 호스트에게 접속하기 위한 관문역할의  proxy를 모두가 가능하도록 변경

~~~shell
$ cd $HADOOP_HOME/etc/hadoop
~~~

~~~shell
vi core-site.xml

<property>
  <name>hadoop.proxyuser.hadoop.groups</name>
  <value>*</value>
</property>
<property>
  <name>hadoop.proxyuser.hadoop.hosts</name>
  <value>*</value>
</property>
~~~


~~~shell
$ hdfs dfs -chmod -R 777 /tmp
$ hdfs dfs -chmod -R 777 /user/hive/warehouse
~~~~

~~~shell
$ scp core-site.xml hadoop@dn01:/opt/hadoop/current/etc/hadoop
$ scp core-site.xml hadoop@dn02:/opt/hadoop/current/etc/hadoop
~~~

# hive 실행
## mariadb.service 상태
## 메타스토어 실행 (백그라운드 실행)

~~~shell
$ hive --service metastore &
~~~

## hive 서버 구동 (백그라운드 실행)

~~~shell
$ hive --service hiveserver2 &
~~~

~~~shell
$ show databases;

$ create database sample1;

$ use sample1;

$ create table employees (name string);

$ insert into employees (name) values('KIM');
~~~

~~~shell
$ beeline -u jdbc:hive2://hd4.cluster.kr:10000
~~~

# hive table 생성


~~~sql
CREATE TABLE movies (
    movie_id INT,
    movie_title STRING,
    release_date STRING,
    video_release_date STRING,
    imdb_url STRING,
    unknown INT,
    action INT,
    adventure INT,
    animation INT,
    children INT,
    comedy INT,
    crime INT,
    documentary INT,
    drama INT,
    fantasy INT,
    film_noir INT,
    horror INT,
    musical INT,
    mystery INT,
    romance INT,
    sci_fi INT,
    thriller INT,
    war INT,
    Western INT
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY '|'
STORED AS TEXTFILE;
~~~

~~~sql
CREATE EXTERNAL TABLE users (
 user_id INT,
 age INT,
 gender STRING,
 occupation STRING,
 zip_code STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY '|'
STORED AS TEXTFILE;
~~~

~~~sql
CREATE EXTERNAL TABLE users (
 user_id INT,
 age INT,
 gender STRING,
 occupation STRING,
 zip_code STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY '|'
STORED AS TEXTFILE
LOCATION '/user/hadoop/hive_data/userinfo2';
~~~


~~~sql
$ show databases;

$ use kdatademo

create table customers (
    id bigint,
    name string,
    address string
)

$ show tables;

$ desc customers

insert into customers values
(1111, 'kim', 'WA'),
(2222, 'park', 'WA'),
(3333, 'lee', 'WA'),
(4444, 'meng', 'CA'),
(5555, 'bae', 'NJ'),
(6666, 'jeon', 'NY');


create table if not exists orders (
    id bigint,
    product_id string,
    customer_id bigint,
    quantity int,
    amount double
);

desc orders;

insert into orders values
(111111, "phone", 1111, 3, 1200),
(222222, "camera", 1111, 1, 5200),
(100003, "notebook", 1111, 1, 10),
(100004, "bag", 2222, 2, 50),
(100005, "t-shirt", 4444, 5, 20)

select * from orders;

select * from customers;


[연습]
1. 주소가 'WA'인 고객 검색
2. 주소가 'WA'이면서 id가 2222보다 큰 고객명단 출력
3. 주소로 정렬하여 고객명과 주소 출력
4. 고객 명단수 검색
5. 첫번째 고객명단 출력 (mysql 명령어 이용)
6. 주소별 인원수 검색
7. 고객아이디, 고객명과 고객이 주문한 상품명 출력

~~~



# hiverserver2기동시 connection refused가 발생하는 경우 조치방법

### 1. hiveserver2를 기동한다.
hadoop@bigdata-host:~/hive/bin$ hive --service hiveserver2


### 2. 외부 프로그램에서 hive server에 접근한다.
org.apache.thrift.transport.TTransportException: java.net.ConnectException: Connection refused: connect


> hive-conf.xml에서 아래 항목의 value를 localhost에서 서비스가 되는 서버의 ip로 바꿔줘야.. 외부에서 접속이 가능함

~~~shell
<property>
<name>hive.server2.authentication</name>
<value>NOSASL</value>
</property>

<property>
  <name>hive.server2.thrift.bind.host</name>
  <value>192.168.0.39</value>
  <description>Bind host on which to run the HiveServer2 Thrift interface.
  Can be overridden by setting $HIVE_SERVER2_THRIFT_BIND_HOST</description>
</property>

<property>
  <name>hive.server2.thrift.port</name>
  <value>10000</value>
  <description>Port number of HiveServer2 Thrift interface.
  Can be overridden by setting $HIVE_SERVER2_THRIFT_PORT</description>
</property>


<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
<property>
  <name>beeline.hs2.connection.user</name>
  <value>hiveuser</value>
</property>
<property>
  <name>beeline.hs2.connection.password</name>
  <value>hivepw</value>
</property>

<property>
  <name>beeline.hs2.jdbc.url.tcpUrl</name>
  <value>jdbc:hive2://localhost:10000/default</value>
</property>

<property>
  <name>beeline.hs2.jdbc.url.httpUrl</name>
  <value>jdbc:hive2://localhost:10000/default;transportMode=http;httpPath=cliservice</value>
</property>

<property>
  <name>beeline.hs2.jdbc.url.default</name>
  <value>tcpUrl</value>
</property>
</configuration>
~~~


~~~sql
create table mobilephones
(id string,
title string,
cost double,
colors array<string>,
scree_size array<float>,
features map<string, Boolean>
)
ROW FORMAT delimited field terminated by ','
collection items terminated by '#'
map keys terminated by ':';
~~~

~~~sql
$ load data inpath '/home/hadoop/hive_data/mobilephones.csv' into table mobilephones;
~~~


~~~sql
create table mobilephone2
(id string,
title string,
cost double,
colors array<string>,
screen_size array<float>,
features map<string, Boolean>,
information struct<battery:string,camera:string>
)
row format delimited field terminated by ','
collection items terminated by '#'
map keys terminated by ':';

load data local inpath '/home/hadoop/hive_data/mobilephones2.csv'
into table mobilephones2;
~~~

- 제품명과 카메라 특성 출력

~~~shell
# 제품 아이디, 첫번째 색상, 카메라 특성, 배터리 정보 출력
select id, colors[0], features['camera'], information.battery
from mobilephones2;
~~~

~~~shell
$ wget httP://www.grouplens.org/system/files/ml-100k.zip

$ unzip ml-100k.zip

# u.item 파일은 hdfs에 /user/hadoop/movies 폴더 복사
# u.user 파일은 hdfs에 /user/hadoop/userinfo 폴더 복사
~~~


HIVE    | HDFS
:------:|:--------:
테이블   | 디렉토리
파티션   | 서브디렉토리
데이터   | 파일

1.테이블 생성
- 내부테이블
- 외부테이블: drop을 해도 파일 삭제 안함 (메타데이타를 지워짐)
2. 파티션 테이블
- insert
- load
- 외부테이블파티션
- 다중컬럼파티션
3. 연습
SQOOP 설치


hdfs dfs -ls -R /user/hive/warehouse

~~~sql
CREATE TABLE movies (
    movie_id INT,
    movie_title STRING,
    release_date STRING,
    video_release_date STRING,
    imdb_url STRING,
    unknown INT,
    action INT,
    adventure INT,
    animation INT,
    children INT,
    comedy INT,
    crime INT,
    documentary INT,
    drama INT,
    fantasy INT,
    film_noir INT,
    horror INT,
    musical INT,
    mystery INT,
    romance INT,
    sci_fi INT,
    thriller INT,
    war INT,
    Western INT
)
ROW FORMAT DELIMITED  FIELDS TERMINATED BY '|'
STORED AS TEXTFILE;
~~~

~~~sql
desc formatted movies;

load data inpath 'hdfs://user/hadoop/movies/u.item' into table movies;

delete from 테이블명 : 데이터만 삭제 (메모리 자리 남김, commit안했으면 살아있음)
drop 테이블명 : 데이터 + 구조 삭제
truncate 테이블명 : 데이터 삭제 (삭제후에 메모리 반환)

truncate

hdfs dfs -ls /user/hive/warehouse/hivedemo.db/movies
-> 사라짐

hdfs dfs -ls -R /user/hadoop/movies
- 사라짐
~~~

~~~sql
load data local inpath '/home/hadoop/hive_data/ml-100k/u.item' into table movies;

select * from movies limit 2;


CREATE EXTERNAL TABLE users (
    user_id INT,
    age INT,
    gender STRING
)
ROW FORMAT DELIMITED FIELDS TERMINATED  BY '|'
STORE AS TEXTFILE;


-- 파일을 로드하는 경우 DROP TABLE를 하면 메타스토어와 실질적인 파일 지워짐
DROP TABLE USERS;

-- 외부 테이블로 설정하면 테이블을 지워도 파일은 안지워짐


CREATE EXTERNAL TABLE users (
    user_id INT,
    age INT,
    gender STRING,
    occupation STRING,
    zip_code STRING
)
~~~


# 파티션

~~~sql
create table orders
 (
 id string,
 customer_id string,
 product_id string,
 quantity int,
 amount double,
 zipcode char(5)
 )
 partitioned by (state char(2))
 row format delimited fields terminated by ',';

 load data local inpath '/home/hadoop/hive_data/orders_CA.csv' into table orders
 partition(state='CA')


create table all_orders
(
id string,
customer_id string,
product_id string,
quantity int,
amount double,
postalcode string
)
partitioned by (country string, state string)
row format delimited fields terminated by ',';

load data local inpath '/home/hadoop/hive_data/orders_CA.csv' into table all_orders
partition(country='US' state='CA');

load data local inpath '/home/hadoop/hive_data/orders_CT.csv' into table all_orders
partition(country='KR' state='CT')
~~~




# SQOOP 설치

~~~shell
$ wget http://mirror.apache-kr.org/sqoop/1.4.7/sqoop-1.4.7.bin__hadoop-2.6.0.tar.gz

$ tar xvzf sqoop-1.4.7.bin__hadoop-2.6.0.tar.gz

$ mkdir -p /opt/sqoop/1.4.7

$ cp -r sqoop-1.4.7.bin__hadoop-2.6.0/* /opt/sqoop/1.4.7

$ ln -s /opt/sqoop/1.4.7 /opt/sqoop/current
~~~

## Mysql connector 다운받아서 sqoop 폴더 /lib에 복사 (mysql-connector-java-5.1.38-bin.jar)

~~~shell
$ wget http://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.38.tar.gz

$ tar xvzf mysql-connector-java-5.1.38.tar.gz

$ cd mysql-connector-java-5.1.38/

$ cp mysql-connector-java-5.1.38/mysql-connector-java-5.1.38-bin.jar /opt/sqoop/current/lib/
~~~

## /opt/sqoop 경로를 hadoop 소유자 변경

~~~shell
$ sudo chmod -R 775 /opt/sqoop/1.4.7/

$ sudo chown -R hadoop:hadoop /opt/sqoop/
~~~

## hadoop 계정에서 환경변수 설정

~~~shell
$ vi ~/.bash_profile
~~~

~~~shell
# SQOOP_HOME bin 폴더를 PATH에 설정
export SQOOP_HOME=/opt/sqoop/current
export PATH=$PATH:$SQOOP_HOME/bin
~~~

~~~shell
$ source ~/.bash_profile

$ cd $SQOOP_HOME/conf

$ cp sqoop-env-template.sh sqoop-env.sh

$ vi sqoop-env.sh
~~~

~~~bash
export HADOOP_COMMON_HOME=/opt/hadoop/current
export HADOOP_MAPRED_HOME=/opt/hadoop/current
export HIVE_HOME=/opt/hive/current
~~~

~~~bash
$ cp $SQOOP_HOME/sqoop-1.4.7.jar $HADOOP_HOME/share/hadoop/tools/lib

$ sqoop -version
~~~

~~~sql
-- Maria DB 접속
show databases;

create database sqoopdemo;

use sqoopdemo;


CREATE TABLE departments (
    department_id int(11) unsigned NOT NULL,
    department_name varchar(30) NOT NULL,
    PRIMARY KEY (department_id)
);

INSERT INTO departments (department_id, department_name)
VALUES (1, 'fitness'),
    (2, 'sportware'),
    (3, 'apparel'),
    (4, 'gold'),
    (5, 'outdoor'),
    (6, 'fat shop');


SELECT * FROM departments;
~~~

~~~shell
sqoop list-databases --connect jdbc:mysql://dn01 --username hive --password hive

sqoop list-tables --connect jdbc:mysql://dn01/sqoopdemo --username hive --password hive


# sqoop을 통해 mysql -> hdfs (import) 확인
# target-dir 지정하지 않으면 /user/hadoop/departments에 저장된다
# --target-dir sqoopdemo 지정하면 /user/hadoop/sqoopdemo/departments에 저장된다
sqoop import --connect jdbc:mysql://dn01/sqoopdemo --table departments --username hive --password hive --target-dir sqoopdemo/departments

hdfs dfs -ls /user/hadoop/sqoopdemo/departments

hdfs dfs -cat /user/hadoop/sqoopdemo/departments/part-m-*
~~~

### sqoop을 통해 hdfs -> mysql (export) 확인
- DDL(테이블 구조)는 미리 있어야 함

~~~shell
$ mysql -u hive -p hive
~~~

~~~sql
USE sqoopdemo;

CREATE TABLE dept LIKE departments;
~~~

~~~shell
# Hadoop /user/hadoop/sqoopdemo/departments에 있는 데이터를 Maria DB dept 테이블로 export
$ sqoop export --connect jdbc:mysql://dn01:3306/sqoopdemo --table dept --table dept --username hive --password hive --export-dir sqoopdemo/departments
~~~

~~~sql
INSERT INTO departments value (7, 'it items');
~~~

~~~shell
# deparments 테이블에 데이터를 추가하고 다시 import 하면 이미 테이블이 존재하기 때문에 에러
# overwrite 되는 개념이 아님에 주의
sqoop import --connect jdbc:mysql://dn01/sqoopdemo --table departments --username hive --password hive --target-dir sqoopdemo/departments
~~~~

~~~shell
# 기존 데이터 삭제 후 import 시도
$ hdfs dfs -rm -R /user/hadoop/sqoopdemo/departments

$ sqoop import --connect jdbc:mysql://dn01/sqoopdemo --table departments --username hive --password hive --target-dir sqoopdemo/departments

$ hdfs dfs -cat /user/hadoop/sqoopdemo/departments/*
~~~

-----------------

~~~sql
INSERT INTO departments values(8, 'food');

SELECT * FROM departments;
~~~

~~~shell
$ sqoop import --connect jdbc:mysql://dn01/sqoopdemo --table departments --username hive --password hive --target-dir sqoopdemo/departments --check-column department_id --incremental append --last-value 7

$ hdfs dfs -cat /user/hadoop/sqoopdemo/departments/*
~~~

-----------------


~~~sql
INSERT INTO departments values(9, 'computer');

SELECT * FROM departments;
~~~

~~~shell
$ sqoop import --connect jdbc:mysql://dn01/sqoopdemo --table departments --username hive --password hive --target-dir sqoopdemo/departments --check-column department_id --incremental append --last-value 7

$ hdfs dfs -cat /user/hadoop/sqoopdemo/departments/*
1,fitnes
2,sportw
3,appare
4,gold  
5,outdoo
6,fat sh
7,it ite
8,food  
8,food  
9,compuster
~~~

------------

~~~shell
$ sqoop export --connect jdbc:mysql://dn01:3306/sqoopdemo --table dept --username hive --password hive --export-dir sqoopdemo/departments

# Maria DB의 primary key가 중복된 상황이라 error
8,food  
8,food
~~~

~~~shell
$ sqoop export --connect jdbc:mysql://dn01:3306/sqoopdemo --table dept --username hive --password hive --export-dir sqoopdemo/departments --update-key department_id --update-mode allowinsert
~~~



~~~shell
$ hive --service metastore &

$ hive --service hiveserver2 &

$ hive
~~~

~~~sql
hive> show databases;

hive> create database kdatademo;

hive> use kdatademo;

hive (kdatademo)> show tables;
~~~


~~~shell
# map은 하나만 씀
# 에러 발생: departments already exists
$ sqoop import --connect jdbc:mysql://dn01:3306/sqoopdemo --username hive --password hive --table departments --hive-import --hive-table kdatademo.departments -m 1

# departments 경로 삭제
$ hdfs dfs -rm -R /user/hadoop/departments

# 재시도
# 에러발생:
# $SQOOP_HOME/lib 경로에 hive-common-0.10.0.jar 가 필요함
$ sqoop import --connect jdbc:mysql://dn01:3306/sqoopdemo --username hive --password hive --table departments --hive-import --hive-table kdatademo.departments -m 1

# $SQOOP_HOME/lib 경로에 hive-common-0.10.0.jar 넣고 다시 시도
$ hdfs dfs -rm -r /user/hadoop/departments
$ sqoop import --connect jdbc:mysql://dn01:3306/sqoopdemo --username hive --password hive --table departments --hive-import --hive-table kdatademo.departments -m 1
~~~

~~~sql
hive (kdatademo)> show tables;
departments

hive (kdatademo)> select * from departments;
~~~