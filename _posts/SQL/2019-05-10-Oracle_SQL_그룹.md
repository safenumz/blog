---
layout: post
title: '[Oracle] 그룹'
category: SQL
tags: [sql, oracle, 그룹]
comments: true
---

# 그룹
## 1. ALL, DISTINCT
- select all job from emp;
- select distinct job from emp;
- (*)rownum


~~~sql
SELECT rownum, empno, enmame
FROM emp
WHERE rownum<=5;
~~~

## 2. 집계함수
- 집계함수에 한에서는 null값도 처리해 준다.
- 함수 인자에 distinct를 이용하면 중복되지 않는 값으로 계산

AVG           | 평균
COUNT         | 행의 갯수
SUM           | 합계
MIN / MAX     | 최소값, 최대값
VARIANCE      | 분산값
STDDEV        | 표준편차값

~~~sql
-- 업무가 ‘SALESMAN’인 사람들의 월급의 평균, 총합, 최소값, 최대값을 구하기
SELECT avg(sal) avg, sum(sal) sum, min(sal) min, max(sal) max
FROM emp
WHERE job='SALESMAN';

-- 전체 사람 수
SELECT count(*) cnt
FROM emp;

--커미션(COMM)을 받는 사람들의 수는?
SELECT count(comm) cnt
FROM emp
WHERE nvl(comm, 0)!=0;
~~~

## 3. 데이터 그룹

~~~sql
SELECT columns  FROM  table_name  WHERE condition
GROUP BY group_by_expression
HAVING condition
ORDER BY column;
~~~

~~~sql
-- 부서별로인원수, 평균급여, 최저급여, 최고급여, 급여의 합을 구하기
SELECT deptno, count(*) cnt, round(avg(sal), 2) avg, min(sal) min, max(sal) max, sum(sal) sum
FROM emp
GROUP BY deptno;

-- 부서별로인원수, 평균급여, 최저급여, 최고급여, 급여의 합을 구하기 (부서별 급여의 합이 높은 순으로
SELECT deptno, count(*) cnt, round(avg(sal), 2) avg, min(sal) min, max(sal) max, sum(sal) sum
FROM emp
GROUP BY deptno
ORDER BY sum DESC;
          

-- 부서별업무별 그룹하여 부서번호, 업무, 인원수, 급여의 평균, 급여의 합을 구하기
SELECT deptno, job, count(*) cnt, round(avg(sal), 2) avg, sum(sal) sum
FROM emp
GROUP BY deptno, job
ORDER BY deptno;
          

-- 급여가 최대 2900 이상인부서에 대해 부서번호, 평균 급여, 급여의 합을 출력
SELECT deptno, round(avg(sal), 2) avg, sum(sal) sum
FROM emp
GROUP BY deptno
HAVING max(sal) >= 2900;

 

-- 업무별 급여의 평균이 3000이상인 업무에 대해 업무명, 평균 급여, 급여의 합을 출력
SELECT job, round(avg(sal), 2) avg, sum(sal) sum
FROM emp
GROUP BY job
HAVING avg(sal) >= 3000;
 

-- 전체 합계 급여가 5000를초과하는 각 업무에 대해서 업무와 급여 합계를 출력
-- 단, SALESMAN은제외하고 급여 합계가 높은 순으로 정렬
SELECT job, sum(sal) sum
FROM emp
WHERE job!='SALESMAN'
GROUP BY job
HAVING sum(sal) > 5000
ORDER BY sum DESC;
 

--  업무별최고 급여와 최소 급여의 차이를 구하라
SELECT job, max(sal)-min(sal) AS cha
FROM emp
GROUP BY job;
 

-- 부서 인원이 4명보다 많은 부서의 부서번호, 인원수, 급여의 합을 출력
SELECT deptno, count(*), sum(sal)
FROM emp
GROUP BY deptno
HAVING count(deptno)>4;
~~~


## GROUP BY 절에 사용하는 함수

~~~sql
select job, sum( sal ) from emp group by job;
select job, sum( sal ) from emp group by rollup(job);
select job, sum( sal ) from emp group by cube(job);
~~~

## hr계정의 employees 테이블 연습문제

~~~sql
-- # HR 계정에서 ( employees 테이블 )
SELECT * FROM employees
ORDER BY JOB_ID;

-- 1. 2003년에 입사한 사원들의 사번, 이름, 입사일을 출력
SELECT employee_id, first_name || ' ' || last_name AS name, hire_date
FROM employees
WHERE to_char(hire_date, 'YY')='03';

-- 2. 업무가 FI_ACCOUNT / FI_MGR / SA_MAN / SA_REP 인 사원들의 정보를 출력
SELECT *
FROM employees
WHERE job_id IN ('FI_ACCOUNT', 'FI_MGR', 'SA_MAN', 'SA_REP');

-- 3. 커미션을 받는 사원들의 명단을 출력
SELECT first_name || ' ' || last_name AS name, commission_pct
FROM employees
WHERE nvl(commission_pct, 0)!=0
ORDER BY commission_pct DESC;

-- 4.업무가 SA_MAN 또는 SA_REP이면 "판매부서"를 그 외는 "그 외 부서"라고 출력
SELECT first_name || ' ' || last_name AS name, 
CASE job_id
WHEN 'SA_MAN' THEN '판매 부서'
WHEN 'SA_REP' THEN '판매 부서'
ELSE '그 외 부서'
END as department
FROM employees
ORDER BY department DESC;
~~~

~~~sql
<< 도전 문제 >>

1. 업무별, 부서별 급여 합계와 인원수를 출력하되, 
   10번 부서를 제외하고 업무가 ‘SALESMAN’과 ‘MANAGER’만 출력한다.
SELECT job, deptno, sum(sal), count(*)
FROM emp
GROUP BY job, deptno
HAVING deptno != 10 and (job in ('SALESMAN', 'MANAGER'));

2. 업무별로 평균급여와 최대급여를 출력하되, 평균급여가 2000이상인 것만 출력하고 평균급여가 높은 순으로 정렬

SELECT job, round(avg(sal), 2) avg_sal, max(sal) max_sal
FROM emp
GROUP BY job
HAVING avg(sal) >= 2000
ORDER BY avg_sal DESC;

3. 5개씩 급여합계와 인원수를 출력 (rownum이용)

SELECT ceil(rownum/5) grp, sum(sal) sum_sal, count(*) cnt
FROM emp
GROUP BY ceil(rownum / 5);


4. 같은 입사년도 별로 인원수를 출력

SELECT to_char(hiredate, 'YYYY') AS year, count(*)
FROM emp
GROUP BY to_char(hiredate, 'YYYY');


5. 다음과 같이 출력

   CLERK     SALESMAN MANAGER       (업무명)
---------------------------------------------
     4           4       3           (인원수)

SELECT job, count(*)
FROM emp
GROUP BY job
HAVING job = 'CLERK' or job = 'SALESMAN' or job = 'MANAGER';

    

6. 다음과 같이 출력

업무명  10번부서 20번부서 30번부서 급여합계
--------------------------------------
CLERK      1300  1900    950    4150
SALESMAN   0     0       5600   5600
PRESIDENT  5000  0       0      5000
MANAGER    2450  2975    2850   8275
ANALYST    0     6000    0      6000

-- decode 활용
SELECT job "업무명",
sum(decode(deptno,10,sal, 0)) "10번부서" ,
sum(decode(deptno,20,sal, 0)) "20번부서" ,
sum(decode(deptno,30,sal, 0)) "30번부서" ,
sum(sal) "급여합계"
FROM emp
GROUP BY job;

-- case문 활용
SELECT job "업무명",
sum(CASE deptno WHEN 10 THEN sal else 0 END) as "10번부서",
sum(CASE deptno WHEN 20 THEN sal else 0 END) as "20번부서",
sum(CASE deptno WHEN 30 THEN sal else 0 END) as "30번부서",
sum(sal) "급여합계"
FROM emp
GROUP BY job;

-- 피봇테이블 활용 풀이
WITH temp AS (
    SELECT job, deptno, sal
    FROM emp
)

SELECT job AS "업무명",
nvl("n10", 0) AS "10번부서",
nvl("n20", 0) AS "20번부서",
nvl("n30", 0) AS "30번부서", 
nvl("n10", 0) + nvl("n20", 0) + nvl("n30", 0) AS "급여합계"
FROM temp
PIVOT(sum(sal)
FOR deptno 
IN (10 AS "n10", 20 AS "n20", 30 AS "n30")
);
~~~

