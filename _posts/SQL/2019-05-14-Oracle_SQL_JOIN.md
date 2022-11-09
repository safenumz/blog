---
layout: post
title: '[Oracle] JOIN'
category: SQL
tags: [sql, oracle, join]
comments: true
---

# JOIN
- 하나 이상의 테이블로부터 데이터를 검색할 때
- 사원테이블(emp)에 사원명과 부서테이블(dept)에 그 사원의 부서명을 출력

## 1. 두 테이블만 기술 
- Cartesian Product

~~~sql
SELECT ename, dname
FROM emp, dept;
~~~

## 2. 내부조인 
- 두 테이블 연결 

~~~sql
SELECT e.ename AS ename, d.dname AS danme, e.deptno AS deptno
FROM emp e, dept d
WHERE e.deptno = d.deptno;

SELECT e.ename, d.dname, d.deptno
FROM emp e INNER JOIN dept d
ON e.deptno = d.deptno;
~~~

## 3. 외부조인 
- 없는 데이터를 포함하여 조인
- 기존문법에서 (+)가 데이터가 없는 테이블에 붙였다면,
- LEFT나 RIGHT는 반대로 데이터가 있는 테이블에 붙인다.
- 기존 문법에서는 양 쪽에 (+)에 붙일 수 없는데,FULL OUTER JOIN으로 가능.

~~~sql
-- emp 테이블에는 있는데 dept 테이블에 없는 경우도 출력
SELECT e.ename ename, d.dname dname, e.deptno deptno
FROM emp e, dept d
WHERE e.deptno=d.deptno(+);

SELECT e.ename, d.dname, d.deptno
FROM emp e LEFT OUTER JOIN dept d
ON e.deptno = d.deptno;

-- dept 테이블에는 있는데 emp 테이블에 없는 경우도 출력
SELECT e.ename ename, d.dname dname, e.deptno deptno
FROM emp e, dept d
WHERE e.deptno(+)=d.deptno;

SELECT e.ename, d.dname, d.deptno
FROM emp e RIGHT OUTER JOIN dept d
ON e.deptno = d.deptno;

-- 에러
SELECT e.ename ename, d.dname dname, e.deptno deptno
FROM emp e, dept d
WHERE e.deptno(+)=d.deptno(+);
~~~

### Full OUTER JOIN

~~~sql
SELECT e.ename, d.dname, d.deptno
FROM emp e FULL OUTER JOIN dept d
ON e.deptno = d.deptno;
~~~

## 3. self JOIN

~~~sql
-- 각 사원의 매니저를 검색
SELECT e1.empno, e1.ename, e1.mgr, e2.ename
FROM emp e1, emp e2
WHERE e1.mgr = e2.empno;

SELECT e1.empno, e1.ename, e1.mgr, e2.ename
FROM emp e1 INNER JOIN emp e2
ON e1.mgr = e2.empno;



SELECT e1.empno, e1.ename, e1.mgr, e2.ename
FROM emp e1, emp e2
WHERE e1.mgr = e2.empno(+);

SELECT e1.empno, e1.ename, e1.mgr, e2.ename
FROM emp e1 LEFT OUTER JOIN emp e2
ON e1.mgr = e2.empno;
~~~


### HR 계정 연습문제

~~~sql
-- 1. 사원번호, 사원명, 부서명 출력
SELECT e.employee_id, e.first_name || ' ' || e.last_name, d.department_name
FROM employees e LEFT OUTER JOIN departments d
ON e.department_id = d.department_id;

-- 2. 사원번호, 사원명, 업무명, 부서명 출력
SELECT e.employee_id, e.first_name || ' ' || e.last_name, j.job_title, d.department_name
FROM employees e 
LEFT OUTER JOIN departments d
ON e.department_id = d.department_id
LEFT OUTER JOIN jobs j
ON e.job_id = j.job_id;

-- 3. 사원번호, 사원명, 부서명, 위치 출력
SELECT e.employee_id, e.first_name || ' ' || e.last_name AS name, d.department_name,
l.country_id || ' ' || l.state_province || ' ' || l.city || ' ' || l.street_address AS location
FROM employees e
LEFT OUTER JOIN departments d
ON e.department_id = d.department_id
LEFT OUTER JOIN locations l
ON d.location_id = l.location_id;

-- 4. 사원번호, 사원명, 업무명, 부서명, 위치출력
SELECT e.employee_id, e.first_name || ' ' || e.last_name AS name,
j.job_title,
d.department_name,
l.country_id || ' ' || l.state_province || ' ' || l.city || ' ' || l.street_address AS location
FROM employees e
LEFT OUTER JOIN jobs j
ON e.job_id = j.job_id
LEFT OUTER JOIN departments d
ON e.department_id = d.department_id
LEFT OUTER JOIN locations l
ON d.location_id = l.location_id;

-- 5. 입사년도별 최고 월급을 받는 사원의 이름과 부서명을 출력
WITH temp AS (
    SELECT to_char(e.hire_date, 'YY') AS hire_year,
    max(e.salary) AS max_salary,
    max(e.first_name) keep(dense_rank first order by e.salary DESC) 
    || ' ' || max(e.last_name) keep(dense_rank first order by e.salary DESC) AS maxsal_name,
    max(e.department_id) keep(dense_rank first order by e.salary DESC) AS maxsal_depid
    FROM employees e
    GROUP BY to_char(e.hire_date, 'YY')
)

SELECT t.hire_year, t.max_salary, t.maxsal_name name, d.department_name 
FROM temp t
LEFT OUTER JOIN departments d
ON t.maxsal_depid = d.department_id
ORDER BY hire_year;

~~~

## 집합(SET)
- UNION (합집합)
- UNION ALL (합집합 + 중복 2번 출력)
- INTERSECT (교집합)
- MINUS (차집합)

~~~sql
-- 컬럼명이 동일할 경우에 합집합을 구할 수 있다.
-- 업무가 CLERK 인 사원의 사번, 사원명, 업무, 부서번호
SELECT empno, ename, job, deptno
FROM emp
WHERE job='CLERK'
UNION
-- 10번 부서의 사번, 사원명, 업무, 부서번호
SELECT empno, ename, job, deptno
FROM emp
WHERE deptno=10;
--> 5명

SELECT empno, ename, job, deptno
FROM emp
WHERE job='CLERK'
UNION ALL
SELECT empno, ename, job, deptno
FROM emp
WHERE deptno=10;
--> 6명

-- 교집합
SELECT empno, ename, job, deptno
FROM emp
WHERE job='CLERK'
INTERSECT
SELECT empno, ename, job, deptno
FROM emp
WHERE deptno=10;
--> 1명

-- 차집합
SELECT empno, ename, job, deptno
FROM emp
WHERE job='CLERK'
MINUS
SELECT empno, ename, job, deptno
FROM emp
WHERE deptno=10;
--> 3명
~~~

