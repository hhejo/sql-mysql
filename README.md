# SQL with MySQL

## 1. 데이터베이스

```sql
SHOW DATABASES; -- 데이터베이스 목록 조회
DROP DATABASE IF EXISTS mydb; -- 데이터베이스가 있다면 삭제
CREATE DATABASE mydb; -- 데이터베이스 생성
USE mydb; -- 데이터베이스 사용
```

## 2 - 1. 테이블 생성, 조회, 삭제

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

## 2 - 2. 테이블 컬럼 조작

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
