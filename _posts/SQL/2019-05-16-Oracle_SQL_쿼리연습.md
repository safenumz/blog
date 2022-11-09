---
layout: post
title: '[Oracle] SQL 쿼리 연습'
category: SQL
tags: [sql, oracle, 쿼리]
comments: true
---

# SQL 쿼리 연습
## scott 계정
##### 1. 업무가 jones와 같거나 월급이 ford의 월급 이상인 사원의 정보를 이름, 업무, 부서번호, 급여를 출력  단, 업무별, 월급이 많은 순으로

~~~sql
SELECT ename, job, deptno, sal
FROM emp
WHERE job = (SELECT job FROM emp WHERE ename='JONES')
OR sal >= (SELECT sal FROM emp WHERE ename='FORD')
ORDER BY job, sal DESC;
~~~


##### 2. scott 또는 ward와 월급이 같은 사원의 정보를 이름, 업무, 급여를 출력

~~~sql
SELECT ename, job, sal
FROM emp
WHERE ename='SCOTT'
OR sal=(SELECT sal FROM emp WHERE ename='WARD');
~~~

##### 3. chicago에서 근무하는 사원과 같은 업무를 하는 사원의 이름, 업무를 출력

~~~sql
WITH temp AS (
    SELECT e.job job
    FROM emp e
    INNER JOIN dept d
    ON e.deptno=d.deptno
    WHERE d.loc='CHICAGO'
)

SELECT ename, job
FROM emp
WHERE job in (SELECT job FROM temp);
~~~

##### 4. 부서별로 월급이 평균 월급보다 높은 사원을 부서번호, 이름, 급여를 출력

~~~sql
SELECT e1.deptno, e1.ename, sal, avg_sal
FROM emp e1
LEFT OUTER JOIN (SELECT e.deptno deptno, avg(e.sal) avg_sal FROM emp e GROUP BY e.deptno) e2
ON e1.deptno=e2.deptno
WHERE e1.sal > e2.avg_sal;

----

WITH temp AS (
    SELECT e1.deptno, e1.ename, sal, avg_sal
    FROM emp e1
    LEFT OUTER JOIN (SELECT e.deptno deptno, avg(e.sal) avg_sal FROM emp e GROUP BY e.deptno) e2
    ON e1.deptno=e2.deptno
)

SELECT * FROM temp
WHERE sal > avg_sal;

----

SELECT deptno, ename, sal
FROM (
    SELECT *
    FROM emp
    NATURAL JOIN (SELECT e.deptno deptno, avg(e.sal) avg_sal FROM emp e GROUP BY e.deptno)
)
WHERE sal > avg_sal;
~~~

##### 5. 말단 사원의 사번, 이름, 업무, 부서번호를 출력

~~~sql
SELECT empno, ename, job, deptno, hiredate
FROM emp
WHERE hiredate = (SELECT max(hiredate) FROM emp);
~~~


## HR 계정
##### 1. Zlotkey와 동일한 부서에 속한 모든 사원의 이름과 입사일을 표시하는 질의를 작성하십시오. Zlotkey는 제외하십시오.

~~~sql
SELECT first_name || ' ' || last_name name, hire_date
FROM employees
WHERE department_id = (SELECT department_id FROM employees WHERE last_name='Zlotkey')
AND last_name!='Zlotkey';
~~~

##### 2. 급여가 평균 급여보다 많은 모든 사원의 사원 번호와 이름을 표시하는 질의를 작성하고 결과를 급여에 대해 오름차순으로 정렬하십시오.

~~~sql
SELECT employee_id, first_name || ' ' || last_name name, salary
FROM employees
WHERE salary > (SELECT avg(salary) FROM employees)
ORDER BY salary;
~~~

##### 3. King에게 보고하는 모든 사원의 이름과 급여를 표시하십시오.

~~~sql
SELECT first_name || ' ' || last_name name, salary
FROM employees
WHERE manager_id in (SELECT employee_id FROM employees WHERE last_name='King');
~~~

##### 4. Executive 부서의 모든 사원에 대한 부서 번호, 이름 및 업무 ID를 표시하십시오.

~~~sql
SELECT e.department_id, e.first_name || ' ' || e.last_name name, e.job_id
FROM employees e
LEFT OUTER JOIN departments d
ON e.department_id = d.department_id
WHERE d.department_name='Executive';
~~~

##### 5. 평균 급여보다 많은 급여를 받고 이름에 u가 포함된 사원과 같은 부서에서 근무하는 모든 사원의 사원 번호, 이름 및 급여를 표시하는 질의를 작성하십시오.

~~~sql
SELECT e1.employee_id, e1.first_name || ' ' || e1.last_name name, e1.salary
FROM employees e1
WHERE e1.salary > (SELECT avg(e2.salary) FROM employees e2)
AND e1.department_id IN (
	SELECT e3.department_id 
	FROM employees e3 
	WHERE REGEXP_LIKE(e3.last_name, 'u|U')
);
~~~
