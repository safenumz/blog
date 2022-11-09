---
layout: post
title: '[Oracle] 서브쿼리'
category: SQL
tags: [sql, oracle, 서브쿼리]
comments: true
---

# 서브쿼리
- 하나의 SQL문장 내부에 존재하는 또 다른 SELECT 문장
- DML(SELECT, INSERT, UPDATE, DELETE)에 사용가능
- 서브쿼리는 ( ) 로 묶는다 [권장]
- 서브쿼리는 연산자의 오른쪽에 [권장]
- 단일 행인 경우 비교 연산자 가능 ( >, >=, <, <=, =, !=(<>) )
- 복수 행인 경우 IN, NOT IN, ANY, ALL, EXISTS
- ORDER BY에선 사용 못한다

~~~sql
-- 전체 사원들 중 평균 급여보다 낮은 급여를 받는 사원의 명단을 조회
SELECT empno, ename, sal
FROM emp
WHERE sal < (SELECT avg(sal) FROM emp);


-- 평균급여보다 높고 최대 급여보다 낮은 월급을 받는 사원 명단 조회
SELECT ename, sal
FROM emp
WHERE sal > (SELECT avg(sal) FROM emp) 
and sal < (SELECT max(sal) FROM emp);

-- 월급순으로 상위 10명의 명단을 출력 (rownum 이용)
SELECT * 
FROM (
    SELECT ename, sal
    FROM emp 
    ORDER BY nvl(sal, 0) DESC
) e1
WHERE rownum <= 10;
~~~



~~~sql
--<< 연습문제 >> 서브쿼리
--1. SCOTT의 급여보다 많은 사원의 정보를 사원번호, 이름, 담당업무, 급여를 출력
--select sal from emp where ename = 'SCOTT';

select empno, ename,job,sal
from emp
where sal > (select sal from emp where ename = 'SCOTT');

--2. 30번 부서의 최소 급여보다 각부서의 최소 급여가 높은 부서를 출력
--select min(sal) from emp where deptno = 30;

select deptno, min(sal) min
from emp
group by deptno
having min(sal) >(select min(sal) from emp where deptno = 30);

--3. 업무별로 평균 급여 중에서 가장 적은 급여를 가진 직업을 출력
--select job,round(avg(sal)) avg from emp group by job;
--select min( round(avg(sal))) min_avg from emp group by job;

select job
from (select job,round(avg(sal)) avg from emp group by job) e1, (select min( round(avg(sal))) min_avg from emp group by job) e2
where e1.avg = e2.min_avg;


--4. 사원번호가 7521의 업무와 같고 사번 7934인 직원보다 급여를 많이 받는 사원의 정보를 출력
--select job from emp where empno = 7521;
--select sal from emp where empno = 7934;

select *
from emp
where job =(select job from emp where empno = 7521) and sal>(select sal from emp where empno = 7934);


--5. 업무가 ‘MANAGER’인 사원의 정보를 이름, 업무, 부서명, 근무지를 출력

select e.ename, e.job, d.dname,d.loc 
from emp e inner join dept d 
on e.deptno=d.deptno 
where e.job= 'MANAGER';


--6. 'WARD'와 부서와 업무가 같은 사원 명단 출력
--select deptno, job from emp where ename = 'WARD';

select *
from emp e1,(select deptno, job from emp where ename = 'WARD') e2
where e1.deptno =e2.deptno and e1.job = e2.job;--<< 연습문제 >> 서브쿼리
--1. SCOTT의 급여보다 많은 사원의 정보를 사원번호, 이름, 담당업무, 급여를 출력
--select sal from emp where ename = 'SCOTT';

select empno, ename,job,sal
from emp
where sal > (select sal from emp where ename = 'SCOTT');

--2. 30번 부서의 최소 급여보다 각부서의 최소 급여가 높은 부서를 출력
--select min(sal) from emp where deptno = 30;

select deptno, min(sal) min
from emp
group by deptno
having min(sal) >(select min(sal) from emp where deptno = 30);

--3. 업무별로 평균 급여 중에서 가장 적은 급여를 가진 직업을 출력
--select job,round(avg(sal)) avg from emp group by job;
--select min( round(avg(sal))) min_avg from emp group by job;

select job
from (select job,round(avg(sal)) avg from emp group by job) e1, (select min( round(avg(sal))) min_avg from emp group by job) e2
where e1.avg = e2.min_avg;


--4. 사원번호가 7521의 업무와 같고 사번 7934인 직원보다 급여를 많이 받는 사원의 정보를 출력
--select job from emp where empno = 7521;
--select sal from emp where empno = 7934;

select *
from emp
where job =(select job from emp where empno = 7521) and sal>(select sal from emp where empno = 7934);


--5. 업무가 ‘MANAGER’인 사원의 정보를 이름, 업무, 부서명, 근무지를 출력

select e.ename, e.job, d.dname,d.loc 
from emp e inner join dept d 
on e.deptno=d.deptno 
where e.job= 'MANAGER';


--6. 'WARD'와 부서와 업무가 같은 사원 명단 출력
--select deptno, job from emp where ename = 'WARD';

select *
from emp e1,(select deptno, job from emp where ename = 'WARD') e2
where e1.deptno =e2.deptno and e1.job = e2.job;
~~~


~~~sql
-- 업무별로 최소 급여를 받는 사원의 정보를 사원번호, 이름, 담당업무, 급여를 출력
SELECT empno, ename, job, sal
FROM emp
WHERE (job, sal) in (SELECT job, min(sal) FROM emp GROUP BY job);


-- 10번 부서사원들의 업무와 동일한 직원들 검색
SELECT *
FROM emp
-- 하나라도 맞는 것이 있으면 걸러줌
WHERE job = ANY (SELECT job FROM emp WHERE deptno=10);

-- 부서별 최소 급여를 받는 사원보다 많은 급여를 받는 사원 정보 출력
-- ANY: 여러가지 조건 중 하나의 조건이라도 만족하면 참
-- ALL: 모든 조건을 만족해야 참
SELECT *
FROM emp
WHERE sal > ANY (SELECT min(sal) FROM emp GROUP BY deptno);

-- 적어도 한명의 사원으로부터 보고를 받을 수 있는 사원의 정보를 사원번호, 이름, 업무를 출력
SELECT e.empno, e.ename, e.job
FROM emp e
WHERE exists(SELECT * FROM emp e2 WHERE e.empno=e2.mgr);

-- 부하직원이 없는 사원들을 검색
SELECT e.empno, e.ename, e.job, e.mgr
FROM emp e
WHERE not exists(SELECT * FROM emp e2 WHERE e.empno=e2.mgr);
~~~

~~~sql
-- INSERT / UPDATE / DELETE에서

-- (1) 부서별 급여통계 테이블 생성

create table stat_salary (
    stat_date   date  not  null,        -- 날짜
    dept_no   number,                    --부서
    emp_count number,      --사원수
    tot_salary number,        --급여총액
    avg_salary number 
    );     -- 급여평균
    
    
-- (2) 날짜와 부서번호 입력
INSERT INTO stat_salary(stat_date, dept_no)
    SELECT sysdate, deptno FROM dept;
 
SELECT * FROM stat_salary;

-- (3) 사원수, 급여총액, 평균급여 입력(?) -> 수정
UPDATE stat_salary s
SET (s.emp_count, s.tot_salary, s.avg_salary) 
= (SELECT count(*), sum(sal), avg(sal) FROM emp e WHERE e.deptno=s.dept_no GROUP BY deptno);

DROP TABLE emp_copy;
CREATE TABLE emp_copy
AS SELECT * FROM emp;


-- INSERT / UPDATE / DELETE에서

-- (1) 부서별 급여통계 테이블 생성

create table stat_salary (
    stat_date   date  not  null,        -- 날짜
    dept_no   number,                    --부서
    emp_count number,      --사원수
    tot_salary number,        --급여총액
    avg_salary number 
    );     -- 급여평균
    
    
-- (2) 날짜와 부서번호 입력
INSERT INTO stat_salary(stat_date, dept_no)
    SELECT sysdate, deptno FROM dept;
 
SELECT * FROM stat_salary;

-- (3) 사원수, 급여총액, 평균급여 입력(?) -> 수정
UPDATE stat_salary s
SET (s.emp_count, s.tot_salary, s.avg_salary) 
= (SELECT count(*), sum(sal), avg(sal) FROM emp e WHERE e.deptno=s.dept_no GROUP BY deptno);

DROP TABLE emp_copy;
CREATE TABLE emp_copy
AS SELECT * FROM emp;


-- 1. scott의 업무와 급여로 jones의 업무와 급여를 수정
UPDATE emp_copy e
SET (e.job, e.sal) 
= (SELECT e2.job, e2.sal FROM emp e2 WHERE e2.ename = 'SCOTT')
WHERE e.ename = 'JONES';

-- 2. 부서명이 sales인 사원의 정보를 삭제
DELETE FROM emp_copy
WHERE empno in (
    SELECT e.empno
    FROM emp_copy e
    LEFT OUTER JOIN dept d
    ON e.deptno = d.deptno
    WHERE d.dname='SALES'
);

DELETE FROM emp_copy
WHERE deptno = (SELECT deptno FROM dept WHERE dname='SALES');


DROP TABLE emp_copy;
CREATE TABLE emp_copy
AS SELECT * FROM emp;

-- 3.  King에게 보고하는 모든 사원의 이름과 급여를 출력
SELECT e1.ename, e1.sal
FROM emp_copy e1
WHERE e1.mgr = (SELECT e2.empno FROM emp_copy e2 WHERE e2.ename='KING');


-- 4. 월급이 30번 부서의 최저 월급보다 많은 사원들을 출력
SELECT *
FROM emp_copy e1
WHERE e1.sal > (SELECT min(e2.sal) FROM emp_copy e2 WHERE deptno=30);


-- 5. 10번 부서의 직원들 중 30번 부서의 사원과 같은 업무를 맡고 있는 사원의 이름과 업무를 출력
SELECT e1.ename, e1.job
FROM emp_copy e1
WHERE e1.deptno=10
AND e1.job in (SELECT e2.job FROM emp_copy e2 WHERE e2.deptno=30);
~~~

