---
layout: post
title: '[MySQL] INTERMEDIATE (1)'
category: SQL
tags: [sql, sqld]
comments: true
---


# 1. CEIL, ROUND, TRUNCATE
- CEIL, ROUND, TRUNCATE는 소수점 올림, 반올림, 버림 함수임

## 1.1 CEIL
- CEIL은 실수 데이터를 올림할 때 사용

```sql
-- 12.345를 올림하여 정수로 나타냄
SELECT CEIL(12.345)

-- 국가별 언어 사용비율을 소수 첫번째자리에서 올림하여 정수로 나타냄
SELECT CountryCode, Language, Percentage, CEIL(Percentage) FROM countrylanguage
```

## 1.2. ROUND
- ROUND는 실수 데이트를 반올림할 때 사용

```sql
-- 12.345를 소수 둘째자리까지 나타내고 소수 셋째자리에서 반올림
SELECT ROUND(12.345, 2)

-- 국가별 언어 사용비율을 소수 첫번째 자리에서 반올림하여 정수로 나타냄
SELECT CountryCode, Language, Percentage, ROUND(Percentage, 0) FROM countrylanguage
```

## 1.3. TRUNCATE
- TRUNCATE는 실수데이터를 버림할 때 사용

```sql
-- 12.345를 소수 둘째자리까지 나타내고 소수 셋째자리에서 버림
SELECT TRUNCATE(12.345, 2)

-- 국가별 언어 사용비율을 소수 첫번째 자리에서 버림하여 정수로 나타냄
SELECT CountryCode, Language, Percentage, TRUNCATE(Percentage, 0) FROM countrylanguage

SELECT CountryCode, Language, Percentage, ROUND(Percetage, 0), TRUNCATE(Percentage, 0) FROM countrylagnuage
```


# 2. Conditional
- SQL에서도 다른 언어에서 처 조건문 사용이 가능함.

## 2.1 IF
- syntax : IF(조건, 참, 거짓)

```sql
-- 도시의 인구가 100만이 넘으면 'big city' 그렇지 않으면 'small city를 출력하는 city_scale 칼럼을 추가
SELECT name, population, IF(population > 1000000, 'big city', 'samll city') AS city_scale FROM city
```

## 2.2 IFNULL
- syntax : IFNULL(참, 거짓)

```sql
-- 독립연도가 없는 데이터는 0으로 출력
SELECT IndepYear, IFNULL(IndepYear, 0) as IndepYear FROM country
```

## 2.3 CASE
- syntax

```sql
CASE
  WHEN (조건1) THEN (출력1)
  WHEN (조건2) THEN (출력2)
END AS (컬럼명)
```

```sql
-- 나라별로 인구가 10억 이상, 1억이상, 1억 이하인 컬럼을 추가하여 출력
SELECT name, population,
  CASE
    WHEN population > 1000000000 THEN 'upper 1 bilion'
    WHEN populat4444444444444444444444on > 100000000 THEN 'upper 100 milion'
    ELSE 'below 100 milion'
  END AS result
FROM country
```


# 3. DATA_FORMAT
- DATE_FORMAT은 날짜 데이터에 대한 포맷을 바꿔줌

```sql
SELECT DATE_FORMAT(payment_date, '%Y-%m') AS mounthly, SUM(amount) AS amount
FROM paymnet
GROUP BY monthly
```

# 4. MAKE TEST TABLE & DATA
- JOIN은 여러개의 테이블에서 데이터를 모아서 보여줄 때 사용. JOIN에는 INNER JOIN, LEFT JOIN, RIGHT JOIN이 있음

## 4.1 MAKE TEST TABLE & DATA

```sql
-- create table & data
CREATE TABLE user (
  user_id int(11) unsigned NOT NULL AUTO_INCREMENT,
  name varchar(30) DEFAULT NULL,
  PRIMARY KEY (user_id)
) ENGINE=InnoDB DEFAULT CHARSET=uft8;

CREATE TABLE addr (
  id int(11) unsigned NOT NULL AUTO_INCREMENT,
  addr varchar(30) DEFAULT NULL,
  user_id int(11) DEFAULT NULL,
  PRIMARY KEY(id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

INSERT INTO user(name)
VALUES ('jin'),
('po'),
('alice'),
('petter');

INSERT INTO addr(addr, user_id)
VALUES ('seoul', 1),
('pusan', 2),
('deajeon', 3),
('deagu', 5),
('seoul', 6);
```

## 4.2 INNER JOIN
- 두 테이블 사이에 공통된 값이 없는 row는 출력하지 않음

```sql
-- 두 테이블을 합쳐 id, name, addr 출력
SELECT id, user.name, addr.addr
FROM user
JOIN addr
ON user.user_id = addr.user_id

-- world 데이터베이스에서 도시이름과 국가이름을 출력
SELECT country.name AS city_name, city.name AS country_name
FROM city
JOIN country
ON city.CountryCode = country.code
```

## 4.3 LEFT JOIN
- 왼쪽 테이블을 기준으로 왼쪽 테이블의 모든 데이터가 축적되고 매핑되는 키값이 없으면 NULL을 출력함

```sql
-- 두 테이블을 합쳐 id, name, addr 출력
SELECT id, user.name, addr.addr
FROM user
LEFT JOIN addr
ON user.user_id = addr.user_id
```

## 4.4 RIGHT JOIN
- 오른쪽 테이블을 기준으로 왼쪽 테이블의 모든 데이터가 출력되고 매핑되는 키값이 없으면 NULL을 출력함

```sql
-- 두 테이블을 합쳐 id, name, addr 출력
SELECT id, user.name, addr.addr
FROM user
RIGHT JOIN addr
ON user.user_id = addr.user_id
```

# 5. UNION
- UNION은 SELECT문의 결과 데이터를 하나로 합쳐서 출력함
- 컬럼의 갯수와 타입 순서가 같아야 함
- UNION은 자동으로 distinct를 하여 중복을 제거해 줌
- 중복제거를 안하고 컬럼데이터를 합치고 싶으면 UNION ALL을 사용함
- 또한 UNION을 이용하면 Full Outer Join을 구현할 수 있음

## 5.1 UNION

```sql
-- user 테이블의 name 칼럼과 addr의 데이터를 하나로 합쳐서 출력
SELECT name
FROM user
UNION
SELECT addr
FROM ADDR
```

## 5.2 UNION ALL

```sql
-- 중복데이터를 제거하지 않고 결과 데이터 합쳐서 출력
SELECT name
FROM user
UNION ALL
SELECT addr
FROM addr
```

## 5.3 FULL OUTER JOIN
- union을 이용하여 full outer join 구현

```sql
-- union을 이용하여 full outer join 구현
SELECT id, user.name, addr.addr
FROM user
LEFT JOIN addr
ON user.user_id = addr.user_id
UNION
SELECT id, user.name, addr.addr
FROM user
RIGHT JOIN addr
ON user.user_id = addr.user_id
```

# 6. Sub-Query
- sub query는 query 문 안에 있는 query를 의미함. SELECT절 FROM절, WHERE 등에 사용이 가능함

```sql
-- 전체 나라수, 전체 도시수, 전체 언어수를 출력 (SELECT 절에 사용)
SELECT
  (SELECT count(name) FROM city) AS total_city,
  (SELECT count(name) FROM country) AS total_country,
  (SELECT count(DISTINCT(Language)) FROM countrylanguage) AS total_language
FROM DUAL

-- 800만 이상도는 도시의 국가코드, 이름, 도시인구수를 출력 (FROM 절에 사용)
SELECT *
FROM
  (SELECT coutrycode, name, population
  FROM city
  WHERE population > 8000000) AS CITY
JOIN
  (SELECT code, name
  FROM country) AS coungry
ON city.countrycode = country.code

-- 800만 이상 도시의 국가코드, 국가이름, 대통령이름을 출력(WHERE 절에 사용)
SELECT code, name, HeadOfState
FROM country
WHERE code IN (
  SELECT DISTINCT(countrycode) FROM city WHERE population > 8000000
)
```
