---
layout: post
title: '[Oracle] Partition'
category: SQL
tags: [sql, oracle, partition]
comments: true
---


# Partition

~~~shell
# 파이션 파일 생성
SQL> CREATE TABLESPACE p1 datafile '/u01/app/oracle/oradata/XE/p1.DBF' size 5m;
SQL> CREATE TABLESPACE p2 datafile '/u01/app/oracle/oradata/XE/p2.DBF' size 5m;
SQL> CREATE TABLESPACE p3 datafile '/u01/app/oracle/oradata/XE/p3.DBF' size 5m;
~~~

~~~sql
-- 테이블 생성
CREATE TABLE sawon_p
(   sabun number(4),
    saname varchar2(20),
    job varchar2(20),
    sal number
)

-- 파티션 생성
partition by range(sabun)
(
    partition sawon_p1 values less than (2000) tablespace p1,
    partition sawon_p2 values less than (4000) tablespace p2,
    partition sawon_p3 values less than (8000) tablespace p3
);

INSERT INTO sawon_p VALUES(101, '삼순이', '개발', 5000);
INSERT INTO sawon_p VALUES(101, '사순이', '디자인', 5000);
INSERT INTO sawon_p VALUES(101, '오순이', '디자인', 5000);

SELECT * FROM sawon_p;

-- 테이블에 일부 파티션 p2만 검색된다.
SELECT * FROM sawon_p partition(sawon_p2);
~~~

