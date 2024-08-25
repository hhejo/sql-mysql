# SQL with MySQL

## 1. 데이터베이스

```sql
SHOW DATABASES; -- 데이터베이스 목록 조회
DROP DATABASE IF EXISTS mydb; -- 데이터베이스가 있다면 삭제
CREATE DATABASE mydb; -- 데이터베이스 생성
USE mydb; -- 데이터베이스 사용
```

## 2-1. 테이블 생성, 조회, 삭제

```sql
CREATE TABLE product (
MYKEY INT UNSIGNED NOT NULL AUTO_INCREMENT,
PRODUCT_ID TEXT,
TITLE VARCHAR(50),
PRICE INT UNSIGNED,
DISCOUNT INT,
DELIVERY TEXT,
PRIMARY KEY(MYKEY)
);
DESC product; -- 테이블 내림차순 조회 정렬
SHOW TABLES; -- 테이블 목록 조회
DROP TABLE IF EXISTS product; -- 테이블이 있다면 삭제
```

```sql
SHOW TABLES;
CREATE TABLE customer (
no INT UNSIGNED NOT NULL AUTO_INCREMENT,
name CHAR(20) NOT NULL,
age TINYINT,
phone VARCHAR(20),
email VARCHAR(30) NOT NULL,
address VARCHAR(50),
PRIMARY KEY(no)
);
DESC customer;
```

## 2-2. 테이블 컬럼 조작

```sql
ALTER TABLE mytable ADD COLUMN newcol VARCHAR(10) NOT NULL; -- 컬럼 추가
ALTER TABLE mytable MODIFY COLUMN col VARCHAR(20) NOT NULL; -- 컬럼 타입 변경
ALTER TABLE mytable CHANGE COLUMN col1 col2; -- 컬럼 이름 변경
ALTER TABLE mytable CHANGE COLUMN col1 col2 VARCHAR(10); -- CHANGE로 타입까지도 변경 가능
ALTER TABLE mytable DROP COLUMN col; -- 컬럼 삭제
```

```sql
SHOW DATABASES;
DROP DATABASE IF EXISTS mydata;
CREATE DATABASE mydata;
USE mydata;

CREATE TABLE mytable (
id INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
title VARCHAR(10) NOT NULL,
content VARCHAR(40),
PRIMARY KEY(id)
);
DESC mytable;

ALTER TABLE mytable MODIFY COLUMN title VARCHAR(15) NOT NULL;
ALTER TABLE mytable CHANGE COLUMN content VARCHAR(50) NOT NULL;
ALTER TABLE mytable CHANGE COLUMN content con VARCHAR(40) NOT NULL;
```

## 3-1. 데이터 입력

```sql
USE mydata;
SHOW TABLES;
DESC mytable;

-- id, name, age, language

INSERT INTO mytable VALUES( 1, 'hejo', 31, 'javascript');
SELECT * FROM mytable;

INSERT INTO mytable (name, age, language) VALUES('hijo', 21, 'python');
```

## 3-2. 데이터 검색

```sql
USE mydata;
SHOW TABLES;
DESC mytable;

SELECT * FROM mytable;

SELECT nickname, lang FROM mytable;

SELECT nickname AS n_name FROM mytable;

SELECT age, n_name FROM mytable ORDER BY id DESC;
SELECT lang FROM mytable ORDER BY age;
SELECT n_name, lang FROM mytable ORDER BY id ASC;

SELECT age, n_name FROM mytable WHERE age < 18 DESC;
SELECT lang, age FROM mytable WHERE age = 20;
SELECT n_name FROM mytable WHERE age > 33 ASC;

SELECT age, n_name FROM mytable WHERE age < 20 AND age > 10;
SELECT age, n_name FROM mytable WHERE age = 30 OR n_name = 'hello';

SELECT id, n_name FROM mytable WHERE name LIKE 'o';
SELECT id, n_name FROM mytable WHERE name LIKE 'o%';
SELECT id, n_name FROM mytable WHERE name LIKE '%o';
SELECT id, n_name FROM mytable WHERE name LIKE '%o%';
SELECT id, n_name FROM mytable WHERE name LIKE 'o__';
SELECT id, n_name FROM mytable WHERE name LIKE '_o';

SELECT * FROM mytable LIMIT 5; -- 처음부터, 5개만
SELECT * FROM mytable LIMIT 100, 10; -- 101번째부터, 10개만
```

조합 순서

```
SELECT FROM WHERE ORDER BY LIMIT
```

## 3-3. 데이터 수정

```sql
UPDATE mytable SET n_name = 'hi' WHERE id = 1;

UPDATE mytable SET n_name = 'hi', age = 24 WHERE id = 2;
```

## 3-4. 데이터 삭제

```sql
DELETE FROM mytable WHERE id = 1;

DELETE FROM mytable; -- 모든 데이터 삭제
```

## 4. 데이터 분석

`LIMIT`

- 결과 개수 제한

```sql
SELECT * FROM tb LIMIT 10;
```

`COUNT`

- 데이터 행의 수

```sql
SELECT COUNT(*) FROM tb;
```

`DISTINCT`

- 특정 컬럼값 출력 시 중복된 값을 출력하지 않음
- 유일한 컬럼값 확인

```sql
SELECT DISTINCT col FROM tb;
```

`SUM`

- 특정 컬럼값의 합계

```sql
SELECT SUM(col) FROM tb;
```

`AVG`

- 특정 컬럼값의 평균

```sql
SELECT AVG(col) FROM tb;
```

`MAX`

- 특정 컬럼값의 최댓값

```sql
SELECT MAX(col) FROM tb;
```

`MIN`

- 특정 컬럼값의 최솟값

```sql
SELECT MIN(col) FROM tb;
```

`GROUP BY`

- 특정 컬럼값을 기반으로 그룹핑

```sql
SELECT col1 FROM tb GROUP BY col2;
SELECT COUNT(*) FROM tb GROUP BY col;
```

`ORDER BY`

- 특정 컬럼값을 기준으로 데이터 정렬

```sql
SELECT * FROM tb ORDER BY col;
SELECT * FROM tb ORDER BY col DESC;
SELECT * FROM tb ORDER BY col ASC;
```

`AS`

- 표시할 컬럼명 변경

```sql
SELECT COUNT(*) AS new FROM tb;
```

SQL 조건 순서

```
SELECT 컬럼
FROM 테이블
WHERE 조건
GROUP BY 컬럼
ORDER BY 컬럼
LIMIT
```

## 5. FOREIGN KEY

`FOREIGN KEY`

```sql
DROP DATABASE IF EXISTS db;
CREATE DATABASE db;
USE db;

-- userTb
DROP TABLE IF EXISTS userTb;
CREATE TABLE userTb (
  userid CHAR(8) NOT NULL PRIMARY KEY,
  username VARCHAR(10) UNIQUE NOT NULL,
  birthyear INT NOT NULL,
  addr CHAR(2) NOT NULL,
  phone CHAR(8),
  height SMALLINT,
  mdate DATE,
  -- UNIQUE INDEX idx_userTb_name (name),
  -- INDEX idx_userTb_addr (addr)
);

-- buyTb
DROP TABLE IF EXISTS buyTb;
CREATE TABLE buyTb (
  num INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
  userid CHAR(8) NOT NULL,
  prodname CHAR(4),
  groupname CHAR(4),
  price INT NOT NULL,
  amount SMALLINT NOT NULL,
  FOREIGN KEY (userid) REFERENCES userTb(userid)
);

-- INSERT userTb
INSERT INTO userTb VALUES('HHJ', '하하주', 1990, '서울', '12344321', '2024-1-1');

-- INSERT buyTb
INSERT INTO buyTb (userid, prodname, groupname, price, amount) VALUES('HHJ', '냉장고', '가전', 40, 7);

-- DELETE 에러 (buyTb에 해당 userid를 참조하는 데이터 존재)
DELETE FROM userTb WHERE userid = 'HHJ';
```

## 6. HAVING

`HAVING`

- 집계함수를 가지고 조건비교를 할 때 사용
- `GROUP BY`와 함께 사용

```sql
SELECT col FROM tb GROUP BY col HAVING COUNT(*) > 50;
```

## 7. JOIN

`JOIN`

- 두 개 이상의 테이블로부터 필요한 데이터를 연결해 하나의 포괄적인 구조로 결합시키는 연산
- `INNER JOIN`, `OUTER JOIN`이 있음

`INNER JOIN`

- 두 테이블에 해당 필드값이 매칭되는 (두 테이블의 모든 필드로 구성된) 레코드만 가져옴
- 조인하는 테이블의 `ON` 절의 조건이 일치하는 결과만 출력

```sql
SELECT * FROM tb1 JOIN tb2 ON tb2.id = tb1.id;

SELECT * FROM tb1 INNER JOIN tb2 ON tb2.id = tb1.id WHERE tb2.age > 20;

SELECT * FROM tb1 a JOIN tb2 b ON a.id = b.id;
```

`OUTER JOIN`

- `LEFT OUTER JOIN`, `RIGHT OUTER JOIN`이 있음

`LEFT OUTER JOIN`

- 왼쪽 테이블에서 모든 레코드와 함께, 오른쪽 테이블에 왼쪽 테이블의 레코드와 매칭되는 레코드를 붙여서 가져옴

```sql
SELECT * FROM tb1 a LEFT OUTER JOIN tb2 b ON a.id = b.id;
```

`RIGHT OUTER JOIN`

- 오른쪽 테이블에서 모든 레코드와 함께, 왼쪽 테이블에 왼쪽 테이블 레코드와 매칭되는 레코드를 붙여서 가져옴

```sql
SELECT * FROM tb1 a RIGHT OUTER JOIN tb2 b ON a.id = b.id;
```

## 8. Sub Query

SubQuery

- SQL문 안에 포함된 SQL문
- SQL문 안에서 괄호를 사용해 서브쿼리문 추가 가능
- 테이블과 테이블 간의 검색 시, 검색 범위(테이블 중 필요한 부분만 먼저 가져오도록)를 좁히는 기능에 주로 사용
- JOIN은 출력 결과에 여러 테이블의 열이 필요한 경우에 유용
- 대부분의 서브쿼리는 JOIN문으로 처리 가능

```sql
SELECT col FROM tb1 INNER JOIN tb2 ON tb1.code = tb2.code WHERE tb2.age = 30;

SELECT col FROM tb1 WHERE code IN (SELECT code FROM tb2 WHERE tb2.age = 20);
```
