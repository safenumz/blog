---
layout: post
title: '[Oracle] 제약조건'
category: SQL
tags: [sql, oracle]
comments: true
---

# 제약조건

NOT NULL    | null 값을 허용하지 않음 ( 컬럼레벨 방식으로만 적용 )
UNIQUE      | 유일하게 식별하는 값만 허용 -> 중복허용안함 
^           | ( PRIMARY KEY와 유사하나 null값 허용됨 )
PRIMARY KEY | null값 허용하지 않고 유일하게 식별 
^           | ( UNIQUE + NOT NULL )
^           | UNIQUE INDEX 자동생성
FOREIGN KEY | 다른 테이블의 PK를 참조
CHECK       | 제한적인 입력처리
(*)DEFAULT  | 제약조건은 아니지만, 입력값이 없을 때 디폴트 설정값으로 자동 입력됨

 

## [1] PRIMARY KEY 지정시

~~~sql
-- 컬럼 레벨 제약조건 방식
CREATE TABEL  table_name (
    column_name  data_type [ CONSTRAINT pk_name ]  PRIMARY KEY
);
~~~

* 모든 제약 조건은DATA DICTIONARY에 저장하는데, 제약 조건 이름을 의미 있게 부여하면 참조하기 쉽기에 규칙적인 제약조건이름을 권장한다.
- ex) pk_tablename_columnname 또는 tablename_columnname_pk

 
~~~sql
-- 테이블 레벨 제약조건 방식
CREATE TABEL table_name (
    column_nam  data_type,
    CONSTRAINT pk_name PRIMARY  KEY  [ column_name ]
);
~~~
 
## [2] FOREIGNKEY 지정시

~~~sql
-- 컬럼 레벨 제약조건 방식
CREATE TABEL table_name (
    column_nam data_type 
	CONSTRAINT fk_name 
	REFERENCES ref_table_name  ( ref_column_name)
);
~~~

~~~sql
-- 테이블 레벨 제약조건 방식
CREATE TABEL  table_name (
    column_name  data_type,
    CONSTRAINT  fk_name FOREIGN  KEY (column_name)
	REFERENCES  ef_table_name ( ref_column_name )
);
~~~

~~~sql
-- 테이블 생성 후 제약조건을 추가한다면 -> 테이블 수정
ALTER  TABLE   table_name
ADD  CONSTRAINTS  pk_name  PRIMARY  KEY ( column_name );

ALTER  TABLE   table_name
ADD  CONSTRAINTS  fk_name  FOREIGN  KEY ( column_name )
REFERENCES  ref_table_name ( ref_column_name );
~~~
 

## [3] CHECK  지정시

~~~sql
gender  varchar2(10) NOT NULL
CONSTRAINT  check_gender CHECK ( gender  IN (‘남자’,’여자’) )
~~~
 
## [4] DEFAULT 지정시

~~~sql
-- DEFAULT는 NOT NULL 과 같이 기술시 앞에 기술
gendervarchar2(10) DEFAULT ‘남자’  NOT NULL
~~~
   

### 테이블 생성 후 제약조건을 추가한다면 -> 테이블 수정

~~~sql
ALTER TABLE table_name
MODIFY gender DEFAULT ‘남자’;
~~~
 

## 참고
### 제약조건 추가
- constraint_type :  Primary Key, Foreign Key,  Unique

~~~sql
ALTER TABLE table_name 
ADD CONSTRAINT constraint_name constraint_type( column );
~~~


### 제약조건 삭제

~~~sql
ALTER TABLE table_name 
DROP CONSTRAINT constraint_name [CASCADE];
~~~
 

### 제약조건 (비)활성화

~~~sql
ALTER TABLE table_name 
DISABLE/ENABLE CONSTRAINT constraint_name [CASCADE];
~~~
 
### 제약조건 확인

~~~sql
SELECT constraint_name, constraint_type  
FROM user_constraints
WHERE table_name=’대문자로 테이블명’;
~~~

## [ 연습문제 ]

---
### 1. sawon 테이블을 생성하세요.
- sabun은 6자리 숫자
- sawon_name은 최대 한글 5자리
- ubmu는최대 한글 10자리
- weolgub는정수형 8자리와 소수점 2자리
- buseo는숫자 3자리

~~~sql
CREATE TABLE sawon (
    sabun char(6),
    sawon_name varchar2(10),
    ubmu varchar2(20),
    weolgub number(10 ,2),
    buseo number(3)
);
~~~

### 2. 위의 테이블에서 sabun을 기본키로 설정

~~~sql
ALTER TABLE sawon
ADD CONSTRAINT pk_sawon_sabun PRIMARY KEY (sabun);
~~~

### 3. 최대 한글 10자리의 jiyeok 컬럼을 추가하되 NULL 값이 입력되지 않도록 지정

~~~sql
ALTER TABLE sawon
ADD jiyeok varchar2(20) NOT NULL;
~~~
 
### 4. weolgub 컬럼은 정수형 7자리로 변경

~~~sql
ALTER TABLE sawon
MODIFY weolgub number(7);
~~~
 
### 5. ubmu컬럼은 ‘개발’, ‘유지보수’, ‘관리’ 만데이터 값으로 입력되도록 지정

~~~sql
ALTER TABLE sawon 
ADD CONSTRAINT sawon_ubmu 
CHECK (ubmu IN ('개발', '유지보수', '관리'));
~~~
 
### 6. ubmu컬럼에 데이터가 입력이 안될 경우 디폴드값으로 ‘개발’을 설정

~~~sql
ALTER TABLE sawon
MODIFY ubmu DEFAULT '개발';
~~~
 
### 7. buseo 테이블을 생성하세요
- buseo_no는숫자 3자로 이루어진 번호로 기본키
- buseo_name은최대 한글 10자리

~~~sql
CREATE TABLE buseo (
    buseo_no number(3) PRIMARY KEY,
    buseo_name varchar2(20)
);
~~~

### 8. sawon 테이블의 buseo컬럼을 buseo 테이블의 buseo_no 를참조하는 외래키로 설정

~~~sql
ALTER TABLE sawon 
ADD CONSTRAINT fk_sawon_buseo
FOREIGN KEY (buseo) REFERENCES buseo(buseo_no);
~~~

### 9. buseo 테이블에 데이터 입력

buseo_no  | buseo_name
:--------:|:--------:
101       | 소프트웨어유지보수부
202       | 관리부
303       | 인적자원부

~~~sql
INSERT INTO buseo(buseo_no, buseo_name) VALUES (101, '소프트웨어유지보수부');
INSERT INTO buseo(buseo_no, buseo_name) VALUES (202, '관리부');
INSERT INTO buseo(buseo_no, buseo_name) VALUES (303, '인적자원부');
~~~

### 10. sawon 테이블에 데이터 입력 (입력이 안될 경우도 있음)

sabun | sawon_name | ubmu   | weolgub  | buseo | jiyeok
:----:|:----------:|:------:|:--------:|:-----:|:-----:
8001  | 홍길동이군    |        | 100000   | 101   | 부산 
8002  | 홍길자       | 사무    | 200000   | 202   | 인천
8003  | 홍길순       | 개발    |          | 111   | 대전
8004  | 홍길석       |        | 12345678 |       | 서울 
8005  | 홍길철       | 유지보수 |          | 303   | 판교

~~~sql
INSERT INTO sawon(sabun, sawon_name, weolgub, buseo, jiyeok) 
VALUES ('8001', '홍길동이군', 100000, 101, '부산');
INSERT INTO sawon(sabun, sawon_name, ubmu, weolgub, buseo, jiyeok) 
VALUES ('8002', '홍길자', '사무', 200000, 202, '인천');
INSERT INTO sawon(sabun, sawon_name, ubmu, buseo, jiyeok) 
VALUES ('8003', '홍길순', '개발', 111, '인천');
INSERT INTO sawon(sabun, sawon_name, weolgub, jiyeok) 
VALUES ('8004', '홍길석', 12345678, '서울');
INSERT INTO sawon(sabun, sawon_name, ubmu, buseo, jiyeok) 
VALUES ('8005', '홍길철', '유지보수', 303, '판교');
~~~


### 11. sawon 테이블에서 jiyeok 컬럼을 제거

~~~sql
ALTER TABLE sawon DROP (jiyeok);
~~~

### 12. buseo 테이블에서 buseo 명이 ‘인적자원부’인 행을 삭제

~~~sql
-- 부서 테이블을 참조하고 있어서 삭제 불가능
DELETE FROM buseo WHERE buseo_name='인적자원부';
~~~

### 13. sawon 테이블이 모든 내용을 삭제하고 저장공간을 해제

~~~sql
TRUNCATE TABLE sawon;
~~~

### 14. 1~8 단계까지를 하나의CREATE 문으로 작성

~~~sql
CREATE TABLE buseo (
    buseo_no number(3),
    buseo_name varchar2(20),
    CONSTRAINT pk_buseo_no PRIMARY KEY(buseo_no)
);

CREATE TABLE sawon (
    sabun char(6),
    sawon_name varchar2(10),
    ubmu varchar2(20) DEFAULT '개발',
    weolgub number(7),
    buseo number(3),
    jiyeok varchar2(10) NOT NULL,
    CONSTRAINT sawon_pk PRIMARY KEY (sabun),
    CONSTRAINT sawon_ubmu CHECK (ubmu IN ('개발', '유지보수', '관리')),
    CONSTRAINT sawon_fk FOREIGN KEY (buseo) REFERENCES buseo(buseo_no)
    ON DELETE CASCADE
);
~~~

---