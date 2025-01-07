# 서브쿼리 

## SELECT절의 서브쿼리(스칼라 서브쿼리)
- 반드시 결과값이 하나의 값이어야 함
```sql
SELECT number, name,
(SELECT height FROM ability WHERE number=25) AS height
FROM mypokemon 
WHERE name='pikachu';
```

## FROM절의 서브쿼리(인라인 뷰 서브쿼리)
- 반드시 결과값이 하나의 테이블이어야 함
- 서브쿼리로 만든 테이블은 반드시 별명을 가져야 함
```sql
SELECT number, height_rank
FROM (SELECT number, rank() OVER(ORDER BY height DESC) AS height_rank 
	FROM ability) AS A
WHERE height_rank = 3; 
```

## WHERE절의 서브쿼리(중첩 서브쿼리)
- 반드시 결과값이 하나의 컬럼이어야 함
- 단 EXISTS는 결과값이 여러 컬럼이어도 됨
```sql
SELECT number
FROM ability
WHERE height < (SELECT AVG(height) FROM ability);

SELECT number 
FROM ability
WHERE attack < ALL(SELECT attack FROM ability WHERE type='electric');

SELECT number
FROM ability
WHERE defense < ANY(SELECT attack FROM ability WHERE type='electric');

SELECT number
FROM ability
WHERE EXISTS(SELECT * FROM ability WHERE type ='bug');
```


## quiz
```sql
1. 내 포켓몬 중에 가장 많이 나가는 포켓몬의 번호를 가져오기
SELECT number 
FROM ability
WHERE weight = (SELECT max(weight) FROM ability); 

2. 공격력이 방어력보다 큰 포켓몬이 있다면 모든 포켓몬의 이름을 가져오기
SELECT name
FROM mypokemon 
WHERE EXISTS (SELECT number FROM ability WHERE attack > defense); 

3. 이브이의 번호 133을 활용해서, 이브비의 영문 이름, 키, 몸무게를 가져오기. 
SELECT name, (SELECT height FROM ability WHERE number = 133) as height, 
	(SELECT weight FROM ability WHERE number = 133) as weight
FROM mypokemon
WHERE number = 133;

4. 방어력이 모든 전기 포켓몬의 방어력보다 큰 포켓몬의 이름 가져오기
SELECT name 
FROM mypokemon
WHERE number IN (SELECT number FROM ability WHERE defense > ALL(SELECT defense FROM ability WHERE type = 'electric')); 
```
 







