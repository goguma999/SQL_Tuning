# MySQL - DDL: CREATE, ALTER, DROP, TRUNCATE
## CREATE, ALTER
테이블 만들기
```
CREATE TABLE idol(
    name VARCHAR(20),
    age INT,
    group VARCHAR(50)
);
```

테이블 이름 변경하기
```
ALTER TABLE customor RENAME customers;
```
컬럼 추가하기
```
ALTER TABLE customers ADD COLUMN age INT;
```
컬럼 타입 변경하기
```
ALTER TABLE customers MODIFY COLUMN age FLOAT;
```
컬럼 이름과 타입 변경하기
```
ALTER TABLE customers CHANGE COLUMN age new_age FLOAT;
```
컬럼 지우기
```
ALTER TABLE customers DROP COLUMN new_age;
```
## DROP, TRUNCATE
데이터베이스 지우기
```
DROP DATABASE ___;
```
테이블 지우기
```
DROP TABLE ___;
```
테이블 값만 지우기
```
TRUNCATE TABLE ___;
```
데이터베이스/테이블이 존재한다면 지우기
```
DROP DATABASE IF EXISTS ___;
DROP TABLE IF EXISTS ___;
```













  
