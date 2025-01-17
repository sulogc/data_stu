<p>
형식은 자유롭게 작성하시고, 템플릿 활용시 cp ./Template.md ./sulog.md 와 같이 복사해서 사용해주세요. 
</p>

# Subject : SQL Basics and Implement


Contents
---

[Chapter 1. SQL 기본](#sql-기본)
  1.  [데이터 모델의 이해](#데이터-모델의-이해)
  2. [관계형 데이터 베이스 개요](#관계형-데이터-베이스-개요)
  3. [DDL](#ddl)
  4. [DML](#dml)
  5. [TCL](#tcl)
  6. [WHERE](#where)
  7. [함수](#함수)
  8. [GROUP By, Having](#group-by-and-having)
  9. [ORDER BY](#order-by)
  10. [JOIN](#join)

[Chapter 2. SQL 활용](#sql-활용)
- [Subject : SQL Basics and Implement](#subject--sql-basics-and-implement)
  - [Contents](#contents)
  - [SQL 기본](#sql-기본)
    - [관계형 데이터 베이스 개요](#관계형-데이터-베이스-개요)
    - [DDL](#ddl)
    - [DML](#dml)
    - [TCL](#tcl)
    - [WHERE](#where)
    - [함수](#함수)
    - [GROUP By and Having](#group-by-and-having)
    - [ORDER BY](#order-by)
    - [JOIN](#join)
  - [SQL 활용](#sql-활용)
    - [STANDARD JOIN](#standard-join)
    - [SET OPERATOR](#set-operator)
    - [계층형 질의와 셀프 조인](#계층형-질의와-셀프-조인)
    - [서브쿼리](#서브쿼리)
    - [GROUP Function](#group-function)
    - [WINDOW Function](#window-function)
    - [DCL](#dcl)
    - [절차형 SQL](#절차형-sql)


<br>

---
<br>

## SQL 기본
<br>

### 관계형 데이터 베이스 개요
1. 데이터베이스의 발전 과정과 관계형 데이터베이스의 기능에 대해 알고있다.
2. SQL과 SQL의 4가지 종류를 알고있다.
3. 테이블과, 행, 열, 정규화, 기본키, 외부키, 와 같은 용어들에 대해 알고있다.
4. ERD 를 읽을 수 있다.

### DDL
- 데이터 유형: CHAR과 VARCHAR의 차이점을 이해하고 NUMERIC, DATETIME에 대해 이해한다.
```
CHAR (CHAR)
VARCHAR (VARCHAR2)
NUMERIC (NUMBER)
DATETIME
```

- 테이블 생성: CREATE TABLE
```
CREATE TABLE PLAYER
( 
    PLAYER_ID CHAR(7) NOT NULL,
    PLAYER_NAME VARCHAR2(20) NOT NULL,
    TEAM_ID CHAR(3) NOT NULL,
    E_PLAYER_NAME VARCHAR2(40),
    NICKNAME VARCHAR2(30),
    JOIN_YYYY CHAR(4),
    POSITION VARCHAR2(10),
    BACK_NO NUMBER(2),
    NATION VARCHAR2(20),
    BIRTH_DATE DATE,
    SOLAR CHAR(1),
    HEIGHT NUMBER(3),
    WEIGHT NUMBER(3),
    CONSTRAINT PLAYER_PK PRIMARY KEY (PLAYER_ID),
    CONSTRAINT PLAYER_FK FOREIGN KEY (TEAM_ID) REFERENCES TEAM(TEAM_ID)
); 

CREATE TABLE TEAM
( 
    TEAM_ID CHAR(3) NOT NULL,
    REGION_NAME VARCHAR2(8) NOT NULL,
    TEAM_NAME VARCHAR2(40) NOT NULL,
    E_TEAM_NAME VARCHAR2(50),
    ORIG_YYYY CHAR(4),
    STADIUM_ID CHAR(3) NOT NULL,
    ZIP_CODE1 CHAR(3),
    ZIP_CODE2 CHAR(3),
    ADDRESS VARCHAR2(80),
    DDD VARCHAR2(3),
    TEL VARCHAR2(10),
    FAX VARCHAR2(10),
    HOMEPAGE VARCHAR2(50),
    OWNER VARCHAR2(10),
    CONSTRAINT TEAM_PK PRIMARY KEY (TEAM_ID),
    CONSTRAINT TEAM_FK FOREIGN KEY (STADIUM_ID) REFERENCES STADIUM(STADIUM_ID)
); 
```


- 제약조건: PRIMARY KET, UNIQUE KEY, NOT NULL, CHECK, FOREIGN KEY
- 생성된 테이블 구조 확인: DESC PLAYER;
- SELECT 문장을 통한 테이블 생성:
```
CREATE TABLE TEAM_TEMP
AS SELECT * FROM TEAM;
```

- ALTER TABLE: ADD COLUMN, DROP COLUMN, MODIFY COLUMN, RENAME COLUMN, DROP CONSTRAINT, ADD CONSTRAINT
```
ALTER TABLE PLAYER
ADD (ADDRESS VARCHAR2(80));

ALTER TABLE PLAYER
DROP COLUMN ADDRESS;

ALTER TABLE TEAM_TEMP
MODIFY (ORIG_YYYY VARCHAR2(8) DEFAULT '20020129' NOT NULL);

ALTER TABLE PLAYER
RENAME COLUMN PLAYER_ID TO TEMP_ID;

ALTER TABLE PLAYER
DROP CONSTRAINT PLAYER_FK;

ALTER TABLE PLAYER
ADD CONSTRAINT PLAYER_FK FOREIGN KEY (TEAM_ID) REFERENCES TEAM(TEAM_ID);
```

- REANEM TABLE
```
RENAME TEAM TO TEAM_BACKUP;
```

- DROP TABLE
```
DROP TABLE PLAYER;
```

- TRUNCATE TABLE
```
TRUNCATE TABLE TEAM;
```

### DML
0. 칼럼의 데이터 유형이 문자 유형일 경우 ' SINGLE QUOTATION으로 입력할 값을 입력한다. 숫자일 경우 붙이지 않아야 한다.
칼럼 별명을 적용할 때 별명 중간에 공백이 들어가는 경우 "" 를 사용해야 한다.

1. INSERT: 테이블에 데이터를 입력하는 방법은 두 가지 유형이 있으며 한 번에 한 건만 입력된다.
```
INSERT INTO PLAYER
(PLAYER_ID, PLAYER_NAME, TEAM_ID, POSITION, HEIGHT, WEIGHT, BACK_NO)
VALUES ('2002007', ' ', 'K07', 'MF', 178, 73, 7);

INSERT INTO PLAYER
VALUES ('2002010','이청용','K07','','BlueDragon','2002','MF','17',NULL, NULL,'1',180,69); 
```

2. UPDATE
```
UPDATE PLAYER
SET BACK_NO = 99;
```

3. DELETE
```
DELETE FROM PLAYER;
```

4. SELECT
```
SELECT PLAYER_ID, PLAYER_NAME, TEAM_ID, POSITION, HEIGHT, WEIGHT, BACK_NO FROM PLAYER;

SELECT ALL POSITION FROM PLAYER;

SELECT DISTINCT POSITION FROM PLAYER;

SELECT * FROM PLAYER;

SELECT PLAYER_NAME AS 선수명 FROM PLAYER;
칼럼 별명에서 를 꼭 사용하지 않아도 되므로 아래 SQL은 위SQL 과 같은 결과를 출력한다.
SELECT PLAYER_NAME 선수명 FROM PLAYER;

칼럼 별명을 적용할 때 띄어쓰기나 기호가 들어가는 경우 ""를 이용해야 한다.
```

5. 산술연산자, 합성연산자
- 합성 연산자: 칼럼과 문자 또는 다른 칼럼과 연결시킨다.
- 문자 표현식의 결과에 의해 새로운 칼럼을 생성한다.
```
SELECT PLAYER_NAME 이름, HEIGHT - WEIGHT "키-몸무게" FROM PLAYER;

SELECT PLAYER NAME || '선수, '|| HEIGHT 'cm, ' || WEIGHT || 'kg' 체격정보 FROM PLAYER;
```


### TCL

### WHERE

### 함수

### GROUP By and Having

### ORDER BY

### JOIN

<br>

---
## SQL 활용
<br>

### STANDARD JOIN

### SET OPERATOR

### 계층형 질의와 셀프 조인

### 서브쿼리

### GROUP Function

### WINDOW Function

### DCL

### 절차형 SQL
