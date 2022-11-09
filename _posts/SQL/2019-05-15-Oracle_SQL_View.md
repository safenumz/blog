---
layout: post
title: '[Oracle] View'
category: SQL
tags: [sql, oracle, view]
comments: true
---

# View
- 가상 테이블
- 데이터 보안
- 복잡한 쿼리의 사용빈도가 높은 경우

~~~sql
CREATE OR REPLACE VIEW v_emp AS
	SELECT empno, ename, deptno FROM emp;
~~~

## View 생성권한 부여
- scott 계정에 뷰 생성 권한을 부여해야 함

~~~shell
$ sql plus "/as sysdba"
$ GRANT create view TO scott;
~~~

## View의 장점
- View는 데이터베이스의 선택적인 내용을 보여줄 수 있기 때문에 데이터베이스에 대한 액세스를 제한한다
- 조인을 한 것처럼 여러 테이블에 대한 데이터를 View를 통해 볼 수 있다
- 한 개의 View로 여러 테이블에 대한 데이터를 검색한다
- 특정 기준에 따른 사용자 별로 다른 데이터를 액세스 할 수 있다

## updatable 뷰
- 뷰에 대한 데이터의 입력, 변경, 삭제 가능하지만 제약조건으로 테이블처럼 가능하진 않음
- INSERT 쿼리 시 check 조건은 무시되는데 생성시 with check option 추가

~~~sql
CREATE OR REPLACE VIEW v_emp AS
	SELECT empno, ename, deptno FROM emp;
~~~

~~~sql
CREATE OR REPLACE VIEW v_emp AS
	SELECT empno, ename, deptno FROM emp;
    
SELECT * FROM emp; -- 진짜 테이블
SELECT * FROM v_emp; -- 가상 테이블

-- 가상테이블이라도 원본에 영향을 미친다. 가령 VIEW에서 INSERT가 되는 문제가 있다.
-- 이런문제를 해결하기 위해 VIEW는 읽기전용으로 만드는 경향이 있다. (with read only 옵션)
INSERT INTO v_emp(empno, ename, deptno) VALUES(8888, '맹심이', 30);

INSERT INTO v_emp(empno, ename, deptno) VALUES(8888, '꽁심이', 30); -- 원본 pk 제약조건
INSERT INTO v_emp(empno, ename, deptno) VALUES(7777, '박심이', 60); -- 원본 FK 제약조건


-- 읽기 전용으로 뷰생성
CREATE OR REPLACE VIEW v_emp AS
	SELECT empno, ename, deptno FROM emp
with read only;

INSERT INTO v_emp(empno, ename, deptno) VALUES(7777, '박심이', 20);

-- 사원번호, 사원명, 부서명 -> v_emp_info
CREATE OR REPLACE VIEW v_emp_info AS
	SELECT e.empno empno, e.ename ename, d.dname dname
    FROM emp e
    LEFT OUTER JOIN dept d
    ON e.deptno=d.deptno;

SELECT * FROM v_emp_info;

-- 테이블이 두개이상 조인해서 생성된 뷰는 입력이 안된다.
INSERT INTO v_emp_info(empno, ename, deptno) VALUES(7777, '박심이', 20);


-- [ 연습 ] 부서별로 부서명, 최소급여, 최대급여, 부서의 평균 급여를 포함하는 DEPT_SUM 뷰를생성하여라.

CREATE OR REPLACE VIEW v_dept_sum AS
    SELECT d.dname dname,
    max(e.sal) max_sal,
    min(e.sal) min_sal,
    round(avg(e.sal), 2) avg_sal
    FROM emp e
    INNER JOIN dept d
    ON e.deptno = d.deptno
    GROUP BY d.dname;
    
SELECT * FROM v_dept_sum;

-- 원본 테이블을 변경하면 뷰도 변경된다.
INSERT INTO emp(empno, ename, sal, deptno) VALUES(8890, '홍홍홍', 10000, 20);
~~~

## read-only 뷰

~~~sql		
CREATE or REPLACE VIEW v_emp AS
SELECT empno, ename, deptno FROM emp
WITH READ ONLY;
~~~

## 복합뷰

~~~sql
CREATE OR REPLACE VIEW v_emp AS
	SELECT e.empno empno e.ename ename, d.dname dname
		FROM emp e, dept d
	WHERE e.deptno = d.deptno;
-- 입력은 두 테이블이라 안되지만, 사번으로 삭제를 한다면 가능
~~~

~~~sql
-- [ 연습 ] 부서별로 부서명, 최소급여, 최대급여, 부서의 평균 급여를 포함하는 DEPT_SUM 뷰를 생성하여라. 
~~~