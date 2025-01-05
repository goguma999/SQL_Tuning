## INNER JOIN 
```sql
SELECt *
FROM mypokemon A
INNER JOIN ability B
ON A.number = B.number;
```

## LEFT/RIGHT JOIN 
```sql
SELECt *
FROM mypokemon A
LEFT JOIN ability B
ON A.number = B.number;
```

## OUTER JOIN 
- MySQL에서는 LEFT JOIN + UNION RIGHT JOIN 해야함 
```sql
SELECT *
FROM mypokemon A
LEFT JOIN ability B
ON A.number = B.number
UNION
SELECt *
FROM mypokemon A
RIGHT JOIN ability B
ON a.number = B.number;
```

## CROSS JOIN
```sql
SELECT *
FROM mypokemon
CROSS JOIN ability;
```

## SELF JOIN 
- 테이블 별칭 필수
```sql
SELF *
FROM mypokemon t1
INNER JOIN mypokemon t2
ON t1.number = t2.number;
```















