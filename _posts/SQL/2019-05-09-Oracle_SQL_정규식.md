---
layout: post
title: '[Oracle] 정규식'
category: SQL
tags: [sql, oracle, 정규식]
comments: true
---

# 정규식




## 1. REGEXP_LIKE

~~~sql
-- 소문자 영문자가 들어있는 행 출력
SELECT * FROM reg_test WHERE REGEXP_LIKE(text, '[a-z]');

-- 대문자 영문자가 들어있는 행 출력
SELECT * FROM reg_test WHERE REGEXP_LIKE(text, '[A-Z]');

-- 대소문자 영문자가 들어있는 행 출력
SELECT * FROM reg_test WHERE REGEXP_LIKE(text, '[a-zA-z]');

-- 소문자로 시작하고 뒤에 공백이 있는 모든 행 출력
SELECT * FROM reg_test WHERE REGEXP_LIKE(text, '[a-z] ');

-- 소문자로 시작하고 공백이 1칸 있고 숫자로 끝나는 행 출력
SELECT * FROM reg_test WHERE REGEXP_LIKE(text, '[a-z] [0-9]');

-- 공백이 있는 모든 데이터를 찾고 싶은 경우
SELECT * FROM reg_test WHERE REGEXP_LIKE(text, '[[:SPACE:]]');

-- 대문자가 연속적으로 2글자 이상 오는 경우 출력
SELECT * FROM reg_test WHERE REGEXP_LIKE(text, '[A-Z]{2}');

시작 문자 지정 ^(캐럿), 끝나는 문자 지정 $(달러)
-- 시작을 대문자나 소문자로 하는 행 출력
SELECT * FROM reg_test WHERE REGEXP_LIKE(text, '^[a-zA-Z]');

-- 시작을 숫자나 대문자로 시작하는 행 출력
SELECT * FROM reg_text WHERE REGEXP_LIKE(text, '^[0-9A-Z]');

여러가지 조건을 줄 경우 바 기호(|)를 사용하여 연결할 수도 있음
-- 소문자로 시작하거나, 숫자로 시작하는 경우
SELECT * FROM reg_test WHERE REGEXP_LIKE(text, '^[a-z]|^[0-9]');

-- STUDENT 테이블에서 학생의 ID중 첫 글자가 s(소문자)로 시작하고 두번째 글자가 a나 t가 나오는 id 출력
SELECT name, id FROM student WHERE REGEXP_LIKE(id, '^s(a|t).');

-- 소문자로 끝나는 모든 행 출력
SELECT * FROM reg_test WHERE REGEXP_LIKE(text, '[a-z]$');

^(캐럿)문자가 대괄호 안에 들어갈 경우에는 대괄호 안의 문자가 아닌 다른 것만 출력하라는 의미
-- 소문자로 시작하지 않은 행을 모두 출력
SELECT * FROM reg_test WHERE REGEXP_LIKE(text, '^[a-z]');

-- student 테이블에서 학생의 id를 조사해 4번쨰 자리에 v(소문자)가 있는 행을 출력
SELECT name, id FROM STUDENT WHERE REGEXP_LIKE(id, '^...v.');

특정 조건을 제외한 결과 출력(NOT)
-- 영문자(대소문자)를 포함하지 않는 행을 출력
SELECT * FROM reg_test WHERE NOT REGEXP_LIKE(text, '[A-Za-z]');

특수문자 찾기 (* 또는 ? 기호는 모든 것이라는 뜻을 가진 메타캐릭터 문자이기 때문에 \를 붙여준다)
-- ?가 들어간 행 출력
SELECT * FROM reg_test WHERE REGEXP_LIKE(text, '\?');

-- ?가 들어가지 않는 행 출력
SELECT * FROM reg_test WHERE NOT REGEXP_LIKE(text, '\?');

소문자가 들어 있는 모든 행을 출력
SELECT * FROM reg_test WHERE REGEXP_LIKE(text, '*[a-z]');
SELECT * FROM reg_test WHERE REGEXP_LIKE(text, '?[a-z]');

-- 지역번호가 2자리이고 국번이 4자리가 나오는 경우
SELECT name, tel
FROM student
WHERE REGEXP_LIKE(tel, '^[0-9]{2}\)[0-9]{4}');

-- 대괄호 안에 있는 숫자의 순서와 상관없이 해당 숫자가 있는 행은 모두 출력됨
SELECT * FROM t_reg2
WHERE REGEXP_LIKE(ip, '^[172]{3}\.[16]{2}\.[168]{3}');
~~~



## 예제

### reg_tab 테이블 생성

~~~sql
CREATE TABLE reg_tab(text varchar2(20));

INSERT INTO reg_tab VALUES('TIGER');
INSERT INTO reg_tab VALUES('TIGGER');
INSERT INTO reg_tab VALUES('elephant');
INSERT INTO reg_tab VALUES('tiger');
INSERT INTO reg_tab VALUES('tiger2');
INSERT INTO reg_tab VALUES('tiger3');
INSERT INTO reg_tab VALUES('doggy');
INSERT INTO reg_tab VALUES('5doggy');
INSERT INTO reg_tab VALUES('DOG');
INSERT INTO reg_tab VALUES('DOG2');
INSERT INTO reg_tab VALUES('개');
INSERT INTO reg_tab VALUES('cat');
INSERT INTO reg_tab VALUES('catty');
INSERT INTO reg_tab VALUES('9catty');
INSERT INTO reg_tab VALUES('catwoman');
INSERT INTO reg_tab VALUES('고양이');
INSERT INTO reg_tab VALUES('BAT');
INSERT INTO reg_tab VALUES('BATMAN');
INSERT INTO reg_tab VALUES('BATGIRL');
INSERT INTO reg_tab VALUES('0BATGIRL');
INSERT INTO reg_tab VALUES('박쥐');

COMMIT;
~~~

#### 1.  text 컬럼의 문자열에서 't'로시작하는 데이터 검색

~~~sql
SELECT * FROM reg_tab
WHERE REGEXP_LIKE(text, '^t'); 
~~~

#### 2. text 컬럼의 문자열에서 't'로 끝나는 데이터 검색

~~~sql
SELECT * FROM reg_tab
WHERE REGEXP_LIKE(text, 't$');
~~~


#### 3. 첫번째 't'로시작하여 5번째 'r'이 있는 데이터 검색

~~~sql
SELECT * 
FROM reg_tab 
WHERE REGEXP_LIKE( text,'^t...r');
~~~

~~~sql
SELECT *
FROM reg_tab
WHERE REGEXP_LIKE (text,'^t|____r');
~~~

~~~sql
SELECT *
FROM reg_tab
WHERE REGEXP_LIKE(text,'[^t][++++r]'); 
~~~

#### 4. 숫자로 끝나는 데이터 검색

~~~sql
SELECT * 
FROM reg_tab 
WHERE REGEXP_LIKE( text,'[0-9]$'); 
~~~
 

#### 5. 숫자로 시작하는 데이타 검색

~~~sql
SELECT * 
FROM reg_tab 
WHERE REGEXP_LIKE(text,'^[0-9]');
~~~

~~~sql
SELECT *
FROM reg_tab
WHERE REGEXP_LIKE(text, '^\d');
~~~

#### 6. 숫자가 아닌 문자로 시작하는 데이터 검색

~~~sql
SELECT * 
FROM reg_tab 
WHERE NOT REGEXP_LIKE(text,'^[0-9]');
~~~

~~~sql
SELECT * 
FROM reg_tab 
WHERE REGEXP_LIKE(text, '^[^0-9]');
~~~

~~~sql
SELECT *
FROM reg_tab
WHERE REGEXP_LIKE(text, '^\D');
~~~

#### 7. 대문자로 시작하는 데이터 검색

~~~sql
SELECT * 
FROM reg_tab 
WHERE REGEXP_LIKE( text,'^[A-Z]');
~~~

~~~sql
SELECT *
FROM reg_tab
WHERE REGEXP_LIKE(text, '^[[:upper:]]');
~~~
 

#### 8. 소문자 외의 문자로 시작하는 데이터 검색

~~~sql
SELECT * 
FROM reg_tab 
WHERE NOT REGEXP_LIKE(text, '^[a-z]|^[0-9]');
~~~

~~~sql
SELECT *
FROm reg_tab
WHERE REGEXP_LIKE(text, '^[^[:lower:]]');
~~~

#### 9. 한글로 시작하는 데이터 검색

~~~sql
SELECT * 
FROM reg_tab 
WHERE REGEXP_LIKE( text,'^[ㄱ-힣]');
~~~

#### 10. 데이터 중 'gg'나 'GG'가 들어있는 데이터 검색

~~~sql
SELECT *
FROM reg_tab
WHERE REGEXP_LIKE(text, 'gg|GG');
~~~

~~~sql
SELECT *
FROM reg_tab
WHERE REGEXP_LIKE(text, '-*gg-*|-*GG-*');
~~~


## REGEXP_REPLACE 함수
- 주어진 문자열에서 특정 패턴을 찾아서 다른 모양으로 치환하는 함수

~~~sql
-- 숫자 부분을 @기호로 변경
SELECT text, REGEXP_REPLACE(text, '[[:digit:]]', '@') FROM reg_test;

-- 숫자를 찾아서 숫자 뒤에 '-*'을 추가
SELECT text, REGEXP_REPLACE(text, '[0-9]', '\1-*') FROM reg_test;

-- reg_test2 테이블에서 ip의 .(dot) 부분을 모두 삭제하고 출력
SELECT no, ip, REGEXP_REPLACE(ip, '\.', '') FROM reg_test2;

-- reg_test2 테이블에서 ip의 첫번째 .(dot) 부분을 '/' (슬래쉬) 기호로 변경해서 출력
SELECT no, ip, REGEXP_REPLACE(ip, '\.', '/', 1, 1) FROM reg_test2;

사용자에게 입력받은 문자 가운데 공백이 여러 개 들어있는 경우 그 공백을 제거하는 방법
-- 사용자가 ID를 'abc 123' 이렇게 입력했을 경우 'abc' 와 '123' 사이의 공백을 없애고 싶은 경우
-- {} 내의 숫자는 앞문자가 나타나는 횟수 또는 번위, {,}은 이상을 의미
SELECT REGEXP_REPLACE('abc 123', '(){1,}', '') FROM dual;

사용자가 검색어를 입력할 때 공백 문자를 가장 먼저 입력하고 아이디 중간에도 공백이 있어서 모든 공백을 제거하는 예
-- 아이디 입력시 : (공백)  75   true 를 입력하였을때 중간 중간 공백을 모두 제거하는 방법
SELECT studno, name, id FROM student WHERE id=REGEXP_REPLACE('&id', '(){1,}', '');

SELECT no, ip, REGEXP_REPLACE(ip, '\.', '/', 1, 1) "REPLACE"
FROM t_reg2;


SELECT REGEXP_REPLACE('20160905',
 '([[:digit:]]{4})([[:digit"]]{2})([[:digit:]]{2})',
 '\1-\2-\3';

-- 2016-09-05
~~~

## 3. REGEXP_INSTR 함수
- 특정 패턴이 출현하는 첫 위치 값을 반환하는 함수
- 0이 아니라 1부터 시작한다.
- 0은 특정 패턴을 만족하는 값이 없다는 의미이다.

~~~sql
-- text 중에서 '*'의 위치를 찾는 방법
SELECT text, REGEXP_INSTR(text, '\*') FROM reg_test;
~~~

## 4. REGEXP_SUBSTR 함수
- SUBSTR 함수의 확장판으로 특정 패턴에서 주어진 문자를 추출해 내는 함수

~~~sql
-- 'abc* *def %ghi,jkl' 이란 문자열에서 첫 글자가 공백이 아니고 ('[^ ]') 그 후에 'def'가 나오는 부분을 추출
SELECT REGEXP_SUBSTR('abc* *def %ghi,jkl', '[^ ]+[def]') FROM dual;


-- Professor 테이블에서 101번 학과와 201번 학과 교수들의 메일 주소의 도메인 주소를 출력하시오. 단 메일주소는 @뒤에 있는 주소만 출력하시오.
-- 'http://' 부분을 제거하고 .으로 구분되는 필드를 최소 3개에서 최대 4개까지 출력하라는 의미
-- 그 후 왼쪽부분에 나오는 '/' 기호를 LTRIM 함수로 제거함
SELECT LTRIM(REGEXP_SUBSTR(email, '@([[:alnum:]]+\.?){3, 4}?'), '@') domain
FROM professor
WHERE deptno IN (101, 201);

-- abc.net

특정기호나 문자를 기준으로 데이터를 추출할 때
-- : 기호를 기준으로 3번째의 문자열을 추출
SELECT REGEXP_SUBSTR('sys/oracle@racdb:1521:racdb', '[^:]+', 1, 3) result
FROM dual;
-- racdb

-- 슬래쉬를 기준으로 출력
SELECT REGEXP_SUBSTR('sys/oracle@racdb:1521:racdb', '[^/:]+', 1, 2) result
FROM dual;

-- oracle@racdb
~~~


## 5. REGEXP_COUNT 함수
- 특정 문자의 개수를 세는 함수

~~~sql
-- 주어진 문자열에서 소문자 'a'가 몇개인지 찾아주는 예
SELECT text, REGEXP_COUNT(text, 'a') FROM reg_test;

-- 검색 위치를 3으로 지정해서 3번쨰 문자 이후부터 해당 소문자 'c'가 나오는 개수를 세는 예
SELECT text, REGEXP_COUNT(text, 'c', 3) FROM t_reg;

-- 대소문자 구분 없이 갯수 체크
SELECT text, REGEXP_COUNT(text, 'c', 1, 'i') FROM t_reg;
~~~

~~~sql
SELECT text, 
REGEXP_COUNT(text, 'aa') RESULT1,
REGEXP_COUNT(text, 'a{2}') RESULT2,
REGEXP_COUNT(text, '(a)(a)') RESULT3
FROM t_reg;
~~~
