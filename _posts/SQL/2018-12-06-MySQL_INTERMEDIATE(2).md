---
layout: post
title: '[MySQL] INTERMEDIATE (2)'
category: SQL
tags: [sql, sqld]
comments: true
---

# 7. INDEX
- 테이블에서 데이터를 검색할때 빠르게 찾을 수 있도록 해주는 기능
- WHERE 절에 들어가는 컬럼을 INDEX로 설정해 놓으면 설정할 컬럼을 조건으로 검색할 때 빠르게 검색할 수 있음
- 자주 검색하는 조건은 INDEX로 설정해 놓으면 좋지만 너무 많은 인덱스가 설정되게 되면 데이터가 입력될때마다 INDEX에 데이터를 넣어야 함으로 데이터 입력시 속도가 느려질 수 있음
- 그러므로 인덱스는 검색 조건으로 자주 사용하는 컬럼에 설정해 놓으면 좋음

## 7.1 syntax

```sql
CREATE INDEX 인텍스이름
ON 테이블이름 (컬럼이름1, 컬럼이름2)
```

## 7.2 example

```sql
-- city 테이블에 Population 컬럼을 인덱스로 추가
CREATE INDEX Population
ON city (Population)

-- city 테이블에 Population 인덱스 제거
DROP INDEX Population
ON city
```

## 7.3 explain
- query를 실행하기 전에 index로 검색을 하는지 확인할 수 있음

```sql
-- 100만이 넘는 도시의 데이트를 출력의 실행계획을 확인
EXPLAIN
SELECT *
FROM city
WHERE population > 1000000
```

- Extra 컬럼에 Index가 없으면 Using where로 검색이 되지만 Index가 있으면 Using index로 검색됨을 확일할 수 있음
- Type 컬럼의 값이 ALL이면 Index를 사용하지 않고 있다는 의미

- 검색절차 : 스토리지(Index + Data) -> (row) -> Mysql 엔진(필터링 전) -> (filtered) -> Mysql 엔진 (필터링후)


## 7.4 emplyees의 salaries로 테스트
- employees.sql을 데이터 베이스에 저장
- employee.sql 파일을 strp를 이용하여 ubuntu 서버로 이동
- ubuntu 서버에서 musql 로그인

```
$ mysql -u root -p
mysql> create database employee;
mysql> use employees;
mysql> source employees.sql
```

```sql
-- employees 데이터 베이스로 이동
USE employees

-- salaries 테이블에 fdate 인덱스 제거
DROP INDEX fdate
ON salaries

-- 실행계획 확인 및 쿼리 실행
EXPLATIN
SELECT *
FROM salaries
WHERE from_date<'1985-02-01'

-- 여러개의 조건을 인덱스로 설정
-- 여러개의 조건이 올때는 여러개의 컬럼을 함께 인덱스로 사용하는게 좋음
CREATE INDEX ftdate
ON salaries (from_date, to_date)

-- salaries 테이블에 ftdate 인덱스 제거
DROP INDEX fdate
ON salaries

-- 실행계획 확인 및 쿼리 실행
EXPLAION
SELECT *
FROM salaries
WHERE from_date<'1985-02-01' and to_date<'1990-01-01'
```

## 8. View
- 가상 테이블로 특정 데이터만 보고자 할때 사용함
- 실제 데이터를 저장하고 있지는 않음
- 한번 생성된 뷰는 수정이 불가능하며 인덱스 설정이 불가능함

## 8.1 syntax

```sql
CREATE VIEW <뷰이름> AS
(QUERY)
```

## 8.2 example

```sql
-- 국가코드와 국가이름이 있는 뷰 생성
CREATE VIEW code_name AS
SELECT code, name
FROM country

-- city 테이블에 국가 이름 추가
SELECT *
FROM city
JOIN code_name
ON city.countrycode=code_name.code
```
