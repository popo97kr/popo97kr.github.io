---
layout: post
title:  "SQL 명령어 훑기 (키워드/예제 중심)"
subtitle: "SQL 명령어 훑기 (키워드/예제 중심)"
categories: db
tags: db
comments: true
---





SQL 명령어 상기시키기

- DDL, DML, DCL, TCL 명령어와 예제들

- t재귀함수를 활용한 하노이 탑 풀이 

---

SQL 문장들의 종류에는 **DDL, DML, DCL, TCL**이 있다.

### DDL (Data Definition)

**[CREATE]**

- 데이터 유형
  - CHARACTER - 고정 길이
  - VARCHAR(n) - 가변 길이
  - NUMERIC/INTEGER/FLOAT/...
  - DATE
  - ...

- 제약조건 (constraints)
  - NOT NULL
  - UNIQUE KEY - null있어도 괜찮지만 중복허가 X
  - PRIMARY KEY - not null & unique
  - FOREIGN KEY
  - CHECK - 특정 condition 만족 확인
  - DEFAULT - default로 들어가는 값 설정

~~~MYSQL
CREATE TABLE employees(
	id			INTEGER	    PRIMARY KEY,
	first_name 	VARCHAR(50)	NOT NULL,
    last_name 	VARCHAR(50)	NOT NULL,
    fname 		VARCHAR(50)	NOT NULL,
    dateofbirth	DATE 		NOT NULL
);
~~~



**[DROP]**

테이블 초기화(destroys an existing DB)

~~~mysql
DROP TABLE employees;
~~~



**[TRUNCATE]**

테이블 내의 모든 행 제거 BUT 구조만 남김

~~~mysql
TRUNCATE TABLE employees;
~~~



**[ALTER]**

column 추가/삭제/변경

- ADD COLUMN

~~~MYSQL
ALTER TABLE sink ADD bubbles INTEGER;
~~~

- DROP COLUMN

~~~mysql
ALTER TABLE sink DROP COLUMN bubbles;
~~~

- MODIFY COLUMN

~~~mysql
ALTER TABLE sink MODIFY COLUMN bubb FLOAT;
~~~

- DROP CONSTRAINT

~~~MYSQL
ALTER TABLE sink DROP CONSTRAINT last_name;
~~~

- ADD CONSTRAINT

~~~mysql
ALTER TABLE sink ADD CONSTRAINT first_name PRIMARY KEY(first_name);
~~~





### DML(Data Manipulation)

**[INSERT]**

데이터 입력할 때 사용

- 입력 column 지정해서 입력할 때

  ~~~mysql
  INSERT INTO phone_book (name, number) VALUES ('John', '555-1212');
  ~~~

- 입력되는 column들이 다 해당될 때

  ~~~MYSQL
  INSERT INTO phone_book VALUES ('John', 'Seoul', '555-1212');
  ~~~



**[SELECT]**

데이터 가져올 때 사용

~~~MYSQL
SELECT name, city FROM customers;
~~~

~~~mysql
SELECT * FROM customers;
~~~

- w/ parameters

~~~mysql
SELECT city, state, count(city) '도시카운트'
FROM customers
WHERE state LIKE 'A%'
GROUP BY city
HAVING count(city)>5
ORDER BY state;
~~~

select 파라미터(WHERE, GROUP BY, ORDER BY, ...) 에 대한 정보는 [여기](https://popo97kr.github.io/programming/2020/07/14/db-select/)



**[DELETE]**

데이터 행 삭제할 때 사용 

- 주의 : WHERE절이 없으면 모든 데이터가 지워지는 참사가..

~~~MYSQL
DELETE FROM mytable
WHERE mycol>100 AND item = 'Hammer';
~~~



**[UPDATE]**

데이터 변경할 때 사용

- 주의 : WHERE절이 없으면 모든 데이터가 바뀌는 참사가..

~~~MYSQL
UPDATE customer
SET name = "Alfredo", city="New York"
WHERE customerID = 1;
~~~



tip) DELETE 와 UPDATE의 경우 쓰기 이전에 SELECT절을 통해 데이터가 존재하는지 확인하는게 좋다.





### DCL (Data Control)

유저/OBJECT/ROLE를 생성하고 권한을 제어하는 명령어

**[CREATE USER|OBJECT|ROLE]**

유저 생성할 때 사용

~~~MYSQL
CREATE USER pjs IDENTIFIED BY korea;
~~~

**[GRANT CREATE USER]**

유저 생성 권한을 부여할 때 사용

~~~mysql
GRANT CREATE USER TO scott;
~~~

**[GRANT CREATE SESSION]**

로그인 권한 부여할 때 사용

~~~MYSQL
GRANT CREATE SESSION TO pjs;
~~~

**[GRANT CREATE TABLE]**

테이블 생성 권한을 부여할 때 사용

~~~mysql
GRANT CREATE TABLE TO pjs;
~~~





### TCL (Transaction Control)

트랜잭션 = 분리될 수 없는 DB 조작 단위

트랜잭션의 특성인 **atomicity, consistency, isolation, durability (ACID)**를 만족시키며 이행하는 명령어

- atomicity (원자성) : "all or nothing" - 다 수행되거나 아예 수행 안되거나
- consistency (일관성) : 트랜잭션 실행결과는 바뀌지 X
- isolation (고립성) : 트랜잭션끼리 서로 방해하지 X
- durability (지속성) : 트랜잭션 한번 끝나면, system fail 해도 영구적으로 저장된다.

**[COMMIT]**

**[ROLLBACK]**

**[SAVEPOINT]**

rollback할 때 savepoint 까지만 복구하도록 한다.

~~~mysql
SAVE TRANSACTION svpt;
...
ROLLBACK TRANSACTION svpt;
~~~



(DCL, TCL에 관한 사항은 추후 업뎃 예정)