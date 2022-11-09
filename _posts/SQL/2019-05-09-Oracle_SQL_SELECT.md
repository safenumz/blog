---
layout: post
title: '[Oracle] SELECT'
category: SQL
tags: [sql, oracle, select]
comments: true
---

# SELECT

~~~sql
SELECT 컬럼명
FROM 테이블명
WHERE 조건문
ORDER BY 정렬기준;
~~~

## 조건절을 구성하는 항목

~~~
1. 컬럼

2. 연산자

  - 산술 연산자 : +, -, *, /, mod

  - 비교 연산자 : >, <, >=, <=, =, != (<>)

  - 논리 연산자 : not, and, or

  - 문자열 연산자 : like, ||

3. IN / BETWEEN / EXISTS / NOT

4. IS NULL, IS NOT NULL

5. 함수

6. ANY / SOME / ALL
~~~

## scott계정 EMP 테이블 연습문제

### 모든 사원의 사원명과 급여, 급여와 보너스를 더한 합계 출력
- 함수나 연산자가 끼어 있으면 별칭을 부여해야 나중에 불러 올 수 있다.

~~~sql
SELECT ename || ' ' || job AS enamejob,
sal, comm,
sal+NVL(comm, 0) AS YEONBONG
FROM EMP;
~~~

### 사원테이블에서 부서 번호를 중복제거하고 출력

~~~sql
-- 사원테이블에서 부서 번호를 출력
SELECT all deptno FROM EMP;

-- 중복제거
SELECT distinct deptno FROM EMP;
~~~

### 20번 부서에서 근무하는 사원의 사원번호, 이름, 부서번호 출력

~~~sql
SELECT empno, ename, deptno
FROM EMP
WHERE deptno=20;
~~~


### 입사일이 81/01/01에서 81/06/09인 사원의 사원번호, 이름, 입사일을 출력

~~~sql
SELECT empno, ename, hiredate
FROM EMP
WHERE hiredate>='81/01/01' AND hiredate<='81/06/09';
~~~

~~~sql
-- BETWEEN 구문을 쓸 수도 있다.
SELECT empno, ename, hiredate
FROM emp
WHERE hiredate BETWEEN '81/01/01' AND '81/06/09';
~~~


### 담당업무가 salesman, clerk인 사원들의 이름과 업무를 출력

~~~sql
SELECT ename, job
FROM EMP
WHERE lower(job)='SALESMAN' OR lower(job)='CLERK';
~~~

### 업무가 president이고 급여가 1500이상이거나 업무가 salesman인 사원의 정보를 출력

~~~sql
SELECT *
FROM EMP
WHERE (job='PRESIDENT' AND sal>1500) OR (job='SALESMAN');
~~~

### 부서번호 정렬된 월급이 높은 순서대로 사원테이블 출력

~~~sql
SELECT *
FROM emp
ORDER BY deptno, sal desc;
~~~

### 최근 입사순으로 사원명, 급여, 입사일자 출력

~~~sql
SELECT *
FROM emp
ORDER BY hiredate DESC;
~~~

### 커미션이 높은 순으로 출력

~~~sql
SELECT *
FROM emp
ORDER BY nvl(comm, -1) DESC;
~~~

### 커미션(comm)이 없는 사원의 이름, 급여, 커미션을 출력

~~~sql
SELECT ename, sal, comm
FROM emp
WHERE comm IS NULL OR comm=0;
~~~

~~~sql
SELECT ename, sal, comm
FROM emp
WHERE nvl(comm, 0)=0;
~~~

### 이름 A로 시작하는 사원명 출력

~~~sql
SELECT ename
FROM emp
WHERE ename LIKE 'A%';
~~~

### 이름이 두번째 문자가 L인 사원명 출력

~~~sql
SELECT ename
FROM emp
WHERE ename LIKE '_A%';
~~~

### 이름에 L이 두 번 이상 포함된 사원명 출력

~~~sql
SELECT ename
FROM (SELECT ename, length(ename)-length(replace(ename,'L')) AS num FROM emp)
WHERE num>=2;
~~~

~~~sql
SELECT ename
FROM emp
WHERE ename LIKE '%L%L%';
~~~

### 커미션(COMM)이 NULL이 아닌 사원의 모든 정보를 출력

~~~sql
SELECT *
FROM emp
WHERE COMM IS NOT NULL;
~~~

### 보너스가 급여보다 10%가 많은 모든 사원에 대해 이름, 급여, 보너스를 출력

~~~sql
SELECT ename, sal, comm
FROM emp
WHERE nvl(comm, 0)>sal*1.1;
~~~

### 업무가 clerk이거나 analyst이고 급여가 1000, 3000, 5000이 아닌 모든 사원의 정보를 출력

~~~sql
SELECT *
FROM emp
WHERE (job ='CLERK' OR job='ANALYST') AND (sal!=1000 AND sal!=3000 AND sal!=5000);
~~~

~~~sql
SELECT *
FROM emp
WHERE (job ='CLERK' OR job='ANALYST') AND sal not in (1000, 3000, 5000);
~~~

### 업무가 clerk이거나 analyst이고 급여가 1000, 3000, 5000인 모든 사원의 정보를 출력

~~~sql
SELECT *
FROM emp
WHERE (job ='CLERK' OR job='ANALYST') AND (sal=1000 OR sal=3000 OR sal=5000);
~~~

~~~sql
SELECT *
FROM emp
WHERE (job ='CLERK' OR job='ANALYST') AND sal in (1000, 3000, 5000);
~~~

### 부서가 30이거나 또는 관리자가 7782인 사원의 모든 정보를 출력

~~~sql
SELECT *
FROM emp
WHERE deptno=30 or mgr=7782;
~~~



## 인사관리(hr 계정) EMPLOYEES 테이블 연습문제


### EMPLOYEES 테이블에서 사원 이름을 first_name과 last_name를 합쳐 full_name으로 출력

~~~sql
SELECT first_name || ' ' || last_name AS full_name
FROM employees;
~~~
 
### 부서번호가 30(구매부서)이고 급여 10000미만인 사원의 full_name과 월급,부서번호를 출력

~~~sql
SELECT first_name || ' ' || last_name AS full_name, salary, department_id
FROM employees
WHERE department_id=30 AND salary<10000;
~~~
 
### 부서번호가 30이고 급여가 10000달러 이하를 받는 2006년도 이전 입사한 사원의 full_name을 출력

~~~sql
SELECT first_name || ' ' || last_name AS full_name
FROM employees
WHERE department_id=30 AND salary=<10000 AND hire_date<'06/01/01';
~~~
 
### 부서번호가 30(구매부서)이고 급여가 10000이하인 사원과 부서번호가 60(IT부서)에서 급여가 5000이상인 사원을 조회

~~~sql
SELECT first_name || ' ' || last_name AS full_name
FROM employees
WHERE (department_id=30 AND salary<10000) OR (department_id=60 AND salary>5000);
~~~
 
### 사원번호가 110번에서 120번인 사원 중 급여가 5000에서 10000사이의 사원명단을 조회

~~~sql
SELECT first_name || ' ' || last_name AS full_name
FROM employees
WHERE (employee_id>=110 AND employee_id<=120) AND (salary>=5000 AND salary<=10000);
~~~
 
### 사원번호가 110번에서 120번이 아닌 사원을 조회

~~~sql
SELECT employee_id, first_name || ' ' || last_name AS full_name
FROM employees
WHERE (employee_id < 110 OR employee_id > 120);
~~~
 
### 부서번호가 30(구매부서), 60(IT부서), 90(경영부서)에 속한 사원을 조회

~~~sql
SELECT employee_id, first_name || ' ' || last_name AS full_name
FROM employees
WHERE (department_id=30 OR department_id=60 OR department_id=90);
~~~
 
### 부서번호가 30(구매부서), 60(IT부서), 90(경영부서) 외의 부서에 속한 사원을 조회

~~~sql
SELECT employee_id, first_name || ' ' || last_name AS full_name
FROM employees
WHERE (department_id!=30 AND department_id!=60 AND department_id!=90);
~~~
 
### 전화번호에서 590으로 시작하는 사원명과 전화번호를 조회

~~~sql
SELECT first_name || ' ' || last_name AS full_name, phone_number
FROM employees
WHERE phone_number LIKE '590%'
~~~