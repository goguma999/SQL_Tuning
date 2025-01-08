# Constraint(MySQL) 
## PRIMARY KEY
- NOT NULL + UNIQUE 한 컬럼
- 자동으로 인덱스가 만들어짐
- 테이블 만들 때 PK 생성
```sql
CREATE TABLE new_mypokemon(
	number INT PRIMARY KEY,    
	name VARCHAR(20)
	); 
```
```sql
CREATE TABLE new_mypokemon(
	number INT,
	name VARCHAR(20),
	PRIMARY KEY (number)
	);
```
```sql
CREATE TABLE new_mypokemon(
  number INT,
  name VARCHAR(20),
  CONSTRAINT pk_number PRIMARY KEY (number)
  );
```
- 만들어진 테이블에 PK 추가
```sql
# 중복 데이터, NULL값 처리 후
ALTER TABLE new_mypokemon
ADD PRIMARY KEY (number);
```
```sql
# 중복 데이터, NULL값 처리 후
ALTER TABLE new_mypokemon
ADD CONSTRAINT pk_number PRIMARY KEY (number);
```
- PK 삭제
```sql
ALTER TABLE new_mypokemon
DROP PRIMARY KEY;
```

## FOREIGN KEY
- 다른 테이블의 특정 컬럼을 참조함
- 참조하는 테이블의 PK여야 함 
- 테이블 만들 때 PK, FK 같이 생성 
```sql
CREATE TABLE new_mypokemon(
	number INT,
	name VARCHAR(20),
	PRIMARY KEY (number),
	FOREIGN KEY (number) REFERENCES mypokemon(number)
	);
```
- 만들어진 테이블에 FK 추가
```sql
ALTER TABLE new_mypokemon
ADD CONSTRAINT fk_number 
FOREIGN KEY (number) REFERENCES mypokemon(number);
``` 
- FK 삭제
```sql
#FK이름 확인
SHOW CREATE TABLE new_mypokemon;

ALTER TABLE new_mypokemon
DROP FOREIGN KEY fk_number;  #FK이름
```

## UNIQUE
- 자동으로 인덱스가 만들어짐
- 테이블 만들 때 생성
```sql
CREATE TABLE new_mypokemon(
	number INT,
	name VARCHAR(20) UNIQUE
  );
```
```sql
CREATE TABLE new_mypokemon(
	number INT,
	name VARCHAR(20),
  CONSTRAINT unique_name UNIQUE (name) 
  );
```
- 만들어진 테이블에 추가
```sql
ALTER TABLE new_mypokemon
ADD UNIQUE (name);
```
```sql
ALTER TABLE new_mypokemon
ADD CONSTRAINT unique_name UNIQUE (name);
```
- 삭제
```sql
ALTER TABLE new_mypokemon
DROP INDEX name;
```

## NOT NULL
- 테이블 만들 때 생성
```sql
Create table new_mypokemon(
number INT,
name VARCHAR(20) UNIQUE,
type VARCHAR(10) NOT NULL
);
```
- 만들어진 테이블에 추가
```sql
ALTER TABLE new_mypokemon
MODIFY COLUMN tyep VARCHAR(10) NOT NULL;
```
- 삭제
```sql
ALTER TABLE new_mypokemon
MODIFY COLUMN type VARCHAR(10) NULL;
```

## DEFAULT
- 테이블 만들 때 생성
```sql
Create table new_mypokemon(
number INT,
name VARCHAR(20) UNIQUE,
type VARCHAR(10) NOT NULL,
attack INT DEFAULT 0
);
```
- 만들어진 테이블에 추가
```sql
ALTER TABLE new_mypokemon
MODIFY COLUMN attack INT DEFAULT 0;
```
- 삭제
```sql
ALTER TABLE new_mypokemon
ALTER COLUMN attack DROP DEFAULT;
```

## CHECK (MySQL 8.0 이상)
```sql
ALTER TABLE new_mypokemon
ADD CONSTRAINT chk_attack CHECK (attack >= 0);
```

## 모든 CONSTRAINT 확인
```sql
SHOW CREATE TABLE new_mypokemon;
```
```sql
SELECT CONSTRAINT_NAME, CONSTRAINT_TYPE
FROM information_schema.TABLE_CONSTRAINTS
WHERE TABLE_NAME = 'new_mypokemon';
```











