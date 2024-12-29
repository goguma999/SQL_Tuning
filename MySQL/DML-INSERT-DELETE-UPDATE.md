# MySQL - DML: INSERT, DELETE, UPDATE
데이터 여러 개 삽입하기
```
INSERT idol(name, age, group)
VALUES('제니',28,'블랙핑크'),
      ('해찬',24,'NCTdream');
```
데이터 삭제하기
```
DELETE FROM idol
WHERE (조건);
```
데이터 수정하기
```
UPDATE idol 
SET (컬럼명) = (수정할 값)
WHERE (조건);
```
