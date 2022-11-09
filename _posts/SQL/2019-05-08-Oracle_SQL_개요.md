---
layout: post 
title: '[Oracle] 개요'
category: SQL
tags: [oracle, sql]
comments: true
---

# SQL
- Structured Query Language
- 데이터베이스에서 데이터를 조회, 입력, 수정, 삭제하는데 사용하는 언어
- DDL (Data Definition Language) : 데이터 정의 언어
- DML (Data Manipulation Language) : 데이터 조작 언어
- DCL (Data Control Language) : 데이터 제어 언어

## 오라클 기본 셋팅
- 제어판 > 서비스
- 필요없는 것들은 중지한다.
- Oracle ORCL VSS Writer Service: 중지, 수동
- OracleDBConsoleorcl : 중지, 수동
- OracleMTSRecoveryService : 중시, 수동

### 용어설명
- data/ column, row / table / database

### SQL 문장 작성법
- 대소문자 구별안함
- [ 권장 ] 키워드는 대문자 / 테이블명, 열이름 등은 소문자
- 한줄 또는 여러 줄에 입력 가능
- [ 권장 ] 보기 편하게 여러줄로나누고 탭과 들여쓰기
- 명령어의 끝에 ; (세미콜론) 표시하여 SQL 문장 실행

### 이름 지정 규칙 – 테이블명 / 컬럼명
- 문자,  _ , $ , # 조합 (한글도 가능)
- 첫글자는 문자로 시작
- 예약어 사용안됨
- 길이 제한 ( 1 ~ 30 )

### (1) DML (Data Manipulation Language)
###  INSERT : 입력

~~~sql
INSERT INTO table_name(columns) VALUES (values);
~~~

###  UPDATE : 수정

~~~sql
UPDATE table_name SET column=value WHERE condition;
~~~

###  DELETE : 삭제

~~~sql
DELETE FROM table_name WHERE condition;
~~~

###  SELECT : 검색

~~~sql
SELECT columns FROM table_name WHERE condition;
~~~
 

### (2) DDL (Data Definition Language)

###  CREATE

~~~sql
CREATE TABLE  table_name ( [column_name  data_type] );
~~~

- DROP

~~~sql
DROP TABLE  table_name [CASCADE     CONSTRAINT];
~~~

###  ALTER

~~~sql
ALTER   TABLE  table_name 
ADD ( [ column_name  data_type ] );
MODIFY( [ column_name  data_type ] );
DROP( [ column_name ] );
~~~

### TRUNCATE 
- 테이블에 있는 데이터들을 삭제
- DELETE : 데이터를 삭제시 rollback으로 복구 가능
- TRUNCATE : 삭제하면 복구할 수 없음

 

### (3) DCL (Data Control Language)
- 데이터베이스에 있는 데이터에 접근을 제어하는 언어
- GRANT : 접근제어나 어떤 작업을 허용하는 권한을 주는 역할
- REVOKE : 권한을 박탈


~~~sql
select * from emp;

select * from dept;

-- hr 계정 접속
conn hr/hr

select * from employees;

-- 부서 검색
select * from departments;
~~~

~~~sql
-- sql (DLL / DML / DCL)
-- DDL
select * from emp;

CREATE TABLE emp_copy AS
SELECT * FROM emp;

-- [ 연습문제 ] emp 전체emp_copy 복사본 테이블 생성 후
SELECT * FROM emp_copy;

-- 1. 사원번호가 7782인 사원의 부서를 10번으로 변경
UPDATE emp_copy SET deptno=30 WHERE empno=7782;

-- 2. 사원번호가 7782인 사원의 이름을 홍길숙으로 변경하고 급여를 3500으로 변경
UPDATE emp_copy SET ename='홍길숙', sal=3500 WHERE empno=7782;

-- 3. 모든 부서원의 보너스를 300씩 인상 변경
-- nvl null 값을 만나면 0으로 변경후 300 더함
UPDATE emp_copy SET comm=nvl(comm, 0)+300;

-- 4. 사원번호가 7499인 사원의 정보를 삭제
DELETE FROM emp_copy WHERE empno=7499;

-- 5. 입사일자가 81년 6월 1일 이전인 사원의 정보를 삭제
DELETE FROM emp_copy WHERE hiredate<'81/06/01';

-- 6. 입사(사번 : 8000, 사원명 : 본인명, 업무 : CLERK, 월급 : 5000)
INSERT INTO emp_copy(empno, ename, job, sal) 
VALUES (8000, 'JASON', 'CLERK', 5000);

DELETE FROM emp_copy;

DROP TABLE emp_copy;
~~~

## 2. 테이블 만들기
### (0) 이름 지정 규칙
- 문자,  _ , $ , # 조합 (한글도 가능)
- 첫글자는 문자로 시작
- 예약어 사용안됨
- 길이 제한 ( 1 ~ 30 )

### (1) 기본 데이터 타입

VARCHAR2(n) | 가변 길이 문자 데이터 (4000byte)
CHAR(n) | 고정 길이 문자 데이터 (2000byte)
NUMBER(p,s) | 전체 p 자리 중 소수점 이하 s 자리
DATE | 날짜형
LONG | 가변 길이 문자 데이터 (2Gbyte)
BLOB | 가변 길이 이진 데이터 (4Gbyte)
CLOB | 단일 바이트 가변 길이 문자 데이터 (4Gbyte)

* varchar도 현재는 varchar2와 동일하지만,
- 추후에 오라클에서 별도의 데이터 타입으로 지정한다고 사용하지 말라고 권고한다.

* 한글은 2bytes
- number ( 전체자릿수, 소수점 이하 자릿수) : 지정된자릿수를 맞추기 위해 반올림됨
- timestamp : date보다 정밀

### (2) 테이블 연습
- create / alter / drop
- desc
- insert / update /delete / select
- commit / rollback

#### 테이블명: student

학번 | id | char(4)
학생이름 | name | varchar2(10)
국어점수 | kor | number(3)
수학점수 | math | number(3)
총점 | total | number(3)
평균 | sum | number(5 ,2)

~~~sql
-- student 테이블 생성
CREATE TABLE student (
    id char(4),
    name varchar2(10),
    kor number(3),
    math number(3),
    avg number(5 ,2),
    total number(5 ,2)
    );
    
-- 테이블 구조 확인
DESC student;

-- 데이터 (레코드) 확인
SELECT * FROM student;

-- 영어점수 컬럼 추가 (eng number(3))
ALTER TABLE student ADD (eng number(3));

-- 총점 컬럼 삭제
ALTER TABLE student DROP (total);

-- 평균 컬럼에서 소숫점 1자리 변경
ALTER TABLE student MODIFY(avg number(4 ,1));

-- 데이터 입력
INSERT INTO student VALUES ('8001', '홍길순', 100, 80, 50, 0);

INSERT INTO student(id, name, kor, math, eng, avg) VALUES ('8088', '홍길동', 55, 66, 88, 0);

-- 홍길동 학생의 평균값을 입력하세요.
UPDATE student SET avg=(kor+math+eng)/3 WHERE name='홍길동';

-- 데이터 (레코드) 확인
SELECT * FROM student;
~~~

~~~
1. 오라클 IP 접속
--> SQLDeveloper에서 localhost -> 본인컴퓨터IP

2. 사용자계정 만들기
- 각 조의 한개의 계정 (ourjo / 1234)
~~~

~~~
* SQL
- DDL : 테이블 관련 정의
    - CREATE / DROP / ALTER

- DML : 데이타 조작
    - INSERT / DELETE / UPDATE / SELECT

- DCL : 권한
    - GRANT / REVOKE

* 목표
1. DDL + 제약조건
2. DML (SELECT)
~~~
