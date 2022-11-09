---
layout: post
title: '[Oracle] PL-SQL'
category: SQL
tags: [sql, oracle, pl-sql]
comments: true
---

# PL-SQL
- Procedural language extension to Structured Query Language
- SQL을 확장한 순차적 처리 언어
- SQL과 일반 프로그래밍 언어의 결합
- 성능향상 : 여러 SQL 문장을 하나의 블록으로묶어서 한 번에 처리하기 속도 개선


## PL/SQL 블록의 유형
### Anonymous

~~~sql
[DECLARE]
BEGIN
      statement;
      statement;
      statement;
[EXCEPTION]
END;
~~~

### Procedure

~~~sql
CREATE PROCEDURE p_name
 IS

BEGIN
      statement;
      statement;
      statement;
[EXCEPTION]
END;
~~~

### Function

~~~sql
CREATE FUNCTION f_name
   RETURN datatype
 IS

BEGIN
      statement;
      statement;
      statement;
      RETURN value;
[EXCEPTION]
END;
~~~


### 익명블록 (Anonymous block)
- 선언부 : 변수나 상수를 선언
- 실행부 : 실제 처리할 로직을 기술
- 예외처리부 : 로직을 처리하다 발생할 수 있는 예외에 대한 처리

## 1.  PL/SQL 변수
- Scalar : 단일 값
- Composite : 레코드 같은 조합 데이터형
- Reference : 참조 데이터형 (pointer)
- LOB : 큰 객체 데이터형

###  (1) 기본 스칼라 데이터형

데이터형                | 설   명
:--------------------|:------------------------
VARCHAR2 (n)          | 가변길이 문자 데이터
NUMBER (p, s)         | 숫자 데이터
DATE                  | 날짜 데이터
CHAR (n)              | 고정길이 문자 데이터 (32767바이트까지)
BOOLEAN               | TURE, FALSE, NULL 데이터
BINARY_INTEGER        | 정수에 대한 기본형

##### [문제-1] emp 테이블에서 scott의 사원번호, 이름, 입사일을 처리할 변수를 선언하고 값을 지정한후 화면 출력

~~~sql
SET serveroutput ON;

-- 명령문 실행이 아니라 스크립트 실행 (F5)
DECLARE
	v_empno number;
	v_ename varchar2(30);
	v_hiredate date;

BEGIN
	SELECT empno, ename, hiredate
	INTO v_empno, v_ename, v_hiredate
	FROM emp
	WHERE ename=upper('SCOTT');

	-- “” 사용하면 에러 ‘’사용해야
	dbms_output.put_line('사번 #' || v_empno);
	dbms_output.put_line('이름 #' || v_ename);
	dbms_output.put_line('입사일 #' || v_hiredate);

END;
~~~

## (2) 조합 데이터 형(Composite Datatype)

데이터형             | 설   명
:-------------------|:------------------------
TABLE (nested table) | 배열과 유사하나 동적으로 크기가 지정될 수 있음, 데이터 참조시 순서대로 지정하지 않아도 됨
RECORD               | 서로 다른 데이터형들을 하나로 묶은 단위, C언의 structure와 유사한 개념으로 테이블의 row를 읽어올 때 주로 사용
VARRAY               | 고정 길이를 가진 배열

##### [문제-2] EMP 테이블의 컬럼의 데이터 타입으로 레코드 선언

~~~sql
SET SERVEROUTPUT ON
ACCEPT p_ename PROMPT '조회할 사원 이름은?'
DECLARE
    -- EMP 테이블의 컬럼들로 레코드타입 변수 선언
    emp_record  emp%ROWTYPE;
    -- 입력한 이름을 저장할 변수 선언
    -- PL-SQL 대입 연산자 :=
    -- 자바에서 STRING ename = msg;와 같음
    v_ename   emp.ename%TYPE := '&p_ename'; 

BEGIN

SELECT *              -- *는 emp%ROWTYPE으로 정의시 가능
    INTO emp_record
    FROM emp
    WHERE ename = upper( v_ename );

    dbms_output.put_line('사원번호 : ' || to_char(emp_record.empno) );
    dbms_output.put_line('사 원 명 : ' || emp_record.ename );
    dbms_output.put_line('업    무 : ' || emp_record.job );

 
EXCEPTION

WHEN NO_DATA_FOUND THEN dbms_output.put_line('&p_ename' || '의 자료는 없습니다');
WHEN TOO_MANY_ROWS THEN dbms_output.put_line('&p_ename' || '자료가 2건 이상입니다.');
WHEN OTHERS THEN dbms_output.put_line('검색시 오류가 발생하였습니다.');

END;

SET SERVEROUTPUT OFF

-- # 한줄 주석 -- 과 여러줄 주석 /* */이 있다
-- # &문자 인식 못하면 set verify off 지정
~~~

## 2. PL/SQL 제어
### 조건문

~~~sql
IF condition THEN statement;
ELSIF condition THEN statement;
ELSE statement;
END IF;
/ -- sqlplus 사용시 실행 기호
~~~

~~~sql
CASE condition
	WHEN state THEN statement;
	WHEN state THEN statement;
	ELSE statement;
END CASE;
-- * 만일 ELSE에서 아무것도 하지 않을 때는 NULL 문을기술. (ex. ELSE NULL;)
~~~

##### 1. 이름, 급여, 부서번호를 입력 받아 사원 테이블에 입력시 부서번호가 20이면 급여의 30%를 추가하고, 사번은 시퀀스를 이용한다.

~~~sql
ACCEPT p_name PROMPT '이름?'
ACCEPT p_sal PROMPT '월급?'
ACCEPT p_deptno PROMPT '부서번호?'

DECLARE
    -- emp 테이블에 있는 자료형과 같은 것으로 사용
    v_name   emp.ename%type := '&p_name'; -- 대입연산자 
    v_sal    emp.sal%type := '&p_sal';
    v_deptno emp.deptno%type := '&p_deptno';

BEGIN
    IF v_deptno = 20 THEN v_sal := v_sal * 1.3;
    END IF;
    INSERT INTO emp(empno, ename, sal, deptno)
        VALUES(seq_emp_empno.nextval, v_name, v_sal, v_deptno);
    COMMIT;
END;
~~~


##### 2. 이름을 입력받아 그 사람의 업무가 MANAGER이면 10% , ANALYST이면 20%, SALESMAN이면 30%, PRESIDENT이면 40%, CLERK이면 50% 급여를 증가한다.

~~~sql
-- if문
ACCEPT p_name PROMPT '이름?'

DECLARE
    v_name   emp.ename%type  := '&p_name';
    v_job    emp.job%type;
    v_sal    emp.sal%type;
    v_empno  emp.empno%type;

BEGIN
    SELECT empno, sal, job
    INTO v_empno, v_sal, v_job
    FROM emp WHERE ename=v_name;
    
    IF v_job = 'MANAGER' THEN v_sal := v_sal * 1.1;
    ELSIF v_job = 'ANALYST' THEN v_sal := v_sal * 1.2;
    ELSIF v_job = 'SALESMAN' THEN v_sal := v_sal * 1.3;
    ELSIF v_job = 'PRESIDENT' THEN v_sal := v_sal * 1.4;
    ELSIF v_job = 'CLERK' THEN v_sal := v_sal * 1.5;
    END IF;
    UPDATE emp SET sal = v_sal WHERE ename=v_name;
    COMMIT;
END;


SELECT * FROM emp;


---- CASE문
ACCEPT p_name PROMPT '이름?'

DECLARE
    v_name   emp.ename%type := '&p_name';
    v_job    emp.job%type;
    v_sal    emp.sal%type;
    v_empno  emp.empno%type;

BEGIN
    SELECT empno, sal, job
    INTO v_empno, v_sal, v_job
    FROM emp WHERE ename=v_name;
    CASE v_job
        WHEN 'MANAGER'   THEN v_sal := v_sal * 1.1;
        WHEN 'ANALYST'   THEN v_sal := v_sal * 1.2;
        WHEN 'SALESMAN'  THEN v_sal := v_sal * 1.3;
        WHEN 'PRESIDENT' THEN v_sal := v_sal * 1.4;
        WHEN 'CLERK'     THEN v_sal := v_sal * 1.5;
    END CASE;
    UPDATE emp SET sal=v_sal WHERE ename=v_name;
    dbms_output.put_line(v_name || '님 정보 수정');
END;
~~~

### 반복문

~~~sql
LOOP
   IF condition THEN EXIT;
   END IF;
END LOOP;
~~~

~~~sql
LOOP
   EXIT WHEN condition;
END LOOP;
~~~

~~~sql
WHILE condition LOOP
END LOOP;
~~~

~~~sql
FOR i IN [REVERSE] min..max LOOP
END LOOP;
~~~

* continue 조건문
- 반복문 안에서 사용 (11g에서 )
- 조건에 맞으면 반복문의 끝으로 가서 반복 실행

##### 1. 1부터 9까지의숫자를 입력 받아 해당하는 구구단을 출력

~~~sql
ACCEPT p_dan PROMPT  '구구단 단을 입력 : '

DECLARE
	v_dan   INTEGER := &p_dan;
	cnt     INTEGER;

BEGIN
	-- 1부터 9까지 반복문 돌림
    FOR i IN 1..9 LOOP
        cnt := v_dan * i;
        dbms_output.put_line( v_dan || ' * ' || i || ' = ' || cnt );
    END LOOP;   
END;
~~~

##### 2. 1부터 100까지의 홀수의 합과 짝수의 합을 출력
-- 나머지 연산자 : mod

~~~sql
DECLARE
	even_sum INTEGER := 0;
    odd_sum INTEGER := 0;
    
BEGIN
    FOR i IN 1..100 LOOP
		-- i mode 2
        IF MOD(i, 2) = 0 THEN even_sum := even_sum + i;
        ELSIF MOD(i, 2) != 0 THEN odd_sum := odd_sum + i;
        END IF;
    END LOOP;   
    dbms_output.put_line('짝수의 합: ' || even_sum);
    dbms_output.put_line('홀수의 합: ' || odd_sum);
END;
~~~


## 3. 커서(CURSOR)
- :SQL문장을 실행할 때마다 오라클 서버는 명령이 분석되고 실행되는 곳에서 메모리영역을 개방하는데, 이 영역을 커서라 한다

### (1) 묵시적커서
- 오라클에서자동적으로 선언
- 묵시적 커서에 저장되는 데이터는 단 1행


#### 묵시적 커서의 속성

종 류               | 설 명
:------------------|:-----------------------------------------------------
SQL%ROWCOUNT       | 가장 최근의 SQL문장에 의해 영향을 받은 행의 수
SQL%FOUND          | 가장 최근의 SQL문장이 하나 이상의 행에 영향을 미친다면 TRUE
SQL%NOTFOUND       | 가장 최근의 SQL문장이 어떤 행에도 영향을 미치지 않는다면 TURE
SQL%ISOPEN         | PL/SQL이 실행된 후에 즉시 암시적 커서를 닫기 때문에 항상 FALSE



##### 1. EMP 테이블에서 사번이 8001인 자료를 삭제
~~~sql
--VARIABLE rows_deleted VARCHAR2(60)

DECLARE
    v_empno emp.empno%TYPE  := 8001;

BEGIN
    DELETE FROM emp WHERE empno=v_empno;
	IF SQL%FOUND THEN  dbms_output.put_line( SQL%ROWCOUNT || '행을 삭제하였습니다');
    END IF;

END;   
-- PRINTrows_deleted;
~~~

## (2) 명시적 커서 (FETCH CURSOR) : 사용자가 선언하는 커서
- SELECT 문장을 실행하면 여러 건의 결과 집합이 추출되는데,
- 이결과 집합의 각 개별 데이터에 접근하기 위해 커서를 사용


> 커서 선언: CURSOR 커서명 IS SELECT문장;  
커서 열기: OPEN 커서명; ( 실제 커서가 사용할 메모리 공간이 할당 )  
패치: FETCH 커서명 INTO 변수...; ( 명시적 커서로부터 데이터를 읽어서 변수로 할당 )  
커서 닫기: CLOSE 커서명; (메모리 공간 반환)  


##### 2. 부서번호를 입력 받아 사원번호,이름, 급여를 출력

~~~sql
SET SERVEROUTPUT ON;

ACCEPT p_deptno PROMPT '부서번호?'

DECLARE
    v_deptno emp.deptno%type := &p_deptno;
    v_empno  emp.empno%type;
    v_ename  emp.ename%type;
    v_sal    emp.sal%type;
    CURSOR emp_cursor IS SELECT empno, ename, sal 
    FROM emp 
    WHERE deptno=v_deptno 
    ORDER BY ename;

BEGIN
    OPEN emp_cursor;
    LOOP
        FETCH emp_cursor INTO v_empno, v_ename, v_sal;
        EXIT WHEN emp_cursor%NOTFOUND;
        dbms_output.put_line(lpad(v_empno, 6) || lpad(v_ename, 10) || lpad(v_sal, 10));
    END LOOP;
    CLOSE emp_cursor;
END;
~~~


##### 업무를 입력 받아 평균급여와 최소/최대 급여를 출력

~~~sql
SET SERVEROUTPUT ON;

ACCEPT p_job PROMPT '업무?'

DECLARE
    v_job      emp.job%type := '&p_job';
    v_avg_sal  number(7 ,2);
    v_min_sal  emp.sal%type;
    v_max_sal  emp.sal%type;
    
    CURSOR emp_cursor IS
        SELECT round(avg(sal), 2) avg_sal,
        min(sal) min_sal, max(sal) max_sal
        FROM emp
        GROUP BY job
        HAVING job=v_job;

BEGIN
    OPEN emp_cursor; 
    LOOP
        FETCH emp_cursor 
        INTO v_avg_sal, v_min_sal, v_max_sal;
        
        EXIT WHEN emp_cursor%NOTFOUND;

        dbms_output.put_line(v_job 
        || ' 평균급여 ' || v_avg_sal 
        || ' 최대급여: ' || v_max_sal 
        || ' 최소급여: ' || v_min_sal);
        
    END LOOP;
    CLOSE emp_cursor;
END;
~~~

##### 각 업무별 평균급여와 최소/최대 급여를 출력

~~~sql
DECLARE
    v_job      emp.job%type;
    v_avg_sal  number(7 ,2);
    v_min_sal  emp.sal%type;
    v_max_sal  emp.sal%type;
    
    CURSOR emp_cursor IS
        SELECT nvl(job, 'NULL') job, 
        round(avg(sal), 2) avg_sal,
        min(sal) min_sal, max(sal) max_sal
        FROM emp
        GROUP BY job;

BEGIN
    OPEN emp_cursor; 
    LOOP
        FETCH emp_cursor 
        INTO v_job, v_avg_sal, v_min_sal, v_max_sal;
        
        EXIT WHEN emp_cursor%NOTFOUND;
        
        dbms_output.put_line(v_job 
        || ' 평균급여 ' || v_avg_sal 
        || ' 최대급여: ' || v_max_sal 
        || ' 최소급여: ' || v_min_sal);
        
    END LOOP;
    CLOSE emp_cursor;
END;
~~~
