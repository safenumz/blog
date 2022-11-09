---
layout: post
title: '[Oracle] Sequence Index'
category: SQL
tags: [sql, oracle, sequence]
comments: true
---

# 시퀀스 (Sequence)
- 자동증가수

~~~sql
CREATE SEQUENCE sequence_name
	[ minvalue 1                  -- 시퀀스 최소 숫자
	maxvalue 999999999999 -- 시퀀스 최대 숫자
	increment by 1             -- 증가치
	start with 1                  -- 시작숫자
	nocache                      -- cache를 사용하면 미리 값을 할당하여 조금 빠르게 접근
	noorder                      -- 요청되는 순서대로 값 할당
	nocycle ];                    -- 다시 시작할지 여부
~~~

~~~sql
-- 시퀀스(auto_increment)
DROP SEQUENCE seq_emp_empno;
CREATE SEQUENCE seq_emp_empno start with 5000;

-- 홍은 5000번 사번으로 들어간다.
INSERT INTO emp(empno, ename, deptno) VALUES(seq_emp_empno.nextval, '홍', 10);
SELECT * FROM emp;

-- 홍2는 5001번 사번으로 들어간다.
INSERT INTO emp(empno, ename, deptno) VALUES(seq_emp_empno.nextval, '홍2', 10);
SELECT * FROM emp;

-- 현재 몇번까지 들어가 있는지 확인
SELECT seq_emp_empno.currval FROM dual;
~~~

## 시퀀스에 사용되는 의사 컬럼
- CURRVAL: 현재 시퀀스 값
- NEXTVAL: 다음 생성될 시퀀스 값

- 의사컬럼(Pseudocolumns): 테이블에 있는 컬럼처럼 사용되지만 실제로 저장되어 있지 않은 컬럼


# 인덱스 (Index)
- 빠른 검색을 위해 메모리 상주

## [ 참고 ]  rowid 확인

~~~sql
-- rownum, rowid
SELECT rownum empno, ename, job, rowid 
FROM emp;
~~~

- 별로로 인덱스 컬럼과 ROWID 정보를 관리하고 이 정보를 먼저 찾아 실제 테이블에 있는데이터를 검색
- 테이블에 새로운 행이 입력되거나 변경 제거되면 인덱스 정보도 갱신된다.
- 너무 많은 인덱스나 테이블의 데이터가 적은 경우는 성능을 저하
- PRIMARY KEY와 UNIQUE KEY는 자동으로 UNIQUEINDEX로 자동 생성

~~~sql
CREATE UNIQUE INDEX emp_email_uk ON emp( email );
CREATE INDEX emp_deptno_ix ON emp(deptno, ename);
~~~

- unique index는 중복값을 허용하지 않음

## [ 확인 ] hr 계정에서 - Sql Developer에서 F6으로 autotrace로 실행

~~~sql
SELECT employee_id, first_name, last_name,phone_number 
FROM employees
WHERE salary=3000;  -> FULL SCAN   COST(3)
~~~
 

## [ 에러 ] 데이터베이스 관리자로부터 카탈로그 읽기 권한을 얻으십시오.

~~~shell
$ sqlplus “/as sysdba”
~~~

~~~shell
grant SELECT_CATALOG_ROLE to HR
grant SELECT ANY DICTIONARY to HR
~~~
 

## [ 확인 ] 인덱스 생성후 F6으로 확인

~~~sql
CREATE UNIQUE INDEX emp_name_idx ONemployees(  salary );
SELECT employee_id, first_name, last_name,phone_number 
FROM employees
WHERE salary=3000;        -> INDEX (RANGE SCAN)
~~~
 

## [ 인덱스 검색 ]
- 인덱스가 생성되어 메모리에 올라가면 검색 속도가 빨라진다.

~~~sql
SELECT  index_name, index_type, uniqueness         
FROM   user_indexes
WHERE  table_name=’EMPLOYEES’;
~~~

~~~sql
SELECT employee_id, first_name, last_name, phone_number FROM employees
WHERE salary=100;

SELECT employee_id, first_name, last_name, phone_number FROM employees
WHERE salary=3000;

-- 메모리에 인덱스가 안올라왔기 때문에 cost 3
SELECT employee_id, first_name, last_name, phone_number FROM employees
WHERE phone_number='515.123.4567';

-- 메모리에 올라와있는 인덱스 확인
SELECT index_name, index_type FROM user_indexes
WHERE table_name='EMPLOYEES';

-- 메모리에 인덱스가 올라와 있기 때문에 cost 1
SELECT employee_id, first_name, last_name, phone_number FROM employees
WHERE last_name='King';

-- 전화 번호 메모리에 올림
CREATE INDEX emp_phone_ix ON employees(phone_number);

-- cost3에서 cost 1로 변경
SELECT employee_id, first_name, last_name, phone_number FROM employees
WHERE phone_number='515.123.4567';
~~~

## 인덱스 종류
- unique 인덱스
- non-unique 인덱스

- 단일 인덱스 : 컬럼 한 개
- 복합 인덱스 : 2개 이상의 컬럼

- 수동인덱스 : 사용자가 create index로생성
- 자동인덱스 : unique나 primary key 생성시

 
## 인덱스에 대한 가이드 라인
- 자주 조회되는 컬럼을 인덱스 컬럼으로 선택
- 데이터 량이 많은 테이블에서 15% 이하의 데이터를 조회할 경우
- 검색속도가 빠를 수 있음
- 조인에 사용된 컬럼을 인덱스 컬럼으로 하면 조인 성능이 향상
- 테이블 데이터가 적은 경우는 직접 테이블 검색이 더 빠름
- 삽입, 갱신, 삭제가 많이 발생하는 테이블에서는인덱스로 성능이 저하될 수 있음

## 인덱스 생성을 위한 지침
###  많은 것이 항상 더 좋은 것은 아니다.
- 인덱스를 가지고 있는 테이블에 대한 각 DML 작업은 인덱스도 갱신되어야 함을 의미한다.
- DML 후에 모든 인덱스를 갱신하기 위해 많은 작업을 한다

### 언제 인덱스를 생성하는가?
- 열은 WHERE 절 또는 조인 조건에서 자주 사용한다
- 열은 광범위한 값을 포함한다
- 열은 많은 수의 NULL 값을 포함한다
- 둘 또는 이상의 열은 WHERE 절 또는 조인 조건에서 자주 함께 사용한다
- 테이블은 대형이고 대부분의 질의들은 행의 2~4%보다 적게 읽어 들일 것으로 예상한다

### 언제 인덱스를 생성해서는 안되는가?
- 테이블이 작다
- 옆이 질의의 조건으로 자주 사용되지 않는다
- 대부분의 질의들은 행의 2~4% 이상을 읽어 들일 것으로 예상한다
- 테이블은 자주 갱신한다.

