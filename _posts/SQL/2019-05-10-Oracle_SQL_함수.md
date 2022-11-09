---
layout: post
title: '[Oracle] 함수'
category: SQL
tags: [sql, oracle, 함수]
comments: true
---

# SQL 함수
- 함수를 사용하면 alias는 필수

## 1. 숫자형 함수

ABS(n)        | 절대값
SIGN(n)       | 양수(->1), 음수(->-1), 0(->0)을 구분
ROUND(n, i)   | 숫자 n을 소수점 i자리에서 반올림
TRUNC(n, i)   | 숫자 n을 소수점 i자리에서 자름
CEIL(n)       | 가장 큰 정수
FLOOR(n)      | 가장 작은 정수
MOD(n2, n1)   | n2에서 n1을 나눈 나머지 연산
POWER(n2, n1) | n2의 n1 제곱값
SQRT(n)       | n의 제곱근의 값

~~~sql
-- 사원명, 급여, 월급(급여/12)를 출력하되 월급은 십단위에서 반올림하여 출력
-- 한글은 "" 써야 함
SELECT ename, sal, round(sal/12, -2) "월급"
FROM emp;
 

-- 사원명, 급여, 세금(급여의 3.3%)를 원단위 절삭하고 출력
SELECT ename, sal, trunc(sal*0.033, -1) AS TAX FROM emp;
~~~

## 2. 문자형 함수

LOWER(str)                    | 알파벳을 소문자로 변환
UPPER(str)                    | 알파벳을 대문자로 변환
INITCAP(str)                  | 첫번째 글자만 대문자로 변환
CONCAT(str1, str2)            | 두 문자열을 연결
SUBSTR(str, i, n)             | 문자열 중 i번째에서 n개의 일부 문자를 선택
INSTR(str, ch, n, i)          | 문자열 중에서 ch문자열이 n번째부터 i번쨰 있는 위치
LENGTH(str)                   | 문자열의 길이
LPAD / RPAD(str, n, ch)       | n자리수 만큼 확보 후 빈 공백에 특정 문자로 채움
TRIM / LTRIM / RTRIM          | 문자를 제거 양쪽 공백 제거에 사용
TRANSLATE(column, str1, str2) | 문자열에서 str1을 str2로 대체
REPLACE(column, str1, str2)   | 문자열에서 str1을 str2로 대신


~~~sql

SELECT
'-' || trim(' 이 순신 ')|| '-' as col1,
'-'|| ltrim(' 이 순신 ') || '-' as col2,
'-'|| rtrim(' 이 순신 ') || '-' as col3
from dual;

SELECT replace('  a   b c ', ' ', '') FROM dual;

-- TRANSLATE
-- 월급의 숫자에서 0을 ‘$’로 3을 &로 바꾸어 출력
SELECT sal, TRANSLATE(sal, 53, '$&') FROM emp;

-- REPLACE
-- 월급의 숫자에서 50을 $로 바꾸어 출력
SELECT REPLACE(sal, 50, '$') FROM emp;


-- smith의정보를 사원번호, 성명, 담당업무(소문자) 출력
SELECT empno, ename, lower(job) 
FROM emp
WHERE ename='SMITH';
          
-- 사원번호, 사원명(첫글자만 대문자), 담당업무(첫글자만대문자)로 출력
SELECT empno, INITCAP(ename) as ename, INITCAP(job) as job
FROM emp;
          
-- 이름의 첫글자가 ‘K’보다크고 ‘Y’보다 작은 사원의 정보( 사원번호, 이름, 업무, 급여, 부서번호)를 출력하되 이름순으로 정렬
SELECT empno, ename, job, sal, deptno
FROM emp
WHERE SUBSTR(ename, 1, 1)>'K' and SUBSTR(ename, 1, 1)<'Y'
ORDER BY ename;
        
-- 이름이 5글자 이상인사원들을 출력
SELECT *
FROM emp
WHERE LENGTH(ename)>=5;

-- 이름을 15자로 맞추고글자는 왼쪽에 오른쪽에는 ‘*’로 채운다
SELECT RPAD(ename, 15, '*') as ename
FROM emp;
 
-- 월급은 10자로 맞추고숫자는 오른쪽에 왼쪽엔 ‘-‘로 채운다
SELECT LPAD(sal, 15, '-') as ename
FROM emp;
 
-- 양쪽 공백을 제거
SELECT 
'-' || trim(' 이순신 ')|| '-' as col1,
'-'|| ltrim(' 이순신 ') || '-' as col2,
'-'|| rtrim(' 이순신 ') || '-' as col3 
FROM dual;
 

-- (*) dual : dummy 테이블로 sys user가 owner이고 모든 사용자가 사용가능.
-- 임의의 값을 알고자 할 경우 유용하게 사용하는 테이블.

 
-- 월급을 숫자에서 ‘영일이삼사오육칠팔구’ 글자로 대체

-- 월급의 숫자에서 0을‘$’로 바꾸어 출력
~~~

## 3. 날짜형 함수
- 시스템의 날짜를 가져오는 함수
- SYSDATE, CURRENT_DATE, SYSTIMESTAMP, CURRENT_TIMESTAMP
- 이 함수들은 사용수 ()가 필요없다.
- sysdate와 current_date의 차이는 current_date는 세션 시간을 따른다.
- 만약 한 명은 한국에서 한 명은 미국 본사 시스템에 접속시 각각 세션에 따른 시간을 확ㅇ니하게 됨

### 날짜 연산

연산                 | 결과        | 설명
:------------------:|:----------:|:--------------:
Date + number       | Date       | 일수에 날짜를 더함
Date - number       | Date       | 일수에 날짜를 뺌
Date - Date         | 일수        | 일수에 다른 일수를 뺌
Date + number / 24  | Date       | 시간을 날짜에 더함

### 날짜 함수

MONTHS_BETWEEN    | 두 날짜 사이의 월수를 계산
ADD_MONTHS        | 월을 날짜에 더함
NEXT_DAY          | 다음 요일에 대한 날짜
LAST_DAY          | 월의 마지막 날을 계산
ROUND             | 날짜를 반올림
TRUNC             | 날짜를 절삭

~~~sql
-- 현재까지 근무일 수가 많은 사람 순으로 출력
SELECT *
FROM emp
ORDER BY sysdate - nvl(hiredate, sysdate) DESC;

-- 현재까지 근무일 수가 몇 주 몇 일인가를 출력
SELECT ename, hiredate,
round(sysdate-hiredate, 7) AS total_days,
trunc((sysdate-hiredate)/7) AS weeks,
ceil(mod(sysdate-hiredate,7)) AS days
FROM emp;

-- 10번 부서의 사원의 현재까지의 근무 월수를 계산
SELECT ename, hiredate, TRUNC(months_between(sysdate, hiredate)) AS "근무월수"
FROM emp
WHERE deptno='10'
ORDER BY "근무월수" DESC;

-- 현재 날짜에서 3개월후의 날짜 구하기
SELECT add_months( sysdate, 3 ) AS mydate FROM dual;

-- 현재 날짜에서 돌아오는 ‘월’요일의 날짜 구하기
SELECT next_day(sysdate, '화') FROM dual;
          
-- 현재 날짜에서 해당 월의 마지막 날짜 구하기
SELECT last_day(sysdate) FROM dual;
~~~

## 4. 변환 함수
- TO_CAHR
- TO_DATE
- TO_NUMBER

~~~sql
-- 입사일자에서 입사년도를 출력
SELECT ename, hiredate, to_char(hiredate, 'YYYY') hireyear
FROM emp;

-- 입사월 출력
SELECT emane, hiredate, to_char(hiredate, 'MM') hiremonth
FROM emp;

-- 입사년도-월 출력
SELECT ename, hiredate, to_char(hiredate, 'YYYY-MM') hireyearmonth
FROM emp;


-- 입사일 출력
SELECT ename, hiredate, to_char(hiredate, 'DD') hireday
FROM emp;

-- 입사일자를 ‘1999년 1월 1일’ 형식으로 출력
SELECT ename, hiredate,
to_char(hiredate, 'YYYY') || '년 ' ||
to_char(hiredate, 'MM') || '월 ' ||
to_char(hiredate, 'DD') || '일 '  AS "입사일자"
FROM emp;

SELECT enamen, hiredate, 
to_char(hiredate,'YYYY"년" MM"월" DD"일"') hireyear
FROM emp;

-- 1981년도에 입사한 사원 검색
SELECT ename, hiredate
FROM emp
WHERE to_char(hiredate, 'YYYY')='1981';

-- 5월에 입사한 사원 검색
SELECT ename, hiredate
FROM emp
WHERE to_char(hiredate, 'MM')=5;

-- 급여 앞에 $를 삽입하고 3자리 마다 ,를 출력
~~~

## 5. 조건 함수

~~~sql
DROP TABLE info;
CREATE TABLE info (
    hakbun varchar2(30),
    name   varchar2(30),
    jumin  varchar2(30),
    gender varchar2(30),
    column_id number(30)
);

INSERT INTO info VALUES ('1000', '홍길동', '801232-1234567', '남자', 1);
INSERT INTO info VALUES ('1001', '홍길자', '811232-2234567', '여자', 2);

-- 주민번호에서 성별 구하기
SELECT  decode( substr(jumin, 8, 1), 1, '남자', 3, '남자', '여자') AS gender  
FROM table_name;


SELECT CASE substr(jumin, 8, 1)
WHEN '1' THEN '남자'
WHEN '3' THEN '여자'
ELSE '여자'
END as gender
FROM emp;


-- 부서번호가 10이면영업부, 20이면 관리부, 30이면 IT부 그 외는 기술부로 출력 
SELECT decode(deptno, 10, '영업부', 20, '관리부', 30, 'IT부', '기술부') AS department
FROM emp;

SELECT ename,
CASE deptno
WHEN 10 THEN '영업부'
WHEN 20 THEN '관리부'
WHEN 30 THEN 'IT부'
ELSE '기술부'
END as department
FROM emp;


-- 업무(job)이 analyst이면 급여 증가가 10%이고 clerk이면 15%, manager이면 20%인 경우 사원번호, 사원명, 업무, 급여, 증가한 급여를 출력
SELECT empno, ename, job, sal,
decode(job, 'analyst', sal*1.10, 'clerk', sal*1.15, 'manager', sal*1.2, sal) AS "증가한급여"
FROM emp;

SELECT empno, ename, job, sal,
CASE job
WHEN 'analyst' THEN sal*1.10
WHEN 'clerk' THEN sal*1.15
WHEN 'manager' THEN sal*1.2
ELSE sal END as "증가한급여"
FROM emp;
~~~



