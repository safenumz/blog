---
layout: post
title: '[Oracle] Procedure / Function'
category: SQL
tags: [sql, oracle, procedure, function]
comments: true
---

# Procedure / Function
- 익명블록 (anonymous block)은 실행할 때마다 컴파일을 수행하고 작업을 처리
- 프로시져와 함수는 데이터베이스객체로 저장되어 필요할 때마다 호출
- 프로시져와 함수의 차이는반환값의 여부

## Procedure

~~~sql
CREATE OR REPLACE PROCEDURE 프로지셔명 (파리미터 [ IN|OUT|INOUT ]  데이터타입)
IS
    변수선언;
BEGIN
    처리내용;
END;
~~~

## Fuction

~~~sql
CREATE OR REPLACE FUNCTION 함수명 ( 파리미터 데이터타입 )

RETURN 데이터타입 IS
    변수선언;
BEGIN
    처리내용;
    RETURN 리턴값;
END;
~~~

- 실행 : EXEC / EXECUTE
- VARCHAR2 타입으로파라미터 선언시 구체적인 문자수를 기술하면 오류 발생

##### 1. 사번과 급여를 넘겨받아 수정하는 프로시져

~~~sql
CREATE OR REPLACE PROCEDURE emp_sal_update (
p_empno IN emp.empno%TYPE,
p_sal IN emp.sal%TYPE )

IS

BEGIN
    UPDATE emp SET sal = p_sal WHERE empno = p_empno;
    IF SQL%NOTFOUND THEN dbms_output.put_line( p_empno || '는 없는 사원번호입니다.');
    ELSE dbms_output.put_line( p_empno || '의 급여를 수정하였습니다.');
    END IF;
    COMMIT;
END;
/
~~~

##### 2. 사원명, 업무, 매니저, 급여를 넘겨받아 등록하는 프로시져를 생성
- 단, 부서번호는 매니저의 부서 번호와 동일하고 사원번호는 시퀀스를 이용

~~~sql
CREATE OR REPLACE PROCEDURE emp_input (
        v_name    IN  emp.ename%TYPE,
        v_job     IN  emp.job%TYPE,
        v_mgr     IN  emp.mgr%TYPE,
        v_sal     IN  emp.sal%TYPE )

IS
    -- 변수선언
    v_deptno  emp.deptno%TYPE;

BEGIN
    SELECT deptno INTO v_deptno  FROM emp WHERE empno=v_mgr;
    INSERT INTO emp(empno, ename, job, mgr, sal, deptno )
        VALUES( emp_empno_seq.nextval, upper(v_name), upper(v_job), v_mgr, v_sal, v_deptno );

--EXCEPTION
   --WHEN NO_DATA_FOUND THEN dbms_output.put_line('입력한 매니저 번호가 없습니다');
   --WHEN OTHERS THEN dbms_output.put_line('에러발생');
END;
~~~

set serveroutput on  
execute emp_input('KIM', 'SALESMAN', 7788, 3000 );

##### 3.넘겨 받은 사원명의 정보 중 부서명과 급여를 검색하는 프로시져

~~~sql
CREATE OR REPLACE PROCEDURE dname_sal_display (
        v_ename   IN  emp.ename%TYPE,
        v_dname   OUT dept.dname%TYPE,
        v_sal     OUT emp.sal%TYPE  )

IS

BEGIN
    SELECT e.sal , d.dname INTO v_sal, v_dname 
FROM emp e, dept d WHERE e.deptno=d.deptno AND ename=upper(v_ename);     

EXCEPTION
    WHEN NO_DATA_FOUND THEN dbms_output.put_line('해당 데이터가 없습니다');
    WHEN TOO_MANY_ROWS THEN dbms_output.put_line('검색 데이터가 2건 이상입니다.');
    WHEN OTHERS THEN dbms_output.put_line('에러발생');
END;
~~~

VAR g_dname VARCHAR2(15)  
VAR g_sal NUMBER  
set serveroutput on  
EXECUTE dname_sal_display('SCOTT', :g_dname, :g_sal );  
PRINT g_dname  
PRINT g_sal  

##### 4. 사원명을 입력받아 부서번호, 부서명, 급여를 출력하는 FUNCTION을 작성하되 부서번호를 리턴한다.

~~~sql
CREATE OR REPLACE FUNCTION emp_dis_func ( v_ename   IN  emp.ename%TYPE )
RETURN emp.deptno%TYPE

IS
    v_deptno  emp.deptno%TYPE;
    v_dname   dept.dname%TYPE;
    v_sal     emp.sal%TYPE;

BEGIN
    SELECT deptno, sal INTO v_deptno, v_sal  FROM emp WHERE ename=upper(v_ename);
    SELECT dname INTO v_dname FROM dept WHERE deptno=v_deptno;
    dbms_output.put_line('사 원 명 : ' || v_ename );
    dbms_output.put_line('부서번호 : ' || v_deptno );
    dbms_output.put_line('부 서 명 : ' || v_dname );
    dbms_output.put_line('급   여  : ' || v_sal );
    RETURN v_deptno;

EXCEPTION
    WHEN NO_DATA_FOUND THEN dbms_output.put_line('해당 데이터가 없습니다');
    WHEN TOO_MANY_ROWS THEN dbms_output.put_line('검색 데이터가 2건 이상입니다.');
    WHEN OTHERS THEN dbms_output.put_line('에러발생');
END;
~~~

VAR g_deptno NUMBER  
set serveroutput on  
EXECUTE :g_deptno := emp_dis_func('WARD');  
PRINT g_deptno  

##### [ 도전 ]  사번과 이동할 부서를 넘겨받아 수정하는 프로시져
- 단, 연봉은 기본 연봉의 30%증가 하되, 이동할 부서의 최고연봉보다높으면 최고연봉으로 지정하고 최소연봉보다 낮으면최소연봉으로 지정한다.

# Package

## 패키지 선언

~~~sql
CREATE OR REPLACE PACKAGE emp_package AS
-- 사번과 급여를 넘겨받아 수정하는 프로시져
  PROCEDURE emp_sal_update ( p_empno IN emp.empno%TYPE, p_sal IN emp.sal%TYPE );
  -- 사원명, 업무, 매니저, 급여를 넘겨받아 등록하는 프로시져를 생성
  PROCEDURE emp_input (
        v_name    IN  emp.ename%TYPE,
        v_job     IN  emp.job%TYPE,
        v_mgr     IN  emp.mgr%TYPE,
        v_sal     IN  emp.sal%TYPE );
END emp_package;
~~~

## 패키지 구현

~~~sql
CREATE OR REPLACE PACKAGE BODY emp_package AS
           -- 사번과 급여를 넘겨받아 수정하는 프로시져
    PROCEDURE emp_sal_update ( p_empno IN emp.empno%TYPE, p_sal IN emp.sal%TYPE )
        IS
        BEGIN
            UPDATE emp SET sal = p_sal WHERE empno = p_empno;
            IF SQL%NOTFOUND THEN dbms_output.put_line( p_empno || '는 없는 사원번호입니다.');
            ELSE dbms_output.put_line( p_empno || '의 급여를 수정하였습니다.');
            END IF;
        END emp_sal_update;

  --사원명, 업무, 매니저, 급여를 넘겨받아 등록하는 프로시져를 생성
      PROCEDURE emp_input (
            v_name    IN  emp.ename%TYPE,
            v_job     IN  emp.job%TYPE,
            v_mgr     IN  emp.mgr%TYPE,
            v_sal     IN  emp.sal%TYPE )
        IS
            -- 변수선언
            v_deptno  emp.deptno%TYPE;

        BEGIN
            SELECT deptno INTO v_deptno  FROM emp WHERE empno=v_mgr;
            INSERT INTO emp(empno, ename, job, mgr, sal, deptno )
                VALUES( emp_empno_seq.nextval, upper(v_name), upper(v_job), v_mgr, v_sal, v_deptno );
        EXCEPTION
            WHEN NO_DATA_FOUND THEN dbms_output.put_line('입력한 매니저 번호가 없습니다');
            WHEN OTHERS THEN dbms_output.put_line('에러발생');
        END emp_input;
END emp_package;
~~~

## 3. 패키지 실행

~~~sql
exec emp_package.emp_sal_update ( 8021, 2000 );
exec emp_package.emp_input('홍','SALESMAN',8021, 3000);
~~~
