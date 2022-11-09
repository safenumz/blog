---
layout: post
title: '[MongoDB] MongoDB Cluster 구축'
category: Architecture
tags: [nosql, mongo, cluster]
comments: true
---

# MongoDB 설치
- 3.6.4 버전 설치

~~~sql

> show collections

> db.sarams.insert({ name : '홍길동', age: 33})

> db.sarams.insert({name: '홍길자', hobby: ['축구']})

> db.sarams.find();

> db.sarams.insert({'_id': 11})
~~~


~~~sql
db.members.insert( { name : '송승헌', age : 33, grade:'A' }  )
db.members.insert( { name : '송승남', age : 23, grade:'C' }  )
db.members.insert( { name : '김남송', age : 25, grade:'C' }  )
db.members.insert( { name : '김남길', age : 27, grade:'A' }  )
db.members.insert( { name : '여진구', age : 30, grade:'B' }  )
db.members.insert( { name : '진구', age : 29, grade:'A' }  )
db.members.insert( { name : '조인성', age : 39, grade:'B' }  )
db.members.insert( { name : '소지섭', age : 33, grade:'A' }  )

-- 33번만 검색
> db.members.find({age: 33})

-- 나이가 25세 이상인 명단 출력
db.members.find({age:{$gte: 25 }})

-- 나이가 25세 이상 33세 이하 명단 출력
db.members.find({age:{$gte: 25}} $and {age:{$lte : 33}})

db.members.find({age:{$gte: 25, $lte: 33 }})

~~~


# Master Node

~~~shell
$ mongod --dbpath /data/data1 --master
~~~

~~~sql


$ mongo localhost:27017

$ db.printReplicationinfo()

$ db.persons.insert({name: '홍길동'})
$ db.persons.insert({name: '홍길자'})

-- 오름차순 정렬
$ db.persons.find().sort({name: 1})

-- 내림차순 정렬
$ db.persons.find().sort({name: -1})


~~~

# Slave Node1

~~~sql
$ mongod --dbpath /data/data2 --slave --source localhost:27017 --port 20000

$ mongo localhost:20000

 -- 슬래이브에 관한 정보 보여줌
> db.printSlaveReplicationinfo()

> use persondb

> db.psersons.setSlaveOk()

> db.persons.find()

-- 에러 라이팅 불가, 라이팅은 마스터만 불가
> db.persons.insert({name: '홍홍이'})
~~~
sudo iptables -I INPUT -p tcp --dport 80 -j ACCEPT


mongo -u <유저명 developer> -p <패스워드 developer> <public_ip:>:27017/<데이터베이스명 mongo>

# Slave Node2

~~~sql
$ mongod --dbpath /data/data3 --slave --source localhost:27017 --port 30000

$ mongo localhost:30000

 -- 슬래이브에 관한 정보 보여줌
> db.printSlaveReplicationinfo()

> use persondb

> db.psersons.setSlaveOk()

> db.persons.find()

> db.persondb.insert(name: '홍홍이')
~~~



~~~sql
use admin

db.createUser({
  user: "dbadmin",  
  pwd: "password",  
  roles: [          
    {
      role: "root",
      db: "admin"   
    }
  ]}
)

db.createUser({
  user: "rhostem",
  pwd: "password",
  roles: [
    {
      role: "readWrite",
      db: "test"         
    }
  ]}
)
~~~



# 1번

~~~sql
> mongod --dbpath c:\data\work\data1\ --port 10000 -replSet bokjeSet

> mongo localhost:10000

> config = { _id: 'bokjeSet',members:[
  {_id:0, host:'localhost:10000'},
  {_id:1, host:'localhost:20000'},
  {_id:2, host:'localhost:30000'}]}

> rs.initiate(config)

-- 1번을 마스터로 설정
> db.isMaster()

> use persondb

> db.persondb.insert({name: 'kim'})

> db.persondb.insert({name: 'bae'})

> db.persondb.find()
~~~

# 2번

~~~sql
> mongod --dbpath c:\data\work\data2\ --port 20000 -replSet bokjeSet

> mongo localhost:20000


> use persondb

> rs.slaveOk()
~~~

# 3번

~~~sql
> mongod --dbpath c:\data\work\data3\ --port 30000 -replSet bokjeSet

> mongo localhost:30000

> use persondb

> rs.slaveOk()
~~~


~~~sql
netstat
: 실행중인 port 찾기


netstat -a -o
: 실행중인 port 표시, 프로세스id(pid) 표시


taskkill /f /pid 1234
: 1234 프로세스id(pid) kill하기
~~~



~~~shell
> mongod --shardsvr --dbpath c:\data\work\sh1 --port 10000


> mongod --shardsvr --dbpath c:\data\work\sh2 --port 20000

> mongod --shardsvr --dbpath c:\data\work\sh3 --port 30000

> mongod --configsvr --dbpath c:\data\work\cfg --port 40000

5번째
> mongos --configdb localhost:40000 --chunkSize 1

6번째 창
> mongos
mongos> use admin
mongos> db
mongos> db.runCommand( {addshard: 'localhost:10000'})
mongos> db.runCommand( {addshard: 'localhost:20000'})
mongos> db.runCommand( {addshard: 'localhost:30000'})

mongos> use persondb
mongos> db.persons.insert({name: 'Kim'})
monbos> db.persons.insert({name: 'Park'})
~~~

~~~sql
7번째 창
mongos> mongo
mongos> use admin
mongos> db.runCommand({enableSharding: 'persondb'})
mongos> db.runCommand({shardCollection:'persondb.persons'.key:{_id:1}})
mongos>
~~~

~~~sql
6번째 창
mongos> for (var i = 0; i < 1000000; i++) {db.persons.insert({i: i+1});}
~~~


# mongodb 실행

~~~sh
$ sudo service mongod start
$ mongo
~~~

# 1. mongodb root 계정 설정

~~~sh
> show databases;

> use admin;

> db;
~~~

~~~sh
> db.createUser({user: "아이디", pwd: "비밀번호", roles:["root"]});
> exit;
~~~

# 2.mongodb config 변경

~~~sh
> sudo vi /etc/mongod.conf
~~~

~~~
# mongod.conf
# ...

net:
  port: 27017
  bindIp: 0.0.0.0

# ...

security:
  authorization: 'enabled'

# ...
~~~

~~~
> sudo service mongod restart
~~~

# 3. root 계정으로 접속

~~~
> mongo

> show databases;

>exit
~~~

~~~
> mongo -u 아이디 -p 비밀번호
> show databases;
~~~

# 4. database, user 생성

~~~
> use chatbot_service;

> db;

> db.createUser({user: "developer", pwd:"developer", roles:["readWrite"]});
~~~

# 6. 외부 접속

~~~
> mongo -u developer -p developer public_ip:27017/chatbot_service
> db;
~~~


# 원격 접속 설정
## 사용자 생성

~~~shell
use admin # admin 데이터베이스 선택

db.createUser({
  user: "dbadmin",  # 계정 이름
  pwd: "password",  # 비밀번호
  roles: [          # 사용자에게 주어진 권한 목록. 여러 데이터베이스에 대한 권한을 할당할 수 있다.
    {
      role: "root", # built-in 권한인 root. 문자 그대로 모든 데이터베이스를 관리할 수 있다.
      db: "admin"   # 어떤 데이터베이스에 대한 권한인지 명시
    }
  ]}
)
~~~


~~~shell
db.createUser({
  user: "rhostem",
  pwd: "password",
  roles: [
    {
      role: "readWrite", # 읽기, 쓰기 권한
      db: "test"         # 위의 권한을 부여할 데이터베이스로 test를 지정
    }
  ]}
)
~~~



## mongod.conf 파일 설정

~~~
$ service mongod status
~~~

~~~shell
security:
  authorization: enabled

net:
port: 27017
bindIp: 0.0.0.0
~~~


## TCP 포트 개방
- 기본 포트 27107

## 접속 명령어

~~~
$ sudo mongo -u dbadmin -p --authenticationDatabase admin

$ mongo -u dbadmin -p --host <아이피 또는 도메인> --authenticationDatabase admin
~~~

# 기타설정
## 시스템 재시작 시 MongoDB를 자동으로 실행하도록 설정

~~~
$ crontab -e
~~~

맨 아래쪽에 재부팅 시점에 실행할 명령어를 추가한다.

~~~
@reboot sudo service mongod start
~~~

## 데이터베이스 접속을 위한 쉘 스크립트 작성
- git 저장소에서 작업하고 있다면 쉘 스크립트에 비밀번호가 노출되면 안 되므로 별도에 파일에 저장해서 관리하는 편이 보안상 좋다. 예를들어 비밀번호를 .dbpasswd 파일에 저장했다면 해당 파일은 .gitignore 파일에 등록하고 쉘 스크립트를 아래와 같이 작성한다.

~~~shell
# connect-db.sh

passwd=$(<.dbpasswd) # 변수 passwd에 .dbpasswd 파일의 내용을 할당.
mongo -u rhostem -p $passwd --host db.host.name --authenticationDatabase admin
~~~
