# IF, IFNULL, CASE, COALSCE (MySQL)
## IF 
```sql
SELECT name, IF(attack >= 60, 'strong', 'weak') AS attack_class
FROM mypokemon;
```
## IFNULL
```sql
SELECT name, IFNULL(name, 'unknown') AS full_name
FROM mypokemon;
```
## CASE
```sql     
SELECT name,
CASE
  WHEN attack >= 60 THEN 'strong'
  ELSE 'weak'
  END AS attack_class
FROM mypokemon;
```
```sql
SELECT name,
CASE type
  WHEN 'bug' THEN 'grass'
  WHEN 'electric' THEN 'water'
  WHEN 'grass' THEN 'bug'
  END AS rival_type
FROM mypokemon;
```
## COALESCE: 여러 열 또는 값 중 NULL이 아닌 첫 번째 값 반환 
```sql
SELECT 
    id,
    COALESCE(phone_number, email, 'No Contact Info') AS contact_info
FROM users;
```
&nbsp;

# NVL, NVL2, DECODE, CASE, COALSCE (Oracle)
## NVL     
```sql
SELECT ename, nvl(to_char(mgr),'no manager')    # 첫 번째 인자의 type에 맞추기
FROM emp;
```
## NVL2(comm, A, B): comm이 null이 아니면 A, null이면 B
```sql     
SELECT ename, sal, comm, nvl2(comm, sal+comm, sal)
FROM emp;
```
## DECODE
```sql
DECODE(컬럼명, A조건에 해당하는 값, A조건 만족 때 출력값,
             B조건에 해당하는 값, B조건 만족 때 출력값,
             그 외 출력값)
```
```
select ename, sal, job, 
			decode(job, 'SALESMAN', 5000, 'ANALYST',4000,100) as 보너스 
from emp; 
```







